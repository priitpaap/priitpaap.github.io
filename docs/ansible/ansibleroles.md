# Ansible rollid – koodi struktureerimine ja korduvkasutus

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

Kui playbookid muutuvad suureks ja keeruliseks, on raskem:

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

Tüüpiline rolli struktuur näeb välja nii:

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

Rolli kõige olulisem fail on:

roles/ROLINIMI/tasks/main.yml


Siit käivitab Ansible rolli töövoo.

---

## Rolli loomine `ansible-galaxy` käsuga

Rolli ei pea käsitsi nullist tekitama, seda saab automaatselt teha `ansible-galaxy` käsuga. Nii on palju lihtsam:

```bash
ansible-galaxy init nginx
```

See loob automaatselt kataloogi ja õiged failid. Antud näite puhul luuakse uus Ansible roll nimega nginx.

## Rolli kasutamine playbook'is

Playbook võib välja näha nii:

```yaml
---
- name: Webserverid
  hosts: webservers
  become: yes
  roles:
    - nginx
```
Ansible otsib rolli kataloogist `roles/nginx/`.


## Rolli sees olevad põhikomponendid

`tasks/main.yml`

Siia lähevad rolli põhitegevused:

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
    # notify pole siin alati vajalik
```

`handlers/main.yml`

Handler'id rolli sees:

```yaml
---
- name: Taaskäivita nginx
  service:
    name: nginx
    state: restarted
```

`templates/`

Siia pannakse mallifailid (Jinja2 .j2):

`nginx.conf.j2`


## Rolli muutujad

Rollides on kaks peamist muutujate kohta:

`defaults/main.yml`

- madalaim prioriteet
- saab hõlpsasti playbookis üle kirjutada

`nginx_port: 80`


`vars/main.yml`

- kõrgem prioriteet
- tavaliselt kasutatakse rollisisesteks konstantideks