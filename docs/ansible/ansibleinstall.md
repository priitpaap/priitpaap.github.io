# Ansible paigaldamine

## Eesmärk

Selles peatükis õpid:

- Millised on erinevad Ansible paigaldumeetodid
- Kuidas Ansible't paigalda
- Kuhu salvestada Ansible failid

## Eeldused

Ansible’i saab paigaldada erinevatele Linuxi distributsioonidele ja WSL-ile (Windows Subsystem for Linux). Ilma WSL-ita Windowsit ei toetata.

Nõuded: 

- Linuxi distributsioon (nt **Debian**, **Ubuntu**, **CentOS**, **Fedora**, **Arch** või WSL)
- Internetiühendus
- Administraatori (root) õigused
- **Python 3** (tavaliselt on see juba olemas)

Meie vaatame täpsemalt paigaldust Debiani virtuaalmasinale.
Ametliku dokumentatsiooni paigalduse kohta erienevatele Linuxi distributsioonidele leiad [siit](https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html).

---

## Ansible paigaldamine Debianile

Debiani paketihalduris olev Ansible versioon on enamasti aegunud. Soovitus on paigaldada **värskeim versiooni Ansible’i ametlikust PPA-st (Personal Package Archive), pipx-i** või **pip-i kaudu** (`pip` - Pythoni standardne pakihaldur, `pipx` - pip’i peale ehitatud tööriist, mille eesmärk on paigaldada Pythoni käsurea rakendusi isoleeritult, nii et iga rakendus jookseb oma virtuaalkeskkonnas).

- **Paketihaldur**: hea valik üldiseks kasutamiseks, kus uusim versioon pole kriitiline.
- **pipx / pip**: sobib hästi arenduseks ja testimiseks, kui soovitakse uusimaid võimalusi ning eraldatud keskkonda.

### 1. Paigaldamine APT pakihalduri kaudu Ansible'i PPA-st

Debiani kasutajatel tuleb lisada külge Ubuntu PPA.

!!! info
    Tegemist on Ansible ametliku PPA-ga, aga mitte Debiani ametliku PPA-ga. Debiani ametlik backports on alternatiiv, kui ei soovita Ubuntu PPA-d kasutada.


**Debian 12/13** kasutajad saavad kasutada Ubuntu 24.04 PPA-d mille koodnimeks on `noble`. Ansible paigaldub järgmiste käskude abil:

```bash
UBUNTU_CODENAME=noble

wget -O- "https://keyserver.ubuntu.com/pks/lookup?fingerprint=on&op=get&search=0x6125E2A8C77F2818FB7BD15B93C4A3FD7BB9C367" | sudo gpg --dearmour -o /usr/share/keyrings/ansible-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/ansible-archive-keyring.gpg] http://ppa.launchpad.net/ansible/ansible/ubuntu $UBUNTU_CODENAME main" | sudo tee /etc/apt/sources.list.d/ansible.list

sudo apt update && sudo apt install ansible
```
Selliselt paigaldatud Ansible eeliseks on see, et see on uuendatav pakihalduri abil.

### 2. Paigaldamine `pipx` abil (sobib peaaegu igale Linuxile)

Debianil pole `pipx` veel tavaliselt paigaldatud, seega paigaldame kõigepealt selle:
```bash
sudo apt install pipx -y
```

Nüüd paigaldame Ansible ja lisame selle teekonna ka süsteemsete käskude hulka ja kontrollime:
```bash
pipx install --include-deps ansible
pipx ensurepath

```
!!! info
    `--include-deps` on oluline, sest ilma selleta jäetakse osad tööriistad paigaldamata.

**Peale selle logi korra kasutajast välja ja uuesti sisse** muidu Ansible käsud otse ei tööta.

Tulevikus saab sellist paigaldust uuendada järgmiselt:
```bash
pipx upgrade --include-injected ansible
```

### 3. Paigaldamine `pip` abil

!!! info

    Debian 12+ ja teised uuemad distributsioonid nagu Ubuntu 23.04+ järgivad standardit, mis kaitseb süsteemset Pythonit juhusliku süsteemi rikkumise eest. Seetõttu midagi `pip` abil paigaldades antakse veateade "externally-managed environment". Ehk süsteemi haldab APT pakihaldur ja sinna ei tohiks isegi kasutaja tasmele enam pip abil pakette paigaldada.

Soovituslik on kasutada **virtuaalset keskkonda**. Järgnevalt paigaldame keskkonna loomiseks vajaliku paki ja loome uue virtuaalse keskkonna nimega **ansible-venv**. Seejärel paigaldame sinna Ansible (virtuaalne keskkond on isoleeritud Python'i keskkond, kus saab paigaldada sõltuvusi ilma süsteemi tasemel muudatusi tegemata):

```bash
apt install python3-venv
python3 -m venv ansible-venv
source ansible-venv/bin/activate
pip install ansible
```

Selliselt paigaldatud Ansible puhul on vaja iga kord sisse logides virtuaalkeskkond uuesti käivitada:
```bash
source ansible-venv/bin/activate
```
---

## Versiooni kontroll

```bash
ansible --version
```

See näitab Ansible'i versiooni ja ka seda, kust ta on paigaldatud (pip, pakihaldur jne).

---


## Kas Ansible töötab? – esimene test

```bash
ansible localhost -m ping
```

Kui kõik on korras, peaksid nägema midagi sellist:
```json
localhost | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

See tähendab, et Ansible töötab sinu enda masina (localhost) peal.

---

## Tüüpilised probleemid

| Probleem                   | Lahendus                                                                 |
|----------------------------|--------------------------------------------------------------------------|
| `ansible: command not found` | Kontrolli, kas paigaldus õnnestus ja kas PATH sisaldab pipi `--user` kausta või kas virtuaalkeskkond on aktiveeritud. |
| `Permission denied`         | Kasutasid käsku ilma `sudo`-ta? Või SSH-ühendusel puuduvad õigused?         |
| Python puudub               | Paigalda Python: `sudo apt install python3`.                                 |

---

## Kuhu salvestada Ansible failid?

Ansible töötab väga paindlikult — faile võid hoida erinevates kohtades, sõltuvalt sellest, kuidas Ansible paigaldasid ja millist struktuuri projektile soovid. Alljärgnevalt mõned juhised ja soovitused.

| Paigaldusmeetod | Mõju / muutused |
|------------------|----------------------|
| **APT pakihaluri kaudu paigaldades (nt Ubuntu PPA)** | Ansible installitakse süsteemi tasemel PATH-i; varasemad versioonid lõid kausta `/etc/ansible/` ja ka alamkausta `roles/`. **Viimased versioonid enam automaatselt `/etc/ansible/` kausta ei loo** kausta , pead ise sobivasse kohta projektikausta looma ja seadeid haldama `ansible.cfg` kaudu. |  
| **pip / pipx (kasutajapõhine paigaldus)** | Ansible programmifailid paigutatakse kasutaja tasemele. Automaatselt `/etc/ansible/` kausta ei looda, pead ise sobivasse kohta projektikausta looma ja seadeid haldama `ansible.cfg` kaudu. |

**Ansible faile on seega mõistlik hoida tänapäevalt projektipõhiselt eraldi kaustas.** Kausta võib luua kodukasuta, näiteks:

- `~/ansible/`
- `~/projekti-nimi/`

Projektikausta struktuuri näide. Näites olevate failide ja kautsde otstarbest räägime eraldi järgnevates materjalides:

```bash
~/ansible-project/
├─ ansible.cfg # projekti konfiguratsioon
├─ inventory/  # projekti inventory kaust
│ ├─ hosts.yaml
│ └─ group_vars/ # gruppide muutujad
│ └─ webservers.yml
│ └─ host_vars/ # hostide muutujad
│ └─ web1.yml
├─ templates/ # projekti mallide kaust
│ └─ nginx.conf.j2
└─ site.yml
└─ roles/  # rollide kaust (kui kasutad rolle)
```

!!! info
    Soovitus on luua iga projekti jaoks eraldi `ansible.cfg` fail kuhu määrata selle projekti jaoks vajalikud Ansible seaded.

## Harjutus

📌 **Tee ise järele:**

1. Paigalda Debian virtuaalmasin Xfce kasutajaliidesega.
2. Paigalda sinna ansible kasutades ühete ülalpool toodud meetoditest.
2. Kontrolli versiooni `ansible --version`
3. Käivita test: `ansible localhost -m ping`
4. Kui tuleb veateade, proovi see lahendada.

---

## Rohkem infot

[Installing Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html){:target="_blank"}  
[Installing Ansible on specific operating systems](https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html){:target="_blank"}