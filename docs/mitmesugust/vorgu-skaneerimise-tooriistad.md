# Praktiline töö: võrgu ja portide skaneerimine

## Eesmärk

Praktilise töö eesmärk on õppida tuvastama võrgus olevaid seadmeid, avatud TCP-porte ja nende taga töötavaid teenuseid ning võrrelda kahe võrguskannimise tööriista võimalusi.

Töö käigus kasutad:

-   **Advanced Port Scannerit** lihtsamaks hostide ja portide
    avastamiseks;
-   **Nmap/Zenmapi** detailsemaks portide, teenuste ja
    operatsioonisüsteemide tuvastamiseks;
-   Zenmapi **Topology** vaadet võrgu visuaalseks kaardistamiseks.
  

Töö lõpuks oskad:

-   skaneerida ühte hosti ja tervet alamvõrku erinevate vahenditega;
-   selgitada, mida tähendab avatud port;
-   seostada levinud porte nende taga töötavate teenustega.

!!! warning "Skaneeri ainult lubatud süsteeme" 

    Skaneeri ainult kooli laborivõrgu seadmeid või süsteeme, mille skaneerimiseks on sul    selge luba. Avaliku Interneti skaneerimiseks kasutatakse selles ülesandes ainult
    `scanme.nmap.org` serverit, mille Nmap projekt on loonud Nmap skannide
    harjutamiseks.

------------------------------------------------------------------------

# 1. Advanced Port Scanner

Advanced Port Scanner on Windowsi-põhine võrgu skannimise tööriist,
millega saab otsida võrgus olevaid seadmeid ning nende avatud porte ja
teenuseid.

## 1.1. Advanced Port Scanneri paigaldamine

Kasuta ülesande sooritamiseks mõnda kooli laborikeskkonnas olevat **Windowsi virtuaalmasinat**, mis asub **"Internet"** võrgukaardiga võrgus (näiteks PRTG labori masin). Sobiva VM-i puudumisel tuleb see paigaldada.

!!! warning "Microsoft Defender SmartScreeni hoiatus"
    
    Advanced Port Scanneri allalaadimisel võib Windows kuvada Microsoft
    Defender SmartScreeni hoiatuse. SmartScreen kasutab muu hulgas
    rakenduse ja väljaandja reputatsiooni ning hoiatus ei tähenda
    automaatselt, et fail sisaldab pahavara. Kontrolli enne jätkamist, et installer on alla laaditud Advanced Port Scanneri ametlikult veebilehelt ja võid jätkata.

1.  Ava veebileht [Advanced Port
    Scanner](https://www.advanced-port-scanner.com/){ target="_blank" rel="noopener" }.
2.  Vajuta **Free Download**.
3.  Käivita alla laaditud installer.
4.  Vali inglise keel ja vajuta **OK**.
5.  Vali **Install** ja jätka paigaldamist.
6.  Nõustu litsentsitingimustega.
7.  Lõpeta paigaldamine.

## 1.2. Ühe hosti skaneerimine Advanced Port Scanneriga

Selles osas skaneerid kahte enda laborikeskkonnas olevat seadet, mille lõid Zabbixi laboris:

-   **MikroTik ruuterit** (ükskõik millist enda varem loodud Mikrotik ruuterit);
-   **Zabbix serverit**.

Tee nii:

1.  Käivita **Advanced Port Scanner**.
2.  Ava **Settings → Options → Resources**.
3.  Lülita sisse vähemalt **Shared folders and printers**, **HTTP**,
    **HTTPS** ja **RDP**.
4.  Ava **Settings → Options → Performance**.
5.  Sea **Scanning speed** sobivalt kiireks (võid jätta ka vaikevaliku).

![apc options](assets/vorgu-skaneerimise-tooriistad/apc-options.png)


!!! info "Skannimise kiirus" 
    
    Suurem skannimiskiirus võimaldab kontrollida porte kiiremini, kuid tekitab rohkem võrguliiklust ja kasutab rohkem arvuti ressursse.

## 1.3. MikroTik ruuteri skaneerimine

Sisesta aadressiväljale oma MikroTik ruuteri IP-aadress.

Portideks vali (tee portide lahter tühjaks siis saad valida):

``` text
All TCP ports 1-65535
```

Käivita skann. Kui skann on lõppenud, ava leitud hosti detailid (parempoolne tulp) ja vaata:

-   kas host vastab;
-   milline MAC-aadress tuvastati;
-   milline tootja kuvatakse;
-   millised TCP-pordid on avatud;
-   millised teenused on portidega seotud.

*Tulemuse näide:*

![apsc details](assets/vorgu-skaneerimise-tooriistad/apsc-details.png)

- **Status:** Näitab, kas masin töötab.
- **Operating system:** Näitab, mis operatsioonisüsteem masina peal jookseb.
- **MAC:** Näitab skannitud masina MAC-aadressi.
- **Manufacturer:** Näitab skannitud masina tootjat (pole alati täpne).
- **Service:** siit näeb ära, mis pordid skannitud masinal lahti on ja mille jaoks neid kasutatakse. Antud masinal on näha, et avatud on pordid 22, 80, 443 ja 623. Pordi numbrite järgi on võimalik leida ka infot internetist, isegi kui kirjeldust pole.


!!! info "Operatsioonisüsteemi tuvastamine" 

    Võrguskanner võib võrgust saadud info põhjal proovida hinnata, milline operatsioonisüsteem seadmel töötab. Selline tulemus ei pruugi alati olla täpne.

!!! tip "Kuvatõmmis"
    
    Tee skanni tulemuse detailidest kuvatõmmis 1!


## 1.4. Zabbix serveri skaneerimine

Korda sama skanni oma **Zabbix serveri** IP-aadressiga.

!!! tip "Kuvatõmmis"
    
    Tee skanni tulemuse detailidest kuvatõmmis 2!

**Vasta kirjalikult nend kahe kuvatõmmise juurde:**

1.  Millised TCP-pordid olid MikroTik ruuteril avatud?
2.  Millised TCP-pordid olid Zabbix serveril avatud?
3.  Millised teenused suutis Advanced Port Scanner tuvastada?
4.  Millised pordid esinesid mõlemal seadmel?
5.  Mille poolest kahe seadme tulemused erinesid?

## 1.5. Alamvõrgu skaneerimine Advanced Port Scanneriga

Sisesta aadressiks:

``` text
172.16.200.0/24
```

Portideks vali:

``` text
Well-known TCP ports 1-1023
```

![adpc subnet small](assets/vorgu-skaneerimise-tooriistad/adpc-subnet-small.png)


Käivita skann ja oota selle lõppemist.


!!! tip "Kuvatõmmis"
    
    Tee skanni tulemusest kuvatõmmis 3 (leitud hostid)!

## 1.6. Tulemuste analüüsimine

Vali tulemustest vähemalt **kolm erinevat hosti** ja koosta nende põhjal
tabel, mis tuleb lisada ka töö esitusse.


| IP-aadress | Avatud pordid | Tuvastatud teenused | Arvatav seadme roll |
| --- | --- | --- | --- |
|host1   |   |   |   |
|host2   |   |   |   |
|host3   |   |   |   |

!!! question "Mõtle" 

    Mille põhjal saab avatud portide ja teenuste järgi hinnata, millist rolli seade võrgus täidab?

# 2. Nmap ja Zenmap

**Nmap (Network Mapper)** on avatud lähtekoodiga võrgu avastamise ja
turbeauditi tööriist.

**Zenmap** on Nmap graafiline kasutajaliides. Zenmap võimaldab kasutada
Nmap võimalusi graafilise kasutajaliidese kaudu ning näitab samal ajal
ka käsku, millega Nmap tegelikult käivitatakse.

## 2.1. Nmap ja Zenmap paigaldamine Windowsile

1.  Ava [Nmap Download](https://nmap.org/download.html){ target="_blank" rel="noopener" }.
2.  Laadi alla Windowsi **Latest stable release self-installer**.
3.  Käivita installer.
4.  Jäta paigaldatuks vähemalt **Nmap**, **Zenmap** ja **Npcap**.
5.  Jäta Npcapi paigaldamisel vaikimisi valikud.
6.  Lõpeta paigaldamine.
7.  Kontrolli, et Windowsi Start-menüüs oleks olemas **Nmap - Zenmap GUI**.

## 2.2. Ühe hosti skaneerimine Zenmapiga

Käivita **Nmap - Zenmap GUI**.

Sisesta **Target** väljale kõigepealt oma MikroTik ruuteri IP-aadress.

Vali profiil:

``` text
Intense scan, all TCP ports
```

!!! info "Zenmap ja Nmap" 

    Zenmap ei kasuta eraldi skannimismootorit. Graafiline kasutajaliides koostab valikute põhjal Nmap käsu ja käivitab selle. Käsk on nähtav command väljal.

![zenmap scan1](assets/vorgu-skaneerimise-tooriistad/zenmap-scan1.png) 


Käivita skann.

Kui skann on lõppenud:

1.  Vali vasakult skannitud host. klõpsa IP aadressil;
2.  Ava **Ports/Hosts** lehekülg;
3.  Vaata avatud porte;
4.  Vaata tuvastatud teenuseid;
5.  Vaata, kas Nmap suutis tuvastada teenuste versioone.

![zenmap scan2](assets/vorgu-skaneerimise-tooriistad/zenmap-scan2.png){ width="75%" }


!!! tip "Kuvatõmmis"
    
    Tee skanni tulemusest kuvatõmmis 4 (Zenmap aken koos leitud portide ja hostidega)!

Korda skanni oma** Zabbix serveriga**.

!!! tip "Kuvatõmmis"

    Tee skanni tulemusest kuvatõmmis 5 (Zenmap aken koos leitud portide ja hostidega)!


## 2.3. Alamvõrgu skaneerimine Zenmapiga

Sisesta **Target** väljale:

``` text
172.16.200.0/24
```

Vali profiil:

``` text
Intense scan
```

Käivita skann.

Kui skann on lõppenud:

1.  Vaata vasakul **Hosts** nimekirja;
2.  Vali erinevaid hoste;
3.  Ava **Ports/Hosts**;
4.  Võrdle avatud porte ja teenuseid;
5.  Vaata, kas Nmap suutis tuvastada seadmete operatsioonisüsteeme.

## 2.4. Võrgu topoloogia

Pärast alamvõrgu skannimist ava **Topology** ja seejärel vajadusel **Fisheye**.

Tutvu leitud topoloogiaga.

*Zenmapi joonistatud topoloogia näide:*

![zenmap topology](assets/vorgu-skaneerimise-tooriistad/zenmap-topology.png)



## 2.5. Lubatud välise serveri skaneerimine

Nmap projekt pakub õppimiseks serverit:

``` text
scanme.nmap.org
```

!!! warning "Piira skannimist" 

    `scanme.nmap.org` on mõeldud Nmap skannide harjutamiseks. Ära tee sellele koormusteste, exploit-katseid ega suurel hulgal korduvaid skanne.

Sisesta Zenmapi **Target** väljale:

``` text
scanme.nmap.org
```

Kasuta profiili:

``` text
Intense scan
```

Käivita skann.

Kui skann on valmis:

1.  Ava **Ports/Hosts**;
2.  Vaata, millised TCP-pordid leiti;
3.  Vaata, milliseid teenuseid Nmap tuvastas;
4.  Ava **Topology** ja vaata, kuidas väline host seal paikneb.

!!! question "Mõtle" 

    Kas Zenmapi topoloogia näitab kindlasti võrgu tegelikku füüsilist ülesehitust? Millise info põhjal saab Nmap võrgus olevate seadmete kaugust hinnata?

!!! tip "Kuvatõmmis"

    Tee topoloogia aknast kuvatõmmis 6 (Zenmap aken, kus on näha kõik skannitud hostid koos topoloogiaga)!

1. Navigeeri üleval ribal olevale menüüle **Scan** → **Save scan**.
2. Salvesta ühe skanni tulemus oma töölauale. Nii on võimalik tulemusi hiljem uuesti avada ja analüüsida. 

# Töö esitamine

Esita töö **ühe PDF-failina**.

PDF peab sisaldama järgmisi kuvatõmmiseid:

1.  Advanced Port Scanneri Mikrotiki ja Zabbix serveri skanni tulemus (kuvatõmmised 1 ja 2);
2.  Advanced Port Scanneri alamvõrgu skanni tulemus (3);
3.  Zenmapi hostide **Ports/Hosts** tulemus (4 ja 5).
4.  Zenmapi **Topology** vaade, kus on näha laborivõrk ja
    `scanme.nmap.org` (6).

**Lisaks peavad töös olema**:

-   Advanced Port Scanneriga skanneeritud ruuteri ja Zabbix serveri tulemuste analüüs ehk vastused küsimustele (ülesande punkt 1.4);
-   Vähemalt kolme alamvõrgu hosti analüüsi tabel (ülesande punkt 1.6).

!!! success "Töö tulemus" 

    Kui oled praktilise töö lõpetanud, oskad kasutada võrguskannereid hostide, avatud portide ja teenuste leidmiseks ning oskad hinnata, millist infot saab võrgu kohta Nmapiga koguda.
