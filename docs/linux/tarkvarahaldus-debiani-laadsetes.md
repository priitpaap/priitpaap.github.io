icon:material/debian

# Tarkvarahaldus Debiani-laadsetes Linuxites

Debianis ja teistes debiani baasil olevates distributsioonides kasutatakse tarkvara tavaliseks haldamiseks **APT-i**. Madalama taseme vahend **`dpkg`** haldab kohalikke `.deb`-pakette ja pakettide andmebaasi.

Enne alustamist loe [Linuxi tarkvarahalduse ülevaadet](tarkvarahaldus_ulevaade.md).

## Õpieesmärgid

Pärast materjali läbimist oskad:

- kasutada APT-i pakettide otsimiseks, uurimiseks ja paigaldamiseks;
- eristada käske `apt update`, `apt upgrade` ja `apt full-upgrade`;
- eemaldada paketti ja puhastada mittevajalikke sõltuvusi;
- paigaldada kohalikku `.deb`-faili;
- kasutada põhilisi `dpkg` käske;
- kontrollida tulemust ja lahendada lihtsamaid probleeme.

## APT ja `dpkg`

| Vahend | Milleks seda kasutada? |
|---|---|
| `apt` | hoidlatest otsimine, sõltuvuste lahendamine, paigaldamine, uuendamine ja eemaldamine |
| `dpkg` | paigaldatud pakettide ja üksikute `.deb`-failide täpsem uurimine |

!!! tip
    Tavaliseks paigaldamiseks eelista APT-i. APT kasutab taustal `dpkg`-d, kuid oskab lisaks hoidlatest sõltuvusi hankida.

## Hoidlad ja pakettide nimekiri

APT-i hoidlate seadistus võib asuda järgmistes kohtades:

```text
/etc/apt/sources.list
/etc/apt/sources.list.d/*.list
/etc/apt/sources.list.d/*.sources
```

Hoidlate info laadimiseks arvutisse:

```bash
sudo apt update
```

`apt update` **ei paigalda uuendusi**. See värskendab kohalikku pakettide nimekirja, mille järgi APT teab saadaolevaid versioone.

## Paketi otsimine ja uurimine

```bash
apt search nginx
apt show nginx
apt policy nginx
apt depends nginx
```

| Käsk | Vastab küsimusele |
|---|---|
| `apt search nginx` | millised sobiva nime või kirjeldusega paketid leiduvad? |
| `apt show nginx` | mida pakett sisaldab ja milleks see mõeldud on? |
| `apt policy nginx` | milline versioon on paigaldatud ja milline on saadaval? |
| `apt depends nginx` | milliseid sõltuvusi pakett vajab? |

!!! note
    `apt search` ei vaja tavaliselt `sudo` õigusi, sest see ei muuda süsteemi.

## Paigaldamise eelvaade

Enne muudatust saad tegevust simuleerida:

```bash
apt install -s nginx
```

Vaata väljundist:

- millised paketid paigaldatakse;
- kas midagi eemaldatakse;
- kui palju kettaruumi kasutatakse.

Simulatsioon ei muuda süsteemi.

## Paketi paigaldamine

```bash
sudo apt install nginx
```

Mitme paketi paigaldamine ühe käsuga:

```bash
sudo apt install nginx mc
```

APT küsib enne muudatuse tegemist tavaliselt kinnitust. Ära vasta automaatselt `Y`, vaid loe kokkuvõte läbi.

## Paigalduse kontrollimine

Paketi olek:

```bash
dpkg -s nginx
```

Käsu asukoht:

```bash
command -v nginx
```

Teenuse olek:

```bash
systemctl status nginx
```

!!! note "Pakett ja teenus pole sama asi"
    Paketi paigaldamine lisab failid süsteemi. Teenus võib käivituda kohe, jääda peatatud olekusse või vajada seadistamist. Kontrolli alati tegelikku olekut.

## Tarkvara uuendamine

Uuendatavate pakettide vaatamine:

```bash
apt list --upgradable
```

Tavaline tööjärjekord:

```bash
sudo apt update
sudo apt upgrade
```

| Käsk | Tähendus |
|---|---|
| `apt update` | värskendab pakettide nimekirja |
| `apt upgrade` | uuendab paigaldatud pakette; võib lisada uusi sõltuvusi, kuid ei eemalda paigaldatud pakette |
| `apt full-upgrade` | uuendab süsteemi ja võib sõltuvusprobleemi lahendamiseks pakette eemaldada |

!!! warning
    Enne `full-upgrade` kinnitamist vaata eriti hoolikalt eemaldatavate pakettide nimekirja.

Ühe paketi uuendamine:

```bash
sudo apt install --only-upgrade nginx
```

## Tarkvara eemaldamine

```bash
sudo apt remove nginx
sudo apt purge nginx
sudo apt autoremove
```

| Käsk | Tulemus |
|---|---|
| `remove` | eemaldab paketi, kuid jätab tavaliselt süsteemse konfiguratsiooni alles |
| `purge` | eemaldab paketi ja APT-i hallatava süsteemse konfiguratsiooni |
| `autoremove` | eemaldab automaatselt paigaldatud sõltuvused, mida enam ei vajata |

!!! warning
    Vaata `autoremove` loend enne kinnitamist üle. Kui mõnda paketti on tegelikult vaja, ära eemalda seda pimesi.

Pakettide allalaadimisvahemälu puhastamine:

```bash
sudo apt autoclean
sudo apt clean
```

- `autoclean` eemaldab vananenud paketifailid, mida hoidlast enam alla laadida ei saa;
- `clean` eemaldab APT-i vahemälust kõik allalaaditud paketifailid.

Need käsud ei eemalda paigaldatud programme.

## Kohaliku `.deb`-faili paigaldamine

Eelista APT-i:

```bash
sudo apt install ./programm.deb
```

`./` ütleb, et tegu on praeguses kaustas oleva failiga, mitte hoidlast otsitava paketinimega. APT proovib vajalikud sõltuvused hoidlatest lahendada.

Madalama taseme variant:

```bash
sudo dpkg -i programm.deb
```

Kui `dpkg` teatab puuduvatest sõltuvustest, saab APT sageli olukorra parandada:

```bash
sudo apt --fix-broken install
```

!!! warning
    Paigalda veebist laaditud `.deb` ainult usaldusväärsest allikast ja veendu, et pakett sobib sinu Debiani versiooni ning arhitektuuriga.

## Kasulikud `dpkg` käsud

| Käsk | Eesmärk |
|---|---|
| `dpkg -s pakinimi` | näitab paigaldatud paketi olekut |
| `dpkg -L pakinimi` | näitab paketiga paigaldatud faile |
| `dpkg -S /faili/tee` | leiab, millisele paketile fail kuulub |
| `dpkg -l` | loetleb paketid ja nende oleku |
| `dpkg -I fail.deb` | näitab kohaliku `.deb`-faili infot |
| `dpkg -c fail.deb` | näitab kohaliku `.deb`-faili sisu |

## APT, `apt-get` ja skriptid

`apt` annab inimesele mugava ja loetava väljundi. Skriptides kasutatakse sageli stabiilsema käsurealiidesega käske `apt-get` ja `apt-cache`.

Näiteks:

```bash
sudo apt-get update
sudo apt-get install nginx
```

Õppeülesande käsitsi täitmisel kasuta üldjuhul `apt` käsku, kui juhend ei ütle teisiti.

## Levinud probleemid

### Pakihaldur on lukustatud

Kui samal ajal töötab teine APT-i või `dpkg` protsess, võib ilmuda lukustuse viga. Kontrolli esmalt, kas uuendus on juba pooleli, ja oota selle lõppemist.

!!! danger
    Ära kustuta lukufaile juhusliku internetijuhendi järgi. See võib pakettide andmebaasi rikkuda.

### Pooleli jäänud seadistamine

```bash
sudo dpkg --configure -a
sudo apt --fix-broken install
```

Käivita need ainult siis, kui veateade viitab katkisele või lõpetamata paketitoimingule.

### Logide vaatamine

```bash
less /var/log/apt/history.log
less /var/log/dpkg.log
```

## Praktilise töö soovituslik järjekord

1. Tuvasta süsteem käsuga `cat /etc/os-release`.
2. Käivita `sudo apt update` ja vaata uuendatavaid pakette.
3. Otsi paketti, näiteks `nginx`, ning vaata selle infot ja sõltuvusi.
4. Simuleeri paigaldust käsuga `apt install -s nginx`.
5. Paigalda pakett ja kontrolli paketi ning teenuse olekut.
6. Harjuta paketi eemaldamist, `purge`-käsku ja `autoremove`-eelvaadet.
7. Paigalda õpetaja antud kohalik `.deb`-fail APT-iga ja uuri seda `dpkg` abil.
8. Puhasta vajaduse korral APT-i vahemälu.

## Käskude spikker

| Käsk | Eesmärk |
|---|---|
| `sudo apt update` | värskendab pakettide nimekirja |
| `apt search nimi` | otsib paketti |
| `apt show nimi` | näitab paketi infot |
| `apt policy nimi` | võrdleb paigaldatud ja saadaolevaid versioone |
| `apt depends nimi` | näitab sõltuvusi |
| `apt install -s nimi` | simuleerib paigaldamist |
| `sudo apt install nimi` | paigaldab paketi |
| `apt list --upgradable` | näitab uuendatavaid pakette |
| `sudo apt upgrade` | uuendab paigaldatud pakette neid eemaldamata |
| `sudo apt full-upgrade` | uuendab ja võib vajadusel pakette eemaldada |
| `sudo apt remove nimi` | eemaldab paketi |
| `sudo apt purge nimi` | eemaldab paketi ja süsteemse konfiguratsiooni |
| `sudo apt autoremove` | eemaldab mittevajalikud sõltuvused |
| `sudo apt install ./fail.deb` | paigaldab kohaliku `.deb`-faili |

## Kontrollküsimused

1. Mida teeb `apt update` ja mida see ei tee?
2. Mis vahe on käskudel `apt show` ja `apt policy`?
3. Miks kasutada enne paigaldamist valikut `-s`?
4. Mis vahe on käskudel `upgrade` ja `full-upgrade`?
5. Mis vahe on käskudel `remove`, `purge` ja `autoremove`?
6. Miks on kohaliku `.deb`-faili paigaldamisel APT tavaliselt parem kui `dpkg -i`?
7. Kuidas kontrollid, kas pakett on paigaldatud ja teenus töötab?
8. Miks ei tohi pakihalduri lukufaile kohe kustutada?

## Lisalugemine

- [Debian Manpages: apt(8)](https://manpages.debian.org/stable/apt/apt.8.en.html)
- [Debian Manpages: sources.list(5)](https://manpages.debian.org/stable/apt/sources.list.5.en.html)
- [Debian Administrator's Handbook: APT](https://www.debian.org/doc/manuals/debian-handbook/apt.en.html)

