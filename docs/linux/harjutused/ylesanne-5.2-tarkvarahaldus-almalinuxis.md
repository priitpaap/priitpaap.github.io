# Ülesanne 5.2: tarkvarahaldus AlmaLinuxis

!!! warning "Enne alustamist"

    - Taasta virtuaalmasin snapshot'ist **„algseis“**.
    - Tee ülesanne oma AlmaLinuxi tööjaamas kasutaja `student` õigustes.
    - Kasutaja kodukaust on `/home/student`.
    - Proovi kodukaustast mitte lahkuda, kui ülesandes pole öeldud teisiti.
    - Tee tegevused ülesandes esitatud järjekorras.
    - Mitmed käsud tuleb käivitada `sudo` abil.
    - Kasuta käsku `history -c` ainult üks kord harjutuse alguses.

## Ülesande ettevalmistamine

**1.** Tühjenda käskude ajalugu:

```bash
history -c
```

**2.** Laadi alla skript, mis loob ülesande jaoks vajaliku keskkonna:

```bash
wget https://raw.githubusercontent.com/priitpaap/lnxpaigaldus/refs/heads/main/yl5.2-setup.sh
```

**3.** Anna skriptile käivitamisõigus ja käivita see:

```bash
sudo chmod a+x yl5.2-setup.sh
sudo bash yl5.2-setup.sh
```

**4.** Käivita järgmine käsk:

```bash
hostnamectl > alma.txt
```

## Süsteemi uuendamine ja veebiserver

**5.** Uuenda saadaolevate tarkvarapakkide nimekirja.

**6.** Paigalda saadaolevad tarkvarauuendused.

**7.** Paigalda tarkvarapakk `httpd`. Seejärel käivita veebiserveri teenus ja seadista see koos operatsioonisüsteemiga automaatselt käivituma:

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

RHEL-i laadsetes distributsioonides ei käivitata paigaldatud teenust automaatselt.

**8.** Kontrolli veebiserveri toimimist brauseris aadressil:

```text
http://<sinu_ip_aadress>
```

**9.** Vaata paki `httpd` teavet ja suuna tulemus kodukausta faili `httpd.txt`.

**10.** Vaata, millistest teistest pakkidest `httpd` sõltub, ja suuna tulemus kodukausta faili `depends.txt`.

## Repositooriumid ja tarkvarapakid

**11.** Lisa AlmaLinuxile repositoorium **Extra Packages for Enterprise Linux (EPEL)**:

```bash
sudo dnf install epel-release
```

Pärast repositooriumi lisamist kontrolli uuendusi ja paigalda need vajaduse korral.

**12.** Paigalda pakk `toilet`. Käivita järgmised käsud ja vaata nende väljundit:

```bash
toilet Tere
toilet --metal -f smblock <eesnimi> <perenimi>
```

Näide:

```bash
toilet --metal -f smblock Peeter Paan
```

**13.** Eemalda süsteemist pakk `samba`.

**14.** Otsi pakke, mille nimes või kirjelduses sisaldub sõna *scanner*.

**15.** Paigalda otsingutulemuste seast pakk, mille kirjeldus on **„Network exploration tool and security scanner“**, ning käivita see. Tegemist on võrgu skaneerimise tööriistaga.

## Kohaliku paketi paigaldamine

**16.** Paigalda kodukaustas olev fail, mille nimi algab sõnaga `webmin`. Kasuta paigaldamiseks tööriista `dnf` või `rpm`.

**17.** Kontrolli Webmini toimimist oma masina brauseris aadressil:

```text
https://<sinu_ip_aadress>:10000/
```

Sisse logida ei saa, sest seda saab teha ainult `root`-kasutajana ja `root`-konto on selles tööjaamas keelatud.

**18.** Pärast edukat paigaldamist kustuta kodukaustast Webmini installifail.

## Tulemuste salvestamine ja kontrollimine

**19.** Lisa faili `alma.txt` käskude ajalugu:

```bash
history >> alma.txt
```

**20.** Lisa faili `alma.txt` käsu `uptime` väljund:

```bash
uptime >> alma.txt
```

**21.** Laadi alla kontrollskript:

```bash
wget https://raw.githubusercontent.com/priitpaap/lnxpaigaldus/refs/heads/main/yl5.2-check.sh
```

**22.** Anna kontrollskriptile käivitamisõigus ja käivita see:

```bash
sudo chmod a+x yl5.2-check.sh
sudo bash yl5.2-check.sh
```

**23.** Vaata kontrollimise tulemust. Tee vajaduse korral parandused ja käivita kontrollskript uuesti.

## Töö esitamine

**24.** Kui oled vead parandanud, esita Moodle'i kursuse ülesande alla:

- fail `alma.txt`;
- kuvatõmmis kontrollskripti tulemusest.
