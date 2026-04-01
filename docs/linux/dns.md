icon:material/debian

# bind9 DNS serveri seadistamine

## Eesmärk

Selles peatükis õpid:

- Mis on DNS ja kuidas see töötab
- Kuidas paigaldada ja seadistada `bind9`
- Kuidas luua forward ja reverse lookup tsoonifailid
- Kuidas kontrollida ja testida DNS serverit

!!! info "bind9 9.20 muutused (Debian 13)"
    Võrreldes vanema versiooniga on mõned olulised muutused:

    - Näidisfailid `db.local` ja `db.127` **enam ei paigaldata** – tsoonifailid tuleb luua nullist.
    - `type master` on asendunud uue terminiga **`type primary`**.

---

## Mis on DNS?

**DNS** (Domain Name System) tõlgib inimloetavaid domeeninimsid (nt `server.firma.lan`) IP-aadressideks (nt `10.100.0.5`) ja vastupidi. Ilma DNS-ita tuleks kõikide serverite aadressid pähe õppida.

### DNS töö põhimõte

| Samm | Osapool | Kirjeldus |
|------|---------|-----------|
| **1. Päring** | Klient → DNS server | Klient küsib: mis IP on `server.firma.lan`? |
| **2. Otsimine** | DNS server | Server otsib tsoonifailist |
| **3. Vastus** | DNS server → klient | DNS vastab: `10.100.0.5` |
| **4. Ühendus** | Klient | Klient loob ühenduse IP-ga |

`bind9` haldab kaht tsoonitüüpi:

- **Forward lookup** – nimi → IP
- **Reverse lookup** – IP → nimi

> **Näidisvõrk:** Selles juhendis kasutatakse näidisvõrku `10.100.0.0/24`, domeeni `firma.lan` ja DNS serveri IP-d `10.100.0.1`. **Muuda need parameetrid vastavalt oma võrgule.**

---

## 1. Tarkvara paigaldus

```bash
apt update
apt install bind9 bind9-utils
```

!!! info
    `bind9-utils` sisaldab olulisi tööriistu: `named-checkconf` (kontrollib konfiguratsioonifailide süntaksit) ja `named-checkzone` (kontrollib tsoonifaile). Soovitav paigaldada koos `bind9`-ga.

Pärast paigaldust on `/etc/bind/` kataloogis ainult neli faili – vanu näidisfaile (`db.local`, `db.127` jt) enam automaatselt ei looda:

```bash
ls /etc/bind/
# named.conf  named.conf.local  named.conf.options  named.conf.root-hints  rndc.key
```

---

## 2. Põhiseadistus – `named.conf.options`

Ava seadistusfail:

```bash
nano /etc/bind/named.conf.options
```

Päringud, mida meie server teenindada ei suuda (nt avalikud domeenid), suunatakse edasi. Lisaks piira server oma võrgule ja keela IPv6 kuulamine.

### Mall – asenda X-id oma võrgu väärtustega

```
options {
    directory "/var/cache/bind";

    forwarders {
        1.1.1.1;  // Cloudflare – või kasuta oma DNS-i
    };

    listen-on { 127.0.0.1; 10.100.0.X; };
    listen-on-v6 { none; };
    dnssec-validation no;
    recursion yes;
    allow-query { localhost; 10.100.0.0/24; };
};
```

### Näide – DNS server IP 10.100.0.1, võrk 10.100.0.0/24

```
options {
    directory "/var/cache/bind";

    forwarders {
        1.1.1.1;
    };

    listen-on { 127.0.0.1; 10.100.0.1; };
    listen-on-v6 { none; };
    dnssec-validation no;
    recursion yes;
    allow-query { localhost; 10.100.0.0/24; };
};
```

### Parameetrite selgitus

| Parameeter | Selgitus |
|------------|----------|
| `forwarders` | Millele suunata päringud, mida server ise lahendada ei suuda. `1.1.1.1` = Cloudflare avalik DNS. |
| `listen-on` | Millistel liidestel server kuulab. Lisa nii `127.0.0.1` (localhost) kui ka serveri IP. |
| `listen-on-v6` | Keelame IPv6 kuulamise, kuna kasutame ainult IPv4 võrku. |
| `dnssec-validation no` | Lülitab välja DNSSEC valideerimise – vajalik sisevõrgu domeenidele (`firma.lan` ei ole allkirjastatud). |
| `allow-query` | Kes tohib DNS päringuid teha. Piira oma alamvõrguga. |

---

## 3. Tsoonide registreerimine – `named.conf.local`

Ava fail:

```bash
nano /etc/bind/named.conf.local
```

Siin registreerime kaks tsooni: forward (nimi → IP) ja reverse (IP → nimi). Tsoonifailid loome järgmises sammus.

```
// Forward lookup tsoon
zone "firma.lan" {
    type primary;
    file "/var/lib/bind/db.forward.firma.lan";
};

// Reverse lookup tsoon (10.100.0.0/24 võrgule)
zone "0.100.10.in-addr.arpa" {
    type primary;
    file "/var/lib/bind/db.reverse.firma.lan";
};
```

!!! info
    **`type primary`** on uus korrektne termin (bind9 9.x alates). Vanem `type master` toimib veel, kuid logib hoiatuse. Kasuta alati `primary`.

!!! info
    Tsoonifailid paiknevad **`/var/lib/bind/`** – see kataloog on `bind` kasutajale kirjutatav. Vana `/etc/bind/` on mõeldud ainult konfiguratsioonifailidele.

Reverse tsooni nimi moodustatakse IP-aadressi oktetid tagurpidi:

```
võrk 10.100.0.0/24  →  tsoon 0.100.10.in-addr.arpa
```

---

## 4. Forward lookup tsoonifail

Loo fail `/var/lib/bind/db.forward.firma.lan`:

```bash
nano /var/lib/bind/db.forward.firma.lan
```

Tsoonifailid luuakse nullist – näidisfaile enam ei ole.

### Mall – asenda X-id oma väärtustega

```
$TTL 604800
@   IN  SOA  ns.firma.lan.  root.firma.lan. (
                6         ; Serial
           604800         ; Refresh
            86400         ; Retry
          2419200         ; Expire
           604800 )       ; Negative Cache TTL

@   IN  NS   ns.firma.lan.
ns  IN  A    10.100.0.X    ; DNS serveri IP

; Lisa siia oma hostid:
; hostinimi  IN  A  10.100.0.X
```

### Näide – DNS server .1, ruuter .1, üks tööjaam .10

```
$TTL 604800
@   IN  SOA  ns.firma.lan.  root.firma.lan. (
                6         ; Serial
           604800         ; Refresh
            86400         ; Retry
          2419200         ; Expire
           604800 )       ; Negative Cache TTL

@         IN  NS   ns.firma.lan.
ns        IN  A    10.100.0.1
ruuter    IN  A    10.100.0.1
tooajaam1 IN  A    10.100.0.10
```

### Parameetrite selgitus

| Parameeter | Selgitus |
|------------|----------|
| `$TTL 604800` | Vaikimisi TTL (Time To Live) sekundites – 604800 = 1 nädal. |
| `@ IN SOA` | Start of Authority – tsooni alguskirje. `@` = tsooni nimi (`firma.lan`). `ns.firma.lan.` = primaarne DNS server. |
| `Serial` | Versiooninumber – **suurenda iga kord kui teed muudatusi** (nt kuupäev: `20240115`). |
| `@ IN NS` | Name Server kirje – millised serverid on selle domeeni DNS serverid. |
| `hostname IN A` | Address kirje – seob hostnime IP-aadressiga. |

---

## 5. Reverse lookup tsoonifail

Loo fail `/var/lib/bind/db.reverse.firma.lan`:

```bash
nano /var/lib/bind/db.reverse.firma.lan
```

Reverse tsoon võimaldab IP → nime lahendamist (PTR kirjed). PTR kirjes kirjutatakse ainult IP **viimane oktet**.

### Mall

```
$TTL 604800
@   IN  SOA  ns.firma.lan.  root.firma.lan. (
                4         ; Serial
           604800         ; Refresh
            86400         ; Retry
          2419200         ; Expire
           604800 )       ; Negative Cache TTL

@   IN  NS   ns.
X   IN  PTR  ns.firma.lan.     ; X = DNS serveri IP viimane oktet
; X  IN  PTR  hostinimi.firma.lan.
```

### Näide – DNS server .1, tööjaam .10

```
$TTL 604800
@   IN  SOA  ns.firma.lan.  root.firma.lan. (
                4         ; Serial
           604800         ; Refresh
            86400         ; Retry
          2419200         ; Expire
           604800 )       ; Negative Cache TTL

@   IN  NS   ns.
1   IN  PTR  ns.firma.lan.
1   IN  PTR  ruuter.firma.lan.
10  IN  PTR  tooajaam1.firma.lan.
```

!!! warning
    PTR kirjes kasuta ainult IP **viimast oktetit**: `10.100.0.1` → kirje on `1`.
    Kõikidele domeeninimedele lisatakse lõppu **punkt** (`ns.firma.lan.`) – see on kohustuslik! Ilma punktita tõlgendab bind seda suhtelise nimena.

---

## 6. Konfiguratsioonifailide süntaksi kontroll

Enne teenuse käivitamist kontrolli süntaksit:

```bash
# Kontrolli named.conf faile
named-checkconf

# Kontrolli tsoonifaile
named-checkzone firma.lan /var/lib/bind/db.forward.firma.lan
named-checkzone 0.100.10.in-addr.arpa /var/lib/bind/db.reverse.firma.lan
```

!!! info
    `named-checkconf` ei anna väljundit kui kõik on korras – viga on ainult siis kui midagi kuvatakse. `named-checkzone` peaks lõppema reaga **`OK`**.

---

## 7. Teenuse käivitamine ja kontroll

```bash
systemctl restart bind9
systemctl enable bind9
systemctl status bind9
```

!!! warning
    Kui teenus ei käivitu, vaata vealogi:
    ```bash
    journalctl -u named -n 30
    ```
    Tõenäoliselt on viga tsoonifailis: **punkt puudu**, vale serial või vale IP.

---

## 8. Testimine `dig`-käsuga

`dig` on `bind9-utils` paketis sisalduv tööriist DNS päringute testimiseks. Kasuta `@`-märki, et määrata millist DNS serverit küsida.

```bash
# Forward lookup – nimi → IP
dig @10.100.0.1 ns.firma.lan
dig @10.100.0.1 ruuter.firma.lan

# Reverse lookup – IP → nimi
dig @10.100.0.1 -x 10.100.0.1

# Kontrolli edasisuunamist (avalik domeen)
dig @10.100.0.1 google.com

# Lühem väljund
dig @10.100.0.1 ns.firma.lan +short
```

!!! info
    Edukas vastus sisaldab real **`ANSWER SECTION`** vastava IP-aadressi. Kui vastus on tühi või staatus on `SERVFAIL`/`NXDOMAIN`, on viga tsoonifailis või konfiguratsioonifailis.

---

## Rohkem infot

`man named.conf` | `man named-checkzone` | [bind9 dokumentatsioon](https://bind9.readthedocs.io){:target="_blank"}
