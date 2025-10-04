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

- Taandamine (indentation) tühikutega, mitte tab-idega
- Võtme-väärtuse paarid: `võti: väärtus`
- Loetelud: `- element`
- Sõnumite või käskude kirjeldused võivad olla jutumärkides kui sisaldavad erisümboleid

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

### Väike näide

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

---

## Praktikaülesanded

1. Kirjuta playbook, mis loob `/tmp/ansible_test` kataloogi kõigis hostides.
2. Tee playbook, mis paigaldab paketi `curl`.
3. Lisa playbooki ülesanne, mis paneb teenuse `ssh` käima ja lubab selle automaatse käivitamise.
4. Käivita oma playbook esmalt `--check` režiimis ja siis päriselt.
5. Proovi suunata playbook ainult ühele hostile `-l` parameetriga.

---

## Kokkuvõte

- **Playbookid** on Ansible keskne automatiseerimise viis.  
- Need on kirjutatud **YAML** formaadis.  
- Playbook koosneb: `hosts`, `tasks` ja vajadusel muutujad, rollid jpm.  
- Käivitatakse käsuga `ansible-playbook`.  
- Playbooke saab turvaliselt testida `--check` režiimis.

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

## Rohkem infot

- [Ansible playbooks](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_intro.html){:target="_blank"}   
- [YAML Syntax](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html){:target="_blank"}   
