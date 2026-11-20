---
icon: material/debian
---

# DHCP-serveri seadistamine (Kea)

Juhendis kasutatav operatsioonisüsteem: **Debian 13**  
Juhend käsitleb **DHCPv4** seadistamist paketiga `kea-dhcp4-server`.
Kasutame lühikest näidiskonfi ja lisame sellele hiljem aadressireserveeringu.

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
| Domeenisufiks | Esialgu ei määrata |

!!! warning "Kui samas masinas töötas ISC DHCP"

    Peata vana teenus ja eemalda see automaatkäivitusest enne Kea käivitamist:

    ```bash
    systemctl disable --now isc-dhcp-server
    ```

---

## 1. Tarkvara paigaldamine

```bash
apt update
apt install kea-dhcp4-server
```

Enne seadistamist ei pruugi teenus käivituda ega klientidele aadresse jagada.

---

## 2. Lühikese põhiseadistuse loomine

Tee enne esimest muutmist algsest failist varukoopia:

```bash
cp -a /etc/kea/kea-dhcp4.conf /etc/kea/kea-dhcp4.conf.algne
```

Ava fail:

```bash
nano /etc/kea/kea-dhcp4.conf
```

**Asenda faili kogu sisu järgmise näitega.** Seejärel kohanda liidese nimi ja võrgu väärtused oma laborile.

```json
{
    "Dhcp4": {
        "interfaces-config": {
            "interfaces": [ "ens192" ]
        },
        "lease-database": {
            "type": "memfile",
            "persist": true
        },
        "valid-lifetime": 3600,
        "subnet4": [
            {
                "id": 1,
                "subnet": "10.100.0.0/24",
                "pools": [
                    { "pool": "10.100.0.10 - 10.100.0.100" }
                ],
                "option-data": [
                    { "name": "routers", "data": "10.100.0.1" },
                    { "name": "domain-name-servers", "data": "1.1.1.1" }
                ],
                "reservations": []
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

### Muudetavad väärtused

| Parameeter | Näidisväärtus | Kasutus |
|---|---|---|
| `interfaces` | `ens192` | Serveri sisevõrguliides. Nime leiad käsuga `ip a`. |
| `subnet` | `10.100.0.0/24` | Teenindatav alamvõrk. `/24` vastab maskile `255.255.255.0`. |
| `pool` | `10.100.0.10 - 10.100.0.100` | Dünaamiliselt jagatavad aadressid. Näites 91 aadressi. |
| `routers` → `data` | `10.100.0.1` | Kliendile edastatav vaikelüüs. |
| `domain-name-servers` → `data` | `1.1.1.1` | Kliendile edastatav DNS-server. |

`valid-lifetime` määrab liisingu kestuse sekundites: `3600` = 1 tund. `lease-database` salvestab liisingud mällu ja CSV-faili. `reservations` on esialgu tühi loend. `loggers` võimaldab teenuse logi vaadata `journalctl` abil.

!!! note "JSON-i vormistus"

    Sulud `{}` ja `[]`, komad ning jutumärgid peavad olema õiged.
    Viimase elemendi järel koma ei ole. Taanded toimimist ei mõjuta, kuid aitavad
    näha seadete kuuluvust ja leida vigu. Kea lubab kommentaare `//` ja `#` märgiga.

---

## 3. Teenuse käivitamine ja kontrollimine

Salvesta fail ja kontrolli seadistust Kea teenuse kasutajana `_kea`:

```bash
sudo -u _kea kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
```

Veateate korral paranda viga enne jätkamist. Edukas kontroll ei tõesta veel teenuse toimimist võrgus.

!!! info "Kontrolli käivitamise õigused"

    Käivita kontroll kasutajana `_kea`. Selles Debian 13 laborikeskkonnas andis kontroll
    root-kasutajana veateate `Unable to open file`, kuid kasutajana `_kea` sai faili lugeda.
    Root-kasutajana ilma `sudo` käsuta saab kasutada:

    ```bash
    runuser -u _kea -- kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
    ```

Taaskäivita teenus, luba automaatkäivitus ja kontrolli olekut:

```bash
systemctl restart kea-dhcp4-server
systemctl enable kea-dhcp4-server
systemctl status kea-dhcp4-server --no-pager
```

Teenus peab olema olekus **active (running)**.

!!! warning "Kui teenus ei käivitu"

    Vaata logi:

    ```bash
    journalctl -u kea-dhcp4-server -b -n 50 --no-pager
    ```

    Kontrolli sulge ja komasid, liidese nime ning liidese IPv4-aadressi.
    Veendu ka, et samas masinas ei töötaks teine DHCP-server.

---

## 4. Kontrollimine kliendist

1. Ühenda klient serveriga samasse laborivõrku.
2. Määra kliendi IPv4-aadressi ja DNS-serveri hankimine automaatseks.
3. Hangi DHCP-seadistus uuesti.
4. Kontrolli saadud võrguseadeid.

| Seade | Oodatav väärtus |
|---|---|
| IPv4-aadress | `10.100.0.10–10.100.0.100` |
| Võrgumask | `255.255.255.0` ehk `/24` |
| Vaikelüüs | `10.100.0.1` |
| DNS-server | `1.1.1.1` |

Serveris saad liisinguid vaadata käsuga:

```bash
less /var/lib/kea/kea-leases4.csv
```

Fail võib sisaldada sama aadressi varasemaid kirjeid; viimane kirjeldab viimast salvestatud olekut. Ära muuda seda töötava teenuse ajal käsitsi.

!!! tip "Logi jälgimine"

    ```bash
    journalctl -u kea-dhcp4-server -f
    ```

    INFO-tase näitab teenuse põhisündmusi ja vigu; kõiki DORA-sõnumeid seal ei kuvata.
    Logi jälgimise lõpetad `Ctrl+C` abil.

---

## 5. Aadressireserveering (MAC → IP)

Kui tavaline aadressijagamine töötab, lisa reserveering: kindla MAC-aadressiga klient saab alati sama IP-aadressi. Kliendis jääb aadressi hankimine **automaatseks**.

Ava uuesti `/etc/kea/kea-dhcp4.conf`. Asenda alamvõrgu sees olev `"reservations": []` järgmisega. Kasuta oma kliendi MAC-aadressi.

```text
"reservations": [
    {
        "hw-address": "bc:30:d9:2a:c9:50",
        "ip-address": "10.100.0.144",
        "hostname": "printer-raamatupidamine"
    }
]
```

| Parameeter | Selgitus |
|---|---|
| `hw-address` | Kliendi võrguliidese MAC-aadress. Linuxi kliendis leiad selle käsuga `ip link show`. |
| `ip-address` | Reserveeritud aadress. See peab kuuluma alamvõrku ja olema mujal kasutamata. |
| `hostname` | Kliendile määratav hostinimi. See ei loo automaatselt DNS-kirjet. |

Näite `.144` jääb dünaamilisest vahemikust välja. Kea lubab vaikeseadistusega alamvõrgu reserveeringuid ka dünaamilise vahemiku sees.

Kontrolli seadistust ja eduka kontrolli järel taaskäivita teenus:

```bash
sudo -u _kea kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
systemctl restart kea-dhcp4-server
```

Vabasta kliendi liising ja hangi aadress uuesti. Reserveeringuga klient peab nüüd saama aadressi **`10.100.0.144`**. Serveri taaskäivitamine üksi kliendi kehtivat liisingut kohe ei muuda.

---

## 6. Sisevõrgu DNS-i lisamine hiljem

Esialgne seadistus töötab ilma domeenisufiksita. Kui seadistad hiljem BINDi juhendi järgi DNS-serveri aadressil `10.100.0.2`, asenda alamvõrgu `option-data` loend järgmisega:

```text
"option-data": [
    { "name": "routers", "data": "10.100.0.1" },
    { "name": "domain-name-servers", "data": "10.100.0.2" },
    { "name": "domain-name", "data": "firma.lan" }
],
```

Domeenisufiksi edastamine ise DNS-tsooni ei loo. Pärast muudatust kontrolli seadistust, taaskäivita DHCP-teenus ja uuenda kliendi liisingut.

---

## Rohkem infot

```bash
man kea-dhcp4
```

- [Kea 2.6.3 DHCPv4 seadistamine ja reserveeringud](https://kea.readthedocs.io/en/kea-2.6.3/arm/dhcp4-srv.html)
- [Kea 2.6.3 logimine](https://kea.readthedocs.io/en/kea-2.6.3/arm/logging.html)
- [Debiani kea-dhcp4 juhendileht](https://manpages.debian.org/trixie/kea-dhcp4-server/kea-dhcp4.8.en.html)

---

*Õppematerjali koostaja: Priit Paap, 2026*
