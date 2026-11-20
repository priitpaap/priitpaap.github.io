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

!!! info "Esimene käivitus"

    Enne oma võrgu ja liidese seadistamist ei pruugi teenus käivituda ega klientidele
    aadresse jagada. Seadista teenus järgmiste sammude järgi.

---

## 2. Olemasoleva seadistusfaili avamine

Tee enne esimest muutmist varukoopia:

```bash
cp -a /etc/kea/kea-dhcp4.conf /etc/kea/kea-dhcp4.conf.algne
```

Ava fail:

```bash
nano /etc/kea/kea-dhcp4.conf
```

Nano redaktoris saad lõike otsida klahvikombinatsiooniga **Ctrl+W**. Järgmistes sammudes on näidatud otsitav seade ja selle uus sisu. Näited põhinevad kaasasoleval vaikimisi failil.

!!! note "Sulud, komad ja kommentaarid"

    Objektid on loogeliste sulgude `{}` ja loendid nurksulgude `[]` vahel.
    Elemendid eraldatakse komadega, viimase elemendi järel koma ei ole.
    Kea lubab kommentaare nii `//` kui ka `#` märgiga. Kommenteeritud rida ei rakendu.
    Säilita näidetes olevad sulud ja komad – need seovad muudetava lõigu ülejäänud failiga.

---

## 3. Kuulatava võrguliidese määramine

Otsi faili algusest `interfaces-config`. Selle sees on tühi liideseloend:

```text
"interfaces": [ ]
```

Lisa sinna oma sisevõrguliidese nimi:

```text
"interfaces": [ "ens192" ]
```

!!! info

    Liidese nime leiad käsuga `ip link show` või `ip a`. Näites peab sellel liidesel
    olema püsiv aadress `10.100.0.2/24`. Liidese nime lisamine ei seadista sellele IP-aadressi.

---

## 4. Üldiste valikute muutmine

### DNS-server ja domeenisufiks

Otsi **esimene aktiivne `option-data` loend** – see asub enne `client-classes` ja `subnet4` jaotisi. Seal on näiteks DNS-aadressid `192.0.2.1, 192.0.2.2`, domeen `example.org` ning muud näidisvalikud.

**Asenda kogu see loend**, alates `"option-data": [` kuni selle sulgeva `],` märgini, järgmisega:

```text
"option-data": [
    { "name": "domain-name-servers", "data": "1.1.1.1" }
],
```

See on `Dhcp4` üldine valik, mis kehtib kõigile alamvõrkudele, kui seda kitsamas jaotises üle ei kirjutata. Esialgu domeenisufiksit ei määra.

!!! tip "Kui lisad hiljem sisevõrgu DNS-serveri"

    Muuda DNS-serveri aadressiks `10.100.0.2` ja lisa vajaduse korral domeenisufiks:

    ```text
    "option-data": [
        { "name": "domain-name-servers", "data": "10.100.0.2" },
        { "name": "domain-name", "data": "firma.lan" }
    ],
    ```

    Domeenisufiks ise DNS-tsooni ega kirjeid ei loo.

### Näidiskliendiklassi eemaldamine

Otsi `client-classes`. Selles failis on seal aktiivne `voip` näide koos võõra serveri aadressi ja alglaadimise valikutega. Asenda **kogu see loend** tühja loendiga:

```text
"client-classes": [],
```

### Liisinguajad

Failis olevad ajad sobivad esimeseks katsetuseks ja võivad jääda samaks:

| Parameeter | Vaikimisi väärtus | Selgitus |
|---|---:|---|
| `renew-timer` | `900` | Klient alustab liisingu pikendamist 15 minuti pärast. |
| `rebind-timer` | `1800` | Kui senine server ei vasta, küsib klient pikendust teistelt serveritelt 30 minuti järel. |
| `valid-lifetime` | `3600` | Liising kehtib vaikimisi 1 tunni. |

---

## 5. Alamvõrgu seadistamine

Otsi `subnet4`. Seal on näidisvõrk `192.0.2.0/24` ja selle sees mitu näidisreserveeringut.

**Asenda kogu `subnet4` loend**, alates `"subnet4": [` kuni selle sulgeva `],` märgini, järgmisega. Nii eemaldad korraga ka kõik võõrad näidisreserveeringud.

```text
"subnet4": [
    {
        "id": 1,
        "subnet": "10.100.0.0/24",
        "pools": [
            { "pool": "10.100.0.10 - 10.100.0.100" }
        ],
        "option-data": [
            { "name": "routers", "data": "10.100.0.1" }
        ],
        "reservations": []
    }
],
```

!!! warning "Asendatava lõigu piirid"

    `subnet4` sees on mitu omaette loendit. Asenda kogu välimine loend, mitte ainult
    esimese `]` märgini ulatuv osa. Sinu vaikimisi failis järgneb sellele `loggers` jaotis.
    Säilita `subnet4` lõpus olev koma, sest pärast seda tuleb järgmine seade.

### Parameetrite selgitus

| Parameeter | Selgitus |
|---|---|
| `id` | Alamvõrgu unikaalne arvuline tunnus, siin `1`. |
| `subnet` | Teenindatav alamvõrk. `/24` vastab maskile `255.255.255.0`. |
| `pools` → `pool` | Dünaamiliselt jagatav aadressivahemik. `.10–.100` sisaldab 91 aadressi. |
| `option-data` → `routers` | Selle alamvõrgu klientidele edastatav vaikelüüsi aadress. |
| `reservations` | Selle alamvõrgu aadressireserveeringud. Esialgu tühi loend. |

DNS-server tuleb eelmises sammus määratud üldisest `option-data` loendist. Vaikelüüs on määratud alamvõrgu sees.

`control-socket`, `lease-database`, `expired-leases-processing` ja `loggers` ei vaja selle labori jaoks uusi väärtusi. `loggers` jaotise võimaliku liigse koma kohta vaata järgmist sammu.

---

## 6. Teenuse käivitamine ja kontrollimine

Salvesta fail ja kontrolli seadistust:

```bash
kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
```

Veateate korral paranda viga enne jätkamist. Kontroll ei ava võrgusokleid ega tõesta veel teenuse toimimist võrgus.

!!! info "Hoiatus Extraneous comma"

    Kaasasoleva faili `loggers` jaotises on `pattern` rea lõpus üleliigne koma,
    sest järgnevad valikud on välja kommenteeritud. Eemalda selle rea lõpust koma:

    ```text
    "pattern": "%-5p %m\n"
    ```

    Sama hoiatus võib tekkida ka mujal, kui viimase aktiivse elemendi järel on koma.

Taaskäivita teenus ja kontrolli olekut:

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

    Kontrolli logis näidatud põhjust: puuduv või liigne koma, valed sulud, vale
    liidesenimi või sobiva IPv4-aadressi puudumine liidesel. Veendu ka, et samas
    masinas ei töötaks teine DHCP-server.

Vaikimisi `memfile` hoidla salvestab Debianis liisingud faili:

```bash
less /var/lib/kea/kea-leases4.csv
```

Failis võib olla ühe aadressi kohta mitu kirjet; viimane kirjeldab viimast salvestatud olekut. See ei ole hetkel ühendatud seadmete nimekiri. Ära muuda faili töötava teenuse ajal käsitsi.

---

## 7. Aadressireserveering (MAC → IP)

Reserveeringuga saab kindla MAC-aadressiga klient alati sama IP-aadressi. Kliendis peab aadressi hankimine jääma **automaatseks**.

Otsi oma `subnet4` seest tühi loend `"reservations": []` ja asenda see järgmisega. Muuda MAC-aadress vastavalt oma seadmele.

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
| `hw-address` | Kliendi võrguliidese MAC-aadress. Linuxi kliendis näeb seda käsuga `ip link show`. |
| `ip-address` | Reserveeritud aadress. See peab kuuluma alamvõrku ja olema mujal kasutamata. |
| `hostname` | Kliendile määratav hostinimi. See ei loo iseenesest DNS-kirjet. Välja võib ka ära jätta. |

!!! tip "Reserveeringu aadressivahemik"

    Näite aadress `.144` jääb dünaamilisest vahemikust `.10–.100` välja.
    Kea lubab vaikeseadistusega alamvõrgu reserveeringuid ka dünaamilise vahemiku sees.

Kontrolli seadistust ja eduka kontrolli järel taaskäivita teenus:

```bash
kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
systemctl restart kea-dhcp4-server
```

Serveri taaskäivitamine ei muuda kliendi kehtivat liisingut kohe. Reserveeringu kontrollimiseks vabasta kliendi liising ja hangi aadress uuesti.

---

## 8. Kontrollimine kliendist

1. Ühenda klient serveriga samasse laborivõrku.
2. Määra kliendi IPv4-aadressi ja DNS-serveri hankimine automaatseks.
3. Hangi DHCP-seadistus uuesti.
4. Kontrolli IP-aadressi, maski, vaikelüüsi ja DNS-serverit.

Tavaklient peab saama aadressi `10.100.0.10–10.100.0.100`, maski `/24`, vaikelüüsi `10.100.0.1` ja DNS-serveri `1.1.1.1`. Reserveeringu MAC-aadressiga klient peab saama aadressi `10.100.0.144`.

!!! tip "Logi jälgimine"

    ```bash
    journalctl -u kea-dhcp4-server -f
    ```

    INFO-tase näitab teenuse põhisündmusi ja vigu; kõiki DORA-sõnumeid seal ei kuvata.
    Saadud aadressi kontrolli kliendist ja liisingufailist. Logi jälgimise lõpetad `Ctrl+C` abil.

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
