# Oma võrguühenduse uurimine

Kui ühendus ei tööta, on kasulik osata öelda rohkem kui „internet on katki”. Seadme ühenduse infost näed, millist võrku kasutad ning millised võrguseaded on seadmel olemas. Selles materjalis õpid neid andmeid leidma ja selgitama.

**Enne alustamist:** loe [Võrgu toimimise](01_vorgu_toimimine.md) materjalist IP-aadressi, vaikelüüsi, DNS-i ja DHCP selgitusi.

!!! info "Õpieesmärgid"

    Pärast materjali läbimist oskad:

    - leida, kas seade kasutab WiFi või kaabelühendust;
    - leida WiFi võrgunime ehk SSID;
    - leida Windowsis aktiivse ühenduse IPv4-aadressi ja vaikelüüsi;
    - eristada oma seadme aadressi vaikelüüsi aadressist;
    - kirjeldada ühendust nii, et sellest oleks abi probleemi lahendamisel.

## 1. Mida ühenduse kohta vaadata?

Alusta ühenduse liigist ja valitud võrgust. Seejärel uuri aadresse ning seadme kuvatud ühenduse olekut.

| Teave | Milleks seda vaja on? | Õppenäide |
| --- | --- | --- |
| Ühenduse liik | Näitab, kas seade kasutab kaablit või WiFi ühendust. | Ethernet või WiFi. |
| WiFi võrgunimi ehk SSID | Aitab kontrollida, millisesse juhtmevabasse võrku seade on ühendatud. | `Oppeklass`. |
| Kohalik IPv4-aadress | Näitab aktiivse võrguliidese aadressi. | `192.168.1.25`. |
| Vaikelüüs | Näitab tavaliselt kohaliku ruuteri aadressi. | `192.168.1.1`. |
| DNS-server | Näitab, kuhu saab saata nimepäringuid; seadistusest sõltuvalt võib selleks olla ruuter või muu teenus. | `192.168.1.1`. |
| IP-seadete määramine | Näitab, kas seaded saadakse automaatselt või on käsitsi määratud. | Automaatne ehk DHCP. |

Tabeli aadressid on õppenäited. Sinu seadme aadressid võivad olla teistsugused.

### Võrgunimi ei ole IP-aadress

**SSID** (*service set identifier*) on WiFi võrgu nimi, näiteks `Oppeklass`. Selle järgi valid võrgu. **IP-aadress** on aadress, mida seade kasutab võrgus suhtlemiseks. Need ei ole teineteise asendused.

Samal WiFi võrgunimel võib olla mitu pääsupunkti. Ka võõras võrk võib kasutada tuttavat nime. Koolis ühendu õpetaja näidatud võrku, järgides kooli ühendusjuhiseid.

### Vaata aktiivse ühenduse andmeid

Arvutil võib olla korraga mitu adapterit, näiteks WiFi, Ethernet ja virtuaalsed adapterid. Igaühe andmed ei kirjelda sinu praegust ühendust. Kui uurid WiFi ühendust, vaata just WiFi adapteri infot.

!!! example "Näide: kaks erinevat aadressi"

    Arvuti IPv4-aadress on `192.168.1.25` ja vaikelüüs `192.168.1.1`. Esimene kuulub selles näites arvutile, teine ruuteri kohaliku võrgu liidesele. Arvuti saadab interneti poole liikuva liikluse tavaliselt vaikelüüsile.

## 2. Ühenduse info leidmine Windowsis

### Graafilised seaded

1. Ava **Sätted** (*Settings*).
2. Vali **Võrk ja Internet** (*Network & internet*).
3. Ava aktiivse ühenduse omadused. WiFi korral vali ühendatud võrk; kaabelühenduse korral **Ethernet**.
4. Leia **IPv4-aadress** (*IPv4 address*).
5. Vaata ka võrgunime, DNS-i ja IP-seadete määramist, kui need on selles vaates nähtavad.

Menüüde nimetused ja väljade asukohad võivad erineda. Vaikelüüsi puudumisel selles vaates kasuta järgmist meetodit.

### Käsk ipconfig

1. Ava Start-menüüst **Käsuviip** (*Command Prompt*).
2. Sisesta käsk ja vajuta Enter:

```text
ipconfig
```

Leia aktiivne **Wireless LAN adapter Wi-Fi** või **Ethernet adapter**. Loe välju **IPv4 Address** ja **Default Gateway**. Märge **Media disconnected** tähendab, et vastava adapteri ühendus on lahti. Lisainfo, näiteks DNS-serverite ja DHCP kasutuse vaatamiseks saab kasutada käsku `ipconfig /all`.

!!! example "Õppenäide käsu tulemusest"

    Järgmised read on lühendatud väljavõte, mitte kogu käsu väljund.

    ```text
    Ethernet adapter Ethernet:
       IPv4 Address. . . . . . . . . . . : 192.168.1.25
       Default Gateway . . . . . . . . . : 192.168.1.1
    ```

    Arvuti Etherneti liidese aadress on `192.168.1.25` ja vaikelüüsi aadress `192.168.1.1`.

Käsurea kasutamine on siin lihtsalt üks võimalus teavet vaadata. Ühenduse andmete leidmine ei nõua administraatori õigusi.

## 3. Ühenduse info leidmine telefonis

Telefonis alusta WiFi seadetest ja ava selle võrgu üksikasjad, millega oled ühendatud. Tavaliselt jõuab sinna võrgunime, teabenupu või hammasratta kaudu. Androidi menüüd sõltuvad telefonitootjast; mõnikord tuleb avada täpsemad seaded. iPhone’is ava **Settings → Wi-Fi** ja puuduta ühendatud võrgu kõrval teabenuppu.

Leia võimaluse korral:

- ühendatud võrgu nimi;
- IP-aadress;
- ruuteri või lüüsi aadress;
- DNS-i info.

Kõik telefonid ei näita neid andmeid ühes vaates. Kui mõnda välja ei leia, märgi see töölehele ja küsi õpetajalt abi.

!!! tip "Vaata õiget aadressivälja"

    Telefon võib näidata ka WiFi- või MAC-aadressi. See ei ole IP-aadress. Käesolevas harjutuses otsi välja „IP address” või „IP-aadress”.

Telefon võib kasutada WiFi asemel või selle kõrval mobiilset andmesidet. Veebilehe avanemine ei tõenda seetõttu alati, et internet tuli just uuritava WiFi võrgu kaudu.

## 4. Väike harjutus oma ühendusega

Uuri õpetaja määratud seadme ühendust. Vaata olemasolevaid andmeid; selle harjutuse jaoks ei ole vaja IP-aadresse ega DNS-i muuta.

| Küsimus | Sinu tulemus |
| --- | --- |
| Millist seadet uurid? | |
| Kas kasutad WiFi või Etherneti ühendust? | |
| Mis on WiFi võrgunimi? Kaabelühenduse korral märgi „ei kohaldu”. | |
| Mis on aktiivse ühenduse IPv4-aadress? | |
| Mis on vaikelüüsi või ruuteri aadress? | |
| Kas IP-seaded saadakse automaatselt? Kui infot ei leia, märgi see. | |
| Kas õpetaja määratud veebileht avaneb? | |

Kirjelda kahe lausega, kuidas sinu seade võrku ühendub ja milline seade aitab sellel teistesse võrkudesse jõuda.

!!! tip "Jaga ainult ülesande jaoks vajalikku infot"

    Ühenduse kirjeldusse ei ole vaja lisada WiFi parooli. Ekraanipildi esitamisel lõika välja muu isiklik info ning teised avatud aknad. Koolivõrgu andmeid kasuta õppeülesandes õpetaja juhiste järgi.

## 5. Kuidas ühendusprobleemi kirjeldada?

Abistajal on lihtsam probleemi uurida, kui ütled, mida kontrollisid ja milline oli tulemus.

!!! example "Näide: abistav probleemikirjeldus"

    „Kasutan klassiarvutit kaabelühendusega. Võrgupordi märgutuli põleb ning arvutil on IPv4-aadress ja vaikelüüs. Õpetaja antud veebileht ei avane, kuid kõrval oleva arvuti samasugune ühendus töötab.”

Selline kirjeldus ei anna veel lõplikku diagnoosi. See aitab otsustada, mida järgmisena kontrollida. Kasulik on märkida ka vea ilmumise aeg ja nähtava veateate tekst.

Kui ühes seadmes ühendus ei tööta, kuid teistes töötab, alusta selle seadme ühenduse kontrollist. Kui probleem puudutab kõiki seadmeid, tuleb uurida ka nende ühiseid ühendusi. Ükski tähelepanek ei välista kõiki võimalikke põhjuseid.

## 6. Enesekontroll

### 1. Kas SSID ja IP-aadress tähendavad sama asja?

??? success "Vaata vastust"

    Ei. SSID on WiFi võrgu nimi, mille järgi võrku valid. IP-aadress on võrguliidese aadress andmete suunamiseks.

### 2. Arvuti aadress on 192.168.1.25 ja vaikelüüs 192.168.1.1. Kumb kuulub selles näites arvutile?

??? success "Vaata vastust"

    `192.168.1.25` on arvuti võrguliidese aadress. `192.168.1.1` on vaikelüüsi, tavaliselt ruuteri kohaliku liidese aadress.

### 3. ipconfig näitab kolme adapterit. Kas võid valida suvalise adapteri aadressi?

??? success "Vaata vastust"

    Ei. Vaata uuritava aktiivse ühenduse adapterit. Lahti ühendatud või mõne muu ühenduse jaoks mõeldud adapteri info ei kirjelda seda ühendust.

### 4. Kas nähtav kohalik IP-aadress tõendab, et internet töötab?

??? success "Vaata vastust"

    Ei. Aadress on üks vajalikest seadetest, kuid interneti kasutamine sõltub ka ühendusteest, muudest seadetest ja kasutatava teenuse toimimisest.

### 5. Miks võib telefonis veebileht avaneda ka katkise WiFi internetiühenduse korral?

??? success "Vaata vastust"

    Telefon võib kasutada mobiilset andmesidet. Seetõttu tuleb arvestada, millise ühenduse kaudu andmed tegelikult liiguvad.

### 6. Kas ühendusprobleemi kirjeldamiseks on vaja abistajale oma WiFi parooli saata?

??? success "Vaata vastust"

    Ei. Esmaseks kirjeldamiseks piisab ühenduse liigist, vajalikust ühenduse infost, sümptomitest ja tehtud kontrollidest. Parooli ei lisata töölehele ega ekraanipildile.

## Kokkuvõte

- Uuri kõigepealt ühenduse liiki ja aktiivset võrku.
- WiFi võrgunimi, seadme IP-aadress ja vaikelüüsi aadress tähendavad eri asju.
- Windowsis saad ühenduse infot vaadata seadetes või käsuga `ipconfig`.
- Mitme adapteri korral vali uuritava ühenduse andmed.
- Interneti toimimist hinnatakse eraldi; IP-aadressi või WiFi märgi olemasolust üksi ei piisa.
- Probleemikirjelduses ütle, mis ei tööta, keda see puudutab ja mida juba kontrollisid.

### Mõtle õpitule

1. Milline ühenduse info leidmise viis oli sinu jaoks kõige lihtsam?
2. Selgita kaaslasele, miks seadme aadress ja vaikelüüsi aadress on erinevad.
3. Kirjelda üht ühendusprobleemi nii, et abistaja teaks, kust uurimist alustada.

## Teema sõnastik

| Mõiste | Selgitus |
| --- | --- |
| SSID (*service set identifier*) | WiFi võrgu nimi. |
| Aktiivne ühendus | Ühendus, mida seade parajasti kasutab; seadmel võib olla ka mitu aktiivset ühendust. |
| IPv4 | IP versioon, mille aadressi kirjutatakse tavaliselt nelja punktidega eraldatud arvuna. |
| Vaikelüüs (*default gateway*) | Tavaliselt ruuter, mille kaudu seade jõuab teistesse võrkudesse. |
| Automaatne seadistus ehk DHCP | Võrguseadete saamine DHCP-serverilt ilma käsitsi sisestamiseta. |
| ipconfig | Windowsi käsk võrguadapterite IP-seadete vaatamiseks. |

## Allikad ja lisalugemine

1. [Microsoft: Essential Network Settings and Tasks in Windows](https://support.microsoft.com/en-us/windows/experience/connectivity-networking/essential-network-settings-and-tasks-in-windows){ target="_blank" rel="noopener" } — ühenduse seaded ja IP-aadressi leidmine.
2. [Microsoft Learn: ipconfig](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/ipconfig){ target="_blank" rel="noopener" } — käsu kasutamine.
3. [Apple: Use a private network address on iPhone](https://support.apple.com/guide/iphone/iph6b324bb33/ios){ target="_blank" rel="noopener" } — WiFi võrgu üksikasjade avamine ja WiFi-aadressi tähendus.

**Järgmine materjal:** [Võrgukaabli valmistamine ja kontrollimine](03_vorgukaabli_valmistamine.md).

---
*Õppematerjali koostaja: Priit Paap, 2026*
