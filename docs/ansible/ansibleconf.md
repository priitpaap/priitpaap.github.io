# Ansible konfiguratsioon ja parimad tavad

## Eesmärk

Selles peatükis õpid:

- Mis on `ansible.cfg`
- Millises järjekorras Ansible seadistusfaili otsib
- Millised levinumad  prameetrid, mida fili lisada projekti tasemel

---

## Mis on `ansible.cfg`?

`ansible.cfg` on Ansible’i seadistusfail, millega juhid vaikimisi käitumist (inventory asukoht, rollide teed, väljundi formaat, ühenduse sätted jpm).

**Otsingujärjekord (kõrgeim → madalaim):**
1. **Käesoleva töökausta** fail `ansible.cfg` (soovitatav asukoht)
2. `~/.ansible.cfg` (kasutajaprofiili tasemel)
3. `/etc/ansible/ansible.cfg` (süsteemne, kui kaust olemas)

Kontrolli, millist faili kasutatakse:

```bash
ansible --version
```

Tulemusest on näha config faili asukoht real:

```bash
    config file = 
```
Kui üldakse et `no config`, siis ei suutnud Ansible sedistusfaili asukohta leida ja fail tuleb ise luua.
---

## Soovitatud asukoht
Soovitav ongi peale Ansible paigaldust luua projektikaust ja sinan `ansible.cfg`, kus asuvad selle projekti põhised Ansible seeaded. Näiteks:

```
~/myproject/
├─ ansible.cfg
```

---

## Näidis `ansible.cfg` koos levinumate seadetega

Järgnevalt on esitatud ühe `ansible.cfg` faili sisu koos levinumate parameetritega. Kopeeri vajadusel oma projekti ja kohanda.

```ini
[defaults]
# Inventory asukoht (võib olla YAML või INI) ja võb viidata ka ainult kautale inventory/ kui kasutatakse mitut inventory faili.
inventory = inventory/hosts.yaml

# Rollide otsinguteed (projekti 'roles' kaust eespool)
roles_path = roles:~/.ansible/roles:/usr/share/ansible/roles

# Vaikimisi väljundi formaatija (väljund loetavam kui 'default'- lihtsam lugeda ja tõrkeid leida)
stdout_callback = yaml

# Salvestame retry-faile? Tavaliselt mitte. Vaikimisi, kui playbooki käivitamine mingil põhjusel ebaõnnestub mõne hosti puhul, loob Ansible sinu töökausta (või määratud kausta) nn *retry-faili*, mille faili lõpp on .retry. See fail sisaldab **nimekirja hostidest, kus playbook ebaõnnestus**. Selle eesmärk on, et saad järgmise käsuga uuesti käivitada playbooki ainult nende probleemsete hostide peal. N: ansible-playbook site.yml --limit @site.retry
retry_files_enabled = False

# Vähenda 'host key checking' probleeme katsetamisel. Production keskkonnas võid selle uuesti True peale panna.
host_key_checking = False

# Kogume fakte (facts) targalt (vähem müra ja kiiremini kui alati). Nendest on räägitud playbookide materjalis.
gathering = smart

# Faktide vahemälu - lubada vastavalt vajadusele (kiirendab playbooke suuremas keskkonnas)
# fact_caching = jsonfile
# fact_caching_connection = .ansible_cache
# fact_caching_timeout = 7200

# Mitu paralleelset ühendust (vaikimisi 5). Laboris 10–20 on ok.
forks = 10

# Üldine mooduli/ühenduse timeout sekundites
timeout = 30

# Python-tõlgi automaatne tuvastus (hea heterogeensetes süsteemides). Kaotab ära Warning teated, mis on seotud Pythoni versiooni tuvastamisega.
interpreter_python = auto_silent

# Lülita välja lõbus lehm :), vajadusel. Ansible suudab vaikimisi kuvada teatud sõnumeid ja hoiatusi cowsay stiilis (lehmakujuline ASCII-kunst), kui süsteemis on paigaldatud `cowsay` pakett.
nocows = 1

# Faili diffitamine  lubada vastavalt vajadusele (need seaded aitavad jälgida failide muutusi, eriti mallide ja konfiguratsioonifailide puhul.)
# diff_always = True
# diff_ignore_lines = ^#

[privilege_escalation]
# Vaikimisi sudo kasutus
become = True
become_method = sudo
become_ask_pass = False   # Kui vajad parooli, kasuta Ansible käsu lõpus --ask-become-pass

[ssh_connection]
# SSH pipelining kiirendab oluliselt. Vähendab SSH ühenduse kaudu tehtavaid ühenduse loomisi ja sulgemisi - suurte ülesannete jaoks kiirem. Vajab sudoers faili muutmist 'sudo visudo' ja lisada faili 'Defaults:student !use_pty' (kui kasutajanimeks on student) 
pipelining = True

# Stabiilsem control_path erinevate süsteemidega - kasuta kui näed file name too long teateid.
control_path = %(directory)s/%%h-%%p-%%r

# 1–2 paralleelset üritust ühe hosti kohta (vaikimisi 5) – laboris ok jätta default
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

Kui soovid kiiresti vaadata, mis on konfiguratsioonis muudetud võrreldes vaikimisi sätetega, siis selleks on eraldi käsk:

```bash
ansible-config dump --only-changed
```

---

## Hea tava

- Hoia **projekti tasemel** `ansible.cfg`, et seadistused oleksid reprodutseeritavad.
- Kui on olemas globaalne fail `/etc/ansible/ansible.cfg`, siis väldi selle muutmist.
- Kontrolli muudatusi käsuga `ansible-config dump --only-changed`.
- Pööra tähelepanu turvalisusele: ära jäta **production** keskkonnas `host_key_checking=False` ja väldi salasõnade hoidmist selges tekstis (kasuta **Ansible Vault**).

---
