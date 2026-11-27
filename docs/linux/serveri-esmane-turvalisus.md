---
icon: material/debian
---

# Linuxi serveri esmane turvamine

Juhendis kasutatav operatsioonisüsteem: **Debian 13**

**Turvamine** ehk *hardening* tähendab serveri ründepinna vähendamist ja turvalisemate seadistuste rakendamist.
Selles juhendis on kirjeldatud mõned levinumad esmese turvalisuse tagamise meetodid.

| Kaitse | Milleks seda kasutame? |
|---|---|
| SSH-võtmed ja turvaseaded | Turvaline kaughaldus ning lubatud autentimisviisid. |
| UFW | Piirab serverile võrgu kaudu ligipääsu. |
| Fail2ban | Tuvastab korduvad ebaõnnestunud sisselogimised ja blokeerib lähteaadressi ajutiselt. |
| Turvauuendused | Parandavad tarkvara teadaolevaid turvanõrkusi. |

Need täiendavad teenuste ligipääsuõigusi, HTTPS-i ja toimivat varundust. Ükski neist eraldi ei taga serveri täielikku turvalisust.

---

## Näidisvõrk ja eeldused

| Seade | Näidisväärtus |
|---|---|
| Turvatav veebiserver | `10.100.0.5` |
| Haldusarvuti, Debian | `10.100.0.10` |
| Eraldi testklient | `10.100.0.20` |
| Serveri halduskonto | `haldur` |

**Asenda näidisväärtused oma võrgu andmetega.** Haldusarvutil peab olema püsiv IP-aadress või DHCP-reservatsioon. Juhend on mõeldud teenuseserverile; ruuteri tulemüüri seadistatakse eraldi.

Serveri seadistamise käsud käivita administraatorina: `su -` või `sudo -i`.

!!! warning "Säilita ligipääs serverile"

    Hoia senine haldusühendus avatuna ja kontrolli iga olulisema muudatuse järel uut ühendust teisest terminalist. Sul peab olema ligipääs ka VMware'i konsoolile, et saaksid eksliku SSH- või tulemüüriseadistuse parandada.

---

## 1. Uuendused ja töötavate teenuste ülevaatus

**Serveris:**

```bash
apt update
apt upgrade
apt install openssh-server sudo ufw unattended-upgrades
```

Uuendamine võib teenuseid taaskäivitada. Töötava ettevõtte serveri hooldus tehakse kokkulepitud ajal.

Vaata töötavaid teenuseid ja kuulatavaid porte:

```bash
systemctl list-units --type=service --state=running
ss -lntup
```

Iga võrku kuulava teenuse kohta peaksid teadma, milleks seda vaja on. Kontrolli ka kuulamisaadressi: `127.0.0.1` on ainult kohalik ühendus, `0.0.0.0` kõik IPv4-liidesed ning `[::]` kõik IPv6-liidesed.

Peata ja eemalda automaatkäivitusest teenused, mida selle serveri roll ei vaja. Enne kontrolli nende sõltuvusi ja otstarvet.

---

## 2. Halduskonto ja SSH-võti

### Serveris

Kasuta tavalist kasutajakontot ja tee haldustoiminguid `sudo` kaudu. Kui sobiv konto on juba olemas, kasuta seda. Uue konto loomine:

```bash
adduser haldur
usermod -aG sudo haldur
systemctl enable --now ssh
```

Vaata VMware'i konsoolist serveri Ed25519 hostivõtme sõrmejälge:

```bash
ssh-keygen -lf /etc/ssh/ssh_host_ed25519_key.pub
```

Esimesel SSH-ühendusel võrdle kliendile kuvatavat sõrmejälge selle tulemusega.

### Haldusarvutis

**Järgmised käsud tee Debian haldusarvutis tavalise kasutajana.** Kui klienditööriistad puuduvad, paigalda administraatorina pakett `openssh-client`.

Loo võtmeparooliga kaitstud võtmepaar. Kui sama nimega võti on juba olemas, kasuta olemasolevat või vali uus failinimi:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_labor -C "haldur-labor"
ssh-copy-id -i ~/.ssh/id_ed25519_labor.pub haldur@10.100.0.5
```

`ssh-copy-id` küsib esialgu serveri kasutaja parooli. Privaatvõti jääb haldusarvutisse; serveri `authorized_keys` faili lisatakse avalik `.pub` võti.

Kontrolli ühendust **ainult võtmega**, et parooliga varuvariant tulemust ei varjaks:

```bash
ssh -i ~/.ssh/id_ed25519_labor -o IdentitiesOnly=yes -o PreferredAuthentications=publickey haldur@10.100.0.5
```

Võtme kaitseparooli küsimine on normaalne. See on haldusarvutis oleva võtme parool.

Serverisse sisenemise järel kontrolli haldusõigust:

```bash
sudo -v
```

See küsib vajadusel kasutaja `haldur` serveri parooli. **Jätka alles siis, kui võtmega sisselogimine ja `sudo` töötavad.**

---

## 3. SSH turvaseaded

**Serveris** loo oma seadistusfail:

```bash
nano /etc/ssh/sshd_config.d/00-kohalik-turve.conf
```

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
KbdInteractiveAuthentication no
MaxAuthTries 3
LoginGraceTime 60
X11Forwarding no
```

| Parameeter | Selgitus |
|---|---|
| `PermitRootLogin no` | Keelab `root`-kontoga SSH-sisselogimise. Haldamiseks kasutatakse oma kontot ja `sudo` käske. |
| `PubkeyAuthentication yes` | Lubab võtmega autentimise. |
| `PasswordAuthentication no` | Keelab SSH paroolimeetodi. |
| `KbdInteractiveAuthentication no` | Keelab ka interaktiivse parooliküsimise, näiteks PAM-i kaudu. |
| `MaxAuthTries 3` | Ühe ühenduse kohta kuni kolm autentimiskatset. |
| `LoginGraceTime 60` | Autentimiseks on aega 60 sekundit. |
| `X11Forwarding no` | Keelab graafiliste X11-rakenduste edastamise SSH kaudu. |

Paroolimeetodite keelamine puudutab ka LDAP-kontodega **SSH kaudu** sisselogimist. Nende kontode jaoks tuleb samuti seadistada võtmega ligipääs.

Kontrolli süntaksit ja tegelikult rakenduvaid väärtusi:

```bash
sshd -t
sshd -T | grep -E '^(permitrootlogin|pubkeyauthentication|passwordauthentication|kbdinteractiveauthentication|maxauthtries|logingracetime|x11forwarding) '
```

`sshd -t` ei anna korras seadistuse puhul väljundit. OpenSSH kasutab nende parameetrite korral esimest leitud väärtust; seetõttu kontrolli alati ka `sshd -T` tulemust. Varasemad seadistusfailid või `Match` plokid võivad tulemust mõjutada.

Kui kontroll õnnestub, laadi seadistus uuesti:

```bash
systemctl reload ssh
```

**Haldusarvutis** ava uus võtmega ühendus eelmise sammu käsuga. Seejärel kontrolli, et paroolimeetod oleks keelatud:

```bash
ssh -o PubkeyAuthentication=no -o PreferredAuthentications=password haldur@10.100.0.5
```

Viimane ühendus peab ebaõnnestuma serveri parooli küsimata.

---

## 4. UFW tulemüür

**Serveris** kontrolli esmalt seniseid reegleid:

```bash
ufw status numbered
```

Kontrolli failist `/etc/default/ufw`, et `IPV6=yes`: nii haldab UFW ka IPv6 liikluse reegleid.

Määra vaikimisi poliitika ja luba SSH ainult haldusarvutist:

```bash
ufw default deny incoming
ufw default allow outgoing
ufw allow from 10.100.0.10 to any port 22 proto tcp
```

Selle näite veebiserveris luba HTTP laborivõrgust:

```bash
ufw allow from 10.100.0.0/24 to any port 80 proto tcp
```

### Reeglid serveri rolli järgi

Lisa **enne tulemüüri aktiveerimist ainult selle serveri teenustele vajalikud reeglid**. Näited teiste rollide ja HTTPS-i jaoks:

| Teenus | Näidisreegel laborivõrgule |
|---|---|
| HTTPS | `ufw allow from 10.100.0.0/24 to any port 443 proto tcp` |
| DNS, UDP | `ufw allow from 10.100.0.0/24 to any port 53 proto udp` |
| DNS, TCP | `ufw allow from 10.100.0.0/24 to any port 53 proto tcp` |
| LDAP / StartTLS | `ufw allow from 10.100.0.0/24 to any port 389 proto tcp` |
| Samba | `ufw allow from 10.100.0.0/24 to any port 445 proto tcp` |
| DHCPv4 sisevõrguliidesel | `ufw allow in on ens192 to any port 67 proto udp` |

DHCP näites asenda `ens192` oma sisevõrguliidesega. Esialgsel DHCP-kliendil ei pruugi veel olla laborivõrgu IP-aadressi. Kea vaikimisi `raw`-soklid võivad IP-tulemüürist mööda minna, seega määra DHCP teenindatavad liidesed ka Kea enda seadistuses.

Samal serveril töötava WordPressi ja MariaDB vaheline kohalik ühendus ei vaja andmebaasi pordi 3306 avamist võrku.

!!! warning "Kontrolli varasemaid lubavaid reegleid"

    Kitsama reegli lisamine ei eemalda vana üldist lubavat reeglit. Näiteks varasem `22/tcp ALLOW Anywhere` lubab endiselt SSH-d mujalt. Eemalda üleliigsed reeglid käsuga `ufw delete REEGLI_NUMBER`, vaadates enne nende numbreid käsuga `ufw status numbered`.

Aktiveeri tulemüür, kinnita küsimus ning kontrolli tulemust:

```bash
ufw enable
ufw status verbose
```

`ufw enable` lubab tulemüüri ka järgmistel käivitustel. Kontrolli haldusarvutist **uut SSH-ühendust** ja veebilehe avanemist. Testkliendist peab veeb avanema, kuid SSH olema blokeeritud.

---

## 5. Fail2bani seadistamine

Fail2ban loeb autentimislogisid ja reageerib korduvatele vigadele. Selles näites lisab ta blokeeringud **UFW kaudu**.

**Serveris** paigalda Fail2ban ja loo seadistusfail:

```bash
apt install fail2ban
nano /etc/fail2ban/jail.d/ssh.local
```

```ini
[sshd]
enabled = true
backend = systemd
port = 22
banaction = ufw
maxretry = 5
findtime = 10m
bantime = 10m
ignoreip = 127.0.0.1/8 ::1 10.100.0.10
```

| Parameeter | Selgitus |
|---|---|
| `[sshd]` | SSH-logide jälgimise ja blokeerimise reeglistik ehk *jail*. |
| `backend = systemd` | Loeb systemd journal'it; eraldi `/var/log/auth.log` faili pole vaja. |
| `banaction = ufw` | Lisab blokeeringu UFW kaudu. UFW peab olema aktiveeritud. |
| `maxretry` / `findtime` | Viis tuvastatud ebaõnnestumist kümne minuti jooksul põhjustavad blokeeringu. |
| `bantime` | Blokeering kestab kümme minutit. |
| `ignoreip` | Aadressid, mida logivigade põhjal automaatselt ei blokeerita. Siin on lisatud haldusarvuti. |

Selle `ufw` tegevuse vaikeseadistus blokeerib lähteaadressilt uued ühendused **kõigile serveri portidele**. Lisa erandiks ainult vajalik haldusaadress, mitte kogu laborivõrk.

Kontrolli seadistust ja käivita teenus:

```bash
fail2ban-client -t
systemctl enable --now fail2ban
systemctl restart fail2ban
fail2ban-client status sshd
```

### Blokeerimise kontroll

Kasuta eraldi testklienti `10.100.0.20`, mida pole `ignoreip` loendis.

1. **Serveris** luba testimise ajaks ka selle kliendi SSH-ühendused, et katsed jõuaksid SSH logisse:

    ```bash
    ufw allow from 10.100.0.20 to any port 22 proto tcp
    ```

2. **Testkliendis** proovi olematu kontoga sisse logida:

    ```bash
    ssh -o ConnectTimeout=5 olematu@10.100.0.5
    ```

    Korda katset vajadusel 5–6 korda. Algul peab SSH autentimise tagasi lükkama; blokeeringu järel peab uus ühendus ebaõnnestuma juba võrgu tasemel. Üks ühendus võib tekitada mitu loendatavat logisündmust.

3. **Serveris** kontrolli tuvastamist ja blokeeringut:

    ```bash
    fail2ban-client status sshd
    ufw status numbered
    journalctl -u ssh -n 30 --no-pager
    ```

    Testkliendi IP peab olema blokeeritud aadresside loendis. Haldusarvuti ühendus peab endiselt töötama.

4. Eemalda pärast kontrolli blokeering ja ajutine lubav reegel:

    ```bash
    fail2ban-client set sshd unbanip 10.100.0.20
    ufw delete allow from 10.100.0.20 to any port 22 proto tcp
    ```

Olemasolev ühendus võib blokeeringu ajal püsima jääda. Seetõttu kontrollitakse alati **uue ühenduse** loomist.

---

## 6. Automaatne uuendamine

**Serveris** luba automaatne uuendamine:

```bash
dpkg-reconfigure unattended-upgrades
```

Vali automaatsete uuenduste lubamiseks **Yes**. Lubatud uuendusallikad on kirjas failis `/etc/apt/apt.conf.d/50unattended-upgrades`; nende hulgas peab olema Debiani turvauuenduste allikas.

Et server ei taaskäivituks automaatselt, loo fail:

```bash
nano /etc/apt/apt.conf.d/52kohalik-uuendamine
```

```text
Unattended-Upgrade::Automatic-Reboot "false";
```

Kontrolli seadistust proovikäivitusega:

```bash
unattended-upgrade --dry-run --debug
systemctl list-timers 'apt-daily*' --all
```

`--dry-run` simuleerib uuendamist. Väljundist peab nägema sobivaid uuendusallikaid; paigaldatavaid uuendusi ei pruugi parasjagu olla. Vajalikud taaskäivitused teeb administraator hooldusajal.

Uuendamise logi:

```bash
less /var/log/unattended-upgrades/unattended-upgrades.log
```

---

## 7. Lõppkontroll

| Kontroll | Oodatud tulemus |
|---|---|
| Võtmega SSH haldusarvutist | Töötab, samuti `sudo`. |
| Ainult paroolimeetodiga SSH | Ebaõnnestub. |
| SSH seadistuse kontroll | `PermitRootLogin no` ja paroolimeetodid keelatud. |
| Serveri vajalikud teenused kliendist | Töötavad pärast tulemüüri aktiveerimist. |
| SSH mujalt kui haldusarvutist | Pärast ajutise testreegli eemaldamist blokeeritud. |
| Fail2ban | Tuvastab testkliendi vead ja lisab blokeeringu. |
| Uuendamine | Proovikäivitus õnnestub ning ajastajad on olemas. |

Salvesta oma seadistus ja kontrolli hooldusajal ka pärast serveri taaskäivitamist, et ligipääs, tulemüür ning vajalikud teenused töötavad.

---

## Rohkem infot

- [Debian 13: SSH serveri seadistus](https://manpages.debian.org/trixie/openssh-server/sshd_config.5.en.html)
- [Debian 13: UFW](https://manpages.debian.org/trixie/ufw/ufw.8.en.html)
- [Debian 13: Fail2bani seadistus](https://manpages.debian.org/trixie/fail2ban/jail.conf.5.en.html)
- [Debian 13: automaatne uuendamine](https://manpages.debian.org/trixie/unattended-upgrades/unattended-upgrade.8.en.html)
- [ISC: Kea raw-soklid ja tulemüür](https://kb.isc.org/docs/i-set-up-a-firewall-but-the-kea-server-still-receives-packets-why)

---

*Õppematerjali koostaja: Priit Paap, 2026*
