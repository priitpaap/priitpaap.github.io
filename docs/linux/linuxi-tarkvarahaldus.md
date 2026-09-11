icon:material/debian

# Linuxi tarkvarahalduse ülevaade

Linuxis paigaldatakse, uuendatakse ja eemaldatakse tarkvara tavaliselt **pakihalduri** abil. Pakihaldur peab arvestust paigaldatud tarkvara üle, laadib paketid hoidlatest ning aitab lahendada sõltuvusi.

See fail selgitab ühist loogikat. Praktilised käsud on eraldi materjalides:

- [Tarkvarahaldus Debianis](tarkvarahaldus_debian.md)
- [Tarkvarahaldus AlmaLinuxis](tarkvarahaldus_almalinux.md)

## Õpieesmärgid

Pärast materjali läbimist oskad:

- selgitada paketi, pakihalduri, hoidla ja sõltuvuse tähendust;
- tuvastada Linuxi distributsiooni ning valida sobiva pakihalduri;
- kirjeldada paketi otsimise, paigaldamise, uuendamise ja eemaldamise töövoogu;
- võrrelda APT-i ja DNF-i põhilisi tegevusi;
- valida tarkvara jaoks usaldusväärse allika;
- kontrollida, kas tehtud muudatus andis soovitud tulemuse.

---

## Miks kasutada pakihaldurit?

Programmi võib veebist alla laadida ja failid käsitsi süsteemi kopeerida, kuid siis on keeruline teada:

- millised failid süsteemi lisati;
- milliseid teisi pakette programm vajab;
- kas saadaval on turvauuendusi;
- kuidas programm hiljem korrektselt eemaldada.

Pakihaldur teeb suure osa sellest tööst automaatselt.

```mermaid
flowchart TD
    A["Kasutaja annab käsu"] --> B["Pakihaldur loeb hoidlate infot"]
    B --> C["Kontrollitakse paketti ja sõltuvusi"]
    C --> D["Näidatakse kavandatud muudatused"]
    D --> E["Paketid laaditakse alla ja paigaldatakse"]
    E --> F["Pakettide andmebaas uuendatakse"]
```

!!! tip
    Eelista distributsiooni ametlikku hoidlat. Nii saab pakihaldur tarkvara koos ülejäänud süsteemiga uuendada ja hiljem korrektselt eemaldada.

## Põhimõisted

| Mõiste | Selgitus |
|---|---|
| **tarkvarapakett** (*software package*) | fail või failikogum, mis sisaldab programmi, metaandmeid ja paigaldusjuhiseid |
| **pakihaldur** (*package manager*) | vahend pakettide otsimiseks, paigaldamiseks, uuendamiseks ja eemaldamiseks |
| **hoidla** (*repository*) | kohalikus seadmes või võrgus asuv korrastatud tarkvarapakettide kogu |
| **pakettide nimekiri** (*package index*) | kohalik info hoidlas saadaolevate pakettide, versioonide ja sõltuvuste kohta |
| **sõltuvus** (*dependency*) | teine pakett, mida programm paigaldamiseks või töötamiseks vajab |
| **konflikt** (*conflict*) | olukord, kus kahte paketti ei saa korraga sobivalt paigaldada |
| **versioon** (*version*) | tarkvara väljalaske tunnus |
| **arhitektuur** (*architecture*) | protsessori või süsteemi tüüp, näiteks `amd64` või `arm64` |
| **pakihaldustoiming** (*package transaction*) | ühe käsuga kavandatud paigaldamiste, uuendamiste ja eemaldamiste terviklik kogum |

### Sõltuvuse näide

Kui programm vajab töötamiseks teeki, mida süsteemis veel pole, võib pakihaldur paigaldada korraga:

```text
soovitud programm + vajalik teek + teegi sõltuvused
```

Seepärast tuleb enne kinnitamist vaadata, milliseid pakette pakihaldur kavatseb paigaldada või eemaldada.

## Kuidas Linuxis tarkvara levitatakse?

| Vorm | Näide | Iseloomustus |
|---|---|---|
| distributsiooni pakett | `.deb`, `.rpm` | mõeldud vastava distributsiooni pakihaldurile |
| universaalne rakenduspakett | Flatpak, Snap | kasutab rakenduse jaoks osaliselt eraldatud keskkonda |
| iseseisev rakendusfail | AppImage | rakendus käivitatakse tavaliselt ühest failist |
| lähtekood | arhiiv või Git-projekt | programm tuleb ise kompileerida ja paigaldada |
| keele paketihaldur | `pip`, `npm`, `cargo` | haldab peamiselt vastava keele teeke ja tööriistu |

Süsteemi põhikomponente, teeke ja teenuseid halda üldjuhul distributsiooni pakihalduriga.

!!! warning "Veebist laaditud fail ei ole automaatselt turvaline"
    Faililaiend `.deb`, `.rpm` või `.AppImage` ei tõesta faili usaldusväärsust. Kontrolli allikat ja eelista ametlikku hoidlat või tarkvara tootja ametlikku juhendit.

## Pakihalduse vahendid

| Distributsioonipere | Paketivorming | Madalama taseme vahend | Tavakasutaja pakihaldur |
|---|---|---|---|
| Debian, Ubuntu | `.deb` | `dpkg` | `apt` |
| AlmaLinux, Fedora, RHEL | `.rpm` | `rpm` | `dnf` |
| openSUSE | `.rpm` | `rpm` | `zypper` |
| Arch Linux | `.pkg.tar.zst` | – | `pacman` |

Madalama taseme vahend töötab üksiku paketifaili ja kohaliku paketiandmebaasiga. Kõrgema taseme pakihaldur kasutab hoidlaid ning lahendab tavaliselt ka sõltuvused.

## Distributsiooni tuvastamine

```bash
cat /etc/os-release
```

Pakihalduri olemasolu saad kontrollida nii:

```bash
command -v apt
command -v dnf
```

!!! warning
    Ära vali käsku ainult internetist leitud näite järgi. Esmalt tee kindlaks distributsioon ja selle versioon.

## APT-i ja DNF-i kiirvõrdlus

| Tegevus | Debian: APT | AlmaLinux: DNF |
|---|---|---|
| vaata hoidlaid | vaata APT-i lähtefaile | `dnf repolist` |
| värskenda metaandmeid | `sudo apt update` | `sudo dnf makecache` |
| vaata uuendusi | `apt list --upgradable` | `dnf check-update` |
| otsi paketti | `apt search nimi` | `dnf search nimi` |
| näita infot | `apt show nimi` | `dnf info nimi` |
| paigalduse eelvaade | `apt install -s nimi` | `dnf install nimi --assumeno` |
| paigalda | `sudo apt install nimi` | `sudo dnf install nimi` |
| uuenda süsteemi | `sudo apt update` ja `sudo apt upgrade` | `sudo dnf upgrade --refresh` |
| eemalda | `sudo apt remove nimi` | `sudo dnf remove nimi` |
| kohalik pakett | `sudo apt install ./fail.deb` | `sudo dnf install ./fail.rpm` |

Täpsed selgitused ja erandid on distributsioonipõhistes failides.

## Tarkvaraallika turvalisus

Enne uue hoidla või veebist laaditud paketi kasutamist kontrolli:

1. kas allikas on distributsiooni või tootja ametlik veebileht;
2. kas juhend sobib sinu distributsiooni versiooniga;
3. kas ühendus kasutab HTTPS-i;
4. kas GPG-võtme sõrmejälg või kontrollsumma vastab ametlikule väärtusele;
5. milliseid pakette ja seadistusi käsk muudab.

!!! danger
    Ära käivita tundmatult veebilehelt kopeeritud käsku kujul `curl ... | sudo bash`. See annab allalaaditud skriptile kohe administraatori õigused.

## Hea töövõte

> **Vaata → mõtle → simuleeri → tee → kontrolli.**

1. **Vaata:** tuvasta süsteem, uuri paketti ja selle päritolu.
2. **Mõtle:** kontrolli sõltuvusi, eemaldatavaid pakette ja kettaruumi vajadust.
3. **Simuleeri:** kasuta võimaluse korral APT-i `-s` või DNF-i `--assumeno` valikut.
4. **Tee:** käivita vajalik käsk ja loe enne kinnitamist kokkuvõtet.
5. **Kontrolli:** vaata paketi olekut ning testi programmi või teenust.

## Muud levitusviisid lühidalt

- **Flatpak** sobib eelkõige töölauarakendustele ja kasutab eraldatumat käituskeskkonda.
- **Snap** on Canonicali hallatav universaalne paketivorming ja teenus.
- **AppImage** on tavaliselt üks käivitatav fail; süsteemi pakihaldur seda enamasti ei uuenda.
- **Lähtekoodist paigaldamine** annab rohkem kontrolli, kuid uuendamine ja eemaldamine jäävad sageli administraatori vastutuseks.
- **`pip`, `npm` ja teised keele pakihaldurid** ei ole operatsioonisüsteemi pakihalduri asendajad. Väldi süsteemsete teekide juhuslikku ülekirjutamist.

## Kontrollküsimused

1. Miks on pakihalduri kasutamine turvalisem ja mugavam kui failide käsitsi kopeerimine?
2. Mis vahe on paketil, pakihalduril ja hoidlal?
3. Mis on sõltuvus?
4. Kuidas tuvastad kasutatava distributsiooni?
5. Mis vahe on kõrgema ja madalama taseme pakihaldusvahendil?
6. Miks tuleb enne kinnitamist kavandatud muudatused üle vaadata?
7. Miks ei tõesta faililaiend paketi turvalisust?
8. Kirjelda head viieosalist tööjärjekorda.

## Edasiõppimine

- Debiani praktilised käsud: [Tarkvarahaldus Debianis](tarkvarahaldus_debian.md)
- AlmaLinuxi praktilised käsud: [Tarkvarahaldus AlmaLinuxis](tarkvarahaldus_almalinux.md)

