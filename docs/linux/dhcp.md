icon:material/debian

# isc-dhcp-server DHCP serveri seadistamine

## Eesmärk

Selles peatükis õpid:

- Mis on DHCP ja kuidas see töötab
- Kuidas paigaldada ja seadistada `isc-dhcp-server`
- Kuidas määrata DHCP kuulatav võrguliides
- Kuidas seadistada staatilisi liisimisi (MAC → IP)

---

## Mis on DHCP?

**DHCP** (Dynamic Host Configuration Protocol) on protokoll, mis jagab võrku ühenduvate seadmete võrguseadeid automaatselt: IP-aadress, võrgumask, vaikelüüs (router) ja DNS-server. Ilma DHCP-ta tuleks kõigil seadmetel seaded käsitsi seadistada.

### DHCP töö põhimõte

DHCP-l on nelja-sammuline protsess:

| Samm | Osapool | Kirjeldus |
|------|---------|-----------|
| **1. DISCOVER** | Klient → võrk | Klient saadab broadcast-sõnumi, et leida DHCP server |
| **2. OFFER** | Server → klient | Server pakub vaba IP-aadressi |
| **3. REQUEST** | Klient → server | Klient küsib pakutud aadressi |
| **4. ACK** | Server → klient | Server kinnitab liisingu |

> **Näidisvõrk:** Selles juhendis kasutatakse näidisvõrku `10.100.0.0/24` ja domeeni `firma.lan`. **Muuda need parameetrid vastavalt oma võrgule.**

---

## 1. Tarkvara paigaldus

```bash
apt update
apt install isc-dhcp-server
```

!!! info
    `isc-dhcp-server` on Debian/Ubuntu standardpakett. Fedora/RHEL puhul on pakett `dhcp-server` ja teenuse nimi `dhcpd`.

---

## 2. Põhiseadistus – `dhcpd.conf`

Ava seadistusfail:

```bash
nano /etc/dhcp/dhcpd.conf
```

**Alamvõrgu blokk** on põhistruktuur, mida pead muutma. Kõik `X`-iga kohad täidad oma võrgu väärtustega.

### Mall – täida X-i kohale oma võrgu väärtused

```
subnet 10.100.0.0 netmask 255.255.255.0 {
    range 10.100.0.X 10.100.0.X;
    option domain-name-servers 10.100.0.X;
    option domain-name "firma.lan";
    option routers 10.100.0.X;
    option broadcast-address 10.100.0.255;
    default-lease-time 600;
    max-lease-time 7200;
}
```

### Näide – server 10.100.0.1, aadressivahemik .10–.100, DNS samal serveril

```
subnet 10.100.0.0 netmask 255.255.255.0 {
    range 10.100.0.10 10.100.0.100;
    option domain-name-servers 10.100.0.1;
    option domain-name "firma.lan";
    option routers 10.100.0.1;
    option broadcast-address 10.100.0.255;
    default-lease-time 600;
    max-lease-time 7200;
}
```

!!! info
    Kui domeeni pole veel loodud, kommenteeri `domain-name` rida välja — lisa rea algusesse `#`:
    ```
    # option domain-name "firma.lan";
    ```
    Rea saab aktiveerida hiljem, kui domeen on seadistatud.

### Parameetrite selgitus

| Parameeter | Selgitus |
|------------|----------|
| `subnet ... netmask ...` | Defineerib alamvõrgu aadressi ja maski. `/24` tähendab 256 võimalikku aadressi (10.100.0.1 – 10.100.0.254). |
| `range X X;` | Vabade aadresside vahemik, mida klientidele jagatakse. Nt `.10 – .100` annab 91 aadressi. |
| `domain-name-servers` | DNS-serveri IP-aadress, mille klient saab. Võib olla sama server või eraldi DNS. |
| `domain-name` | Domeeninimi, mida kliendid kasutavad. Näiteks `firma.lan` sisevõrgu jaoks. |
| `routers` | Vaikelüüs (gateway) IP-aadress — tavaliselt ruuter. |
| `broadcast-address` | Leviedastusaadress — `/24` võrgus alati `x.x.x.255`. |
| `default-lease-time` | Vaikimisi liisiaeg sekundites (600 sek = 10 min). |
| `max-lease-time` | Maksimaalne liisiaeg sekundites (7200 sek = 2 tundi). |

---

## 3. Võrguliidese seadistamine

Määra liides, mida server kuulab. Ava fail `/etc/default/isc-dhcp-server` ja muuda `INTERFACESv4` parameeter:

```bash
nano /etc/default/isc-dhcp-server
```

```
INTERFACESv4="ens192"
# INTERFACESv6=""
```

!!! info
    Liidese nime leiad käsuga `ip link show` või `ip a`. Levinumad nimed: `ens192`, `ens33`, `eth0`, `enp0s3`. Vali see liides, mis on ühendatud sisevõrguga.

---

## 4. Teenuse käivitamine ja kontroll

Taaskäivita DHCP teenus:

```bash
systemctl restart isc-dhcp-server
```

Kontrolli teenuse olekut:

```bash
systemctl status isc-dhcp-server
```

Luba teenus automaatkäivituseks:

```bash
systemctl enable isc-dhcp-server
```

!!! warning
    Kui teenus ei käivitu, on tõenäoliselt viga `dhcpd.conf` failis. Vaata täpsemat veateadet:
    ```bash
    journalctl -u isc-dhcp-server -n 20
    ```

Välja liisitud aadresse saab vaadata:

```bash
less /var/lib/dhcp/dhcpd.leases
```

---

## 5. Staatiline liising (MAC → IP sidumine)

Staatilise liisimisega saab kindlale seadmele (MAC-aadressi järgi) alati sama IP-aadressi anda. See on kasulik serverite, printerite ja muude püsiasukohaga seadmete jaoks.

Lisa staatiline liising `dhcpd.conf` faili — `subnet` bloki sisse või välja:

### Mall – asenda kursiivis olevad väärtused

```
host HOSTINIMI {
    hardware ethernet XX:XX:XX:XX:XX:XX;
    fixed-address 10.100.0.X;
}
```

### Näide – printer raamatupidamises saab alati aadressi .44

```
host printer-raamatupidamine {
    hardware ethernet bc:30:d9:2a:c9:50;
    fixed-address 10.100.0.44;
}
```

### Parameetrite selgitus

| Parameeter | Selgitus |
|------------|----------|
| `host` | Hosti nimi — vali kirjeldav nimi (ei pea vastama tegelikule hostname'ile). |
| `hardware ethernet` | Seadme MAC-aadress. Leiad selle käsuga: `ip link show` või seadme võrguseadetest. |
| `fixed-address` | IP-aadress, mis sellele seadmele alati antakse. Peab olema **väljaspool `range` vahemikku!** |

!!! tip
    **Hea tava:** määra staatilised aadressid `range` vahemikust välja. Näiteks kui `range` on `10.100.0.10–10.100.0.100`, kasuta staatiliste jaoks vahemikku `10.100.0.101–10.100.0.200`. Nii väldid IP-konflikte.

Pärast muudatust taaskäivita teenus:

```bash
systemctl restart isc-dhcp-server
```

---

## Rohkem infot

`man dhcpd.conf` | `man isc-dhcp-server` | Konfiguratsioonifail: `/etc/dhcp/dhcpd.conf`
