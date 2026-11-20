---
icon: material/debian
---

# DHCP-serveri seadistamine (isc-dhcp-server)

Juhendis kasutatav operatsioonisüsteem: **Debian 13**  
Juhend käsitleb **DHCPv4** seadistamist.  

Selles juhendis:

- Mis on DHCP ja kuidas see töötab
- Milliseid porte kasutavad DHCP-klient ja -server
- Kuidas paigaldada ja seadistada `isc-dhcp-server`
- Kuidas määrata DHCP-serveri kuulatav võrguliides
- Kuidas seadistada aadressireserveeringuid (MAC → IP)
- Kuidas kontrollida teenuse tööd serveris ja kliendis

!!! warning "ISC DHCP hooldus on lõppenud" 

    ISC lõpetas ISC DHCP hoolduse 2022. aasta lõpus. Debian 13 paketivalikus on
    `isc-dhcp-server` endiselt olemas, kuid märgitud aegunuks (*deprecated*).
    Uute lahenduste jaoks soovitab ISC Kea DHCP-serverit.
    See juhend sobib ISC DHCP õppimiseks ja olemasolevate seadistuste mõistmiseks.

---

## Mis on DHCP?

**DHCP** (*Dynamic Host Configuration Protocol*) võimaldab võrku ühenduvatel seadmetel saada võrguseaded automaatselt. DHCPv4 abil saab klient näiteks IPv4-aadressi, võrgumaski, vaikelüüsi ja DNS-serverite aadressid. Nii ei pea neid igas seadmes käsitsi määrama.

IP-aadress antakse kliendile kindlaks ajaks ehk **liisinguks**. Enne selle lõppu taotleb klient liisingu pikendamist. Kui pikendamine ei õnnestu ja liising aegub, peab klient lõpetama selle aadressi kasutamise.

### DHCP tööpõhimõte

DHCPv4 esmane aadressi hankimine koosneb tavaliselt neljast sammust. Seda nimetatakse sõnumite algustähtede järgi **DORA** protsessiks.

| Samm | Osapool | Kirjeldus |
|---|---|---|
| **1. DISCOVER** | Klient → võrk | Klient saadab leviedastussõnumi (*broadcast*), et leida DHCP-servereid. |
| **2. OFFER** | Server → klient | Server pakub IP-aadressi ja võrguseadeid. |
| **3. REQUEST** | Klient → võrk | Klient teatab tavaliselt leviedastusega, millise serveri pakkumise ta valis. |
| **4. ACK** | Server → klient | Valitud server kinnitab aadressi ja liisingu tingimused. |

Liisingu uuendamisel ei läbita tavaliselt kõiki nelja sammu: klient saadab serverile REQUEST-sõnumi ja server vastab ACK-sõnumiga.

### DHCP pordid

DHCP kasutab UDP-protokolli. DHCPv4 puhul kuulab DHCP-server klientide päringuid UDP-pordil **67** ja DHCP-klient võtab serveri vastuseid vastu UDP-pordil **68**. Klient saadab päringu pordilt 68 serveri pordile 67 ning server vastab pordilt 67 kliendi pordile 68.

| Osapool | UDP-port | Kasutus |
|---|---:|---|
| DHCP-server | 67 | Võtab vastu klientide päringuid. |
| DHCP-klient | 68 | Võtab vastu serveri vastuseid. |

Kui võrgus kasutatakse tulemüüri, peab see lubama vajaliku DHCP-liikluse ka aadressita kliendi ja leviedastuse korral. Klient ei pruugi esmast päringut saates oma tulevast IP-aadressi veel teada.

Kui klient ja server asuvad eri alamvõrkudes, kasutatakse nende vahel tavaliselt **DHCP-vahendajat** (*DHCP relay*), sest ruuter ei edasta tavapäraseid leviedastussõnumeid teise alamvõrku. Vahendaja edastab päringud DHCP-serverile ja vastused tagasi kliendi võrku.

DHCPv6 kasutab teisi porte: **547** serveril ja **546** kliendil. DHCPv6 seadistamist selles juhendis ei käsitleta.

---

## Eeldused ja näidisvõrk

- Käivita juhendi serveri halduskäsud **root-kasutajana**. Kui kasutad `sudo` õigustega kasutajat, lisa vajalike käskude ette `sudo`.
- Serveri sisevõrguliidesel peab olema **püsiv IPv4-aadress** teenindatavast alamvõrgust.
- Klient ja server peavad olema ühendatud samasse laborivõrku. Selles juhendis DHCP-vahendajat ei kasutata.
- Pakettide paigaldamiseks peab serveril olema ligipääs Debiani paketihoidlatele.

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
apt install isc-dhcp-server
```

!!! info "Esimene käivitus võib ebaõnnestuda"

    Paketi paigaldamisel võidakse proovida teenust käivitada. Enne sobiva alamvõrgu
    ja liidese seadistamist võib käivitus ebaõnnestuda. See on tavapärane tulemus, sest enne tuleb teenus seadistada.

---

## 2. Põhiseadistus – dhcpd.conf

Tee algsest failist varukoopia. Käivita see käsk enne esimest muutmist:

```bash
cp -a /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.algne
```

Ava seadistusfail:

```bash
nano /etc/dhcp/dhcpd.conf
```

!!! note "Märkus"
    
    Failist leiad üldised seaded ja kommenteeritud näiteid. Muuda seadeid failis vastavalt oma võrgule, ära lisa seadeid topelt.
    
    Kuna me pole veel DNS serveri seadistamiseni jõudnud, siis kasutame avalikku DNS serverit. Näites kasutatav avalik DNS-server `1.1.1.1` ei lahenda sisevõrgu `firma.lan` nimesid, selleks on vaja sisevõrgu DNS-serverit. 


### Mall

Asenda kõik `X`-iga väärtused enne salvestamist. Mallis kasutatakse alamvõrku `10.100.0.0/24`; teise alamvõrgu korral tuleb muuta ka võrgu aadressi, maski ja leviedastusaadressi.

```text
authoritative;

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

### Näide

DHCP-server on `10.100.0.2`, dünaamiline vahemik `.10–.100`, vaikelüüs on `10.100.0.1` ja `1.1.1.1`:

```text
authoritative;

subnet 10.100.0.0 netmask 255.255.255.0 {
    range 10.100.0.10 10.100.0.100;
    option domain-name-servers 1.1.1.1;
    option domain-name "firma.lan";
    option routers 10.100.0.1;
    option broadcast-address 10.100.0.255;
    default-lease-time 600;
    max-lease-time 7200;
}
```

### Parameetrite selgitus

| Parameeter | Selgitus |
|---|---|
| `authoritative;` | Server peab oma seadistust teenindatava võrgu jaoks autoriteetseks. |
| `subnet ... netmask ...` | Määrab alamvõrgu ja maski. Näidisvõrgus on 256 aadressi kokku, millest seadmetele saab tavapäraselt kasutada 254: `10.100.0.1–10.100.0.254`. |
| `range X X;` | Dünaamiliselt jagatav aadressivahemik. Mõlemad otspunktid kuuluvad vahemikku; `.10–.100` annab 91 aadressi. |
| `option domain-name-servers` | Klientidele edastatava töötava DNS-serveri aadress. Mitu aadressi eraldatakse komadega. |
| `option domain-name` | Kliendile edastatav domeenisufiks. |
| `option routers` | Kliendi vaikelüüsi aadress. See peab olema kliendi võrgust otse kättesaadav. |
| `option broadcast-address` | Alamvõrgu leviedastusaadress. Näidisvõrgus `10.100.0.255`. |
| `default-lease-time` | Liisingu kestus sekundites, kui klient kindlat kestust ei küsi. `600` sekundit on 10 minutit. |
| `max-lease-time` | Maksimaalne pakutav liisingu kestus. `7200` sekundit on 2 tundi. |

Lühikesed liisinguajad sobivad laborisse, kus seadistusi sageli muudetakse. Püsivas võrgus valitakse kestus võrgu vajaduste järgi.

---

## 3. Kuulatava võrguliidese määramine

Ava fail:

```bash
nano /etc/default/isc-dhcp-server
```

Määra `INTERFACESv4` väärtuseks sisevõrguliidese nimi.

**Mall:**

```bash
INTERFACESv4="XXX"
INTERFACESv6=""
```

**Näide:**

```bash
INTERFACESv4="ens192"
INTERFACESv6=""
```


!!! info
    
    Liidese nime leiad käsuga `ip link show` või `ip a`. Levinumad nimed: `ens192`, `ens33`, `eth0`, `enp0s3`. Vali see liides, mis on ühendatud sisevõrguga.

---

## 4. Teenuse käivitamine ja kontrollimine

Kontrolli enne taaskäivitamist seadistusfaili süntaksit:

```bash
dhcpd -4 -t -cf /etc/dhcp/dhcpd.conf
```

Kui kontroll teatab veast, paranda see enne jätkamist. Edukas süntaksikontroll ei tõesta veel, et liidese ja võrgu seadistus on õige.

Taaskäivita teenus ja kontrolli selle olekut:

```bash
systemctl restart isc-dhcp-server
systemctl status isc-dhcp-server --no-pager
```

Teenus peab olema olekus **active (running)**. Luba automaatkäivitus:

```bash
systemctl enable isc-dhcp-server
```

Vaata viimaseid logikirjeid:

```bash
journalctl -u isc-dhcp-server -b -n 50 --no-pager
```

!!! warning "Kui teenus ei käivitu"

    Kontrolli logis näidatud põhjust. Levinud vead on puuduv semikoolon või sulg,
    vale liidesenimi, liidesel puuduv IPv4-aadress ning selle aadressile vastava
    `subnet`-kirjelduse puudumine. Samuti võib käivitumist takistada teine samas
    masinas töötav DHCP-server. Ainult seadistusfaili süntaksi kontrollimisest ei piisa.

Dünaamiliste liisingute faili saad vaadata käsuga:

```bash
less /var/lib/dhcp/dhcpd.leases
```

Failis võib olla ühe aadressi kohta mitu kirjet; viimane kirjeldab selle viimast salvestatud olekut. Fail võib sisaldada ka aegunud liisinguid ega näita, kas seade on praegu võrku ühendatud. Ära muuda seda töötava teenuse ajal käsitsi.

---

## 5. Aadressireserveering (MAC → IP)

**Aadressireserveeringu** ehk staatilise aadressimääranguga saab kindla MAC-aadressiga klient DHCP kaudu alati sama IP-aadressi. See sobib näiteks printerile või muule seadmele, mille aadress peab püsima samana.

Kliendis peab aadressi hankimine jääma **automaatseks**. Reserveering erineb kliendis käsitsi määratud staatilisest IP-aadressist.

Lisa `host`-plokk faili `/etc/dhcp/dhcpd.conf` **pärast `subnet`-plokki, selle sulgeva loogelise sulu järele**. Nii on võrgu ja klientide kirjeldused lihtsamini eristatavad.

**Mall:**

```text
host HOSTINIMI {
    hardware ethernet XX:XX:XX:XX:XX:XX;
    fixed-address 10.100.0.X;
}
```

**Näide:** printer raamatupidamises saab aadressi `10.100.0.144`:

```text
host printer-raamatupidamine {
    hardware ethernet bc:30:d9:2a:c9:50;
    fixed-address 10.100.0.144;
}
```

| Parameeter | Selgitus |
|---|---|
| `host` | Seadistuses kasutatav kirjeldav nimi. See ei pea kattuma seadme tegeliku hostinimega ega loo iseenesest DNS-kirjet. |
| `hardware ethernet` | Kliendi selle võrguliidese MAC-aadress, mille kaudu ta DHCP-d kasutab. Linuxi kliendis näeb seda käsuga `ip link show`. |
| `fixed-address` | Kliendile antav püsiv aadress. See peab kuuluma sobivasse alamvõrku, olema mujal kasutamata ja jääma dünaamilistest `range`-vahemikest välja. |

!!! warning "Hoia ISC DHCP reserveeringud dünaamilisest vahemikust väljas"

    Selle juhendi dünaamiline vahemik on `10.100.0.10–10.100.0.100`.
    Printeri aadress `10.100.0.144` jääb sellest välja.
    ISC DHCP ei välista `fixed-address` aadressi automaatselt dünaamilisest vahemikust;
    kattumine võib põhjustada IP-aadressikonflikti. See nõue puudutab siin kasutatavat
    ISC DHCP lahendust.

Pärast muutmist kontrolli süntaksit ja eduka kontrolli järel taaskäivita teenus:

```bash
dhcpd -4 -t -cf /etc/dhcp/dhcpd.conf
systemctl restart isc-dhcp-server
```

!!! info "Reserveering ei muuda kliendi aadressi kohe"

    Serveri taaskäivitamine ei sunni klienti kehtivat liisingut kohe uuendama.
    Uue aadressi kontrollimiseks vabasta kliendi liising ja hangi aadress uuesti.
    `fixed-address` määrangud ei kajastu tavapäraste dünaamiliste liisingutena failis
    `dhcpd.leases`; kontrolli tulemust kliendist ja serveri logist.

---

## 6. Kontrollimine kliendist

Teenuse olek `active` näitab, et serveriprotsess töötab. Lõplikult kontrollib lahendust see, kas klient saab õiged võrguseaded.

1. Ühenda klient serveriga samasse laborivõrku.
2. Määra kliendi IPv4-aadressi ja DNS-serveri hankimine automaatseks.
3. Hangi DHCP-seadistus uuesti.
4. Kontrolli saadud aadressi, maski, vaikelüüsi ja DNS-serverit.

!!! tip "Jälgi samal ajal serveri logi"

    Käivita serveris allolev käsk ja uuenda seejärel kliendi liisingut:

    ```bash
    journalctl -u isc-dhcp-server -f
    ```

    Otsi kliendi MAC-aadressiga seotud DHCPDISCOVER-, DHCPOFFER-, DHCPREQUEST-
    ja DHCPACK-kirjeid. Liisingu pikendamisel ei pruugi kõiki nelja sõnumit olla.
    Logi jälgimise lõpetad klahvikombinatsiooniga `Ctrl+C`.

---

## Rohkem infot

Kohalikud juhendilehed:

```bash
man dhcpd
man dhcpd.conf
man dhcpd.leases
```

Ametlikud allikad:

- [ISC DHCP ja hoolduse lõppemine](https://www.isc.org/dhcp/)
- [Debian 13 väljalaskemärkmed: aegunud komponendid](https://www.debian.org/releases/trixie/release-notes/issues.en.html#deprecated-components-for-trixie)
- [isc-dhcp-server Debian 13 paketivalikus](https://packages.debian.org/trixie/isc-dhcp-server)
- [Debiani dhcpd.conf juhendileht](https://manpages.debian.org/trixie/isc-dhcp-server/dhcpd.conf.5.en.html)
- [Debiani dhcpd juhendileht](https://manpages.debian.org/trixie/isc-dhcp-server/dhcpd.8.en.html)

---

*Õppematerjali koostaja: Priit Paap, 2026*
