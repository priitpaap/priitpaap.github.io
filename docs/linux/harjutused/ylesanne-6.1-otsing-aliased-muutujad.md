# Ülesanne 6.1: muutujad, aliased ja otsing

!!! warning "Enne alustamist"

    - Taasta virtuaalmasin snapshot'ist **„algseis“**.
    - Tee ülesanne oma Debiani tööjaamas kasutaja `student` õigustes.
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
wget https://raw.githubusercontent.com/priitpaap/lnxpaigaldus/refs/heads/main/yl6.1-setup.sh
```

**3.** Anna skriptile käivitamisõigus ja käivita see:

```bash
sudo chmod a+x yl6.1-setup.sh
sudo bash yl6.1-setup.sh
```

**4.** Käivita järgmine käsk:

```bash
hostnamectl > k2sud.txt
```

## Failide sisu otsimine ja filtreerimine

**5.** Leia kataloogist `/etc` ja selle alamkataloogidest failid, mille sisus leidub sõna *abiline*. Sõna võib paikneda teise sõna sees, ees või järel ning olla kirjutatud suur- või väiketähtedega. Suuna tulemus kodukausta faili `abiline.txt`.

**6.** Vaata faili `/var/log/syslog` ja filtreeri käsu `grep` abil välja ainult read, mis sisaldavad sõna *Failed*. Tee seda ühe käsuga ning suuna tulemus kodukausta faili `failed.txt`.

**7.** Käivita käsk `ss -tulp` ja filtreeri käsu `grep` abil välja ainult read, mis sisaldavad sõna *ssh*. Tee seda ühe käsuga ning suuna tulemus kodukausta faili `ss.txt`.

## Failide ja kataloogide otsimine

**8.** Otsi kataloogist `/usr/local` ja selle alamkataloogidest kasutajale `peeter` kuuluvaid faile, kuid mitte katalooge. Suuna tulemus kodukausta faili `peeter.txt`.

**9.** Otsi kataloogist `/usr` ja selle alamkataloogidest kasutajale `pille` kuuluvaid katalooge, kuid mitte faile. Suuna tulemus kodukausta faili `pille.txt`.

**10.** Otsi kataloogist `/var/logbackup` faile, mis on vanemad kui viis aastat. Suuna tulemus kodukausta faili `logid.txt`.

**11.** Otsi kataloogist `/var` ja selle alamkataloogidest faile, mis on suuremad kui 300 MB. Suuna tulemus kodukausta faili `big.txt`.

**12.** Otsi ühe käsuga kataloogist `/var` failid, mille nimi algab sõnaga *error*, ja kustuta leitud failid.

**13.** Paigalda otsinguvahend `locate`. Otsi selle abil faili `ssh_config` asukoht ja suuna tulemus kodukausta faili `ssh.txt`.

## Käsualiased

**14.** Seadista kasutajale `student` uus käsualias `list`, mis kuvab kataloogi üksikasjaliku sisu koos peidetud failide ja inimloetavate failisuurustega. Seadista alias püsivalt, et see säiliks pärast taaskäivitamist. **Kontrolli aliase toimimist.**

**15.** Loo kasutajale `student` uus käsualias `vlo`, mis viib kataloogi `/var/log`. Seadista alias püsivalt, et see säiliks pärast taaskäivitamist. **Kontrolli aliase toimimist.**

## Keskkonna seadistamine

**16.** Paigalda tarkvarapakk `cowsay`.

**17.** Seadista kasutaja `student` sisselogimisel käsuga `cowsay` kuvatav teade:

```text
Tere tulemast <kasutajanimi>
```

Kasutajanime kuvamiseks kasuta keskkonnamuutujat `$USER`. **Kontrolli seadistuse toimimist.**

**18.** Lisa kasutaja `student` keskkonnamuutuja `PATH` programmikataloogide hulka kataloog `/srv/programmid`. Seadistus peab säilima ka pärast kasutaja väljalogimist ja uuesti sisselogimist. **Kontrolli seadistuse toimimist.**

## Tulemuste salvestamine ja kontrollimine

**19.** Lisa faili `k2sud.txt` käsu `uptime` väljund:

```bash
uptime >> k2sud.txt
```

**20.** Lisa faili `k2sud.txt` käskude ajalugu:

```bash
history >> k2sud.txt
```

**21.** Laadi alla kontrollskript:

```bash
wget https://raw.githubusercontent.com/priitpaap/lnxpaigaldus/refs/heads/main/yl6.1-check.sh
```

**22.** Anna kontrollskriptile käivitamisõigus ja käivita see:

```bash
sudo chmod a+x yl6.1-check.sh
sudo bash yl6.1-check.sh
```

**23.** Vaata kontrollimise tulemust. Tee vajaduse korral parandused ja käivita kontrollskript uuesti.

## Töö esitamine

**24.** Kui oled vead parandanud, esita Moodle'i kursuse ülesande alla:

- fail `k2sud.txt`;
- kuvatõmmis kontrollskripti tulemusest.

---
*Ülesande koostaja: Priit Paap, 2026*
