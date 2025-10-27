# Ansible rollid ja struktuur

## Eesmärk

Selles peatükis õpid:

- Mis on **rollid** ja miks neid kasutatakse
- Kuidas **rolli struktuur** töötab
- Kuidas **luua rolle** automaatselt `ansible-galaxy` käsuga
- Kuidas **kasutada rolle** playbookides
- Kuidas integreerida **muutujaid, malle, faile, handler'eid** rolli sees
- Kuidas rollidega **projekti struktuuri** korrastada

---

## Mis on rollid?

Kui projektikaust ja playbookid muutuvad suureks ja keeruliseks, on raskem:

- analüüsida, mis kus toimub
- taaskasutada sama loogikat
- hoida koodi puhtana
- teha muudatusi ilma riske ksvatamata

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

See fail võib sisaldada:

- konkreetseid ülesandeid (name, module, args)
- viiteid teistele failidele (include_tasks, import_tasks)
- tingimuslikke käske (when, tags, notify jne)

---

## Rolli loomine `ansible-galaxy` käsuga

Rolli struktuuri ei pea käsitsi nullist tekitama, seda saab automaatselt teha `ansible-galaxy` käsuga. Nii on palju lihtsam:

```bash
ansible-galaxy init nginx
```

See loob automaatselt kataloogi ja õiged failid. Antud näite puhul luuakse uus Ansible roll nimega *nginx*.

## Rolli kasutamine playbook'is

Kui kasutatakse rolle, siis projektikausta playbookis viidatakse neile `roles:` parameetriga. Playbook võib välja näha näiteks nii:

```yaml
---
- name: Veebiserverte seadistamine
  hosts: webservers
  become: yes

  roles:
    - nginx
```
Ansible otsib rolli kataloogist `roles/nginx/`.


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



## Rolli mall ja notify koos

Fail tasks/main.yml:

```yaml
- name: Paigalda nginx konfiguratsioon
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
  notify: Taaskäivita nginx
```

Fail handlers/main.yml:

```yaml
- name: Taaskäivita nginx
  service:
    name: nginx
    state: restarted
```

See käivitab restarti ainult siis, kui mall muutus.

## Mitme rolli kasutamine

```yaml
roles:
  - common
  - firewall
  - docker
  - nginx
```

Ansible täidab rollid ülalt alla järjestuses.

## Näidisprojekt rollidega

myproject/
├─ site.yml
├─ inventory
└─ roles/
   ├─ common/
   ├─ nginx/
   └─ users/


```yaml
---
- hosts: all
  become: yes
  roles:
    - common
    - users
    - nginx
```

## Ansible Galaxy rollid (võrgust)

Samuti saab otsida valmis rolle:

`ansible-galaxy search nginx`

Installeerimine:

`ansible-galaxy install geerlingguy.nginx`

Need rollid on kasutusvalmis ja hästi dokumenteeritud.

## Rollide sees teiste failide viitamine

Näiteks mall:

`roles/nginx/templates/nginx.conf.j2`

Ei pea andma täisteed – Ansible leiab rolli templates/ kataloogist automaatselt.


## Rollide testimine

Rolli saab testida eraldi lihtsa playbook'iga:

```yaml
---
- hosts: webservers
  become: yes
  roles:
    - nginx
```

## Rohkem infot

- [Ansible Roles Documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html){:target="_blank"}

- [Ansible Galaxy](https://galaxy.ansible.com/ui/){:target="_blank"}