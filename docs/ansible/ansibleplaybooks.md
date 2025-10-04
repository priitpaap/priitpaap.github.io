# Playbookid ja YAML

## Eesmärk

Selles peatükis õpid:

- Mis on Playbook
- Mis on YAML ja selle põhitõed 
- Milline on playbooki ülesehitus
- Kuidas playbooke käivitada 

## Mis on Playbook?

**Playbook** on Ansible keskne osa, kus kirjeldatakse samm-sammult, mida masinates teha tuleb.  
Playbookid kirjutatakse **YAML** formaadis, mis on loetav ja lihtne struktuurne keel.

Playbook võimaldab:

- Korrata tegevusi usaldusväärselt mitmes masinas korraga
- Dokumenteerida, mida täpselt tehakse
- Automatiseerida keerulisemaid töövooge (nt tarkvara paigaldus, teenuste seadistamine)

---

## YAML põhitõed

YAML (YAML Ain’t Markup Language) on lihtne andmevorming, mida Ansible kasutab playbookide jaoks.

Olulised reeglid:

- Taandamine (indentation) tühikutega, mitte tab-idega. Tühikute arv taandamisel on vaba, aga peab olema järjepidev (nt 2 või 4 tühikut)
- Võtme-väärtuse paarid: `võti: väärtus`
- Loetelud: `- element`
- Sõnumite või käskude kirjeldused võivad olla jutumärkides kui sisaldavad erisümboleid
- `---` faili alguses pole Ansible jaoks kohustuslik, aga on YAML standardi osa
- YAML-is ei tohi kasutada `:` sümbolit väärtuses ilma jutumärkideta

Näide:

```yaml
---
- name: Näide YAML loendist
  hosts: all
  tasks:
    - name: Kontrolli ühendust
      ping:
    - name: Loo kataloog
      file:
        path: /tmp/minu_kataloog
        state: directory
```

---

## Playbooki ülesehitus

Playbook koosneb **ühest või mitmest play'st**. Iga play ütleb:

1. Milliseid hoste see puudutab
2. Milliseid ülesandeid (tasks) tuleb teha

 - **Play** = seos hostide ja tasks'ide vahel.
 - **Task** = üks konkreetne tegevus mooduliga.

### Väike näide (nginx veebiserveri paigaldamine Debiani laadsetele Linuxitele)

```yaml
---
- name: Lihtne playbook
  hosts: webservers
  become: yes
  tasks:
    - name: Paigalda NGINX
      apt:
        name: nginx
        state: present

    - name: Käivita NGINX
      service:
        name: nginx
        state: started
        enabled: yes
```

Selgitus:

- `hosts: webservers` → see play rakendub gruppi „webservers” kuuluvatele hostidele  
- `become: yes` → käsud tehakse sudo õigustes  
- `apt:` ja `service:` → moodulid, millega vastav tegevus tehakse  

---

## Playbooki käivitamine

Playbook käivitatakse käsuga:

```bash
ansible-playbook failinimi.yml
```

Näiteks:

```bash
ansible-playbook paigalda_nginx.yml
```

Kui tahad testida ainult ühe hosti peal:

```bash
ansible-playbook paigalda_nginx.yml -l web1
```

Kui tahad ainult näha, mida tehakse (tegelikult ei käivitata):

```bash
ansible-playbook paigalda_nginx.yml --check
```

!!! info
  `--check` ei garanteeri 100% identsust tegeliku jooksuga (nt mõne mooduli puhul ei saa muutust ette ennustada)


Playbookid, mis vajavad  ülesannete (tasks) jaoks *become* (sudo) õigusi, saab käivitada nii:

```bash
ansible-playbook paigalda_nginx.yml -l web1 --become
```

### Kasulikud lisaparameetrid käivitamisel

Kui tahad rohkem infot näha, kasuta **verbose** lippu `-v` (või `-vvv` detailsema logi jaoks):

```bash
ansible-playbook paigalda_nginx.yml -v
ansible-playbook paigalda_nginx.yml -vvv
```

- `-v` - näitab lühikest lisainfot, nt millised käsud jooksevad.
- `-vvv` - näitab väga detailset infot, sh mooduli täpseid käske ja ühenduse logi (hea veaotsinguks).

Kui tahad näha, mida muudetakse (eriti konfiguratsioonifailide puhul), kasuta --diff lippu. `--diff` lipp võrdleb **hosti praegust seisundit** (näiteks faili sisu või teenuse olekut) ja **seda, millisesse seisundisse playbook tahab selle viia**.

```bash
ansible-playbook seadista_nginx.yml --diff
```

---

## Head tavad playbookide kirjutamisel

- **Hoia lihtsana** – tee playbookid loetavaks, et ka teised (või sina ise hiljem) saaksid aru, mida need teevad.
- **Kasuta selgeid nimesid** – igal task’il peaks olema `name:` väli, mis kirjeldab täpselt, mis toimub.
- **Testi `--check` režiimis** enne päris käivitamist.
- **Kasuta muutujad** korduvate väärtuste jaoks (nt kataloogiteed, paketid).
- **Grupeeri hostid loogiliselt** inventorys (nt `webservers`, `dbservers`).
- **Kasuta rolle** kui playbook muutub suureks – rollid aitavad hoida koodi korrastatuna.
- **Ära kasuta "shell" või "command" moodulit ilma vajaduseta** – eelista spetsiaalseid mooduleid (`apt`, `service`, `file` jne).
- **Versioonihaldus (git)** – hoia playbookid ja inventory git’is, et oleks võimalik muudatusi jälgida.

---

## Playbookide kvaliteedi kontroll: `ansible-lint`

Kui oled kirjutanud oma esimese playbooki, võib olla raske märgata, kas kõik on tehtud parimate tavade järgi.  
Siin tuleb appi tööriist **`ansible-lint`** – see on lihtne programm, mis kontrollib playbooke ja rolle ning annab vihjeid, kuidas neid paremaks muuta.

`ansible-lint` aitab näiteks:

- Leida valesti kasutatud või vananenud mooduleid
- Märgata YAML-i süntaksi probleeme
- Soovitada paremaid praktikaid (nt muutujate ja tingimuste kasutamisel)
- Tagada, et sinu playbook oleks ühtlaselt loetav ja hooldatav

Näide kasutamisest:

```bash
ansible-lint minu_playbook.yml
```

---

## Harjutus

1. Kirjuta playbook, mis loob `/tmp/ansible_test` kataloogi kõigis hostides (moodul: ansible.builtin.file)
2. Tee playbook, mis paigaldab paketi `curl` (moodul: ansible.builtin.package või ansible.builtin.apt).
3. Lisa playbooki ülesanne, mis paneb teenuse `ssh` käima ja lubab selle automaatse käivitamise (moodul: ansible.builtin.service).
4. Käivita oma playbook esmalt `--check` režiimis ja siis päriselt.
5. Proovi suunata playbook ainult ühele hostile `-l` parameetriga.


## Rohkem infot

- [Ansible playbooks](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_intro.html){:target="_blank"}   
- [YAML Syntax](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html){:target="_blank"}   
