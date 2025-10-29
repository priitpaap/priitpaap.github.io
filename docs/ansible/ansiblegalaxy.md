# Ansible Galaxy Collections

## Eesmärk

Selles peatükis õpid:

- Mis on **collections** ja miks neid kasutatakse
- Kuidas **otsida ja paigaldada** collections Ansible Galaxy'st
- Kuidas **kasutada collection’is olevaid rolle ja mooduleid**
- Kuidas **hallata collection’e requirements.yml failiga**

---

## Mis on collections?

**Collections** on uuem viis Ansible’i sisu jagamiseks ja haldamiseks.  
Kui rollid on mõeldud ühe konkreetse konfiguratsiooni jaoks (nt nginx või mysql), siis *collection* võib sisaldada:

- mitut rolli,
- Ansible mooduleid ja pluginaid,
- playbooke ja dokumentatsiooni.

Collections aitab hoida seotud komponente koos ja võimaldavad neid lihtsamalt uuesti kasutada.

Näiteks:
- `community.general` sisaldab kümneid mooduleid ja rolle erinevate süsteemide jaoks.
- `ansible.posix` pakub mooduleid ja pluginaid Linuxi keskkondadele.

---

## Collection’i paigaldamine Ansible Galaxy’st

Ansible Galaxy on keskkond, kust saab alla laadida nii rolle kui ka collections.

**Otsi collection’e:**

```bash
ansible-galaxy collection search wordpress
```

**Paigalda collection:**

```bash
ansible-galaxy collection install iamgini.wordpress
```

See paigaldab collection’i vaikimisi asukohta:

```
~/.ansible/collections/ansible_collections/iamgini/wordpress/
```

Kui soovid, et collection salvestuks **projekti kausta**, kasuta `-p`:

```bash
ansible-galaxy collection install -p collections iamgini.wordpress
```

Kaustastruktuur näeb välja umbes nii:

```
myproject/
├── playbook.yml
└── collections/
    └── ansible_collections/
        └── iamgini/
            └── wordpress/
```

!!! info
    Collection’e ei paigaldata `roles/` kausta – need elavad eraldi struktuuris.

---

## Collection’is olevate rollide ja moodulite kasutamine

Collection võib sisaldada nii rolle kui mooduleid.

### Rolli kasutamine playbook’is

Kui collection sisaldab rolli, kasutatakse seda **täisnimega (FQCN – Fully Qualified Collection Name):**

```yaml
---
- hosts: web
  become: yes
  roles:
    - iamgini.wordpress.wordpress
```

### Mooduli kasutamine playbook’is

Collection’is olevat moodulit saab kutsuda FQCN nimega:

```yaml
- name: Luba 80/tcp tulemüüris
  community.general.ufw:
    rule: allow
    port: "80"
```

Või lisada playbook’i algusesse `collections:` plokk ja kasutada lühemat nimekuju:

```yaml
---
- hosts: all
  collections:
    - community.general
  tasks:
    - name: Luba 80/tcp tulemüüris
      ufw:
        rule: allow
        port: "80"
```

---

## Collection’ide haldamine

**Kõik paigaldatud collections näed käsuga:**

```bash
ansible-galaxy collection list
```

**Uuendamine:**

```bash
ansible-galaxy collection install --force iamgini.wordpress
```

**Eemaldamine:**

```bash
ansible-galaxy collection remove iamgini.wordpress
```

---

## Collection’ide automaatne paigaldamine `requirements.yml` failiga

Kui projektis kasutatakse mitut collection’it, saad need hallata sama moodi nagu rolle.

### Näide `requirements.yml` failist:

```yaml
collections:
  - name: iamgini.wordpress
  - name: community.general
  - name: ansible.posix
```

### Paigalda kõik korraga:

```bash
ansible-galaxy collection install -r requirements.yml
```

Kui soovid, et need läheks projekti kausta:

```bash
ansible-galaxy collection install -r requirements.yml -p collections
```

### Uuenda kõiki olemasolevaid:

```bash
ansible-galaxy collection install -r requirements.yml --force
```

---

## Rollid vs Collections – lühivõrdlus

| Omadus | Rollid | Collections |
|--------|--------|-------------|
| Sisaldab | Ühte rolli | Mitut rolli, mooduleid, pluginaid |
| Asukoht | `roles/` | `~/.ansible/collections/` või `collections/` |
| Installimine | `ansible-galaxy install` | `ansible-galaxy collection install` |
| Kasutamine | `roles:` all | `roles:` või `collections:` all |
| Nõuete fail | `requirements.yml` (rollid) | `requirements.yml` (collections) |

---

## Rohkem infot

- [Ansible Collections Documentation](https://docs.ansible.com/ansible/latest/collections_guide/index.html){:target="_blank"}
- [Ansible Galaxy Collections](https://galaxy.ansible.com/ui/){:target="_blank"}
