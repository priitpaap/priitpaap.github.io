# Jinja2 mallid

## Eesmärk

Selles peatükis õpid:

- Mis on Jinja2 mallid ja miks neid vaja on
- Kuidas luua mallifaile
- Kuidas kasutada mallides muutujaid, tingimusi ja tsükleid
- Kuidas kasutada `template` moodulit mallide paigaldamiseks
- Head tavad mallide kirjutamisel
- Kuidas kontrollida malli töö `--check` ja `--diff` abil

---

## Mis on Jinja2 mallid ja miks need on kasulikud

**Jinja2** on Ansible vaikimisi kasutatav mallimootor, millega saab dünaamiliselt luua konfiguratsioonifaile, skripte või muud sisu.

- Mall on tavaline tekstifail (nt konfiguratsioonifail või skript), kus kasutatakse **muutujaid** ja **tingimuslauseid** (nt `if`, `for`).
- Faili laiend on tavaliselt `.j2`.
- Käivitamisel asendab Ansible mallis olevad muutujad nende väärtustega ning loob sihtmasinas lõppfaili.

Enamik serveriteenuseid (nt **Nginx, Apache, SSH, MariaDB, Docker** jne) vajavad **konfiguratsioonifaile**, et määrata nende seadistusi.

Kui seadistad servereid käsitsi, pead iga teenuse jaoks:
- kirjutama või muutma konfiguratsioonifaile,
- kopeerima need õigesse kohta (nt `/etc/nginx/sites-available/default`),
- tagama, et õiged väärtused (nt `server_name`, pordid, kaustateed) on igas serveris õiged.

Ansiblega mallide kasutamine teeb selle lihtsamaks:
- **Mall (template)** on konfiguratsioonifaili alus, kus muutuvad osad (nt `server_name`, `web_root`) on kirjutatud muutujatena `{{ ... }}`.
- Playbook koos `template` mooduliga kopeerib malli õigesse asukohta ja asendab muutujad sihtmasina jaoks sobivate väärtustega.

### Lihtne näide mallifailist ja selle kasutamisest

Fail `templates/nginx.conf.j2`:

```jinja2
server {
    listen 80;
    server_name {{ server_name }};

    root {{ web_root }}/html;

    location / {
        index index.html;
    }
}
```

Playbook, mis kasutab kasutab seda malli:

```yaml
---
- name: NGINX konfiguratsiooni loomine mallist
  hosts: webservers
  become: yes
  vars:
    server_name: example.com
    web_root: /var/www
  tasks:
    - name: Loo konfiguratsioonifail mallist
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/sites-available/default
      notify: Taaskäivita nginx

  handlers:
    - name: Taaskäivita nginx
      service:
        name: nginx
        state: restarted
```

Selgitus:

- Mallifaili muutujad (`{{ server_name }}` ja `{{ web_root }}`) asendatakse playbooki jooksutamise ajal nende väärtustega.
- `template:` moodul kopeerib mallifaili sihtmasinasse ja teeb asendused.
- `notify:` ja `handlers:` on viis teha teatud tegevus (nt teenuse taaskäivitamine) ainult siis, kui mallifaili sisu muutus.

---

## Jinja2 süntaksi põhitõed

- Muutuja: `{{ variable_name }}`
- Tingimus:
```jinja2
{% if use_ssl %}
listen 443 ssl;
{% else %}
listen 80;
{% endif %}
```
- Tsükkel:
```jinja2
{% for site in sites %}
server_name {{ site }};
{% endfor %}
```
Kommentaar failis:
```jinja2
{# See on kommentaar, mida lõplikku faili ei lisata #}
```

Jinja2 võimaldab kasutada ka filtreid (nt | default(), | upper, | join(', ')). Filtrid tulevad kasuks, kui on vaja väärtust töödelda või vormindada.

---

## Kataloogistruktuur

Praktikas paigutatakse mallid eraldi `templates/` kataloogi.

Näide:
```
project/
├─ site.yml
├─ templates/
│  └─ nginx.conf.j2
└─ group_vars/
   └─ webservers.yml
```

See struktuur aitab hoida playbooki ja mallifailid korrastatuna.

## Mallide testimine

- Testi malli `--check` ja `--diff` lipuga enne *production* keskkonnas kasutamist.
    ```bash
    ansible-playbook site.yml --check --diff
    ```
    `--check` näitab, mida playbook muudaks, ilma et muudatusi tegelikult teeks.
    `--diff` näitab failide erinevusi enne ja pärast mallist genereerimist.

---

## Head tavad mallide kasutamisel

- Hoia mallifailid eraldi templates/ kataloogis.
- Testi mallid `--check` ja `--diff` abil enne tootmiskeskkonnas käivitamist.
- Kasuta mallides idempotentsust – väldi juhuslikke väärtusi (nt ajatemplid), mis põhjustavad, et fail muutub igal jooksutusel.
- Väldi mallides liigset loogikat – keerulisemad tingimused ja arvutused on parem teha Ansible’i poolel (nt set_fact või vars).

## Harjutus

1. Loo `templates/` kataloog ja sinna fail `demo.conf.j2`, mis sisaldab:
```jinja2
server {
    listen 80;
    server_name {{ server_name }};
    root {{ web_root }};
}
```
2. Loo playbook, mis kopeerib malli sihtmasinasse `/etc/nginx/sites-available/demo`.
3. Käivita playbook ja veendu, et fail on loodud õigete väärtustega.
4. Muuda `server_name` muutujat ja käivita playbook uuesti – vaata, kas fail muutus ja teenus taaskäivitati.

## Rohkem infot

- [Ansible Template Module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/template_module.html){:target="_blank"}  
- [Jinja2 Documentation](https://jinja.palletsprojects.com/en/latest/templates/){:target="_blank"}  
