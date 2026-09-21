# Ülesanne 1.1: Debian Linuxi paigaldus

## 1. Virtuaalmasina loomine

Loo ISO-failist uus Debiani virtuaalmasin järgmiste seadistustega:

- **Nimi:** `LNXH_Debklient1_<omanimi>`
- **VM Storage Policy:** `Default VIKK Thin Policy`
- **Network Adapter:** vali enda nimega võrk
- **RAM:** 4 GB 
- **Video memory:** 16 MB
- **ISO:** Debian (kõige uuem laboris olev versioon)

## 2. Debian Linuxi paigaldamine

Käivita virtuaalmasin ja tee Debian Linuxi paigaldus. Kasuta paigaldamisel järgmisi valikuid.

### Paigaldusviis ja piirkonnasätted

- **Paigaldusviis:** `Graphical Install`
- **Select a language:** `English`
- **Select your location:** `Other` → `Europe` → `Estonia`
- **Configure locales:** `en_US.UTF-8` (vaikevalik)
- **Configure the keyboard:** `Estonian`

### Võrguseadistus

Automaatne võrguseadistus peab ebaõnnestuma, sest võrgus pole DHCP-serverit. Seejärel vali `Configure network manually` ja sisesta järgmised andmed:

| Parameeter | Väärtus |
|---|---|
| IP-aadress | `10.100.0.90` |
| Võrgumask | `255.255.255.0` |
| Vaikelüüs | `10.100.0.1` |
| Nimeserverite aadressid | `1.1.1.1 8.8.8.8` |

Seadista arvuti nimi ja domeen:

- **Hostname:** `debcli1-<sinunimi>` (näiteks `debcli1-peeter`)
- **Domain name:** jäta tühjaks

### Kasutajakonto

- **Root password:** jäta tühjaks
- **Full name for the new user:** `student`
- **Username for your account:** `student`
- **Choose a password for the new user:** `Passw0rd`

!!! info "Miks jätta root-parool tühjaks?"

    Kui root-parool jäetakse Debiani paigaldamisel tühjaks, lukustatakse `root`-konto ja sellega ei saa otse sisse logida. Administraatori õigusi nõudvad käsud käivitatakse tavakasutaja kontolt käsu `sudo` abil. See vähendab volitamata juurdepääsu ohtu ja võimaldab paremini jälgida, milline kasutaja administraatori õigusi kasutas.

### Ketta partitsioneerimine

1. Vali `Guided – use entire disk` ja jätkamiseks `Continue`.
2. Partitsioneerimisskeemiks vali `All files in one partition`.
3. Vali `Finish partitioning and write changes to disk`.
4. Küsimusele `Write changes to disk?` vasta `Yes`.

### Paketihaldus ja tarkvara

- **Scan another CD or DVD?:** `No`
- **Archive mirror country:** `Estonia`
- **Archive mirror:** võid valida sobiva peegli kõik on samaväärsed (aga kiirus võib varieeruda)
- **HTTP proxy:** jäta tühjaks
- **Participate in the package usage survey?:** `No`

Tarkvara valimisel märgi ainult järgmised komponendid:

- [x] Xfce töölaud
- [x] SSH server
- [x] Standard system utilities

### Alglaaduri paigaldamine

- **Install the GRUB boot loader:** `Yes`
- **Device for boot loader installation:** `/dev/sda`

GRUB-i alglaadur on vajalik operatsioonisüsteemi käivitamiseks.

!!! warning "Hoiatus"
    Kui aldlaadur jääb paigaldamata, siis operatsioonisüsteem ei käivitu.


## 3. Töö esitamine

Esita Moodlesse **üks PDF-fail**, mis sisaldab järgmisi kuvatõmmiseid:

1. Virtuaalmasina nimi VMware'i akna ülaosas.
2. Debian Linuxi töölaud koos avatud terminaliga, milles on käivitatud käsk:

    ```bash
    hostnamectl
    ```

3. Seadistatud võrguparameetrid võrguparameetrite seadistuse aknas.

!!! note "Pärast edukat paigaldamist"

    Seda tööjaama kasutatakse edaspidi Linuxi käskude õppimiseks. Sulge masin ja tee pärast paigalduse ja seadistamise lõpetamist virtuaalmasinast **snapshot**, et harjutuste alustamisel oleks alati võimalik taastada puhas masin.

---
*Ülesande koostaja: Priit Paap, 2026*
