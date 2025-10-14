# Handlers ja `notify`

## Eesmärk

Selles peatükis õpid:

- Mis on **handler** ja millal seda kasutatakse  
- Kuidas töötab **`notify`** mehhanism  
- Kuidas handler käivitub ainult siis, kui midagi muutus  
- Kuidas kasutada **mitut handlerit** ühes playbookis  

---

## Mis on handler?

**Handler** on Ansible’i eriline *ülesanne* (task), mis käivitatakse **ainult siis, kui seda teavitatakse (`notify`)**.  
Seda kasutatakse tavaliselt olukorras, kus mingi muudatus nõuab järgnevat tegevust — näiteks teenuse taaskäivitust pärast konfiguratsioonifaili muutmist.

Handlerid asuvad **playbooki lõpus** ja näevad välja peaaegu nagu tavalised taskid, kuid need kuuluvad eraldi plokki nimega `handlers:`.

---

## Notify ja handleri tööpõhimõte

Kui tavapärane task muudab midagi (nt paigaldab paketi, muudab faili, kirjutab malli jms), saab see anda märku handlerile, kasutades võtmesõna `notify:`.

Handler käivitatakse **ainult siis, kui muutus tegelikult toimus** — mitte igal playbooki käivitamisel.  

---

**Näide: Muudame malli kaudu veebiserveri seadeid ja käivitame veebiserveri kui failis toimus muutus:**

```yaml
---
- name: Nginx konfiguratsiooni uuendamine
  hosts: webservers
  become: yes

  tasks:
    - name: Kopeeri nginx konfiguratsioonimall
      template:
        src: templates/nginx.conf.j2
        dest: /etc/nginx/nginx.conf
        mode: "0644"
      notify: Taaskäivita nginx   # Anname handlerile märku, kui faili muudeti

  handlers:
    - name: Taaskäivita nginx   # handleri käivitumine ainult muutuse korral
      service:
        name: nginx
        state: restarted
```

**Selgitus:**

- `template` moodul kopeerib mallifaili sihtkohta ja võrdleb seda olemasolevaga.  
- Kui sisu erineb, märgib Ansible taski *changed* olekusse ja käivitab `notify`.  
- Handler **ei käivitu kohe**, vaid **playbooki lõpus** – pärast kõiki task’e.  
- Kui midagi ei muutunud, handlerit **ei käivitata**.  


Handler käivitub ainult siis, kui:

1. Seda on **teavitatud (`notify:`)**, **ja**
2. Teavituse andnud task **muutus (changed)** olekusse.

Näiteks:

- Kui mallifail oli juba ajakohane → *no change* → handler ei tööta.  
- Kui mallifail muutus → *changed* → handler käivitatakse playbooki lõpus.  

See tagab, et teenus taaskäivitatakse ainult siis, kui see on tõesti vajalik.  

---

## Mitme handleri kasutamine

Ühte taski saab siduda **mitme handleriga** ehk üks *task* saab `notify:` kaudu teavitada mitut handlerit.
Seda tehakse `notify:` all loendina:

```yaml
tasks:
  - name: Uuenda rakenduse konfiguratsioon
    template:
      src: templates/app.conf.j2
      dest: /etc/myapp/app.conf
    notify:
      - Taaskäivita rakendus
      - Logi muudatus

handlers:
  - name: Taaskäivita rakendus
    service:
      name: myapp
      state: restarted

  - name: Logi muudatus
    debug:
      msg: "Rakenduse konfiguratsioon uuendati ja teenus taaskäivitati."
```

**Selgitus:**

- Kui task muudab midagi, kutsutakse mõlemad handlerid.  
- Kui midagi ei muutunud, ei teavitata ühtegi handlerit.  
- Handlerid käivitatakse järjekorras, milles need on määratud.

---

## Handlerite korduvkasutus

Kui mitu taski kasutab sama handlerit, kutsutakse see **ainult üks kord** isegi siis, kui teda teavitati mitu korda.

Näide:

```yaml
tasks:
  - name: Uuenda nginx.conf
    template:
      src: templates/nginx.conf.j2
      dest: /etc/nginx/nginx.conf
    notify: Taaskäivita nginx

  - name: Uuenda vhosti konfiguratsioonid
    template:
      src: templates/vhost.conf.j2
      dest: /etc/nginx/sites-available/default.conf
    notify: Taaskäivita nginx

handlers:
  - name: Taaskäivita nginx
    service:
      name: nginx
      state: restarted
```

**Ansible ei taaskäivita teenust kaks korda!**  
Kui mitu taski teavitab sama handlerit, lisatakse see vaid ühte „järjekorda“ ja käivitatakse **üks kord playbooki lõpus**.  

---

## Handlerite käsitsi käivitamine (`meta: flush_handlers`)

Vaikimisi käivitatakse kõik handlerid **playbooki lõpus**.  
Kui on vaja käivitada need **kohe pärast teatud taski**, saab kasutada:

```yaml
- name: Käivita handlerid kohe
  meta: flush_handlers
```

Näiteks:

```yaml
tasks:
  - name: Uuenda nginx konfiguratsioon
    template:
      src: templates/nginx.conf.j2
      dest: /etc/nginx/nginx.conf
    notify: Taaskäivita nginx

  - name: Käivita handler kohe pärast muudatust
    meta: flush_handlers

  - name: Kontrolli, kas nginx töötab
    shell: systemctl status nginx

handlers:
  - name: Taaskäivita nginx
    service:
      name: nginx
      state: restarted
```

---

## Rohkem infot

- [Ansible Handlers (Playbook Guide)](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_handlers.html){:target="_blank"}  
- [Using `meta: flush_handlers`](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/meta_module.html){:target="_blank"}  
