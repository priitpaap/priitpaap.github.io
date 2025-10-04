# Muutujad ja Jinja2 mallid

## Eesmärk

Selles peatükis õpid:

- Mis on Ansible muutujad ja miks neid kasutada
- Kuidas määrata muutujate väärtusi
- Kus muutujad asuvad (vars, group_vars, host_vars, käsurealt jne)
- Mis on Jinja2 mallid ja miks neid vaja on
- Kuidas kasutada muutujad Jinja2 mallides
- Kuidas mallidest luua konfiguratsioonifaile

---

## Mis on muutujad?

**Muutujad** võimaldavad hoida korduvaid väärtusi ühes kohas.  
See muudab playbookid lihtsamaks, loetavamaks ja hooldatavamaks.

Näiteks kui mitmes ülesandes tuleb kasutada sama kataloogi teed või paketi nime, siis on mõistlik määrata see muutuja alla.

---

## Muutujate määramise viisid

Muutujaid saab määrata mitmel tasemel. Ansible kasutab **prioriteetide hierarhiat**, kus kõrgema taseme väärtus asendab madalama oma.

Olulisemad viisid:

1. **Playbooki sees `vars:` all**

```yaml
---
- name: Lihtne playbook muutujatega
  hosts: webservers
  vars:
    web_package: nginx
    web_root: /var/www/html
  tasks:
    - name: Paigalda veebiserver
      apt:
        name: "{{ web_package }}"
        state: present
```

2. **Muutujad eraldi failis (group_vars või host_vars)**

Kataloogistruktuur:
```
inventor/
├─ hosts.ini
├─ group_vars/
│  └─ webservers.yml
```

Fail `group_vars/webservers.yml`:
```yaml
web_package: nginx
web_root: /var/www/html
```

Playbook kasutab neid muutujad automaatselt, kui `hosts:` määratud grupp on `webservers`.

3. **Muutujad käsurealt**

```bash
ansible-playbook site.yml -e "web_package=nginx"
```

4. **Inventorifailis**

```ini
[webservers]
web1 web_package=nginx web_root=/var/www/html
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

## Jinja2 mallid

**Jinja2** on Ansible vaikimisi kasutatav mallimootor, millega saab dünaamiliselt luua konfiguratsioonifaile, skripte või muud sisu.

Mallid on tavalised tekstifailid, kus kasutatakse muutujate ja kontrolllausete süntaksit (nt if, for).

Faili laiend on tavaliselt `.j2`.

### Näide mallifailist

Fail `nginx.conf.j2`:

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

### Playbook, mis kasutab malli

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

### Selgitus

- Mallifaili muutujad (`{{ server_name }}` ja `{{ web_root }}`) asendatakse jooksutamise ajal nende väärtustega.
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

---

## Head tavad muutujate ja mallide kasutamisel

- Hoia muutujad eraldi failides (group_vars, host_vars) selguse huvides.
- Kasuta selgeid ja ühtseid muutujate nimesid.
- Ära pane salasõnu või tundlikke andmeid otse playbooki – kasuta Ansible Vault’i.
- Testi malli `--check` ja `--diff` lipuga enne tootmiskeskkonnas kasutamist.
- Kasuta mallides idempotentsust – väldi juhuslikke dünaamilisi väärtusi, mis muudavad faili igal jooksutusel.

---

## Harjutus

1. Loo `group_vars/webservers.yml` fail ja määra seal:
   - `server_name: mydemo.local`
   - `web_root: /var/www/demo`
2. Loo `templates/` kataloog ja sinna fail `demo.conf.j2`, mis sisaldab:
```jinja2
server {
    listen 80;
    server_name {{ server_name }};
    root {{ web_root }};
}
```
3. Loo playbook, mis kopeerib malli sihtmasinasse `/etc/nginx/sites-available/demo`.
4. Käivita playbook ja veendu, et fail on loodud õigete väärtustega.
5. Muuda `server_name` muutujat ja käivita playbook uuesti – vaata, kas fail muutus ja teenus taaskäivitati.

---

## Rohkem infot

- [Using Variables in Ansible](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html)
- [Ansible Template Module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/template_module.html)
- [Jinja2 Documentation](https://jinja.palletsprojects.com/en/latest/templates/)

