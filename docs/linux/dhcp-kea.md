---
icon: material/debian
---

# DHCP-serveri seadistamine (Kea)

Juhendis kasutatav operatsioonisüsteem: **Debian 13**  
Juhend käsitleb **DHCPv4** seadistamist paketiga `kea-dhcp4-server`.

Selles juhendis:

- Mis on Kea ja kuidas DHCP töötab
- Milliseid porte kasutavad DHCP-klient ja -server
- Kuidas paigaldada ja seadistada Kea DHCP-server
- Kuidas määrata kuulatav võrguliides
- Kuidas seadistada aadressireserveeringuid (MAC → IP)
- Kuidas kontrollida teenuse tööd serveris ja kliendis

!!! info "Mis on Kea?"

    Kea on ISC arendatav DHCP-server, mis on mõeldud asendama vanemat ISC DHCP-d.
    Kea kasutab JSON-vormingus seadistust. DHCPv4 ja DHCPv6 jaoks on eraldi teenused;
    selles juhendis paigaldame ainult DHCPv4-serveri.

---

## Mis on DHCP?

**DHCP** (*Dynamic Host Configuration Protocol*) võimaldab seadmetel saada võrguseaded automaatselt: IPv4-aadressi, võrgumaski, vaikelüüsi ja DNS-serverite aadressid.

IP-aadress antakse kliendile kindlaks ajaks ehk **liisinguks**. Enne selle lõppu taotleb klient liisingu pikendamist. Kui pikendamine ei õnnestu ja liising aegub, peab klient lõpetama selle aadressi kasutamise.

### DHCP tööpõhimõte

DHCPv4 esmane aadressi hankimine koosneb tavaliselt neljast sammust ehk **DORA** protsessist.

| Samm | Osapool | Kirjeldus |
|---|---|---|
| **1. DISCOVER** | Klient → võrk | Klient otsib leviedastusega (*broadcast*) DHCP-servereid. |
| **2. OFFER** | Server → klient | Server pakub IP-aadressi ja võrguseadeid. |
| **3. REQUEST** | Klient → võrk | Klient teatab tavaliselt leviedastusega, millise pakkumise ta valis. |
| **4. ACK** | Server → klient | Server kinnitab aadressi ja liisingu tingimused. |

Liisingu uuendamisel kasutatakse tavaliselt ainult REQUEST- ja ACK-sõnumeid.

### DHCP pordid

DHCP kasutab UDP-protokolli. DHCPv4 klient saadab päringu pordilt **68** serveri pordile **67** ning server vastab pordilt **67** kliendi pordile **68**.

| Osapool | UDP-port | Kasutus |
|---|---:|---|
| DHCP-server | 67 | Võtab vastu klientide päringuid. |
| DHCP-klient | 68 | Võtab vastu serveri vastuseid. |

Tulemüür peab lubama vajaliku DHCP-liikluse, sealhulgas leviedastuse. Eri alamvõrkudes asuva kliendi ja serveri vahel kasutatakse tavaliselt **DHCP-vahendajat** (*DHCP relay*).

DHCPv6 kasutab porte **547** serveril ja **546** kliendil. DHCPv6 seadistamist siin ei käsitleta.

---

## Eeldused ja näidisvõrk

- Käivita serveri halduskäsud **root-kasutajana** või lisa nende ette `sudo`.
- Serveri sisevõrguliidesel peab olema **püsiv IPv4-aadress** teenindatavast alamvõrgust.
- Klient ja server peavad olema samas laborivõrgus.
- Pakettide paigaldamiseks peab server pääsema Debiani paketihoidlatesse.

Juhendis kasutatav võrk:

| Parameeter | Näidisväärtus |
|---|---|
| Alamvõrk | `10.100.0.0/24` |
| Võrgumask | `255.255.255.0` |
| Vaikelüüs | `10.100.0.1` |
| DNS-server | `1.1.1.1` |
| DHCP-serveri sisevõrguaadress | `10.100.0.2/24` |
| DHCP-serveri sisevõrguliides | `ens192` |
| Dünaamiline aadressivahemik | `10.100.0.10–10.100.0.100` |
| Printerile reserveeritav aadress | `10.100.0.144` |
| Näidisdomeeni sufiks | `firma.lan` |

---

## 1. Tarkvara paigaldamine

```bash
apt update
apt install kea-dhcp4-server
```

!!! info "Esimene käivitus"

    Enne oma võrgu ja liidese seadistamist ei pruugi teenus käivituda ega klientidele
    aadresse jagada. Seadista teenus järgmiste sammude järgi.

---

## 2. Põhiseadistus – kea-dhcp4.conf

Tee enne esimest muutmist algsest failist varukoopia:

```bash
cp -a /etc/kea/kea-dhcp4.conf /etc/kea/kea-dhcp4.conf.algne
```

Ava seadistusfail:

```bash
nano /etc/kea/kea-dhcp4.conf
```

!!! note "JSON-vorming"

    Seaded asuvad objektis `Dhcp4`. Objektid kirjutatakse loogeliste sulgude `{}`
    ja loendid nurksulgude `[]` vahele. Elemendid eraldatakse komadega, kuid viimase
    elemendi järel koma ei ole. Tekst kirjutatakse jutumärkidesse, arvud ilma jutumärkideta.

### Mall

Alamvõrgu kirjeldus asub loendis `subnet4`. Allolev on selle **ühe elemendi mall**, mitte kogu seadistusfail. Asenda `X`-iga väärtused oma võrgu andmetega.

```json
{
    "id": 1,
    "subnet": "10.100.0.0/24",
    "pools": [
        { "pool": "10.100.0.X - 10.100.0.X" }
    ],
    "option-data": [
        { "name": "routers", "data": "10.100.0.X" },
        { "name": "domain-name-servers", "data": "X.X.X.X" },
        { "name": "domain-name", "data": "firma.lan" }
    ]
}
```

### Näide

DHCP-server on `10.100.0.2`, dünaamiline vahemik `.10–.100`, vaikelüüs `10.100.0.1` ja DNS-server `1.1.1.1`.

**Asenda faili sisu järgmise tervikliku näitega** ja kohanda väärtused oma võrgule:

```json
{
    "Dhcp4": {
        "interfaces-config": {
            "interfaces": [ "ens192" ]
        },
        "lease-database": {
            "type": "memfile",
            "persist": true,
            "name": "/var/lib/kea/kea-leases4.csv"
        },
        "authoritative": true,
        "valid-lifetime": 600,
        "max-valid-lifetime": 7200,
        "subnet4": [
            {
                "id": 1,
                "subnet": "10.100.0.0/24",
                "pools": [
                    { "pool": "10.100.0.10 - 10.100.0.100" }
                ],
                "option-data": [
                    { "name": "routers", "data": "10.100.0.1" },
                    { "name": "domain-name-servers", "data": "1.1.1.1" },
                    { "name": "domain-name", "data": "firma.lan" }
                ]
            }
        ],
        "loggers": [
            {
                "name": "kea-dhcp4",
                "output-options": [ { "output": "stdout" } ],
                "severity": "INFO"
            }
        ]
    }
}
```

### Parameetrite selgitus

| Parameeter | Selgitus |
|---|---|
| `interfaces-config` → `interfaces` | Liidesed, mille kaudu server DHCP-teenust pakub. |
| `lease-database` | Liisingute hoidla. `memfile` hoiab andmeid mälus ja `persist: true` korral salvestab need ka CSV-faili. |
| `authoritative` | `true` korral võib server sobimatut aadressi küsivale kliendile saata DHCPNAK-vastuse. Kasuta seda enda hallatavas laborivõrgus. |
| `valid-lifetime` | Vaikimisi liisingu kestus sekundites. `600` sekundit on 10 minutit. |
| `max-valid-lifetime` | Maksimaalne pakutav liisingu kestus. `7200` sekundit on 2 tundi. |
| `subnet4` | Teenindatavate IPv4-alamvõrkude loend. |
| `id` | Alamvõrgu unikaalne arvuline tunnus. Selles näites `1`. |
| `subnet` | Alamvõrgu aadress ja prefiksi pikkus. `/24` vastab maskile `255.255.255.0`. |
| `pools` → `pool` | Dünaamiliselt jagatav aadressivahemik. `.10–.100` sisaldab 91 aadressi. |
| `option-data` | Kliendile edastatavad valikud: vaikelüüs, DNS-server ja domeenisufiks. |
| `loggers` | Logimise seadistus. Näites läheb INFO-taseme logi standardväljundisse, kust seda saab vaadata `journalctl` abil. |

!!! note "Märkus"
    
    Kuna me pole veel DNS serveri seadistamiseni jõudnud, siis kasutame avalikku DNS serverit. Näites kasutatav avalik DNS-server `1.1.1.1` ei lahenda sisevõrgu `firma.lan` nimesid, selleks on vaja sisevõrgu DNS-serverit. 

---

## 3. Kuulatava võrguliidese määramine

Kea puhul määratakse liides **samas failis** `/etc/kea/kea-dhcp4.conf`, jaotises `interfaces-config`.

**Mall** – asenda `XXX` oma sisevõrguliidese nimega:

```json
"interfaces-config": {
    "interfaces": [ "XXX" ]
}
```

**Näide:**

```json
"interfaces-config": {
    "interfaces": [ "ens192" ]
}
```

Need on faili fragmendid. Muuda olemasolevat jaotist, ära lisa seda topelt.

!!! info

    Liidese nime leiad käsuga `ip link show` või `ip a`. Vali sisevõrguga ühendatud
    liides. Näites peab sellel olema püsiv aadress `10.100.0.2/24`.

---

## 4. Teenuse käivitamine ja kontrollimine

Kontrolli enne taaskäivitamist seadistust:

```bash
kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
```

Kui kontroll teatab veast, paranda see enne jätkamist. Kontroll ei tõesta veel teenuse toimimist võrgus.

Taaskäivita teenus ja kontrolli selle olekut:

```bash
systemctl restart kea-dhcp4-server
systemctl status kea-dhcp4-server --no-pager
```

Teenus peab olema olekus **active (running)**. Luba automaatkäivitus:

```bash
systemctl enable kea-dhcp4-server
```

Vaata viimaseid logikirjeid:

```bash
journalctl -u kea-dhcp4-server -b -n 50 --no-pager
```

!!! warning "Kui teenus ei käivitu"

    Kontrolli logis näidatud põhjust. Levinud vead on puuduv või liigne koma,
    valed sulud, vale liidesenimi ning liidesel puuduv sobiv IPv4-aadress.
    Kontrolli ka, et samas masinas ei töötaks teine DHCP-server.

Selles näites salvestatakse liisingud faili:

```bash
less /var/lib/kea/kea-leases4.csv
```

Fail sisaldab ka reserveeritud aadressidele antud liisinguid. Sama aadressi kohta võib olla mitu kirjet; viimane näitab viimast salvestatud olekut. Fail ei ole ühendatud seadmete nimekiri. **Ära muuda seda töötava teenuse ajal käsitsi.**

---

## 5. Aadressireserveering (MAC → IP)

**Aadressireserveeringuga** saab kindla MAC-aadressiga klient DHCP kaudu alati sama IP-aadressi. Kliendis peab aadressi hankimine jääma **automaatseks**.

Lisa `reservations` loend õige alamvõrgu objekti sisse, `pools` ja `option-data` kõrvale.

**Mall:**

```json
"reservations": [
    {
        "hw-address": "XX:XX:XX:XX:XX:XX",
        "ip-address": "10.100.0.X",
        "hostname": "HOSTINIMI"
    }
]
```

**Näide:** printer saab aadressi `10.100.0.144`. Asenda olemasolev `subnet4` loendi alamvõrgu objekt järgmisega; ülejäänud seadistus jääb alles:

```json
{
    "id": 1,
    "subnet": "10.100.0.0/24",
    "pools": [
        { "pool": "10.100.0.10 - 10.100.0.100" }
    ],
    "option-data": [
        { "name": "routers", "data": "10.100.0.1" },
        { "name": "domain-name-servers", "data": "1.1.1.1" },
        { "name": "domain-name", "data": "firma.lan" }
    ],
    "reservations": [
        {
            "hw-address": "bc:30:d9:2a:c9:50",
            "ip-address": "10.100.0.144",
            "hostname": "printer-raamatupidamine"
        }
    ]
}
```

| Parameeter | Selgitus |
|---|---|
| `hw-address` | Kliendi võrguliidese MAC-aadress. Linuxi kliendis näeb seda käsuga `ip link show`. |
| `ip-address` | Reserveeritud aadress. See peab kuuluma alamvõrku ja olema mujal kasutamata. |
| `hostname` | Kliendile määratav hostinimi. See ei loo iseenesest DNS-kirjet. Reserveering töötab ka ilma selle väljata. |

!!! tip "Reserveeringu aadressivahemik"

    Selles juhendis hoitakse reserveeringud dünaamilisest vahemikust väljas, nagu ISC DHCP
    näites. Kea lubab vaikeseadistusega alamvõrgu reserveeringuid ka dünaamilise vahemiku sees
    ja kontrollib neid aadressi jagamisel.

Kontrolli seadistust ja eduka kontrolli järel taaskäivita teenus:

```bash
kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
systemctl restart kea-dhcp4-server
```

!!! info "Reserveering ei muuda kliendi aadressi kohe"

    Serveri taaskäivitamine ei sunni klienti kehtivat liisingut kohe uuendama.
    Uue aadressi kontrollimiseks vabasta kliendi liising ja hangi aadress uuesti.

---

## 6. Kontrollimine kliendist

Teenuse olek `active` näitab, et serveriprotsess töötab. Kontrolli ka kliendile antud võrguseadeid.

1. Ühenda klient serveriga samasse laborivõrku.
2. Määra kliendi IPv4-aadressi ja DNS-serveri hankimine automaatseks.
3. Hangi DHCP-seadistus uuesti.
4. Kontrolli aadressi, maski, vaikelüüsi ja DNS-serverit. Tavaklient peab saama aadressi `.10–.100`, näites kasutatud MAC-aadressiga printer `.144`.

!!! tip "Jälgi samal ajal serveri logi"

    ```bash
    journalctl -u kea-dhcp4-server -f
    ```

    INFO-tase näitab teenuse põhisündmusi ja vigu; kõiki DORA-sõnumeid seal ei kuvata.
    Kliendile antud aadressi kontrolli ka liisingufailist.
    Logi jälgimise lõpetad klahvikombinatsiooniga `Ctrl+C`.

---

## Rohkem infot

Kohalik juhendileht:

```bash
man kea-dhcp4
```

Ametlikud allikad:

- [Kea DHCP – ISC](https://www.isc.org/kea/)
- [kea-dhcp4-server Debian 13 paketivalikus](https://packages.debian.org/trixie/kea-dhcp4-server)
- [Kea 2.6.3 DHCPv4 seadistamine ja reserveeringud](https://kea.readthedocs.io/en/kea-2.6.3/arm/dhcp4-srv.html)
- [Kea 2.6.3 logimine](https://kea.readthedocs.io/en/kea-2.6.3/arm/logging.html)
- [Debiani kea-dhcp4 juhendileht](https://manpages.debian.org/trixie/kea-dhcp4-server/kea-dhcp4.8.en.html)

---

*Õppematerjali koostaja: Priit Paap, 2026*
