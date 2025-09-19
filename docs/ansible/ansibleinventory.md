# Hostide haldus *inventory* abil

## Eesmärk

Selles peatükis õpid:

- Mis on Ansible *inventory* ja miks see on oluline
- Kuidas luua *inventory* fail (INI ja YAML)
- Kuidas lisada hoste, gruppe ja muutujaid
- Kuidas kontrollida ja hallata *inventory*'t

---


## Mis on *inventory*?

*Inventory* on fail või failide kogum, kus on kirjas hallatavad hostid ja nende omadused. *Inventory* on Ansible tööde aluseks – see määrab, milliste masinate või teenustega Ansible ühendub ja milliseid ülesandeid neile saadetakse. 

*Inventory* saab olla loodud:

- **Staatiliselt** – näiteks `INI` või `YAML` failina.
- **Dünaamiliselt** – skripti või plugina kaudu, mis pärib hostide info automaatselt (nt pilveteenuse API kaudu).
- **Kombineeritud** – osa hoste on staatilises failis, osa hangitakse dünaamiliselt.

Siin vaatame põhiliselt staatilist *inventory* loomist, mida kasutatakse kõige enam.

---

## *Inventory* faili formaadid

### INI formaat (lihtsam)

Klassikaline inventory formaat: 

- Lihtne ja kiire alustada – sobib hästi algajatele, väikeste projektide või testkeskkondade jaoks.
- Vähem süntaksireegleid – ei pea muretsema taandete pärast (erinevalt YAML-ist).

Näide:
```ini
[webservers]
web1.example.com
web2.example.com
192.168.1.23
192.168.1.6

[dbservers]
db1.example.com ansible_user=admin
```

### YAML formaat (soovitatav)

YAML formaadis *inventory* faili kasutamisel on mitmeid eeliseid võrreldes klassikalise INI-vorminguga, eriti kui õpetad või töötad suuremate ja keerulisemate Ansible keskkondadega:

- Selgem ja loetavam
- Sobib keerukate struktuuride jaoks
- Tulevikukindel (Ansible liigub YAML-i suunas)

Näide:
```yaml
all:
  children:
    webservers:
      hosts:
        web1.example.com:
        web2.example.com:
      vars:
        http_port: 80
    dbservers:
      hosts:
        db1.example.com:
          ansible_host: 192.168.1.50
```          

!!! warning
    YAML-is tuleb kasutada taanete jaoks tühikuid, mitte TAB-e. Vale taandus = viga. Iga järgmise taseme element = 2 tühikut taanet.


---

## Grupid ja vaikimisi grupid

**Grupp** on inventory failis loogiline kogum hoste (servereid), millel on sarnane roll või funktsioon. 

Gruppide abil saab:

- Hallata mitut hosti korraga – näiteks paigaldada sama tarkvara kõigile veebiserveritele.
- Määrata ühiseid muutujaid – näiteks ansible_user, ansible_python_interpreter või rakenduse konfiguratsioon.
- Luua hierarhiaid – grupp võib sisaldada teisi gruppe (nt all → webservers ja dbservers).

Ansible lisab automaatselt iga *inventory*-sse kaks gruppi:

- **`all`** – sisaldab kõiki hoste
- **`ungrouped`** – sisaldab hoste, mis ei kuulu ühtegi teise gruppi

See tähendab, et ka üksik host ilmub nii `all` kui `ungrouped` nimekirjas. Playbookides saab neid gruppe ka vajadusel kasutada.

---

## Vaikimisi *inventory* fail

Traditsiooniliselt otsib Ansible *inventory*'t failist:
```
/etc/ansible/hosts
```
See kehtib **peamiselt siis, kui Ansible on paigaldatud süsteemi tasemel paketihalduri (nt apt, yum) kaudu**.

Kui paigaldad Ansible'i **pip** või **pipx** abil, siis:

- Ansible ei kasuta vaikimisi `/etc/ansible/hosts`
- Kui sa Ansible käsu puhul `-i` argumenti ei määra ega konfigureeri `ansible.cfg` faili, on vaikimisi *inventory*'ks ainult `localhost`
- Süsteemne `/etc/ansible/ansible.cfg` fail puudub, seega peab seadistuse tegema projekti või kasutaja tasemel

Vaikimisi *inventory* otsimise järjekord pip/pipx puhul on:

1. **Käsurea `-i` argument**
2. **Keskkonnamuutuja `ANSIBLE_INVENTORY`**
3. **`ansible.cfg`** (projekti kaustas või `~/.ansible.cfg`)
4. Kui midagi eelnevat pole, kasutatakse ainult `localhost`


---
## *Inventory* kontroll ja visualiseerimine

`ansible-inventory` on kasulik *inventory* kontrollimiseks.

Näited:
```bash
ansible-inventory -i inventory.yml --list
ansible-inventory -i inventory.yml --graph
```

- `--list` – kuvab kogu *inventory* JSON-vormingus
- `--graph` – näitab gruppide ja hostide struktuuri puuna

Konkreetse hosti kohta *inventory* vaatamine:
```bash
ansible-inventory -i inventory.yml --host web1.example.com
```
---

## *Inventory* kataloog ja mitme allika kasutamine

*Inventory* ei pea asuma ainult ühes failis – Ansible toetab *inventory* katalooge, mis võimaldavad vajadusel jagada hostide nimekirja mitmeks failiks. *Inventory* jaoks eraldi kataloogi kasutamine on hea praktika, eriti suuremates projektides. 

Ansible võib lugeda ***inventory* kataloogi**, mis sisaldab mitut faili:
```
inventory/
├── aws.yml
├── azure.yml
├── hosts.ini
```
Failid loetakse **tähestiku järjekorras**, mis võib mõjutada grupisuhted ja muutujate prioriteete. Nagu näitest näha on, siis failiformaate võib ka segada.

---

## Hostide ja gruppide muutujad

Muutujad (variables) võimaldavad määrata väärtusi, mida Ansible kasutab. Need võivad olla näiteks:

- SSH kasutajanimi (ansible_user)
- SSH võtme asukoht (ansible_ssh_private_key_file)
- Rakenduse konfiguratsioon (nt http_port, db_name)

**Hosti muutujad (host_vars)** – kehtivad ainult ühele hostile.
**Grupi muutujad (group_vars)** – kehtivad tervele grupile.

`group_vars/` ja `host_vars/`
- Kui need kaustad asuvad samas asukohas *inventory* failiga, loeb Ansible need automaatselt.
- Kui *inventory* on eraldi kataloog, peavad `group_vars/` ja `host_vars/` olema **esimese taseme alamkaustad**.

Näide:
```
inventory/
├── hosts.yml
├── group_vars/
│   └── web.yml
└── host_vars/
    └── db1.example.com.yml
```

*Inventory* faili sees saab samuti saab määrata muutujaid. Näide (`INI` ja `YAML`):

```ini
[web]
web1 ansible_user=deploy ansible_port=2222
web2 ansible_user=deploy

[web:vars]
ansible_python_interpreter=/usr/bin/python3
```

```yaml
all:
  children:
    webservers:
      hosts:
        web1:
          ansible_host: 192.168.1.10
        web2:
          ansible_host: 192.168.1.11
      vars:
        http_port: 80
        ansible_python_interpreter: /usr/bin/python3
```

!!! info
    Kui sama muutuja on määratud mitmes kohas, kehtib järgmine prioriteet:  
    **host_vars** > **group_vars** > **all** grupi muutujad.  

    See tähendab, et konkreetsele hostile määratud väärtus **kirjutab alati üle** grupis või `all` grupis määratud väärtuse.


---

## Kuidas luua *inventoy*'t (INI ja YAML)?

**Loo uus fail:**

*Inventory* fail võib olla ükskõik millises kaustas, aga tavaliselt hoitakse seda projekti juurkaustas või eraldi `inventory/` kataloogis.
Faili nimi võib olla näiteks:

- `hosts.ini` (INI-formaat)
- `hosts.yml` (YAML-formaat)

**Lisa hostid ja grupid:**

INI-formaadis:
```ini
[webservers]
web1.example.com
web2.example.com

[dbservers]
db1.example.com
```

YAML-formaadis:
```yaml
all:
  children:
    webservers:
      hosts:
        web1.example.com:
        web2.example.com:
    dbservers:
      hosts:
        db1.example.com:
          ansible_host: 192.168.1.10
          ansible_port: 2222

```

!!! info
    - `children` YAML *inventory* failis tähendab alamgruppe ehk gruppide sees olevaid gruppe.
    - `hosts` all loetletakse kõik selle grupi masinad. Kui hostile on omadusi (nt IP, port), lisatakse need võtme-väärtuse paaridena.



**Lisa vajadusel muutujad**

```ini
[webservers:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=~/.ssh/id_rsa
```

```yaml
all:
  children:
    webservers:
      vars:
        ansible_user: ubuntu
        ansible_ssh_private_key_file: ~/.ssh/id_rsa
```

---

## Dünaamiline *inventory*

Dünaamiline *inventory* loob hostide nimekirja **reaalajas** (nt pilveteenuse API-st).  
Näiteks:
- AWS EC2 plugin
- Azure RM plugin
- OpenStack plugin

Eelised:
- Hostide nimekiri on alati värske
- Ei pea käsitsi *inventory*'t uuendama
- Saab grupeerida hoste automaatselt metatähiste (tagide) alusel

---

## Head tavad *inventory* haldamisel

- Hoia *inventory* versioonihalduses (nt Git), eriti kui tegemist on staatilise failiga.
- Kasuta gruppe, et vältida käskude kogemata valele masinale käivitamist.
- Erinevate keskkondade jaoks (test, staging, production) tee eraldi *inventory* failid või kaustad.
- Kui võimalik, automatiseeri *inventory* loomine (nt dünaamilise *inventory* pluginatega).
- Ära salvesta salasõnu **inventory** failidesse — kasuta Ansible Vault-i (vt. täpsemalt "Turvalisus ja Ansible Vault" materjalis).

---

## Harjutus

1. Loo projektikausta *inventory* kaust ja sinna alla fail `hosts.yml`, kuhu lisad vähemalt kaks gruppi.
2. Lisa gruppidele muutujad kasutades `group_vars/` kataloogi.
3. Kontrolli *inventory* faili:
   ```bash
   ansible-inventory -i hosts.yml --graph
   ```
4. Testi ühendust:
   ```bash
   ansible -i hosts.yml all -m ping
   ```

---

## Rohkem infot

[Ansible Inventory Guide](https://docs.ansible.com/ansible/latest/inventory_guide/index.html){:target="_blank"}  
[Dynamic Inventory Guide](https://docs.ansible.com/ansible/latest/inventory_guide/intro_dynamic_inventory.html){:target="_blank"}
