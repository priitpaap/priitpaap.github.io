# Tingimuslikkus Ansible’is

## Eesmärk

Selles peatükis õpid:

- Kuidas kasutada **tingimuslikke käske (when)** Ansible’is  
- Kuidas teha tegevusi ainult siis, kui teatud tingimus on täidetud  
- Kuidas kasutada **loogikaoperaatoreid** ja **muutujate kontrolli**  
- Kuidas kombineerida tingimusi Jinja2 süntaksiga  
- Kuidas testida ja siluda tingimuslikke ülesandeid

---

## Mis on tingimuslikkus?

Tingimuslikkus (`when:`) võimaldab Ansible’il teha **otsuseid jooksvalt**, olenevalt masinate seisundist või muutujate väärtustest.

Näiteks:

- käivita teenus ainult siis, kui see on paigaldatud;  
- paigalda pakett ainult Debianil;  
- käivita teatud task ainult siis, kui eelmine õnnestus või muutuja on määratud.

---

## Lihtne näide

```yaml
---
- name: Tingimuslik näide
  hosts: all
  become: yes
  tasks:
    - name: Paigalda Apache ainult Debiani süsteemides
      apt:
        name: apache2
        state: present
      when: ansible_facts['os_family'] == "Debian"
```

Selgitus:

- `when:` määrab, et see task täidetakse **ainult siis**, kui sihtmasina `os_family` on *Debian*.
- Kui host on nt CentOS (RedHat), siis see task jäetakse vahele (`skipped`).

---

## Loogikaoperaatorid

Tingimustes saab kasutada **Jinja2 loogikaoperaatoreid**:

| Operaator | Näide | Selgitus |
|------------|--------|-----------|
| `==` | `when: ansible_facts['distribution'] == 'Ubuntu'` | võrdsus |
| `!=` | `when: package_name != 'nginx'` | mitte võrdsus |
| `and` | `when: ansible_facts['os_family'] == 'Debian' and ansible_facts['pkg_mgr'] == 'apt'` | mõlemad tingimused peavad kehtima |
| `or` | `when: use_ssl or enable_https` | vähemalt üks peab kehtima |
| `not` | `when: not debug_mode` | eitab tingimust |
| `in` | `when: ansible_facts['distribution'] in ['Ubuntu', 'Debian']` | väärtus kuulub loendisse |

---

## Mitu tingimust korraga

Tingimusi saab määrata ka nimekirjana. Kõik tingimused peavad siis olema tõesed (AND):

```yaml
- name: Paigalda nginx ainult Debian 12 serveritesse
  apt:
    name: nginx
    state: present
  when:
    - ansible_facts['os_family'] == "Debian"
    - ansible_facts['distribution_major_version'] == "12"
```

---

## Tingimused muutujate põhjal

Sageli kontrollitakse, kas muutuja on olemas või omab kindlat väärtust.

### Muutuja olemasolu kontroll (`is defined`)

```yaml
- name: Loo kataloog ainult siis, kui muutuja on määratud
  file:
    path: "{{ custom_dir }}"
    state: directory
  when: custom_dir is defined
```

### Muutuja väärtuse kontroll (`is not defined`, `is truthy`, `is falsy`)

```yaml
- name: Tee midagi ainult siis, kui lipp on tõene
  debug:
    msg: "Lipp on aktiivne"
  when: my_flag | default(false)
```

---

## Kombineerimine Jinja2 süntaksiga

Tingimused kasutavad **sama Jinja2 süntaksit**, mida kasutatakse mallides.

Näide, kus kontrollitakse loendi pikkust ja väärtust:

```yaml
- name: Näita sõnumit kui serverite arv on üle 3
  debug:
    msg: "Süsteemis on palju servereid!"
  when: servers | length > 3
```

---

## Tingimused mooduli tulemuse põhjal

Paljud moodulid salvestavad oma tulemuse muutujasse (`register:`). Seda saab kasutada järgnevates tingimustes.

```yaml
---
- name: Kontrolli teenuse olekut ja taaskäivita vajadusel
  hosts: webservers
  become: yes
  tasks:
    - name: Kontrolli, kas nginx töötab
      shell: systemctl is-active nginx
      register: nginx_status
      ignore_errors: yes

    - name: Käivita nginx, kui see ei tööta
      service:
        name: nginx
        state: started
      when: nginx_status.rc != 0
```

Selgitus:
- `register:` salvestab mooduli väljundi muutujasse `nginx_status`.
- `rc` (return code) väärtus `0` tähendab, et käsk õnnestus.
- Kui see **ei olnud 0**, käivitatakse teenus uuesti.

---

## Näide: kombineeritud tingimused

```yaml
- name: Paigalda MariaDB, kui OS on Debian ja muutuja db_enabled on true
  apt:
    name: mariadb-server
    state: present
  when:
    - ansible_facts['os_family'] == "Debian"
    - db_enabled | default(false)
```

---

## Testimine ja veaotsing

Tingimuslike taskide kontrollimiseks on kasulik kasutada:
- `--check` – näitab, mis toimuks, ilma et tegelikult muudaks;
- `-v` või `-vvv` – kuvab täpse põhjuse, miks mõni task jäi „skipped“;
- `debug:` – väljastab muutujate väärtusi.

Näide:

```yaml
- name: Kontrolli muutuja väärtust
  debug:
    var: db_enabled
```

---

## Head tavad tingimuslikkuse kasutamisel

- Väldi liiga keerukaid tingimusi – parem on jagada loogika mitmeks lihtsaks taskiks.  
- Kasuta `| default()` filtrit, et vältida „undefined“ vigu.  
- Kontrolli OS-i tüüpi alati `ansible_facts` kaudu (nt `ansible_facts['os_family']`), mitte käsitsi määratud muutujate põhjal.  
- Testi `--check` ja `-vvv` abil, et mõista, miks mõni task jäeti vahele.  
- Väldi shelli käske, kui moodul juba toetab tingimusi või idempotentsust.

---

## Harjutus

1. Loo playbook, mis:
   - paigaldab `nginx` ainult siis, kui OS on *Debian*;
   - käivitab teenuse ainult siis, kui `install_web` muutuja on `true`;
   - väljastab `debug` sõnumi, kui mõlemad tingimused on täidetud.

2. Lisa kontroll, mis loob `/tmp/ansible_condition_test` kataloogi ainult siis, kui `create_dir` muutuja on määratud.

3. Testi oma playbooki `--check` ja `-v` valikutega.  
   Märka, millised taskid jäetakse vahele (`skipped`).

---

## Rohkem infot

- [Ansible Conditionals](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_conditionals.html){:target="_blank"}  
- [Jinja2 Tests and Operators](https://jinja.palletsprojects.com/en/latest/templates/#tests){:target="_blank"}  
