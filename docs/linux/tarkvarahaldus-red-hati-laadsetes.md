icon:material/redhat

# Tarkvarahaldus Red Hati laadsetes Linuxi distributsioonides

Red Hati laadsete Linuxi distributsioonide hulka kuuluvad näiteks Red Hat Enterprise Linux (RHEL), AlmaLinux, Rocky Linux ja Fedora. Tarkvara tavaliseks haldamiseks kasutatakse **DNF-i** ning madalama taseme vahendina **`rpm`-i**.

Selles materjalis kasutatakse näidissüsteemina **AlmaLinuxit**. Põhikäsud töötavad üldjuhul ka teistes sama perekonna distributsioonides, kuid hoidlate nimed, pakettide valik ja süsteemi versioon võivad erineda.

Enne alustamist loe [Linuxi tarkvarahalduse ülevaadet](tarkvarahaldus_ulevaade.md).

## Õpieesmärgid

Pärast materjali läbimist oskad:

- kasutada DNF-i hoidlate ja pakettide haldamiseks;
- otsida pakette ning uurida nende infot ja sõltuvusi;
- paigaldada, uuendada ja eemaldada tarkvara;
- eristada paketi paigaldamist teenuse käivitamisest;
- lisada EPEL-i hoidla ja mõista CRB rolli;
- paigaldada kohalikku `.rpm`-faili ning kasutada põhilisi `rpm` käske;
- vaadata DNF-i toimingute ajalugu ja lahendada lihtsamaid probleeme.

## DNF ja `rpm`

| Vahend | Milleks seda kasutada? |
|---|---|
| `dnf` | hoidlatest otsimine, sõltuvuste lahendamine, paigaldamine, uuendamine ja eemaldamine |
| `rpm` | paigaldatud pakettide ning üksikute `.rpm`-failide täpsem uurimine |

!!! note "Aga `yum`?"
    Vanemates RHEL-i põhistes süsteemides kasutati YUM-i. Tänapäevases AlmaLinuxis kasuta `dnf` käsku. `yum` võib ühilduvuse tõttu endiselt olemas olla, kuid suunab tavaliselt DNF-i kasutama.

## Hoidlad

DNF-i põhiseadistus asub failis:

```text
/etc/dnf/dnf.conf
```

Hoidlad kirjeldatakse tavaliselt failides:

```text
/etc/yum.repos.d/*.repo
```

Lubatud hoidlate vaatamine:

```bash
dnf repolist
```

Kõigi hoidlate, ka keelatute vaatamine:

```bash
dnf repolist --all
```

### AlmaLinuxi põhihoidlad

| Hoidla | Üldine sisu |
|---|---|
| **BaseOS** | operatsioonisüsteemi põhikomponendid |
| **AppStream** | rakendused, serveritarkvara, programmeerimiskeeled ja käituskeskkonnad |
| **Extras** | AlmaLinuxi täiendavad paketid ja hoidlate seadistuspaketid |
| **CRB** | osa arendus- ja lisasõltuvusi; võib olla vaikimisi keelatud |

Paketi juures näidatav hoidla nimi aitab hinnata, millisest allikast tarkvara pärineb.

## Metaandmed ja uuendused

Hoidlate metaandmete vahemälu värskendamine:

```bash
sudo dnf makecache
```

Uuenduste kontrollimine:

```bash
dnf check-update
```

!!! note
    `dnf check-update` võib uuenduste leidmisel lõpetada väljumiskoodiga `100`. See ei tähenda sel juhul viga, vaid seda, et uuendused on saadaval.

Süsteemi pakettide uuendamine:

```bash
sudo dnf upgrade --refresh
```

`--refresh` sunnib DNF-i enne toimingut metaandmeid värskendama.

## Paketi otsimine ja uurimine

```bash
dnf search httpd
dnf search --all veebiserver
dnf info httpd
dnf repoquery --requires --resolve httpd
```

| Käsk | Vastab küsimusele |
|---|---|
| `dnf search httpd` | millised paketid sobivad nime või kokkuvõttega? |
| `dnf search --all sõna` | kas sõna leidub ka pikemas kirjelduses? |
| `dnf info httpd` | mis on paketi versioon, arhitektuur, kirjeldus ja hoidla? |
| `dnf repoquery --requires --resolve httpd` | millised paketid pakuvad vajalikke sõltuvusi? |

Kui `repoquery` pole saadaval, paigalda DNF-i lisatööriistad:

```bash
sudo dnf install dnf-plugins-core
```

## Paigaldamise eelvaade

```bash
sudo dnf install httpd --assumeno
```

DNF arvutab pakihaldustoimingu ja vastab kinnitusküsimusele automaatselt eitavalt. Nii saad üle vaadata:

- paigaldatavad paketid;
- sõltuvused;
- allalaadimise mahu;
- kettaruumi vajaduse.

## Paketi paigaldamine

```bash
sudo dnf install httpd
```

Mitme paketi paigaldamine:

```bash
sudo dnf install httpd nmap
```

Kontrolli paigaldatud paketti:

```bash
rpm -q httpd
dnf info --installed httpd
```

## Pakett ja teenus

Veebiserveri paketi paigaldamine ei tähenda tingimata, et teenus juba töötab või käivitub pärast taaskäivitust.

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
systemctl status httpd
```

| Käsk | Tähendus |
|---|---|
| `systemctl start httpd` | käivitab teenuse praegu |
| `systemctl enable httpd` | määrab teenuse käivituma järgmisel alglaadimisel |
| `systemctl enable --now httpd` | lubab automaatkäivituse ja käivitab teenuse kohe |
| `systemctl status httpd` | näitab teenuse hetkeolekut |

!!! tip
    Kontrolli serveritarkvara puhul eraldi paketti, teenust ja vajaduse korral tulemüüri seadistust.

## EPEL-i hoidla

**EPEL** (*Extra Packages for Enterprise Linux*) pakub Enterprise Linuxi perele lisapakette, mida põhihoidlates alati pole.

Paigalda hoidla seadistuspakett:

```bash
sudo dnf install epel-release
```

Kontrolli tulemust:

```bash
dnf repolist
```

AlmaLinux 9 puhul võib mõne EPEL-i paketi sõltuvuste jaoks olla vaja lubada CRB:

```bash
sudo dnf config-manager --set-enabled crb
```

Kui `config-manager` puudub:

```bash
sudo dnf install dnf-plugins-core
```

!!! warning
    Hoidlate nimed ja soovitused võivad AlmaLinuxi põhiversiooniti erineda. Kontrolli enne muutmist `cat /etc/os-release` väljundit ja AlmaLinuxi ametlikku juhendit.

## Tarkvara eemaldamine ja puhastamine

Paketi eemaldamine:

```bash
sudo dnf remove httpd
```

Mittevajalike sõltuvuste eelvaade ja eemaldamine:

```bash
dnf list --autoremove
sudo dnf autoremove
```

Vahemälu puhastamine:

```bash
sudo dnf clean packages
sudo dnf clean metadata
sudo dnf clean all
```

| Käsk | Tulemus |
|---|---|
| `clean packages` | eemaldab allalaaditud RPM-paketid |
| `clean metadata` | eemaldab hoidlate vahemällu salvestatud metaandmed |
| `clean all` | puhastab mõlemad ja muu DNF-i vahemälu |

!!! note
    DNF-il pole APT-i `purge`-käsu täpset vastet. Programm võib jätta alles administraatori muudetud seadistusfaile või tööandmeid; kontrolli neid eraldi.

## Kohaliku `.rpm`-faili paigaldamine

Eelista DNF-i:

```bash
sudo dnf install ./programm.rpm
```

`./` näitab, et tegu on praeguses kaustas oleva failiga. DNF proovib sõltuvused lubatud hoidlatest lahendada.

Madalama taseme variant:

```bash
sudo rpm -ivh programm.rpm
```

`rpm -i` ei lahenda puuduvaid sõltuvusi sama mugavalt kui DNF. Seetõttu sobib see pigem õppimiseks, kontrollitud olukorda või täpseks tõrkeotsinguks.

Enne paigaldamist saad paketti uurida:

```bash
rpm -qpi programm.rpm
rpm -qpl programm.rpm
rpm -K programm.rpm
```

## Kasulikud `rpm` käsud

| Käsk | Eesmärk |
|---|---|
| `rpm -q pakinimi` | kontrollib, kas pakett on paigaldatud |
| `rpm -qi pakinimi` | näitab paigaldatud paketi infot |
| `rpm -ql pakinimi` | näitab paketiga paigaldatud faile |
| `rpm -qf /faili/tee` | leiab, millisele paketile fail kuulub |
| `rpm -qpi fail.rpm` | näitab kohaliku RPM-faili infot |
| `rpm -qpl fail.rpm` | näitab kohaliku RPM-faili sisu |
| `rpm -K fail.rpm` | kontrollib paketi allkirja ja kontrollsummasid |

## DNF-i ajalugu

Varasemate toimingute vaatamine:

```bash
dnf history
```

Ühe toimingu üksikasjad:

```bash
dnf history info ID
```

DNF võib mõnel juhul toimingu tagasi pöörata:

```bash
sudo dnf history undo ID
```

!!! warning
    `history undo` ei ole ajamasin. Kui vanu paketiversioone enam hoidlates pole või vahepeal on tehtud muid muudatusi, ei pruugi tagasipööramine õnnestuda. Vaata kavandatud toiming alati üle.

## Levinud probleemid

### Paketti ei leita

Kontrolli:

```bash
cat /etc/os-release
dnf repolist --all
dnf search --all otsisõna
```

Pakett võib olla teise nimega, puuduva hoidla sees või sinu versioonile mittesaadav.

### Teine pakihaldustoiming töötab

Oota poolelioleva DNF-i või automaatse uuenduse lõppemist. Ära kustuta lukufaile ega lõpeta protsessi enne, kui oled kindel, mida see teeb.

### Teenus ei käivitu

```bash
systemctl status teenus
journalctl -u teenus --no-pager -n 50
```

Veateate põhjus võib olla puuduv seadistus, hõivatud port või vale failiõigus, mitte ebaõnnestunud pakipaigaldus.

## Praktilise töö soovituslik järjekord

1. Tuvasta süsteem käsuga `cat /etc/os-release`.
2. Vaata hoidlaid ning värskenda metaandmete vahemälu.
3. Kontrolli saadaolevaid uuendusi.
4. Otsi paketti `httpd`, vaata selle infot ja sõltuvusi.
5. Vaata paigaldus ette valikuga `--assumeno`, seejärel paigalda pakett.
6. Käivita ja luba `httpd` teenus ning kontrolli selle olekut.
7. Lisa õpetaja juhisel EPEL ja kontrolli hoidla olekut.
8. Harjuta paketi otsimist, eemaldamist ning kohaliku `.rpm`-faili uurimist ja paigaldamist.
9. Vaata tehtud toiminguid käsuga `dnf history`.

## Käskude spikker

| Käsk | Eesmärk |
|---|---|
| `dnf repolist` | näitab lubatud hoidlaid |
| `sudo dnf makecache` | värskendab metaandmete vahemälu |
| `dnf check-update` | näitab uuendatavaid pakette |
| `dnf search nimi` | otsib paketti |
| `dnf info nimi` | näitab paketi infot |
| `dnf repoquery --requires --resolve nimi` | näitab sõltuvusi pakkuvaid pakette |
| `sudo dnf install nimi --assumeno` | näitab kavandatud paigaldust ja katkestab |
| `sudo dnf install nimi` | paigaldab paketi |
| `sudo dnf upgrade --refresh` | värskendab metaandmed ja uuendab paketid |
| `sudo dnf remove nimi` | eemaldab paketi |
| `sudo dnf autoremove` | eemaldab mittevajalikud sõltuvused |
| `sudo dnf install ./fail.rpm` | paigaldab kohaliku RPM-faili |
| `dnf history` | näitab DNF-i toimingute ajalugu |

## Kontrollküsimused

1. Mis vahe on DNF-il ja `rpm`-il?
2. Mida näitavad `dnf repolist` ja `dnf repolist --all`?
3. Mis vahe on käskudel `dnf makecache`, `dnf check-update` ja `dnf upgrade`?
4. Miks võib `dnf check-update` tagastada väljumiskoodi `100`?
5. Kuidas vaatad paketi sõltuvusi?
6. Mis vahe on paketi paigaldamisel ning teenuse käivitamisel ja lubamisel?
7. Mis on EPEL ja millal võib vaja minna CRB-d?
8. Miks on kohaliku `.rpm`-faili paigaldamisel DNF tavaliselt parem kui `rpm -i`?
9. Mida näitavad `rpm -ql` ja `rpm -qf`?
10. Milleks kasutatakse `dnf history` käsku?

## Lisalugemine

- [AlmaLinux Wiki: Extra Repositories ja EPEL](https://wiki.almalinux.org/repos/Extras.html)
- [Red Hat Enterprise Linux 9: Managing software with the DNF tool](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/htmlsingle/managing_software_with_the_dnf_tool/)
- [DNF Command Reference](https://dnf.readthedocs.io/en/latest/command_ref.html)
