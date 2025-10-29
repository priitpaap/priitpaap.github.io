# Ansible rollid ja struktuur

## Eesmärk

Selles peatükis õpid:

- Mis on **rollid** ja miks neid kasutatakse
- Kuidas **rolli struktuur** töötab
- Kuidas **luua rolle** automaatselt `ansible-galaxy` käsuga
- Kuidas **kasutada rolle** playbookides
- Kuidas integreerida **muutujaid, malle, faile, handler'eid** rolli sees

---

## Mis on rollid?

Kui projektikaust ja playbookid muutuvad suureks ja keeruliseks, on raskem:

- analüüsida, mis kus toimub
- taaskasutada sama loogikat
- hoida koodi puhtana
- teha muudatusi ilma riske kasvatamata

**Rollid** on viis Ansible kood jagada eraldi, **hästi organiseeritud komponentideks**.

Rollis saab hoida:

- ülesandeid (tasks)
- muutujaid
- malle
- faile
- handler'eid
- pakette, seadistusi, teenuseid

Näiteks võib sul olla rollid nagu:

- `nginx`
- `docker`
- `users`
- `firewall`

Need muudavad koodi korduvkasutatavaks nii sinu kui ka teiste jaoks.

---

## Rolli kataloogistruktuur

Ansible roll on nagu kapseldatud töövoog, mis sisaldab kõiki vajalikke komponente (ülesanded, muutujad, mallid, failid jne), et täita kindlat konfiguratsioonieesmärki. Tüüpiline rolli struktuur näeb välja selline:

```yaml
myproject/
├─ roles/
│ └─ nginx/
│ ├─ tasks/
│ │ └─ main.yml
│ ├─ handlers/
│ │ └─ main.yml
│ ├─ templates/
│ │ └─ nginx.conf.j2
│ ├─ files/
│ ├─ vars/
│ │ └─ main.yml
│ ├─ defaults/
│ │ └─ main.yml
│ └─ meta/
│ └─ main.yml
└─ site.yml
```

**Rolli kõige olulisem fail on:**

`roles/ROLLINIMI/tasks/main.yml`

Siit käivitab Ansible rolli töövoo. See fail sisaldab ülesandeid, mida roll täidab.
Kui rolli kutsutakse roles: direktiiviga playbookis, siis Ansible alustab täitmist just sellest failist. Ehk **ülesanded (tasks), mis muidu oleks otse playbooki sees, liiguvad rollide puhul tasks/main.yml faili**.

Failis võivad olla:

- konkreetseid ülesandeid
- viiteid teistele failidele
- tingimuslikud käsud

---

## Rolli loomine `ansible-galaxy` käsuga

Rolli struktuuri ei pea käsitsi looma, seda saab automaatselt teha `ansible-galaxy` käsuga. Nii on palju lihtsam:

```bash
ansible-galaxy init nginx
```

See loob automaatselt kataloogi ja õiged failid. Antud näite puhul luuakse uus Ansible roll nimega *nginx*.

## Rolli kasutamine playbook'is

Kui kasutatakse rolle, siis projektikausta playbookis viidatakse neile `roles:` parameetriga. Playbook võib välja näha näiteks nii:

```yaml
---
- name: Veebiserverite seadistamine
  hosts: webservers
  become: yes

  roles:
    - nginx
```
Ansible otsib rolli kataloogist `roles/nginx/`.

### Mitme rolli kasutamine

Näide playbookist kus kasutusel on 4 erinevat rolli:

```yaml
---
- name: Serverite seadistamine
  hosts: webservers
  become: yes

  roles:
    - common
    - firewall
    - docker
    - nginx
```
Ansible täidab rollid ülalt alla järjestuses.

Kaustastruktuur näeks välja selline:

```yaml
myproject/
├─ server.yml
├─ inventory
└─ roles/
   ├─ common/
   ├─ nginx/
   ├─ docker/
   ├─ firewall/
```

### Rolli kasutamine handleri puhul

Järgnevalt näide rollist, kus kasutusel on `notify` koos `handler`-iga:

Fail `tasks/main.yml`:

```yaml
- name: Paigalda nginx konfiguratsioon
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
  notify: Taaskäivita nginx
```

Fail `handlers/main.yml`:

```yaml
- name: Taaskäivita nginx
  service:
    name: nginx
    state: restarted
```

See käivitab teenuse ainult siis, kui mall muutus.


## Rolli sees olevad põhikomponendid

### **`tasks/main.yml`**

Siia lähevad rolli põhitegevused. Näiteks:

```yaml
---
- name: Paigalda nginx
  apt:
    name: nginx
    state: present

- name: Käivita nginx
  service:
    name: nginx
    state: started
    enabled: true
```

### **`handlers/main.yml`**

Kui rollis on kasutusel *handlers*, siis pannakse need sellesse faili. Näiteks:

```yaml
---
- name: Taaskäivita nginx
  service:
    name: nginx
    state: restarted
```

### **`templates/`**

Kui rollis kasutatakse malle, siis siia kasuta pannakse mallifailid (Jinja2 .j2). Näiteks:

`nginx.conf.j2`


### Rolli muutujad

Rollides on kaks peamist muutujate kohta:

**`defaults/main.yml`**

- madalaim prioriteet
- saab hõlpsasti playbookis üle kirjutada

Näide faili sisust:

`nginx_port: 80`


**`vars/main.yml`**

- kõrgem prioriteet
- tavaliselt kasutatakse rollisisesteks konstantideks

| Prioriteet | Allikas                         | Kirjeldus |
|------------|----------------------------------|-----------|
| 1 (kõrgeim) | `--extra-vars`                  | Kõige kõrgema prioriteediga – need kirjutavad üle kõik muud väärtused. |
| 2           | Playbooki `vars:`               | `vars:` plokis määratud muutujad playbookis. |
| 3           | Inventari muutujad              | Hostivarad (`host_vars`), grupivarad (`group_vars`), inventarifailis määratud muutujad. |
| 4           | Rolli `vars/main.yml`           | Kõrgema prioriteediga kui `defaults`, kuid madalam kui playbooki `vars`. |
| 5 (madalaim) | Rolli `defaults/main.yml`       | Rolli kõige madalama prioriteediga muutujad. Kasutatakse vaikimisi väärtustena, mida saab hiljem üle kirjutada. |


## Ansible Galaxy rollid (võrgust)

Ansible Galaxy on Ansible'i ametlik veebipõhine keskkond, kust saab otsida, jagada ja installida valmis rolle. See toimib nagu pakihaldur Ansible rollide jaoks.

**Otsimine:**

Selle abil saab näiteks otsida võrgust rolli, mis paigaldab ja konfigureerib Nginx veebiserveri:

`ansible-galaxy search nginx`

See kuvab nimekirja rollidest, mis on seotud Nginx-iga – koos autorite, hinnangute ja kirjeldusega.

!!! info
    Galaxy rollid võivad olla erineva kvaliteediga – soovitatav on vaadata reitingu, allalaadimiste arvu ja dokumentatsiooni enne kasutamist.

**Installeerimine:**

Kui leiad sobiva rolli, saad selle lihtsalt installida oma projekti `roles/`kausta. Näiteks:

`ansible-galaxy install geerlingguy.nginx -p roles/`

See laeb rolli alla ja paigutab selle sinu projekti roles/ kausta (või teise määratud asukohta).

Pärast seda tekib sinu projekti selline struktuur:

```yaml
ansible/
├── playbook.yml
└── roles/
    └── geerlingguy.nginx/

```

Nüüd saad rolli kasutada playbook’is, näiteks:

```yaml
---
- hosts: web
  become: true
  roles:
    - geerlingguy.nginx
```

Kui kõik on valmis, käivita:

`ansible-playbook playbook.yml`

Roll teeb kõik vajaliku Nginxi paigaldamiseks ja seadistamiseks. 

Vajadusel kohanda rolli oma soovide järgi. Enne kasutamist vaata rolliga kaasas olevat `README.md` faili, et näha muutujaid ja näiteid.


**Rollide loomine ja jagamine**

Sa saad soovi korral sinu enda loodud rolli registreerida Galaxy keskkonnas, et teised saaksid seda kasutada. Selleks tuleb rollile lisada meta/main.yml fail, kus on info rolli kohta (autor, sõltuvused jne).

**Rollide halduse automatiseerimine**

Kasutades `requirements.yml` faili, saad määrata kõik vajalikud rollid ja nende versioonid. Näiteks:

```yaml
- src: geerlingguy.nginx
  version: 3.1.0
- src: git+https://github.com/sinuorg/rollid.git
  name: minu_roll
```
Ja installida need kõik korraga:

`ansible-galaxy install -r requirements.yml`


## Rollide sees teiste failidele viitamine

Ansible rollides on kindel kaustastruktuur, mis võimaldab viidata failidele lihtsalt nime järgi, ilma et peaks määrama täisteed. See teeb rollide kasutamise mugavamaks ja koodi loetavamaks.

Näiteks mall asukohas:

`roles/nginx/templates/nginx.conf.j2`

Playbookis või rolli taskis viitamine:

```yaml
- name: Loo Nginx konfiguratsioon
  ansible.builtin.template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
```
Ei pea andma täisteed – Ansible leiab rolli templates/ kataloogist automaatselt.


## Rohkem infot

- [Ansible Roles Documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html){:target="_blank"}

- [Ansible Galaxy](https://galaxy.ansible.com/ui/){:target="_blank"}