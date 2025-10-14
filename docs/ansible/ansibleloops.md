# UNFINISHED!!!

# Tsüklid

## Eesmärk

Selles peatükis õpid:

- Kuidas kasutada **`loop:`**-i korduvate tegevuste jaoks
- Kuidas korrata tegevusi loendite, sõnastike ja failide elementide puhul
- Kuidas lahendada **keerukamaid juhtumeid** (nt mitme kasutaja või faili massiline loomine)
- Kuidas kasutada tsükleid **koos mallidega** (Jinja2, `{{ item }}`)

---

## Mis on tsüklid?

Tsüklid võimaldavad sul **sama tegevust korrata** erinevate väärtustega – näiteks paigaldada mitu paketti, luua mitu kataloogi, tekitada mitu kasutajat või kirjutada mitu konfiguratsioonifaili.

---

## `loop:` – kaasaegne viis korduva tegevuse sooritamiseks

`loop:` on **eelistatud ja ühtne** viis tsüklite kirjutamiseks kõigi moodulite juures. Ansible soovitab tänapäeval teostada kõiki korduvaid tegevusi just `loop` abil.

**Lihtne näide – mitme paketi paigaldus:**

```yaml
---
- name: Paigalda mitu paketti
  hosts: webservers
  become: yes
  tasks:
    - name: Paigalda paketid
      apt:
        name: "{{ item }}"
        state: present
      loop:
        - curl
        - htop
        - git
      when: ansible_facts['os_family'] == "Debian"
```

- `{{ item }}` sisaldab hetkel töödeldava elemendi väärtust.

---

## `with_items:` – vanem viis tsüklite kasutamiseks

Varem kasutati eri mustrite jaoks **`with_*`** süntaksit: `with_items`, `with_dict`, `with_fileglob` jne.  
Tänapäeval soovitab Ansible kasutada **`loop:`**-i, sest:

- süntaks on **ühtne** (üks viis kõigi juhtumite jaoks);
- koos **filtritega** (`dict2items`, `subelements`, `flatten`, `unique` jne) saab lahendada kõik endised `with_*` juhtumid;
- kergem lugeda ja hooldada.

!!! info
     `with_items:` pole küll ametlikult eemaldatud, kuid **Ansible dokumentatsioonis soovitatakse kasutada `loop:`-i**. Mõnedes vanemates näidetes võib siinai kohta `with_items` või mõnda muud ``with_*` loopi, seega on hea vähemalt sellest võimaluses teada, et näidetest aru saada.

**Vana süntaksi näide:**

```yaml
---
- name: Paigalda mitu paketti (vana süntaks)
  hosts: webservers
  become: yes
  tasks:
    - name: Paigalda paketid
      apt:
        name: "{{ item }}"
        state: present
      with_items:
        - curl
        - git
        - htop
```

---

## Tsüklite kasutamine loendite puhul

Tsüklite abil saab sama tegevust korrata iga loendi elemendi jaoks — näiteks paigaldada mitu paketti, luua mitu kasutajat või tekitada mitu kataloogi ühe playbooki sees.

**Mitme kasutaja loomine:**

```yaml
- name: Loo kasutajad  
  user:  # Kasutajate haldamiseks mõeldud Ansible moodul
    name: "{{ item.name }}"  # Määrab kasutaja nime vastavalt loendis olevale väärtusele
    state: present    # Loob kasutaja, kui teda pole (või jätab alles, kui juba olemas)
    groups: "{{ item.groups | default(omit) }}"  
      # Lisab kasutaja gruppi, kui 'groups' on määratud
      # Kui 'groups' pole määratud (nt teisel kasutajal), siis 'omit' jätab selle välja
  loop:                # Kordab seda task’i iga elemendi puhul allolevas loendis
    - { name: "alice", groups: "sudo" }  # Esimene kasutaja, lisatakse gruppi sudo
    - { name: "bob" }                    # Teine kasutaja, ilma grupita

```

**Mitme kataloogi loomine:**

```yaml
- name: Loo kataloogid  
  file:                 # Kasutatakse Ansible 'file' moodulit failisüsteemi haldamiseks
    path: "{{ item }}"  # Kataloogi tee, mille väärtus tuleb tsüklis loendist
    state: directory    # Määrab, et objekt peab olema kataloog (loob selle, kui puudub)
    mode: "0755"        # Seab õigused: omaniku kirjutus/lugemisõigus, teistele lugemisõigus
  loop:                 # Käivitab task’i iga alloleva loendi elemendi jaoks kordamööda
    - /opt/app/logs     # Esimene kataloog, mis luuakse
    - /opt/app/data     # Teine kataloog, mis luuakse

```

**Muutuja kasutamine tsüklina**

```yaml
---
- name: Paigalda mitmeid pakette 
  hosts: webservers    
  become: yes                       

  vars:                             
    packages:                       # Muutujana loend paketitest, mida soovime paigaldada
      - nginx
      - curl
      - vim

  tasks:                            
    - name: Paigalda vajalikud paketid       
      apt:                                   # Kasutame 'apt' moodulit (Debiani/Ubuntu süsteemides)
        name: "{{ item }}"                   # Paigaldatav paketi nimi tuleb tsükli elemendist
        state: present                       # Tagab, et pakett on olemas (paigaldatud)
      loop: "{{ packages }}"                 # Tsükkel – kordab ülesannet iga loendis oleva paketi jaoks

```

---

## Tsüklite kasutamine sõnastike (dict) puhul

Tsüklite abil saab töödelda ka sõnastikke, kus iga element koosneb võtme–väärtuse paarist — näiteks seadistused, kasutajanimed koos gruppidega või paketid koos versioonidega. Sõnastikke on mugav tsükliks vormida Jinja2 filtriga **`dict2items`** (teeb `{key:…, value:…}` loendi).

```yaml
- name: Paigalda paketid koos target-versioonidega   
  apt:                                               # Kasutatakse 'apt' moodulit 
    name: "{{ item.key }}={{ item.value }}"          # Iga tsükli käigus ühendatakse paketi nimi (key) ja versioon (value)
    state: present                                   # Tagab, et määratud versioon on paigaldatud
  loop: "{{ packages | dict2items }}"                # Teisendab sõnastiku loendiks (dict → [{key:…, value:…}, …])
  vars:                                              
    packages:                                        # Sõnastik, kus iga kirje koosneb paketi nimest (võti) ja versioonist (väärtus)
      nginx: 1.24.0-1                                # Paigaldatakse konkreetne nginx versioon
      git: 1:2.43.0-1                                # Paigaldatakse konkreetne git versioon

```

**Lihtsam „võti-väärtus” kasutajate halduse näitel:**

```yaml
- name: Loo kasutajad (dict)
  user:                                   # Kasutajate haldamise moodul
    name: "{{ item.key }}"                # Kasutajanimi tuleb sõnastiku võtme (key) väärtusest
    state: present                        # Tagab, et kasutaja eksisteerib (luuakse, kui teda pole)
    groups: "{{ item.value.groups | default(omit) }}"  
      # Kui sõnastiku väärtuses on määratud grupp (groups), lisatakse kasutaja sellesse gruppi
      # Kui gruppi pole määratud, jätab 'omit' selle välja, et vältida tühja väärtuse viga
  loop: "{{ users | dict2items }}"        # Teisendab 'users' sõnastiku loendiks [{key:…, value:…}, …]
  vars:
    users:                                # Sõnastik kasutajatest ja nende omadustest
      alice: { groups: "sudo" }           # Kasutaja 'alice' lisatakse gruppi 'sudo'
      bob: {}                             # Kasutaja 'bob' luuakse ilma grupita

```

---

## Tsüklite kasutamine failide puhul

Sageli on vaja teha sama tegevust mitme faili jaoks — näiteks kopeerida konfiguratsioonifaile või luua linke.
Tsüklite abil saab selliseid korduvaid failitoiminguid automatiseerida, kasutades kas failide loendit või `fileglob` päringut, mis leiab kõik sobivad failid kataloogist.

**Failiglob koos `loop:`-iga**:

```yaml
- name: Kopeeri kõik .conf failid kataloogist files/app/
  copy:                                                    # Kasutatakse 'copy' moodulit failide kopeerimiseks
    src: "{{ item }}"                                      # Allikas – iga tsükli käigus üks fail, mille tee tuleb loendist
    dest: "/etc/myapp/{{ item | basename }}"               # Sihtkoht – fail kopeeritakse /etc/myapp/ kataloogi
                                                           # 'basename' filter eemaldab failiteest kataloogiosa, jättes alles ainult failinime
    mode: "0644"                                           # Seab failidele õigused (omanik kirjutab, teised loevad)
  loop: "{{ lookup('fileglob', 'files/app/*.conf', wantlist=True) }}"  
    # lookup('fileglob', 'muster', wantlist=True) leiab kõik failid, mis vastavad mustrile files/app/*.conf
    # tulemuseks on loend failide täielikest radadest, mida tsüklis ükshaaval töödeldakse

```

- `lookup('fileglob', pattern, wantlist=True)` leiab kõik mustriga sobivad failid ja annab need loendina `loop:`-ile töötlemiseks.

---

## Keerukamad näited

### 1) Alamstruktuurid (`subelements`)

Kui on loend objekte, millest igal on **alamloend**, kasuta filtrit **`subelements`**:

```yaml
- name: Loo kasutajatele SSH võtmed
  copy:
    dest: "/home/{{ item.0.name }}/.ssh/authorized_keys"
    content: "{{ item.1 }}"
    owner: "{{ item.0.name }}"
    group: "{{ item.0.name }}"
    mode: "0600"
  loop: "{{ users | subelements('authorized_keys') }}"
  vars:
    users:
      - name: "alice"
        authorized_keys:
          - "ssh-ed25519 AAAAC... alice@laptop"
          - "ssh-ed25519 AAAAC... alice@work"
      - name: "bob"
        authorized_keys:
          - "ssh-ed25519 AAAAC... bob@home"
```

> `item.0` on vanem objekt (kasutaja), `item.1` on alamloendi üks element (üks võti).

### 2) Mitme mõõtmega tsükkel (tootekombinatsioonid)

```yaml
- name: Loo kaustad keskkondade ja teenuste kombinatsioonidele
  file:
    path: "/srv/{{ item.env }}/{{ item.svc }}"
    state: directory
  loop: "{{ query('cartesian', envs, services) | map('combine') | list }}"
  vars:
    envs:
      - { env: "dev" }
      - { env: "prod" }
    services:
      - { svc: "api" }
      - { svc: "worker" }
```

### 3) Iteratsiooni indeks ja silt

`loop_control` annab ligipääsu indeksile ja inimloetavale märgisele:

```yaml
- name: Loo kaustad koos indeksiga
  file:
    path: "/data/{{ index }}-{{ item }}"
    state: directory
  loop: ["a", "b", "c"]
  loop_control:
    index_var: index
    label: "{{ index }} -> {{ item }}"
```

---

## Tsüklite tulemused ja `register`

Tsükliga task salvestab **tulemustes loendi**. Iga elemendi puhul tekib alamkirje.

```yaml
- name: Kontrolli portide staatust
  shell: "ss -lnt | grep -q ':{{ item }} '"
  register: port_checks
  ignore_errors: yes
  loop:
    - 80
    - 443

- name: Näita tulemused
  debug:
    var: port_checks.results
```

Igal `results` elemendil on näiteks `item`, `rc`, `stdout`, `stderr` jne.

---

## Tsüklid ja Jinja2 mallid ({{ item }})

Tsükleid saab kombineerida **mallide paigaldamisega**, et luua **mitu konfiguratsioonifaili** erinevate väärtustega.

**Mallifail `templates/vhost.conf.j2`:**

```jinja2
server {
    listen 80;
    server_name {{ item.server_name }};
    root {{ item.web_root }};
}
```

**Playbooki väljavõte:**

```yaml
- name: Loo vhostid mallist
  template:
    src: vhost.conf.j2
    dest: "/etc/nginx/sites-available/{{ item.server_name }}.conf"
  loop:
    - { server_name: "example.com", web_root: "/var/www/example" }
    - { server_name: "demo.example.com", web_root: "/var/www/demo" }
  notify: Taaskäivita nginx

handlers:
  - name: Taaskäivita nginx
    service:
      name: nginx
      state: restarted
```

> Mallis saab kasutada `{{ item.* }}` väärtusi, mis tulevad tsükli elemendist.

---

## Testimine ja veaotsing

- Kasuta **`--check`** ja **`--diff`**, et näha muudatusi enne rakendamist.
- Lisa **`-v` / `-vvv`**, et näha, milliseid elemente töödeldi.
- Kui tsükkel on keerukas, **väljastada vaheväärtusi** `debug:` abil.

```yaml
- name: Kuvan töötlemisel kasutatava elemendi
  debug:
    msg: "Töötlen: {{ item }}"
  loop: "{{ my_list }}"
```

---

## Head tavad

- **Kasuta `loop:`** ja filtreid (`dict2items`, `subelements`, `flatten`) – väldi `with_*` süntaksit uues koodis.
- Tee **elementide struktuur selgeks** (nt `{name:…, uid:…}`) – see teeb mallid ja taskid loetavaks.
- Väldi shelli, kui leidub **spetsiaalne moodul** (idempotentsus).
- Kui tsükkel muutub keerukaks, **jaga väiksemateks taskideks** või kasuta **rolle**.
- Vajadusel kasuta `loop_control` **indeksi** ja **sildi** jaoks.

---

## Harjutus

1. Loo playbook, mis:
   - loob kolm kataloogi `/srv/app/{api,worker,web}`;
   - loob kaks kasutajat (`alice`, `bob`), kus `alice` kuulub gruppi `sudo`;
   - kopeerib kõik failid `files/app/*.conf` kataloogi `/etc/myapp/` säilitades nimed.

2. Loo mall `templates/vhost.conf.j2` (nagu ülal) ja genereeri sellest kaks vhosti `example.com` ja `demo.example.com` eraldi juurkataloogidega. Lisa `notify`, mis taaskäivitab nginx’i vaid muutuste korral.

3. Kuva `debug` abil iga sammu juures, millist elementi parajasti töödeldakse.

---

## Rohkem infot

- [Ansible loops (playbook guide)](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_loops.html){:target="_blank"}
- [Jinja2 filters](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_filters.html){:target="_blank"}
- [Lookups: fileglob, cartesian, subelements](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/index.html#lookups){:target="_blank"}
