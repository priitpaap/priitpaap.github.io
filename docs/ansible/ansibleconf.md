# ansible.cfg – seadistused ja parimad tavad

## Eesmärk

Selles peatükis selgitame, mis on `ansible.cfg`, kus see asub, millises järjekorras Ansible seadistusfaili otsib ning millised on **praktilised ja ohutud** võtmesätted, mida lisada projekti tasemel.

---

## Mis on `ansible.cfg`?

`ansible.cfg` on Ansible’i seadistusfail, millega juhid vaikimisi käitumist (inventory asukoht, rollide teed, väljundi formaat, ühenduse sätted jpm).

**Otsingujärjekord (kõrgeim → madalaim):**
1. **Käesoleva töökausta** fail `ansible.cfg` (soovitatav projektipõhiselt)
2. `~/.ansible.cfg` (kasutajaprofiili tasemel)
3. `/etc/ansible/ansible.cfg` (süsteemne, kui olemas)

> Kontrolli, millist faili kasutatakse:
> ```bash
> ansible --version
> ```
> või vaata, mis sätted on muudetud:
> ```bash
> ansible-config dump --only-changed
> ```

---

## Soovitatud asukoht
Projekti põhine lähenemine (nagu kursusel kasutame):
```
/etc/ansible/myproject/
├─ ansible.cfg
├─ inventory/
│  └─ hosts.yaml
├─ templates/
│  └─ nginx.conf.j2
└─ site.yml
```

---

## Näidis `ansible.cfg` (projekti tasemel)

> **Märkus:** kõik read on kommenteeritud, et oleks selge, milleks säte on. Kopeeri vajadusel oma projekti ja kohanda.

```ini
[defaults]
# Inventory asukoht (võib olla YAML või INI)
inventory = inventory/hosts.yaml

# Rollide otsinguteed (projekti 'roles' kaust eespool)
roles_path = roles:~/.ansible/roles:/usr/share/ansible/roles

# Vaikimisi väljundi formaater (loetavam kui 'default')
stdout_callback = yaml

# Salvestame retry-faile? Tavaliselt mitte.
retry_files_enabled = False

# Vähenda 'host key checking' probleeme labokeskkonnas.
# Tootmises võid selle uuesti True peale panna.
host_key_checking = False

# Kogume fakte targalt (vähem müra ja kiiremini kui alati).
gathering = smart

# Faktide vahemälu (kiirendab playbooke suuremas keskkonnas)
# fact_caching = jsonfile
# fact_caching_connection = .ansible_cache
# fact_caching_timeout = 7200

# Mitu paralleelset ühendust (vaikimisi 5). Labos 10–20 on ok.
forks = 10

# Üldine mooduli/ühenduse timeout sekundites
timeout = 30

# Python-tõlgi automaatne tuvastus (hea heterogeensetes süsteemides)
interpreter_python = auto_silent

# Lülita ära lõbus lehm :) (stabiilsem väljund)
nocows = 1

# Faili diffitamine (kasulik templatingu/konfi puhul)
# diff_always = True
# diff_ignore_lines = ^#

# Callback pluginad (vajadusel nt ajastatistikaks)
# callback_whitelist = profile_tasks

[privilege_escalation]
# Vaikimisi sudo kasutus
become = True
become_method = sudo
become_ask_pass = False   # Kui vajad parooli, kasuta --ask-become-pass

[ssh_connection]
# SSH pipelining kiirendab oluliselt (vajab sudoers 'requiretty' = off)
pipelining = True

# Stabiilsem control_path erinevate süsteemidega
control_path = %(directory)s/%%h-%%p-%%r

# 1–2 paralleelset üritust ühe hosti kohta (vaikimisi 5) – labos ok jätta default
# ssh_args = -o ControlMaster=auto -o ControlPersist=60s

# Kui kasutad teistsuguseid võtmeid/porti, võid lisada:
# ssh_extra_args = -o StrictHostKeyChecking=no
```

---

## Selgitused olulisematele sätetele

- **inventory** – viitab projekti inventory failile/kaustale, ei pea iga kord `-i` kasutama.
- **roles_path** – määrab rollide otsinguteed; esimesena projekti `roles/`.
- **stdout_callback = yaml** – teeb väljundi loetavamaks (eriti `debug` ja `register`).
- **retry_files_enabled = False** – ei tekita `*.retry` faile (labos harilikult pole vaja).
- **host_key_checking = False** – lihtsustab labos esmakontakti; *tootmises* pane True.
- **gathering = smart** – kogub fakte optimaalselt, mitte iga kord täismahus.
- **fact_caching** – kui on palju hoste, lülita sisse (nt `jsonfile`) ja määra kaust.
- **forks** – paralleelsus; tõstab kiirust, kuid ära koorma VM hosti üle.
- **interpreter_python = auto_silent** – Ansible leiab ise õige Pythoni (py2/py3/virt).
- **become** plokk – määrab sudo kasutamise vaikimisi.
- **pipelining = True** – vähendab SSH round-trip’e; võib vajada sudoers seadistust.
- **control_path** – lühem/ühtlasem SSH control socketi tee (vältimaks “too long” vigu).

---

## Windowsi hostid (märkus)

Windowsi masinate puhul seadistatakse enamasti **inventorys** vastavad ühenduse argumendid
(nt `ansible_connection=winrm`, `ansible_user`, `ansible_password`, `ansible_winrm_transport` jne).  
`ansible.cfg` tasemel ei ole tavaliselt vaja eraldi Windows-spetsiifilisi sätteid – need jäävad hosti või grupi muutujatesse.

---

## Hea tava

- Hoia **projekti tasemel** `ansible.cfg`, et seadistused oleksid reprodutseeritavad.
- Väldi globaalse `/etc/ansible/ansible.cfg` muutmist, kui see pole vältimatu.
- Kontrolli muudatusi käsuga `ansible-config dump --only-changed`.
- Pööra tähelepanu turvalisusele: ära jäta **tootmises** `host_key_checking=False` ja väldi salasõnade hoidmist selges tekstis (kasuta **Ansible Vault**).

---

## Kiirkontroll

```bash
# Kas see projektikonf on kasutusel?
ansible --version

# Mis on muudetud võrreldes vaikimisi sätetega?
ansible-config dump --only-changed
```
