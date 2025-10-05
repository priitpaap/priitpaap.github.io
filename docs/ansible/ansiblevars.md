# Muutujad ja nende kasutamine

## Eesmärk

Selles peatükis õpid:

- Mis on Ansible muutujad ja miks neid kasutada
- Kuidas ja kus määrata muutujate väärtusi
- Kus muutujad asuvad (vars, group_vars, host_vars, käsurealt jne)
- Muutujate loomise head tavad
---

## Mis on muutujad?

**Muutujad** võimaldavad hoida korduvaid väärtusi ühes kohas. See muudab playbookid lihtsamaks, loetavamaks ja hooldatavamaks. Sageli on mugav hoida korduvaid väärtusi (nt kataloogiteed, paketid või pordid) muutujates. See teeb playbooki lühemaks ja hõlpsamini hallatavaks.

Muutujaid saab määrata mitmel tasemel. Kui sama muutuja on määratud mitmes kohas, peab Ansible otsustama, millist väärtust kasutada. Ansible kasutab **prioriteetide hierarhiat**, kus kõrgema taseme väärtus asendab madalama oma.  

Allpool on levinumad tasemed (madalaimast kõrgeimani):

| Tasand | Näide / Asukoht | Prioriteet |
|--------|-----------------|------------|
| Role defaults | `defaults/main.yml` | 🔽 madalaim |
| Inventory grupimuutujad | `group_vars/webservers.yml` | ↑ |
| Inventory hostimuutujad | `host_vars/web1.yml` | ↑ |
| Playbooki `vars:` | Playbooki sees määratud `vars:` | ↑ |
| Playbooki `vars_files:` | Playbooki sees viidatud eraldi failid | ↑ |
| `set_fact` ülesanded | Määratud käivitamise ajal | ↑ |
| Käsurea muutujad | `-e "var=value"` | 🔼 kõrgeim |

---

## Olulisemad muutujate määramise viisid

### Muutujate määramine Playbooki sees `vars:` all

Muutujad saab defineerida playbooki sees võtme `vars:` all.

```yaml
---
- name: Muutujate näide
  hosts: webservers
  become: yes

  vars:
    web_package: nginx
    web_root: /var/www/html

  tasks:
    - name: Paigalda veebiserver
      apt:
        name: "{{ web_package }}"
        state: present

    - name: Loo veebikataloog
      file:
        path: "{{ web_root }}/demo"
        state: directory
        owner: www-data
        group: www-data
        mode: '0755'
```

Selgitus:

- `vars:` all defineeritakse muutujad web_package ja web_root.
- Muutujaid kasutatakse topeltlainelistes sulgudes {{ }}.
- Kui muutuja väärtust hiljem muuta, ei pea seda igal pool käsitsi asendama.

!!! info
    Kui muutujad on pikemad või peavad kehtima mitmes playbookis, tasub need hoida eraldi failides (nt **group_vars/** või **host_vars/** kataloogides).


### Muutujad eraldi failis (group_vars või host_vars)

Kataloogistruktuur:
```
inventory/
├─ hosts.yaml
├─ group_vars/
│  └─ webservers.yml
```

Fail `group_vars/webservers.yml` sisu:
```yaml
web_package: nginx
web_root: /var/www/html
```
Playbook kasutab neid muutujad automaatselt, kui `hosts:` määratud grupp on `webservers`.

Kui vajad, et **erinevad hostid kasutaksid eri väärtusi**, on mugav kasutada `host_vars/` kataloogi.

```
inventory/
├─ hosts.yaml
├─ host_vars/
│  └─ web1.yml
```

Fail `host_vars/web1.yml` sisu:
```yaml
web_package: nginx
web_root: /var/www/html
```

Playbook kasutab neid muutujad automaatselt, kui `hosts:` määratud on `web1`.

!!! info
    host_vars/ kataloogis olev fail peab kandma sama nime kui inventory’s määratud host.    

### Muutujate määramine käsurealt

```bash
ansible-playbook site.yml -e "web_package=nginx"
```

### Muutujate määramine inventory failis

```ini
[webservers]
web1 web_package=nginx web_root=/var/www/html
```

```yaml
all:
  children:
    webservers:
      hosts:
        web1:
          ansible_host: 192.168.56.11
          web_package: nginx
          web_root: /var/www/html
```

---

## Muutujate kasutamine ülesannetes

Muutujaid kasutatakse Ansible playbookis **topeltlainelistes sulgudes** `{{ }}`.

Näide:

```yaml
- name: Loo kataloog
  file:
    path: "{{ web_root }}/demo"
    state: directory
```

Ansible asendab `{{ web_root }}` jooksutamisel vastava väärtusega.

---

### Loendite (list) kasutamine

Sageli on vaja teha sama tegevust mitme väärtusega (nt paigaldada mitu paketti või luua mitu kataloogi).  
Sellisel juhul on mugav määrata väärtused **loendina (list)** ja kasutada neid tsükliga `loop:`.

```yaml
---
- name: Paigalda mitmeid pakette
  hosts: webservers
  become: yes

  vars:
    packages:
      - nginx
      - curl
      - vim

  tasks:
    - name: Paigalda vajalikud paketid
      apt:
        name: "{{ item }}"
        state: present
      loop: "{{ packages }}"
```

---

## Muutujate andmetüübid

Ansible kasutab YAML-andmetüüpe. Muutujad ei ole ainult tekst (string), vaid võivad olla ka numbrid, tõeväärtused, loendid ja sõnastikud.  
Õige andmetüübi valik muudab playbookid paindlikumaks ja lihtsamaks.

| Tüüp          | Näide | Selgitus |
|---------------|-------|----------|
| String        | `web_package: "nginx"` | Tekstiväärtus. Soovitatav panna jutumärkidesse, eriti kui sisaldab tühikuid või erimärke. |
| Täisarv (int) | `max_clients: 200` | Kasutatakse arvulistes seadistustes. |
| Tõeväärtus (bool) | `debug_mode: true` | Saab väärtusteks `true` või `false`. |
| Loend (list)  | ```yaml<br>packages:<br>  - nginx<br>  - curl<br>  - vim``` | Mitme elemendi kogum, mida saab tsüklis läbi käia. |
| Sõnastik (dict) | ```yaml<br>app:<br>  name: demo<br>  port: 8080``` | Võtme-väärtuse paaride kogum. Kasulik keerukamate seadistuste jaoks. |

### Näited andmetüüpide kasutamisest

```yaml
vars:
  web_package: "nginx"           # string
  max_clients: 200               # int
  debug_mode: true               # bool
  packages:                      # list
    - nginx
    - curl
    - vim
  app:                            # dict
    name: demo
    port: 8080

tasks:
  - name: Paigalda vajalikud paketid
    apt:
      name: "{{ item }}"
      state: present
    loop: "{{ packages }}"

  - name: Kuva rakenduse nime ja pordi info
    debug:
      msg: "Rakendus {{ app.name }} töötab pordis {{ app.port }}"
```

---

## Faktide (facts) kasutamine

Lisaks enda loodud muutujatele kogub Ansible **automaatselt iga hosti kohta süsteemiteavet**, mida nimetatakse *facts*.  
Need sisaldavad näiteks:

- operatsioonisüsteemi nime ja versiooni,
- tuuma (kernel) infot,
- IP-aadresse ja võrguliideseid,
- mälu ja protsessorite infot,
- ajavööndi jpm.

See info on kättesaadav playbookides spetsiaalse muutuja `ansible_facts` kaudu.

### Näide

```yaml
- name: Kuva hosti operatsioonisüsteem
  hosts: all
  tasks:
    - name: Kuva OS info
      debug:
        msg: "Hosti OS on {{ ansible_facts['distribution'] }}"
```

Faktide kogumine toimub automaatselt enne playbooki ülesannete täitmist (välja arvatud juhul, kui `gather_facts: false` on määratud).
Saad vaadata kõiki kättesaadavaid fakte käsuga:

```bash
ansible all -m setup
```

`setup` kuvab kõik saadaval olevad faktid JSON-formaadis.
---
## Head tavad muutujate kasutamisel

- Hoia muutujad eraldi failides (group_vars, host_vars) selguse huvides.
- Kasuta selgeid ja ühtseid muutujate nimesid.
- Pane vaikimisi väärtused madala prioriteediga kohta (nt group_vars/)
- Ära pane salasõnu või tundlikke andmeid otse playbooki – kasuta Ansible Vault’i.

---

## Harjutus

1. Loo `group_vars/webservers.yml` fail ja määra seal:

    ```bash
    web_package: nginx
    web_root: /var/www/web
    ```

2. Loo host_vars/web1.yml fail, kus määrad teise väärtuse web_root muutujale.

    ```bash
    web_root: /srv/web1_site
    ```

3. Loo lihtne playbook, mis:

    paigaldab {{ web_package }}
    loob kataloogi {{ web_root }}

4. Käivita playbook ja testi, kas eri hostidel kasutati erinevaid väärtusi.
5. Katseta muutujat käsurealt, nt:
    ```bash
    ansible-playbook site.yml -e "web_root=/tmp/test_site"
    ```
    Peakid märkama, et käsurealt määratud väärtus kirjutab üle nii grupi- kui ka hostimuutujad.

---

## Rohkem infot

- [Using Variables in Ansible](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html)

