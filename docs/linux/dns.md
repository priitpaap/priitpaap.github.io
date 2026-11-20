---
icon: material/debian
---

# DNS-serveri seadistamine (BIND 9)

Juhendis kasutatav operatsioonisüsteem: **Debian 13**  
Juhend käsitleb **IPv4-nimelahendust** BIND 9.20 abil.

Selles juhendis:

- Mis on DNS ja kuidas see töötab
- Milliseid porte DNS kasutab
- Kuidas paigaldada ja seadistada `bind9`
- Kuidas luua päri- ja pöördteisenduse tsoonid
- Kuidas kontrollida seadistust ja testida nimelahendust
- Kuidas anda DNS-serveri aadress klientidele DHCP kaudu

---

## Mis on DNS?

**DNS** (*Domain Name System*) seob domeeninimed IP-aadressidega. Näiteks saab klient nime `printer.firma.lan` järgi teada aadressi `10.100.0.144`. Pöördteisendus võimaldab küsida IP-aadressile vastavat nime.

### DNS-i tööpõhimõte

| Samm | Osapool | Kirjeldus |
|---|---|---|
| **1. Päring** | Klient → DNS-server | Klient küsib nime `printer.firma.lan` IP-aadressi. |
| **2. Otsimine** | DNS-server | Server vastab oma tsooni või vahemälu põhjal. Välise nime päringu võib ta edastada teisele DNS-serverile. |
| **3. Vastus** | DNS-server → klient | Server vastab: `10.100.0.144`. |
| **4. Ühendus** | Klient | Klient kasutab saadud aadressi seadmega ühenduse loomiseks. |

Selles juhendis loome kaks tsooni:

- **Päriteisenduse tsoon** (*forward lookup*) – nimi → IP-aadress, kasutades A-kirjeid.
- **Pöördteisenduse tsoon** (*reverse lookup*) – IP-aadress → nimi, kasutades PTR-kirjeid.

### DNS-i pordid

DNS kasutab nii UDP- kui ka TCP-protokolli pordil **53**.

| Osapool | Port | Kasutus |
|---|---|---|
| DNS-server | UDP 53 | Tavapäraste DNS-päringute vastuvõtmine. |
| DNS-server | TCP 53 | Näiteks UDP jaoks liiga suurte vastuste edastamine ja tsooniedastused. TCP-d kasutatakse ka tavalisteks päringuteks. |
| DNS-klient | Ajutine lähteport | Klient saadab päringu serveri pordile 53 ja võtab vastuse vastu oma päringu lähtepordil. |

Tulemüüris peab klientide ja DNS-serveri vahel olema lubatud nii **UDP 53** kui ka **TCP 53**.

---

## Eeldused ja näidisvõrk

- Käivita serveri halduskäsud **root-kasutajana** või lisa nende ette `sudo`.
- DNS-serveril peab olema püsiv IP-aadress ja kliendid peavad selleni jõudma.
- Pakettide paigaldamiseks ja avalike nimede lahendamiseks peab serveril olema toimiv välisühendus.

Juhendis kasutatav võrk:

| Parameeter | Näidisväärtus |
|---|---|
| Alamvõrk | `10.100.0.0/24` |
| Domeen | `firma.lan` |
| DNS-serveri aadress | `10.100.0.2` |
| DNS-serveri nimi | `ns.firma.lan` |
| Vaikelüüs | `10.100.0.1` |
| Printeri aadress | `10.100.0.144` |
| Väline DNS-server | `1.1.1.1` |

DNS võib töötada samas masinas nagu DHCP-server. Aadressid vastavad eelnevate DHCP-juhendite näidisvõrgule. Muuda need vastavalt oma laborile.

---

## 1. Tarkvara paigaldamine

```bash
sudo apt update
sudo apt install bind9 bind9-utils bind9-dnsutils
```

| Pakett | Kasutus |
|---|---|
| `bind9` | DNS-server ja selle seadistusfailid. |
| `bind9-utils` | Kontrollivahendid `named-checkconf` ja `named-checkzone`. |
| `bind9-dnsutils` | Päringute testimise vahendid, näiteks `dig`. |

!!! info "Tsoonifailide loomine"

    Debian 13 pakett ei sisalda vanu näidisfaile `db.local` ja `db.127`.
    Loome oma tsoonifailid ise. Varasemast süsteemist uuendamisel võivad vanad failid alles olla.

---

## 2. Põhiseadistus – named.conf.options

Tee enne esimest muutmist varukoopia:

```bash
sudo cp -a /etc/bind/named.conf.options /etc/bind/named.conf.options.algne
```

Ava fail:

```bash
sudo nano /etc/bind/named.conf.options
```

Server vastab oma tsoonide päringutele ise. Oma tsoonidest väljapoole jäävate nimede päringud, mille vastust vahemälus pole, edastab ta välisele DNS-serverile.

**Mall **– asenda X oma DNS-serveri aadressiga

```text
options {
    directory "/var/cache/bind";

    forward only;
    
    forwarders {
    1.1.1.1;    // Cloudflare
    8.8.8.8;    // Google
    };

    listen-on { 127.0.0.1; 10.100.0.X; };
    listen-on-v6 { none; };

    dnssec-validation auto;
    recursion yes;
    allow-query { localhost; 10.100.0.0/24; };
    allow-recursion { localhost; 10.100.0.0/24; };
    allow-transfer { none; };
};
```

**Näide** – DNS-server 10.100.0.2

Asenda faili olemasolev `options`-plokk järgmisega. Ära lisa teist `options`-plokki.

```text
options {
    directory "/var/cache/bind";

    forward only;
    
    forwarders {
    1.1.1.1;    // Cloudflare
    8.8.8.8;    // Google
    };

    listen-on { 127.0.0.1; 10.100.0.2; };
    listen-on-v6 { none; };

    dnssec-validation auto;
    recursion yes;
    allow-query { localhost; 10.100.0.0/24; };
    allow-recursion { localhost; 10.100.0.0/24; };
    allow-transfer { none; };
};
```

### Parameetrite selgitus

| Parameeter | Selgitus |
|---|---|
| `directory` | BINDi töökataloog, näiteks vahemälu ja muude tööfailide jaoks. |
| `forwarders` | DNS-serverid, kellele edastatakse väliste nimede päringud. |
| `forward only` | Edasisuunatavate päringute jaoks kasutatakse ainult määratud edasisuunajaid; nende rikke korral ei küsita otse teistelt interneti nimeserveritelt. |
| `listen-on` | IPv4-aadressid, millel server DNS-päringuid vastu võtab. |
| `listen-on-v6` | `none` keelab DNS-päringute vastuvõtmise IPv6 kaudu. |
| `dnssec-validation auto` | Lubab DNSSEC-allkirjade kontrollimise väliste nimede lahendamisel. |
| `recursion yes` | Lubab serveril kliendi eest nimelahendust teha. |
| `allow-query` | Määrab, kes tohib serverile DNS-päringuid teha. |
| `allow-recursion` | Määrab, kes tohib kasutada serverit rekursiivseks nimelahenduseks. |
| `allow-transfer` | `none` keelab tsooni sisu edastamise teistele DNS-serveritele. Selles laboris teist serverit pole. |

!!! info "Sisevõrgu tsoon ja DNSSEC"

    Oma `firma.lan` tsooni teenindamiseks ei ole vaja DNSSEC-valideerimist välja lülitada.
    Server vastab selle tsooni päringutele oma andmetest. Jätame seadeks `auto`.

---

## 3. Tsoonide registreerimine – named.conf.local

Ava fail:

```bash
sudo nano /etc/bind/named.conf.local
```

Lisa kahe tsooni kirjeldused. Tsoonifailid loome järgmistes sammudes.

```text
// Forward lookup: nimi -> IP
zone "firma.lan" {
    type primary;
    file "/var/lib/bind/db.forward.firma.lan";
};

// Reverse lookup: IP -> nimi
zone "0.100.10.in-addr.arpa" {
    type primary;
    file "/var/lib/bind/db.reverse.firma.lan";
};
```

`type primary` tähendab, et server haldab tsooni algandmeid. Vanem `type master` on sama tähendusega sünonüüm; selles juhendis kasutame terminit `primary`.

Tsoonifaile hoiame kataloogis `/var/lib/bind/`. Staatilised tsoonifailid võivad paikneda ka `/etc/bind/` all, kui failitee ja lugemisõigused on sobivad.

Näidisvõrgu `/24` pöördtsooni nimi moodustatakse võrguosa oktettidest vastupidises järjekorras:

```text
10.100.0.0/24 -> 0.100.10.in-addr.arpa
```

---

## 4. Päriteisenduse tsoonifail

Loo fail:

```bash
sudo nano /var/lib/bind/db.forward.firma.lan
```

!!! info "Mida tähendab ns?"

    `ns.firma lan` on selles näites **DNS-serverile valitud nimi**. Kui sinu servelil on teine nimi, siis kasuta seda, näiteks `dns1`. Sel juhul asenda `ns.firma.lan.` nimega `dns1.firma.lan.` kõigis SOA-, NS- ja PTR-kirjetes ning A-kirje alguses `ns` nimega `dns1`. See DNS-is määratud nimi ei pea kattuma Linuxi reaalse hostinimega.

**Mall** – asenda X oma DNS-serveri aadressiga

```text
$TTL 300
@   IN  SOA  ns.firma.lan. root.firma.lan. (
                2026112001 ; Serial
                3600       ; Refresh
                600        ; Retry
                1209600    ; Expire
                300 )      ; Negative Cache TTL

@   IN  NS  ns.firma.lan.
ns  IN  A   10.100.0.X

; Lisa oma seadmete A-kirjed:
; hostinimi  IN  A  10.100.0.X
```

**Näide** – DNS-server .2, ruuter .1 ja printer .144

```text
$TTL 300
@   IN  SOA  ns.firma.lan. root.firma.lan. (
                2026112001 ; Serial
                3600       ; Refresh
                600        ; Retry
                1209600    ; Expire
                300 )      ; Negative Cache TTL

@        IN  NS  ns.firma.lan.
ns       IN  A   10.100.0.2
ruuter   IN  A   10.100.0.1
printer  IN  A   10.100.0.144
```

### Parameetrite selgitus

| Parameeter või kirje | Selgitus |
|---|---|
| `$TTL 300` | Kirjete vaikimisi vahemälus hoidmise aeg: 300 sekundit ehk 5 minutit. Lühike aeg sobib laborisse. |
| `@` | Praeguse tsooni nimi, siin `firma.lan`. |
| `SOA` | Tsooni põhiandmed. `ns.firma.lan.` on primaarne nimeserver ja `root.firma.lan.` tähistab halduri e-posti aadressi `root@firma.lan`. |
| `Serial` | Tsooni versiooninumber. Suurenda seda iga muudatuse järel, näiteks `2026112001` → `2026112002`. |
| `Refresh`, `Retry`, `Expire` | Teisese DNS-serveri tsoonikoopia uuendamise ja aegumise ajad sekundites. Selles laboris teist serverit ei seadistata. |
| `Negative Cache TTL` | Aitab määrata, kui kaua võib puuduva nime või kirje vastust vahemälus hoida. |
| `NS` | Määrab tsooni nimeserveri. |
| `A` | Seob nime IPv4-aadressiga, näiteks `printer.firma.lan` → `10.100.0.144`. |

!!! warning "Punkt nime lõpus"

    Täieliku domeeninime lõppu lisa tsoonifailis punkt: `ns.firma.lan.`.
    Ilma punktita lisatakse nimele tsooni nimi. Lühikeste kirjete nimed, näiteks
    `ns` ja `printer`, jäetakse siin teadlikult suhteliseks.

---

## 5. Pöördteisenduse tsoonifail

Loo fail:

```bash
sudo nano /var/lib/bind/db.reverse.firma.lan
```

PTR-kirje seob aadressi nimega. **Selle `/24` tsooni puhul** kirjutatakse kirje algusesse ainult IP-aadressi viimane oktett.

**Mall**

```text
$TTL 300
@   IN  SOA  ns.firma.lan. root.firma.lan. (
                2026112001 ; Serial
                3600       ; Refresh
                600        ; Retry
                1209600    ; Expire
                300 )      ; Negative Cache TTL

@   IN  NS   ns.firma.lan.
X   IN  PTR  ns.firma.lan.
; X IN  PTR  hostinimi.firma.lan.
```

**Näide**– DNS-server .2, ruuter .1 ja printer .144

```text
$TTL 300
@   IN  SOA  ns.firma.lan. root.firma.lan. (
                2026112001 ; Serial
                3600       ; Refresh
                600        ; Retry
                1209600    ; Expire
                300 )      ; Negative Cache TTL

@    IN  NS   ns.firma.lan.
2    IN  PTR  ns.firma.lan.
1    IN  PTR  ruuter.firma.lan.
144  IN  PTR  printer.firma.lan.
```

!!! info "A- ja PTR-kirjed"

    A-kirje loomine ei tekita automaatselt PTR-kirjet ega vastupidi.
    Hoia mõlema tsooni andmed kooskõlas. Faili muutmisel suurenda selle tsooni serial-numbrit.

---

## 6. Seadistuse kontrollimine

Kontrolli enne teenuse taaskäivitamist seadistus- ja tsoonifaile:

```bash
named-checkconf
named-checkzone firma.lan /var/lib/bind/db.forward.firma.lan
named-checkzone 0.100.10.in-addr.arpa /var/lib/bind/db.reverse.firma.lan
```

`named-checkconf` ei väljasta õnnestumisel tavaliselt midagi. `named-checkzone` peab lõppema reaga **`OK`**. Veateate korral paranda viga enne jätkamist.

---

## 7. Teenuse käivitamine ja kontrollimine

Debianis on BINDi teenuse põhinimi **`named`**.

```bash
sudo systemctl restart named
sudo systemctl enable named
sudo systemctl status named --no-pager
```

Teenus peab olema olekus **active (running)**. Kontrolli ka logi, sest server võib töötada isegi siis, kui mõne tsooni laadimine ebaõnnestus:

```bash
sudo journalctl -u named -b -n 50 --no-pager
```

!!! warning "Kui tsoon ei laadi või teenus ei käivitu"

    Vaata logis näidatud põhjust. Kontrolli failiteid, lugemisõigusi, sulge,
    semikooloneid ja tsoonifailide nimesid. Puuduv punkt võib põhjustada ka vale
    nime tekkimise ilma süntaksiveata. Seepärast testi alati päringuid.

---

## 8. Testimine dig-käsuga

`dig` kuulub paketti `bind9-dnsutils`. `@`-märgiga määrad, milliselt serverilt vastust küsida.

```bash
# Päriteisendus: nimi -> IP
dig @10.100.0.2 ns.firma.lan A
dig @10.100.0.2 printer.firma.lan A

# Pöördteisendus: IP -> nimi
dig @10.100.0.2 -x 10.100.0.144

# Väline nimi: kontrollib ka edasisuunamist
dig @10.100.0.2 debian.org A

# TCP kaudu tehtud päring
dig @10.100.0.2 ns.firma.lan A +tcp

# Lühem väljund
dig @10.100.0.2 printer.firma.lan A +short
```

| Päring | Oodatav vastus |
|---|---|
| `ns.firma.lan A` | `10.100.0.2` |
| `printer.firma.lan A` | `10.100.0.144` |
| `10.100.0.144` pöördteisendus | `printer.firma.lan.` |
| `debian.org A` | Üks või mitu avalikku IPv4-aadressi. |

Edukas näidispäring annab staatuse **`NOERROR`** ja sobiva kirje jaotises **`ANSWER SECTION`**. Oma tsooni vastuses peaks olema ka lipp **`aa`**, mis näitab autoriteetset vastust.

| Tulemus | Tähendus |
|---|---|
| `NXDOMAIN` | Küsitud nime ei leitud. See on olematu nime korral normaalne vastus. |
| `NOERROR`, vastusekirje puudub | Nimi võib olemas olla, kuid küsitud tüüpi kirjet pole. |
| `SERVFAIL` | Server ei suutnud päringut lahendada. Kontrolli tsooni laadimist, edasisuunajat, ühendust ja logi. |
| `REFUSED` | Server keeldus päringust; kontrolli ligipääsureegleid. |
| Ajalõpp (*timeout*) | Kontrolli aadressi, ühendust, tulemüüri ja teenuse tööd. |

---

## 9. DNS-serveri kasutamine klientides

`dig @10.100.0.2` kontrollib kindlat serverit, kuid ei muuda kliendi DNS-seadistust. Et rakendused kasutaksid sisevõrgu nimesid, määra kliendi DNS-serveriks **`10.100.0.2`** käsitsi või DHCP kaudu.

**ISC DHCP** seadistuses muuda DNS-serveri valik:

```text
option domain-name-servers 10.100.0.2;
option domain-name "firma.lan";
```

**Kea** seadistuses muuda olemasolevaid `option-data` kirjeid:

```json
{ "name": "domain-name-servers", "data": "10.100.0.2" },
{ "name": "domain-name", "data": "firma.lan" }
```

Kontrolli DHCP-seadistust, taaskäivita vastav DHCP-teenus ja uuenda kliendi liisingut. Seejärel testi kliendist ka ilma serverit ette andmata:

```bash
dig printer.firma.lan A
```

!!! info "DHCP ja DNS"

    DHCP aadressireserveering ei loo automaatselt BINDi A- ega PTR-kirjet.
    Selles laboris lisame DNS-kirjed käsitsi. Avalikku DNS-serverit `1.1.1.1`
    ära lisa kliendile teiseks DNS-serveriks: see ei tunne `firma.lan` tsooni.
    Avalike nimede päringud suunab edasi sinu BINDi server.

---

## Rohkem infot

```bash
man named.conf
man named-checkconf
man named-checkzone
man dig
```

Ametlikud allikad:

- [BIND 9 dokumentatsioon](https://bind9.readthedocs.io/en/v9.20.30/)
- [Seadistus ja tsoonifailid](https://bind9.readthedocs.io/en/v9.20.30/chapter3.html)
- [Seadistusvalikute kirjeldused](https://bind9.readthedocs.io/en/v9.20.30/reference.html)
- [Debian 13 bind9 paketi failid](https://packages.debian.org/trixie/amd64/bind9/filelist)
- [Debian 13 bind9-dnsutils pakett](https://packages.debian.org/trixie/bind9-dnsutils)

---

*Õppematerjali koostaja: Priit Paap, 2026*
