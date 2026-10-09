# Ülesanne 7.1: kordamisülesanne

Korda seni õpitud failide ja kataloogide haldamist, kasutajate ja gruppide haldust, õiguste määramist, tarkvarahaldust ning otsingut. Ülesanne aitab valmistuda arvestustööks. Proovi lahendada ülesanne iseseisvalt ja kasuta abimaterjale ainult vajaduse korral.

!!! warning "Enne alustamist"

    - Tee ülesanne kooli laborisse kopeeritud virtuaalmasinas **„lnxkordamine-sinu.nimi“**.
    - Tööta kasutaja `student` õigustes.
    - Kasutaja kodukaust on `/home/student`.
    - Proovi kodukaustast mitte lahkuda, kui ülesandes pole öeldud teisiti.
    - Tee tegevused ülesandes esitatud järjekorras.
    - Mitmed käsud tuleb käivitada `sudo` abil.
    - Enne alustamist tee väljalülitatud virtuaalmasinast snapshot, et saaksid vajaduse korral algseisu taastada või ülesannet uuesti lahendada.

## Failid ja kataloogid

**1.** Loo kodukausta uus kataloog nimega `dokumendid`.

**2.** Loo kataloogi `dokumendid` alamkataloogid aastate **2011–2025** jaoks. Kataloogide nimed peavad olema vastavad aastaarvud.

**3.** Kopeeri kataloogist `/var/backups` fail `sissetulek.txt` kataloogi `dokumendid/2024`.

**4.** Kopeeri fail `/etc/passwd` kodukausta nimega `praegused_kasutajad.txt`.

**5.** Loo kataloogi `dokumendid/2024` uus alamkataloog nimega `kasutajad`.

**6.** Kopeeri kodukausta fail `praegused_kasutajad.txt` kataloogi `dokumendid/2024/kasutajad`.

**7.** Kopeeri kataloog `/var/backups/newdata` koos kogu selle sisuga kataloogi `dokumendid/2025`. Tulemuseks peab olema kataloog `dokumendid/2025/newdata` koos selles olevate failidega.

**8.** Kustuta kataloogist `/var/arhiiv` aastate **2000–2009** kataloogid koos nende sisuga.

**9.** Vaata faili `/var/nimed.txt` viit viimast rida ja suuna need kodukausta faili `viimased.txt`.

## Kasutajad ja grupid

**10.** Loo uued kasutajad `juss` ja `pille`. Määra mõlemale parooliks `Passw0rd`.

**11.** Muuda kasutaja `toomas` kasutajanimeks `tom`. Muuda ka tema kodukausta asukoht `/home/toomas` asemel asukohaks `/home/tom` ja vii olemasoleva kodukausta kogu sisu uude asukohta. Tee need muudatused **ühe käsuga**.

**12.** Loo uus kasutajagrupp nimega `raamatupidajad`.

**13.** Lisa kasutaja `juss` gruppi `raamatupidajad`, säilitades tema senised grupikuuluvused.

**14.** Lisa kasutaja `pille` gruppi `sudo`, säilitades tema senised grupikuuluvused.

**15.** Eemalda kasutaja `kalmer` grupist `juhtkond`.

**16.** Kustuta grupp `ajutine`.

**17.** Lukusta kasutaja `praktikant` konto parool, et ta ei saaks parooliga sisse logida.

**18.** Kustuta kasutaja `laura` koos tema kodukaustaga. Tee seda **ühe käsuga**.

## Omanikud ja ligipääsuõigused

**19.** Määra faili `/var/info.txt` omanikuks `juss` ja grupiks `raamatupidajad`.

**20.** Määra kataloogi `/srv/asjad` omanikuks ja grupiks `student`.

**21.** Määra kataloogile `/usr/local/avalik` järgmised õigused:

| Kellele? | Õigused |
| --- | --- |
| Omanik | Kataloogi sisu kuvamine, kirjutamine ja kataloogi sisenemine. |
| Grupp | Kataloogi sisu kuvamine, kirjutamine ja kataloogi sisenemine. |
| Teised kasutajad | Kataloogi sisu kuvamine, kirjutamine ja kataloogi sisenemine. |

**22.** Määra kataloogile `/usr/local/eeskirjad` järgmised õigused:

| Kellele? | Õigused |
| --- | --- |
| Omanik | Kataloogi sisu kuvamine, kirjutamine ja kataloogi sisenemine. |
| Grupp | Kataloogi sisu kuvamine, kirjutamine ja kataloogi sisenemine. |
| Teised kasutajad | Õigused puuduvad. |

**23.** Määra failile `/srv/skriptid/skript23.sh` järgmised õigused:

| Kellele? | Õigused |
| --- | --- |
| Omanik | Lugemine, kirjutamine ja käivitamine. |
| Grupp | Lugemine ja käivitamine. |
| Teised kasutajad | Õigused puuduvad. |

**24.** Määra failile `/srv/notes.txt` järgmised õigused:

| Kellele? | Õigused |
| --- | --- |
| Omanik | Lugemine ja kirjutamine. |
| Grupp | Lugemine ja kirjutamine. |
| Teised kasutajad | Õigused puuduvad. |

**25.** Loo kodukausta link nimega `sisekord`, mis viitab failile `/usr/local/avalik/sisekord.txt`.

**26.** Määra kataloogi `/var/www/wordpress` omanikuks ja grupiks `www-data`. Muudatus peab kehtima ka kõigile selles olevatele failidele ja alamkataloogidele.

## Tarkvarahaldus

**27.** Värskenda saadaolevate tarkvarapakettide nimekirja ja paigalda Samba failiserver. Paketi nimi on `samba`.

**28.** Paigalda **ühe käsuga** järgmised veebiserveri jaoks vajalikud paketid:

- `apache2`;
- `mariadb-server`;
- `php`;
- `libapache2-mod-php`;
- `php-mysql`.

**29.** Otsi tarkvarahoidlast pakette märksõnaga `cacti`. Suuna otsingu tulemus kodukausta faili `cacti.txt` ja selgita välja, mitu paketti otsinguga leiti.

**30.** Vaata, millistest pakettidest sõltub pakett `htop`. Suuna sõltuvuste loend kodukausta faili `depends.txt`.

**31.** Eemalda süsteemist paketid `fortune-mod` ja `cowsay` koos nende seadistusfailidega.

**32.** Laadi alla ülesandes kasutatav Webmini paigalduspakett:

```bash
wget https://sourceforge.net/projects/webadmin/files/webmin/2.202/webmin_2.202_all.deb
```

Paigalda allalaaditud pakett `apt` abil.

## Failide ja nende sisu otsimine

**33.** Leia kataloogist `/usr/local` ja selle alamkataloogidest failid, mille sisus leidub sõna *peitus*. Sõna võib paikneda teise sõna sees, ees või järel ning olla kirjutatud suur- või väiketähtedega. Suuna **failinimede loend** kodukausta faili `otsing1.txt`.

**34.** Otsi kataloogist `/var` ja selle alamkataloogidest faile, mis on suuremad kui **50 MB**. Suuna leitud failide loend kodukausta faili `otsing2.txt`.

**35.** Otsi kataloogist `/etc` ja selle alamkataloogidest faile, mida on viimati muudetud rohkem kui **neli aastat tagasi**. Suuna leitud failide loend kodukausta faili `otsing3.txt`.

**36.** Kasutaja `boss` salvestas faili `plaan.txt` kogemata valesse kataloogi. Leia fail süsteemist üles ja liiguta see tagasi kataloogi `/home/boss`.

## Tulemuste kontrollimine

**37.** Käivita automaatkontroll:

```bash
sudo /opt/check
```

**38.** Vaata kontrollimise tulemust:

```bash
less /home/student/tulemus.txt
```

**39.** Kui mõni toiming ei ole õigesti tehtud, selgita välja põhjus ja paranda viga. Seejärel käivita automaatkontroll uuesti ning vaata tulemust.

Eesmärk on parandada kõik leitud vead ja mõista nende põhjuseid, et oskaksid samu toiminguid arvestustöös iseseisvalt teha.

---

*Ülesande koostaja: Priit Paap, 2026*
