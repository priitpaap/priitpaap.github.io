# Ansible parimad praktikad

## Eesmärk

Selles peatükis õpid tundma Ansible projektide **parimaid tavasid**, mis aitavad muuta sinu automatiseerimise töökindlamaks, korduvkasutatavamaks ja paremini hallatavaks.

---

## Rollide kasutamine (modulariseerimine)

**Kasuta rolle**, et jagada playbookid väiksemateks ja taaskasutatavateks komponentideks.  
See teeb projektid loogilisemaks ja lihtsustab koostööd tiimides.

Näiteks:

```yaml
roles:
  - common
  - nginx
  - mysql
```

---

## Hoia playbookid lihtsad ja loetavad

- Kasuta **selgeid, kirjeldavaid nimesid** (näiteks `Paigalda Nginx`, mitte lihtsalt `Install`).  
- Lisa **kommentaare**, kui tegevus pole iseenesestmõistetav.  
- Väldi liigseid tingimusi ja pikki shell-käske — kasuta mooduleid.

Hea näide:

```yaml
- name: Paigalda nginx veebiserver
  apt:
    name: nginx
    state: present
```

---

## Kasuta versioonihaldust (Git)

Hoia oma **playbooke, rolle ja inventare** versioonihalduses (nt GitHub, GitLab, Bitbucket).

Eelised:

- muudatuste ajalugu ja taastevõimalus
- koostöö tiimiliikmete vahel
- CI/CD integreerimine

Struktuuriversioonid võivad välja näha nii:

```yaml
ansible-project/
├── playbooks/
├── roles/
├── group_vars/
└── inventory/
```

---

## Parameetriseeri kõik

**Väldi kõvakoodeeritud väärtusi.**  
Kasuta **muutujaid** ja **mal­le** (`templates/`), et muuta playbookid paindlikuks.

Näide:

```yaml
- name: Loo Nginx konfiguratsioonimall
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
```

Ja mallis (`nginx.conf.j2`):

```
server {
    listen {{ nginx_port }};
}
```

---

## Inventari haldus

Jaga hostid ja grupid loogiliselt (nt `dev`, `test`, `prod`).  
Kasuta **grupimuutujaid (`group_vars`)** ja **hostimuutujaid (`host_vars`)**, et vältida dubleerimist.

Näide:

```yaml
inventory/
├── hosts
├── group_vars/
│   ├── web.yml
│   └── db.yml
└── host_vars/
    └── server1.yml
```

---

## Idempotentsus

**Idempotentsus** tähendab, et sama playbooki võib käivitada mitu korda ja tulemus jääb samaks.  
Näiteks kui teenus on juba installitud, ei tee Ansible seda uuesti.

Hea tava: kasuta mooduleid nagu `apt`, `service`, `file` ja `user`, mitte toorest `command` või `shell`, kui võimalik.

---

## Testimine ja valideerimine

Enne tootmiskeskkonda saatmist:

1. **Testi** playbooke virtuaalmasinas või konteineris (nt Vagrant, Docker, Molecule).  
2. Kontrolli süntaksit:  
   ```bash
   ansible-playbook playbook.yml --syntax-check
   ```
3. Kasuta *dry run* režiimi:  
   ```bash
   ansible-playbook playbook.yml --check
   ```

---

## Turvalisus ja konfidentsiaalsus

- Kasuta **Ansible Vault**’i tundlike andmete (paroolid, API võtmed) krüpteerimiseks:
  ```bash
  ansible-vault encrypt vars/secrets.yml
  ```
- Hoia võtmiseid (SSH, Vault paroolid) väljaspool jagatud repo’sid.
- Piira õigusi juhtsõlmel (control node).
- Kasuta **väikseima privileegi printsiipi** (least privilege).

---

## Teised head tavad

- **Defineeri standardsed failistruktuurid** (nt kõik projektid näevad välja sarnaselt).  
- **Kasuta tag’e** (nt `--tags "install"`) kindlate tegevuste sihtimiseks.  
- **Logi tulemusi** ja kasuta `ansible.cfg` failis sobivaid logisätteid.  
- **Dokumenteeri** muutujad ja rollide eesmärgid `README.md` failides.

---

## Rohkem infot

- [Ansible Best Practices (Official Docs)](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html){:target="_blank"}
- [Ansible Lint](https://ansible-lint.readthedocs.io/en/latest/){:target="_blank"}
