---
icon: material/debian
---

# Samba failiserveri seadistamine

Juhendis kasutatav operatsioonisüsteem: **Debian 13**

Selles juhendis:

- Mis on Samba ja millist porti see kasutab
- Kuidas luua Samba kasutajaid ja jagada kaustu
- Kuidas määrata lugemis- ja kirjutamisõigusi
- Kuidas kontrollida jagamisi klientarvutist

---

## Mis on Samba?

**Samba** võimaldab Linuxi serveril jagada faile Windowsi ja Linuxi arvutitele. Ühenduseks kasutatakse **SMB-protokolli**, tänapäeval SMB2 või SMB3. Klient avab serveri jagatud kausta ning saab seal faile lugeda või muuta vastavalt oma õigustele.

Selles laboris töötab Samba iseseisva failiserverina. Kasutajad luuakse serveris, domeenikontrollerit ei ole vaja.

### Samba port

| Osapool | Port | Kasutus |
|---|---|---|
| Failiserver | TCP 445 | Võtab vastu klientide SMB-ühendusi |

Klient algatab ühenduse ajutiselt lähtepordilt serveri pordile 445. Kui serveris on tulemüür, luba sisevõrgust `10.100.0.0/24` ühendused TCP-pordile **445**. Juhendis ühendume otse IP-aadressiga.

---

## Näidisvõrk

Selles juhendis kasutatakse näidisvõrku `10.100.0.0/24` ja failiserveri IP-aadressi `10.100.0.3`. Klientarvuti peab saama failiserveriga võrgu kaudu ühenduse.

**Muuda aadressid vastavalt oma võrgule.** Juhendi kasutajad ja grupid on näited, ülesande lahendamisel kasuta ülesandes nõutud nimesid.

### Jagatavad kaustad

Selles juhends jagame kasutad välja järgmiselt:

| Jagamine | Asukoht serveris | Juurdepääs | Õigused |
|---|---|---|---|
| `tarkvara` | `/srv/tarkvara` | Ainult `haldur` | Lugemine ja kirjutamine |
| `dokumendid` | `/srv/dokumendid` | Grupp `projekt`: `kati` ja `kalle` | Ainult lugemine |
| `public` | `/srv/public` | Kõik, ka anonüümselt | Lugemine ja kirjutamine |

!!! info "public kaust"

    "public" kausta pääseb kasutajanime ja paroolita. Kõik külalised saavad seal faile lisada, muuta ja kustutada.

**Serveri seadistamise käsud käivita failiserveris administraatori käsureal:** ava see käsuga `su -` või `sudo -i`.

---

## 1. Tarkvara paigaldus

```bash
apt update
apt install samba
```

---

## 2. Kasutajad ja grupid

Loo Linuxi kasutajad. Kui kasutaja on juba olemas, jäta konto loomine vahele.

```bash
adduser haldur
adduser kati
adduser kalle
```

Laboris võid panna parooliks Passw0rd. Muud lisaandmed võid **Enteriga** vahele jätta.

Loo grupp ja lisa kasutajad sellesse:

```bash
groupadd projekt
usermod -aG projekt kati
usermod -aG projekt kalle
```

`projekt` annab ligipääsu dokumentidele. Võti `-aG` lisab kasutaja gruppi, säilitades tema senised grupid.

### Samba paroolid

Lisa samad kasutajad Samba kasutajate andmebaasi ja määra neile Samba parool:

```bash
smbpasswd -a haldur
smbpasswd -a kati
smbpasswd -a kalle
```

Määra igale näidiskasutajale enda valitud Samba parool. Labors võid kastuada parooli Passw0rd. Seda parooli kasutad hiljem testimisel.

!!! info "Kaks kasutajate andmebaasi"

    Linuxi kasutaja peab olema loodud enne `smbpasswd -a` käsku. Samba ühenduse loomisel kasuta **Samba parooli** — Linuxi parooli muutmine ei muuda automaatselt Samba parooli.

---

## 3. Kaustad ja Linuxi õigused

Loo jagatavad kaustad:

```bash
mkdir -p /srv/tarkvara /srv/dokumendid /srv/public
```

Määra omanikud ja õigused:

```bash
chown haldur:haldur /srv/tarkvara
chmod 700 /srv/tarkvara

chown root:projekt /srv/dokumendid
chmod 750 /srv/dokumendid

chown nobody:nogroup /srv/public
chmod 700 /srv/public
```

| Õigused | Selgitus |
|---|---|
| `700` | Kausta saab kasutada selle omanik: `tarkvara` puhul `haldur`, `public` puhul `nobody`. |
| `750` | Omanik `root` saab kausta muuta; grupp `projekt` saab sisu vaadata ja faile avada. |

`nobody` on olemasolev Linuxi süsteemikasutaja. Samba kasutab teda public kaustas kõigi külaliste failitoiminguteks. Talle ei ole vaja Samba parooli määrata.

Loo administraatorina dokumentide kausta testfail ning anna grupile selle lugemisõigus:

```bash
echo "See on ainult lugemiseks mõeldud dokument." > /srv/dokumendid/teade.txt
chown root:projekt /srv/dokumendid/teade.txt
chmod 640 /srv/dokumendid/teade.txt
```

!!! info "Õigused kahes kohas"

    Juurdepääsu peavad lubama nii **Linuxi failiõigused** kui ka **Samba seadistus**. Samba `read only = no` ei anna kirjutamisõigust, kui Linuxi õigused seda keelavad.

---

## 4. Jagamiste seadistamine – `smb.conf`

Tee algsest seadistusest varukoopia:

```bash
cp -a /etc/samba/smb.conf /etc/samba/smb.conf.algne
```

Ava fail:

```bash
nano /etc/samba/smb.conf
```

**Asenda või muuda faili sisu** järgmiselt. Kasutamäärangud võid lisada faili lõppu:

```ini
[global]
    workgroup = WORKGROUP
    server role = standalone server
    security = user
    map to guest = Bad User
    guest account = nobody

[tarkvara]
    path = /srv/tarkvara
    valid users = haldur
    read only = no

[dokumendid]
    path = /srv/dokumendid
    valid users = @projekt
    read only = yes

[public]
    path = /srv/public
    guest ok = yes
    guest only = yes
    read only = no
```

### Parameetrite selgitus

| Parameeter | Selgitus |
|---|---|
| `[global]` | Serveri üldseaded. `WORKGROUP` on töörühma nimi, mitte DNS-domeen. |
| `server role = standalone server` | Iseseisev server kohalike kasutajatega. |
| `security = user` | Kasutajapõhine autentimine; külalispääs lubatakse jagamises eraldi. |
| `map to guest = Bad User` | Tundmatu kasutajanimega ühendus suunatakse külaliseks. Olemasoleva kasutaja vale parooliga ühendus lükatakse tagasi. |
| `guest account = nobody` | Külaliste failitoimingud tehakse Linuxi kasutaja `nobody` õigustes. |
| `[tarkvara]` jne | Jagamise nimi, mida näeb klient. |
| `path` | Jagatava kausta asukoht serveris. |
| `valid users` | Lubatud kasutajad; `@` nime ees tähistab gruppi. |
| `read only` | `yes` lubab ainult lugeda; `no` lubab ka kirjutada, kui Linuxi õigused seda võimaldavad. |
| `guest ok = yes` | Lubab jagamisele paroolita juurdepääsu. |
| `guest only = yes` | Selles jagamises kasutatakse kõigi ühenduste puhul külaliskontot. |

---

## 5. Teenuse käivitamine ja kontroll

Kontrolli seadistust:

```bash
testparm -s
```

Kui väljundis on **`Loaded services file OK.`** ja vigaseid parameetreid ei näidata, taaskäivita teenus ning luba automaatkäivitus:

```bash
systemctl restart smbd
systemctl enable smbd
systemctl status smbd
```

Oodatud olek on **`active (running)`**. `testparm` kontrollib seadistust; tegelikke juurdepääsuõigusi kontrollime kliendist.

!!! tip "Kui ühendus ei tööta"

    Vaata teenuse logi ja kontrolli kasutaja gruppe ning kaustade õigusi:

    ```bash
    journalctl -u smbd -n 30 --no-pager
    id kati
    ls -ld /srv/tarkvara /srv/dokumendid /srv/public
    ```

---

## 6. Ühendamine klientarvutist

**Järgmised tegevused tee klientarvutis.**

Paigalda administraatorina klienditööriistad:

```bash
apt update
apt install smbclient gvfs-backends
```

`smbclient` võimaldab testida terminalis. `gvfs-backends` lisab SMB toe näiteks Thunari ja GNOME'i failihaldurile. `cifs-utils` on vajalik siis, kui soovid jagamise käsuga `mount` Linuxi kataloogipuu külge haakida; selles juhendis seda ei kasutata.

### Failihaldur

1. Ava failihaldur tavalise töölauakasutajana ja vajuta **Ctrl + L**.
2. Sisesta soovitud jagamise aadress, näiteks:

    ```text
    smb://10.100.0.3/tarkvara
    ```

3. Vali registreeritud kasutajana sisselogimine ning sisesta `haldur` ja tema **Samba parool**. Kui küsitakse domeeni, kasuta `WORKGROUP`.
4. Dokumentide avamiseks kasuta aadressi `smb://10.100.0.3/dokumendid` ja kasutajat `kati` või `kalle`.
5. publicu kausta avamiseks kasuta aadressi `smb://10.100.0.3/public` ning vali sisselogimisel **anonüümne kasutaja** ehk **Anonymous**.

Kui SMB aadress pärast paketi paigaldamist ei avane, logi töölauast välja ja tagasi sisse.

!!! tip "Teise kasutajaga testimine"

    Failihaldur võib sama serveri ühendustes kasutada varasemat sisselogimist. Enne kasutaja vahetamist haagi kõik selle serveri jagamised failihalduris lahti. Kui kasutaja ei vahetu, logi töölauast välja ja tagasi sisse või kasuta allolevaid `smbclient` käske.

### Terminal

Need käsud käivita **tavalise kasutajana**. Järgmised käsud küsivad nimetatud kasutaja Samba parooli ja proovivad kuvada jagamise sisu:

```bash
smbclient //10.100.0.3/tarkvara -U haldur -c 'ls'
smbclient //10.100.0.3/tarkvara -U kati -c 'ls'
smbclient //10.100.0.3/dokumendid -U kalle -c 'ls'
```

Teine käsk peab andma **`NT_STATUS_ACCESS_DENIED`**: Katil pole `tarkvara` jagamisele ligipääsu.

Kontrolli anonüümset ühendust. `-N` keelab parooli küsimise ja `-U ''` määrab tühja kasutajanime:

```bash
smbclient //10.100.0.3/public -N -U '' -c 'ls'
smbclient //10.100.0.3/tarkvara -N -U '' -c 'ls'
```

public kausta sisu kuvamine peab õnnestuma; `tarkvara` jagamisele peab ligipääs olema keelatud.

---

## 7. Jagamiste toimimise kontroll

Kausta nähtavus ei tõesta juurdepääsuõigusi. Proovi faile päriselt avada, lisada ja muuta.

| Kasutaja | Jagamine | Kontroll | Oodatud tulemus |
|---|---|---|---|
| `haldur` | `tarkvara` | Loo tekstifail ja muuda selle sisu. | Õnnestub. |
| `kati` ja `kalle` | `tarkvara` | Proovi jagamist avada mõlema kasutajana. | Ligipääs keelatud. |
| `kati` ja `kalle` | `dokumendid` | Ava `teade.txt`; proovi seda muuta, kustutada ja uut faili lisada. | Lugemine õnnestub; kõik muutmiskatsed on keelatud. |
| `haldur` | `dokumendid` | Proovi jagamist avada. | Ligipääs keelatud. |
| Anonüümne | `public` | Loo alamkaust ja tekstifail; ava, muuda ja kustuta fail. | Õnnestub parooli sisestamata. |
| Anonüümne | `tarkvara` ja `dokumendid` | Proovi jagamisi avada ilma sisselogimiseta. | Ligipääs keelatud. |

public kaustas saavad külalised ka teiste külaliste faile muuta ja kustutada. Failide omanikuks jääb serveris `nobody`, seega ei saa omaniku järgi külalisi eristada.


---

## Rohkem infot

`man smb.conf` | `man smbpasswd` | `man smbclient`

- [Samba seadistusfaili dokumentatsioon](https://www.samba.org/samba/docs/4.22/man-html/smb.conf.5.html)
- [Debiani pakett smbclient](https://packages.debian.org/trixie/smbclient)
- [Debiani pakett gvfs-backends](https://packages.debian.org/trixie/gvfs-backends)

---

*Õppematerjali koostaja: Priit Paap, 2026*
