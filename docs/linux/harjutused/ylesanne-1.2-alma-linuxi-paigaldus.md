# Ülesanne 1.2: AlmaLinuxi paigaldus

## 1. Virtuaalmasina loomine

Loo ISO-failist uus AlmaLinuxi virtuaalmasin järgmiste seadistustega:

- **Nimi:** `LNXH_Almaklient1_<omanimi>`
- **VM Storage Policy:** `Default VIKK Thin Policy`
- **Network Adapter:** vali enda nimega võrk
- **RAM:** 4 GB
- **Kõvaketas:** 20 GB
- **Video memory:** 16 MB
- **ISO:** AlmaLinux (uusim laboris olev versioon)

## 2. Installeri käivitamine

Käivita virtuaalmasin, vali `Install AlmaLinux` ja oota, kuni paigaldusprogramm käivitub.

## 3. AlmaLinuxi paigaldamine

Paigalda virtuaallaborisse AlmaLinuxi operatsioonisüsteem. Kasuta järgmisi paigaldusvalikuid.

### Keele- ja piirkonnasätted

- **Paigalduskeel:** `English (United States)`
- **Operatsioonisüsteemi keel:** inglise keel
- **Klaviatuur:** `Estonian`
- **Ajavöönd:** `Europe/Tallinn`

### Võrguseadistus

Määra võrguparameetrid käsitsi:

| Parameeter | Väärtus |
|---|---|
| IP-aadress | `10.100.0.91` |
| Võrgumask | `255.255.255.0` |
| Vaikelüüs | `10.100.0.1` |
| DNS-serverid | `1.1.1.1` ja `8.8.8.8` |

- **Hostname ja domain name:** `alm1-<sinunimi>.test.lan`
- **Näide:** `alm1-peeter.test.lan`

### Kasutajakontod

- Ära loo juurkasutajale (`root`) parooli – konto jääb turvakaalutlustel keelatuks.
- **Kasutajanimi:** `student`
- **Parool:** `Passw0rd`
- Määra kasutaja administraatoriks.

!!! info "Miks jätta root-konto keelatuks?"

    Keelatud `root`-kontoga ei saa ründaja proovida otse teadaoleva kasutajanimega administraatorina sisse logida. Administraatori õigusi nõudvad käsud käivitatakse tavakasutaja kontolt käsu `sudo` abil. See vähendab volitamata juurdepääsu ohtu ja võimaldab paremini jälgida, milline kasutaja administraatori õigusi kasutas.

### Ketta ja tarkvara valikud

- Kasuta ketta jagamisel automaatset jaotust.
- Paigaldatava tarkvara valikuks määra `Workstation`.

## 4. Töö esitamine

Esita Moodlesse **üks PDF-fail**, mis sisaldab järgmisi kuvatõmmiseid:

1. Virtuaalmasina nimi VMware'i akna ülaosas.
2. AlmaLinuxi töölaud koos avatud terminaliga, milles on käivitatud käsk:

    ```bash
    hostnamectl
    ```

3. Seadistatud võrguparameetrid võrguparameetrite seadistuse aknas.

!!! note "Pärast edukat paigaldamist"

    Seda tööjaama kasutatakse edaspidi Linuxi käskude õppimiseks. Tee pärast paigalduse ja seadistamise lõpetamist virtuaalmasinast **snapshot**, et ülesande alustamisel oleks alati võimalik taastada puhas masin.

---
*Ülesande koostaja: Priit Paap, 2026*
