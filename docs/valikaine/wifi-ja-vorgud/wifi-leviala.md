# WiFi levi, ühenduse kiirus ja turvaline kasutamine

Ühes toas töötab videokõne hästi, teises katkeb heli ja pilt. Telefon näitab WiFi ühendust, kuid veebileht avaneb aeglaselt. Põhjus võib olla levialas, võrgu koormuses, internetiühenduses või ühes seadmes. Selles materjalis õpid neid olukordi eristama, mõõtmistulemusi mõistma ning ühenduse parandamiseks põhjendatud ettepanekuid tegema.

!!! info "Õpieesmärgid"

    Pärast materjali läbimist oskad:

    - selgitada kauguse, takistuste ja pääsupunkti asukoha mõju WiFi levile;
    - võrrelda 2,4 ja 5 GHz sagedusalade kasutamist;
    - eristada signaalitugevust, alla- ja üleslaadimiskiirust ning viivitust;
    - teha eri kohtades võrreldavaid ühenduse mõõtmisi;
    - põhjendada võimalusi kodu või töökoha WiFi ühenduse parandamiseks;
    - kirjeldada koduse ja avaliku WiFi turvalise kasutamise põhimõtteid;
    - valida lihtsa ühendusprobleemi korral sobiva kontrollimise järjekorra.

## 1. Mida WiFi ühendus näitab?

**WiFi** võimaldab telefonil, arvutil või muul seadmel pääsupunktiga juhtmevabalt ühenduda. Koduses WiFi ruuteris on pääsupunkt ja ruuter sageli samas seadmes. Selles materjalis kasutame sõna **pääsupunkt**, kui räägime WiFi raadioside loomisest.

Telefonis kuvatav WiFi märk näitab juhtmevaba ühendust. Selle põhjal ei saa veel kinnitada, et internet või konkreetne veebiteenus töötab. Ka hästi toimiva kohaliku WiFi korral võib teenusepakkuja ühenduses olla rike.

!!! example "Näide: kaks erinevat probleemi"

    - Telefon kaotab tagumises toas WiFi ühenduse, kuid pääsupunkti lähedal töötab hästi. Esmalt tasub uurida leviala ja takistusi.
    - Kõigis tubades on tugev WiFi signaal, kuid internet ei tööta üheski seadmes. Esmalt tasub uurida ühist ühendusteed ja internetiühenduse olekut.

    Need tähelepanekud annavad uurimisele suuna. Need ei tõenda veel üht kindlat rikke põhjust.

## 2. Mis mõjutab WiFi levi ja ühenduse kvaliteeti?

WiFi kasutab raadiolaineid. Pääsupunkt (*access point*) ja kliendiseade peavad teineteisega andmeid vahetama, seega on ühendus **kahesuunaline**. Oluline on nii pääsupunkti kui ka telefoni või arvuti võime signaali saata ja vastu võtta.

**Leviala** on piirkond, kus ühendus on kasutamiseks piisavalt hea. Sellel pole kõikide ruumide jaoks üht kindlat meetrite arvu. Tulemus sõltub hoonest, seadmetest ja keskkonnast.

| Mõjutaja | Võimalik mõju | Mida tähele panna? |
| --- | --- | --- |
| Kaugus | Pääsupunktist eemaldudes signaal üldiselt nõrgeneb. | Kas kaugemas ruumis ühendus halveneb? |
| Seinad ja vahelaed | Nõrgendavad signaali; mõju sõltub materjalist ja ehitusest. | Mitu takistust jääb seadmete vahele? |
| Metall ja suur mööbel | Võivad raadiosidet varjata või muuta signaali levimise teid. | Kas pääsupunkt on metallkapis või suure eseme taga? |
| Teised võrgud ja häireallikad | Võivad vähendada raadiosideks kasutatavat aega või segada vastuvõttu. | Kas probleem sõltub asukohast või kellaajast? |
| Võrgu kasutamine | Suured allalaadimised, varundamine ja muud tegevused kasutavad ühenduse võimalusi. | Kas samal ajal liigub palju andmeid? |
| Kliendiseade | Adapter, tarkvara ja seadme asend mõjutavad tulemust. | Kas sama koha peal käituvad seadmed erinevalt? |

### Pääsupunkti asukoht

Sobiv koht on tavaliselt kasutusala lähedal, avatult ja põrandast kõrgemal. Pääsupunkti peitmine kappi või hoone äärmisesse nurka võib leviala halvendada. Paigutamisel arvestatakse ka seadme paigaldusjuhendit: lae-, seina- ja lauaseadmed võivad vajada erinevat asendit.

!!! example "Näide: ühendus töökojas"

    Pääsupunkt asub töökoja otsas metallkapis. Teises otsas oleva laua juures katkeb ühendus. Üks põhjendatud ettepanek on paigutada pääsupunkt avatud kohta kasutusala keskele. Kui sellest ei piisa, võib vaja minna täiendavat pääsupunkti.

### Jagatud raadioside

Samal WiFi kanalil töötavad seadmed jagavad raadioside aega. **Kanal** on osa sagedusalast, mida ühendus kasutab. Ka naabervõrkude tegevus võib mõjutada sinu ühendust. Suur arv võrgunimesid loendis ei näita siiski üksi, kui koormatud ühendus tegelikult on.

!!! tip "Rohkem saatevõimsust ei lahenda kõike"

    Telefon peab suutma pääsupunktile ka vastata. Kui peamine probleem on halb asukoht või palju takistusi, ei pruugi ainult pääsupunkti saatevõimsuse suurendamine ühendust parandada.

## 3. Mille poolest erinevad 2,4 ja 5 GHz?

**Sagedusala** kirjeldab raadiosageduste piirkonda, mida WiFi kasutab. Levinud on 2,4 ja 5 GHz; uuemates seadmetes kasutatakse ka 6 GHz sagedusala. Selles kursuses keskendume kahele esimesele.

| Võrdlus | 2,4 GHz | 5 GHz |
| --- | --- | --- |
| Levi sarnastes tingimustes | Ulatub üldjuhul kaugemale ja läbib takistusi paremini. | Nõrgeneb takistuste läbimisel üldjuhul rohkem. |
| Kiiruse võimalused | Sageli väiksemad kui sobivalt toimival 5 GHz ühendusel. | Sageli paremad eeldused suuremaks andmeedastuskiiruseks. |
| Keskkond | Kasutavad ka paljud muud seadmed; raadioside võib olla tihedalt koormatud. | Kasutada on rohkem kanaleid, kuid koormus sõltub asukohast. |
| Võimalik kasutus | Kaugemad ruumid ja seadmed, mis toetavad ainult seda sagedusala. | Pääsupunktile lähemal asuvad arvutid, telefonid ja muud sobivad seadmed. |

Tulemus sõltub seadmetest ja keskkonnast. Nõrk 5 GHz ühendus ei ole tingimata parem kui korralik 2,4 GHz ühendus.

Üks WiFi võrgunimi ehk **SSID** võib hõlmata mitut sagedusala. Seade ja võrk valivad siis ühenduse vastavalt oma võimalustele. Võrgunime põhjal ei pruugi kasutatav sagedusala selguda. 

## 4. Signaalitugevus, kiirus ja viivitus

Ühenduse kvaliteeti ei kirjelda üksainus arv. Oluline on nii ühenduse püsimine, andmete edastamise kiirus kui ka reageerimise aeg.

| Näitaja | Mida see kirjeldab? | Mille puhul seda märkad? |
| --- | --- | --- |
| Signaalitugevus | Kui tugevalt võtab seade pääsupunkti raadiosignaali vastu. | Seadmes kuvatavad WiFi kaared või pulgad. |
| Allalaadimiskiirus (*download*) | Kui kiiresti liiguvad andmed testi käigus internetist seadmesse. | Faili allalaadimine, video vaatamine. |
| Üleslaadimiskiirus (*upload*) | Kui kiiresti liiguvad andmed seadmest internetti. | Faili saatmine, pilvevarundus, oma videopildi edastamine. |
| Viivitus (*latency*) | Kui palju aega kulub andmevahetusele kasutatava mõõtemeetodi järgi. | Vestluse reageerimine, mängu tegevuste hilinemine. |
| Viivituse kõikumine (*jitter*) | Kui palju viivitus mõõtmiste vahel muutub. | Heli või pildi ebaühtlane liikumine reaalajas suhtluses. |

### Mida WiFi kaared ütlevad?

WiFi kaared annavad ligikaudse hinnangu signaalile. Eri seadmete näidud ei pruugi olla võrreldavad. Tugev signaal ei kinnita suurt internetikiirust: ühendus võib olla koormatud või piiratud mõnes teises võrguosas.

Mõni tööriist näitab signaali **dBm-des**, mis on võimsuse väljendamise ühik. Tavaliste negatiivsete WiFi näitude puhul on näiteks `−50 dBm` tugevam kui `−80 dBm`. Selle kursuse vaatluseks piisab ka sama seadme kuvatavate kaarte või sõnalise hinnangu võrdlemisest.

### Millistes ühikutes kiirust mõõdetakse?

Võrgukiirust väljendatakse sageli **Mbps** ehk **Mbit/s**: megabitti sekundis. Failide suurust ja mõne rakenduse ülekandekiirust näidatakse baitides, näiteks MB või MB/s. **Üks bait on kaheksa bitti.**

Näiteks `80 Mbit/s` vastab arvutuslikult `10 MB/s` andmehulgale. Tegelik failiülekanne võib protokollide lisainfo ja muude piirangute tõttu olla väiksem. Suur allalaadimiskiirus ei taga samaväärset üleslaadimiskiirust.

### Millistes ühikutes viivitust mõõdetakse?

Viivitust näidatakse tavaliselt **millisekundites ehk ms**. Üks sekund on 1000 millisekundit. Väiksem viivitus tähendab üldjuhul kiiremat reageerimist sama mõõtemeetodi ja sihtkoha korral.

Käsk `ping` mõõdab kontrollpäringu ja vastuse **edasi-tagasi aega** ehk **RTT-d** (*round-trip time*). Brauseri kiirusetest võib viivitust hinnata teistsuguse päringu abil. Seetõttu võrdle omavahel sama testi ja sama tüüpi näite. Kui test eristab jõudeoleku ja koormuse all mõõdetud viivitust, kasuta võrdluses üht ja sama välja.

!!! example "Näide: kiire allalaadimine, aga katkendlik videokõne"

    Suur fail tuleb kiiresti alla, kuid videokõnes tekivad pausid. Tasub uurida ka üleslaadimiskiirust, viivituse kõikumist ja võimalikke katkestusi. Üks allalaadimiskiiruse arv ei kirjelda kogu ühendust.

## 5. Kuidas teha võrreldavaid mõõtmisi?

Internetipõhine kiirusetest hindab ühendust sinu seadme ja testiteenuse vahel. Tulemust mõjutavad WiFi, internetipakett, võrgu koormus, teel olevad ühendused ning testiteenus. See ei ole ainult WiFi raadioside kiiruse mõõtmine.

Näiteks saad kasutada brauseris [Cloudflare’i kiirusetesti](https://speed.cloudflare.com/){ target="_blank" rel="noopener" }. Jälgi, milliseid välju kasutatav test tegelikult kuvab. Testi kasutamine ei nõua ruuteri haldusliidese avamist.

### Mõõtmise põhimõtted

1. **Kasuta sama seadet ja sama testiteenust.** Võrdle näiteks kogu aeg üht telefoni, mitte telefoni elutoas ja sülearvutit magamistoas.
2. **Kontrolli ühendust.** Veendu, et oled uuritavas WiFi võrgus. Telefonis võid mõõtmise ajaks mobiilse andmeside välja lülitada, et see tulemust ei asendaks. Lülita see hiljem vajaduse järgi tagasi sisse.
3. **Vali erinevad asukohad.** Näiteks pääsupunktiga samas ruumis, ühe seina taga ja kõige kaugemas kasutuskohas. Märgi takistused ning ligikaudne kaugus.
4. **Hoia muud tingimused võimalikult sarnased.** Tee mõõtmised üksteise järel samal ajavahemikul. Väldi enda seadmes suuri paralleelseid allalaadimisi. Kui test võimaldab serverit valida, kasuta sama serverit.
5. **Oota enne mõõtmist, kuni ühendus on paigas.** Ära tee testi kõndimise ajal. Hoia seadet sarnasel viisil ning märgi sagedusala, kui see on nähtav.
6. **Tee igas kohas kaks mõõtmist.** Kirjuta üles mõlemad tulemused ja mõõtühikud. Märgi ka katkestus või ebaõnnestunud test; ära asenda seda väljamõeldud arvuga.
7. **Pane kirja teadaolevad erinevused.** Näiteks oli teleris samal ajal video või seade vahetas sagedusala. VPN-ühendus võib tulemust mõjutada; kooli või töökoha nõutud kaitset selle ülesande jaoks ei muudeta.


### Õppenäide mõõtmistulemustest

Järgmised tulemused on koostatud õppimiseks. Need ei ole päris koduvõrgu mõõtmised. Igas lahtris on esimese ja teise katse tulemus. Kõik testid on tehtud sama seadme, sagedusala ja teenusega; viivitus on loetud sama tüüpi näidust.

| Koht | Takistused | Signaali hinnang | Alla Mbit/s | Üles Mbit/s | Viivitus ms |
| --- | --- | --- | --- | --- | --- |
| A — pääsupunktiga samas toas | Otsesel teel pole seina. | Tugev | 120 / 116 | 36 / 35 | 12 / 13 |
| B — kõrvaltoas | Üks sein. | Keskmine | 70 / 62 | 32 / 30 | 18 / 21 |
| C — kaugemas toas | Kaks seina ja suur metallkapp. | Nõrk | 8 / 5 | 4 / 3 | 54 / 80 |

Selles näites on kaugemas kohas nõrgem signaal, väiksemad kiirused ja suurem viivitus. See toetab oletust, et asukoht ja takistused mõjutavad ühendust. Kaks katset ei tõenda, et põhjuseks on ainult metallkapp või ainult kaugus.

Põhjendatud ettepanek võiks olla proovida pääsupunktile avatumat ja kasutusalale lähemat asukohta. Kui seda saab omaniku loal teha, tuleb pärast muutust mõõtmisi korrata.

!!! tip "Tee järeldus mõõdetud tingimuste kohta"

    „Selles kohas sain kahe testi ajal väiksema kiiruse” on põhjendatum kui „siin on internet alati halb”. Ühenduse omadused võivad sõltuda ajast, koormusest ja kasutatavast seadmest.

## 6. Kuidas ühendust parandada?

Alusta tähelepanekust ja seo ettepanek võimaliku põhjusega. Kõikide probleemide korral pole vaja osta uut seadet või kiiremat internetipaketti.

| Tähelepanek | Põhjendatud ettepanek | Kuidas tulemust kontrollida? |
| --- | --- | --- |
| Ühendus halveneb mitme seina taga. | Proovida pääsupunktile avatumat ja kesksemat asukohta. | Korrata mõõtmisi samades kasutuskohtades. |
| Suurt hoonet ei kata üks pääsupunkt. | Kavandada täiendav pääsupunkt ja selle ühendus olemasoleva võrguga. | Kontrollida levi ning ühenduse kvaliteeti varem nõrgas piirkonnas. |
| Ühes kindlas töökohas on vaja püsivat ühendust. | Kasutada võimaluse korral Etherneti kaablit. | Võrrelda ühenduse stabiilsust ja toimimist. |
| Ühendus aeglustub suure varunduse ajal. | Planeerida mahukas ülekanne sobivamale ajale. | Võrrelda tulemusi koormuse ajal ja pärast seda. |
| Ainult ühel seadmel on probleem. | Kontrollida selle ühendust ja tarkvara ning võrrelda teise seadmega samas kohas. | Vaadata, kas probleem järgneb seadmele või asukohale. |

### Pääsupunkt, levilaiendaja ja mesh

**Täiendav pääsupunkt** saab võrguühenduse näiteks kaabli kaudu. See võimaldab tuua WiFi teenuse vajalikule alale. **Levilaiendaja** (*range extender*) kasutab sageli olemasolevat WiFi ühendust ja edastab seda edasi. Kui selle enda ühendus on juba nõrk, ei paranda tugevam märk telefoni ekraanil internetiühenduse kõiki piiranguid.

**Mesh-võrk** ühendab mitu koostöötavat WiFi tugiseadet. Nendevaheline ühendus võib olla juhtmevaba või kaabliga. Ka sellises lahenduses tuleb tugiseadmete asukohta ja omavahelise ühenduse kvaliteeti arvestada.

!!! example "Ettepanek koos põhjendusega"

    „Pääsupunkt asub esiku kapis ja töölaud on kahe seina taga. Soovitan esmalt proovida pääsupunkti avatud kohta esiku keskel, sest nii jääks ühenduse teele vähem varjavaid esemeid. Pärast muutust võrdleksin töölaua juures tehtud mõõtmisi varasematega.”

Vaatlusülesandes piisab ettepanekust. Seadmete ümberpaigutamist, ostmist ega ruuteri seadistuse muutmist selleks ei nõuta.

## 7. WiFi turvaline kasutamine

### Võrgu parool ja haldusparool

**WiFi parooliga** lubatakse koduses paroolipõhises võrgus seadmel ühenduda. **Haldusparooliga** pääseb ruuteri või pääsupunkti seadistusi muutma. Neil on erinevad ülesanded ning haldusparooli ei ole vaja külalisele WiFi kasutamiseks anda.

Kasuta pikka ja raskesti äraarvatavat parooli, näiteks mitmest juhuslikust sõnast koosnevat paroolifraasi. Ära kasuta võrguparoolina nime, aadressi või lihtsat arvujada. Tehase üldine haldusparool tuleb asendada ning seadme tarkvara ajakohasena hoida.

### Krüpteerimine

**Krüpteerimine** muudab edastatavad andmed kõrvalisele lugejale arusaamatuks. Koduvõrgus kaitseb sobiv turvarežiim koos parooliga seadme ja pääsupunkti vahelist raadiosidet.

| Turvarežiim | Põhimõte koduse võrgu jaoks |
| --- | --- |
| WPA3 Personal | Eelistatud, kui vajalikud seadmed seda toetavad. |
| WPA2/WPA3 üleminekurežiim | Võimaldab ühes võrgus kasutada eri vanusega sobivaid seadmeid. |
| WPA2 Personal koos AES-iga | Võimalik valik vanemate seadmete jaoks, kui WPA3 kasutamine ei ole võimalik. |
| WEP, vana WPA või TKIP | Aegunud valikud, mida tuleb vältida. |

Kooli või ettevõtte võrk võib kasutada ühist parooli asendavat kasutajakonto või sertifikaadiga liitumist. Järgi sellisel juhul asutuse ühendusjuhist.

!!! tip "Võrgunime peitmine ei asenda kaitset"

    Peidetud SSID ei muuda võrku avastamatuks ega kaitse seda tugeva parooli ja sobiva krüpteerimise asemel.

Õigesti eraldatud **külalisvõrk** võimaldab külalistel kasutada internetti ilma ligipääsuta tööarvutitele või muudele sisevõrgu seadmetele. Ainult teise võrgunime lisamine ei taga veel eraldatust.

### Avalik WiFi

Kohviku või hotelli võrku kasutades kontrolli teenindajalt või ametlikult teabest õiget võrgunime. Tuttavat nime saab kasutada ka võõras võrk. Vajaduse korral on alternatiiv enda mobiilne andmeside.

Veebis eelista **HTTPS-ühendust**. See krüpteerib ühenduse brauseri ja veebiteenuse vahel ka avalikus WiFi võrgus. HTTPS ei kinnita siiski, et veebilehe omanik on aus: vaata ka domeeninime ning ära eira brauseri turvahoiatusi.

Avalik võrk ei tähenda automaatselt, et kõik paroolid on teistele nähtavad. Tähtsad on kasutatava teenuse krüpteerimine, õigesse teenusesse pöördumine ja seadme ajakohasus. Ära paigalda võrku pääsemiseks tundmatut tarkvara või turvasertifikaati; töö- ja kooliseadmel järgi asutuse juhiseid. Väldi tarbetut failijagamist ja ära jaga avalikult enda paroole.

## 8. Lihtne tõrkeotsingu järjekord

Enne seadete muutmist kogu infot. Võrdle seadmeid ja asukohti ning muuda korraga üht asja. Nii saad hinnata, mis tulemust mõjutas.

1. **Määratle probleem.** Kas puudub WiFi ühendus, internet või ainult ühe teenuse töö? Kas ühendus puudub või on aeglane?
2. **Kontrolli ulatust.** Kas probleem on ühes seadmes või kõigis? Kas ainult ühes ruumis?
3. **Kontrolli ühendust.** Kas WiFi on sisse lülitatud ja õige võrk valitud? Kaabelühendusel vaata kaablit ning pordimärgistusi.
4. **Võrdle teises kohas või teise seadmega.** Pääsupunkti lähedal kordamine aitab leviprobleemi uurida.
5. **Vaata ühenduse andmeid.** Kas seadmel on sobivad IP-seaded ja vaikelüüs? Vajadusel kasuta varasema materjali käske.
6. **Pane tulemus kirja ja küsi vajadusel abi.** Ütle, mis ei tööta, millal probleem algas ning mida kontrollisid. Võrguomaniku või halduri loata seadistusi ei muudeta.

| Vahend | Mida selle abil uurida? |
| --- | --- |
| `ipconfig` | Windowsi võrguadapteri IP-seaded ja vaikelüüs. |
| `ping` oma vaikelüüsi aadressile | Kas vaikelüüsilt saabub kontrollpäringu vastus. |
| `ping 1.1.1.1` | Kas sellelt internetis asuvalt aadressilt saabub kontrollpäringu vastus. |
| `nslookup example.com` | Kas kasutatud DNS-server tagastab küsitud nime kohta aadressi. |

Pingi vastuse puudumine võib tuleneda kontrollpäringute blokeerimisest. Õnnestunud DNS-päring ei tõenda veebilehe töötamist. Kasuta tähelepanekuid koos, nagu õppisid võrguühenduse uurimise materjalis.

## 9. Enesekontroll

Vasta kõigepealt ise. Seejärel ava vastus ja võrdle oma põhjendust.

### 1. Telefon näitab tugevat WiFi signaali. Kas internetiühendus peab olema kiire?

??? success "Vaata vastust"

    Ei. Signaal kirjeldab raadiosignaali vastuvõttu. Tulemust võivad piirata näiteks internetipakett, võrgu koormus või kasutatav teenus.

### 2. Kas 5 GHz on igas olukorras parem kui 2,4 GHz?

??? success "Vaata vastust"

    Ei. 5 GHz pakub sobivates tingimustes sageli suuremat kiirust, kuid takistused mõjutavad seda üldjuhul rohkem. Kaugemas ruumis võib 2,4 GHz ühendus olla kasutatavam.

### 3. Kas 100 Mbit/s allalaadimiskiirus ja 10 ms viivitus mõõdavad sama asja?

??? success "Vaata vastust"

    Ei. Esimene näitab andmete edastamise kiirust ja teine andmevahetusele kulunud aega. Hea kasutuskogemuse jaoks võivad mõlemad olla olulised.

### 4. Miks kasutada eri kohtades sama mõõteseadet ja sama testiteenust?

??? success "Vaata vastust"

    Nii vähendad seadmete ja testimeetodite erinevuste mõju. Tulemusi on lihtsam asukohaga seostada, kuigi kõiki keskkonna muutusi ei saa välistada.

### 5. Miks ei sobi mõõtmise ajal telefoni mobiilse andmeside kasutamine WiFi asemel?

??? success "Vaata vastust"

    Siis hindaks test mobiilset ühendust, mitte uuritavat WiFi kaudu toimivat ühendust. Tulemus ei kirjeldaks valitud mõõtmiskoha WiFi kasutamist.

### 6. Kas levilaiendaja tuleks panna kohta, kus olemasolevat WiFi ühendust enam üldse ei ole?

??? success "Vaata vastust"

    Tavaline WiFi kaudu edasi ühenduv levilaiendaja vajab ise piisavat ühendust olemasoleva võrguga. Täiesti levita kohas ei saa see puuduvat ühendust lihtsalt juurde tekitada.

### 7. Mille poolest erinevad WiFi parool ja ruuteri haldusparool?

??? success "Vaata vastust"

    WiFi parool võimaldab sobivasse võrku ühenduda. Haldusparool võimaldab seadme seadistusi muuta. Külalisele ei ole WiFi kasutamiseks vaja anda haldusparooli.

### 8. Kas HTTPS-aadress kinnitab, et veebileht kuulub ausale ettevõttele?

??? success "Vaata vastust"

    Ei. HTTPS kaitseb andmevahetust, kuid ka petuleht võib seda kasutada. Kontrolli domeeninime ja teenuse ehtsust.

### 9. Internet ei tööta üheski seadmes, kuid kõik on WiFi võrgus. Kas esimene samm on tehaseseadete taastamine?

??? success "Vaata vastust"

    Ei. Uuri ühiseid ühendusi, kaableid ja teenusepakkuja ühenduse olekut ning küsi vajadusel võrguomanikult abi. Tehaseseadete taastamine võib vajalikud seaded kustutada ja probleemi juurde tekitada.

## Kokkuvõte

- WiFi levi mõjutavad kaugus, takistused, asukoht, häired ja kasutatavad seadmed.
- 2,4 ja 5 GHz sobivus sõltub keskkonnast; suurem sageduse number ei taga paremat ühendust.
- Signaalitugevus, andmeedastuskiirus ja viivitus kirjeldavad erinevaid omadusi.
- Mõõda sama seadme ning sama testiga ja kirjuta üles ka mõõtmistingimused.
- Internetipõhine kiirusetest hindab kogu teed testiteenuseni, mitte ainult WiFi raadiosidet.
- Seo parendusettepanek tähelepanekuga ja kontrolli võimaliku muudatuse mõju kordusmõõtmisega.
- Kaitse võrku sobiva krüpteerimise ja tugevate paroolidega; erista WiFi parooli haldusparoolist.
- Tõrkeotsingul võrdle seadmeid ja asukohti ning kogu enne seadete muutmist infot.

### Mõtle õpitule

1. Milline ühenduse omadus mõjutaks sinu tulevases töös kõige enam: kiirus, viivitus või stabiilsus? Too konkreetne näide.
2. Kui peaksid uurima oma kodu WiFi ühendust, millised kolm mõõtmiskohta valiksid ja miks?
3. Sõnasta üks põhjendatud parendusettepanek ning kirjelda, kuidas hindaksid selle mõju.

## Teema sõnastik

| Mõiste | Selgitus |
| --- | --- |
| Pääsupunkt (*access point*) | Seade või seadme osa, mis võimaldab WiFi ühendust. |
| Leviala (*coverage area*) | Piirkond, kus ühendus on kasutamiseks piisavalt hea. |
| Sagedusala (*frequency band*) | Raadiosageduste piirkond, mida ühendus kasutab, näiteks 2,4 või 5 GHz. |
| Kanal (*channel*) | Osa sagedusalast, mida raadiosideks kasutatakse. |
| SSID (*service set identifier*) | WiFi võrgu nimi. |
| Signaalitugevus (*signal strength*) | Vastuvõetava raadiosignaali tugevus. |
| dBm | Võimsuse väljendamise ühik; WiFi ühenduse andmetes kasutatakse seda ka vastuvõetava signaali tugevuse näitamiseks. |
| Allalaadimiskiirus (*download speed*) | Andmete liikumise kiirus internetist seadmesse. |
| Üleslaadimiskiirus (*upload speed*) | Andmete liikumise kiirus seadmest internetti. |
| Mbit/s; Mbps | Megabitti sekundis, andmeedastuskiiruse ühik. |
| Viivitus (*latency*) | Andmevahetusele kuluv aeg vastavalt mõõtemeetodile. |
| Edasi-tagasi aeg; RTT (*round-trip time*) | Päringu saatmisest selle vastuse saabumiseni kulunud aeg. |
| Viivituse kõikumine (*jitter*) | Viivituse muutumine mõõtmiste vahel. |
| Millisekund; ms | Üks tuhandik sekundist. |
| Levilaiendaja (*range extender*) | Seade, mis laiendab olemasoleva võrgu leviala, sageli WiFi ühendust edasi edastades. |
| Mesh-võrk (*mesh network*) | Mitme koostöötava WiFi tugiseadmega lahendus. |
| Krüpteerimine (*encryption*) | Andmete muutmine kujule, mida kõrvaline isik ei saa sobiva võtmeta lugeda. |
| WPA2 ja WPA3 | WiFi autentimise ja krüpteerimise turvaprotokollid. |
| Külalisvõrk (*guest network*) | Külalistele mõeldud võrk; ligipääsu sisevõrgule piiratakse vastavate seadistustega. |

## Allikad ja lisalugemine

1. [Microsoft: Wi-Fi and your home layout](https://support.microsoft.com/en-us/windows/experience/connectivity-networking/wi-fi-and-your-home-layout){ target="_blank" rel="noopener" }
2. [TP-Link: How to Improve Your Wi-Fi Signal and Range](https://www.tp-link.com/us/support/faq/468/){ target="_blank" rel="noopener" }
3. [FTC: How To Secure Your Home Wi-Fi Network](https://consumer.ftc.gov/articles/how-secure-your-home-wi-fi-network){ target="_blank" rel="noopener" }
4. [Cisco: Troubleshooting a Traditional Cisco Wireless Network](https://www.cisco.com/c/en/us/support/docs/smb/wireless/CB-Wireless-Mesh/1902-tz-Troubleshooting-Traditional-Cisco-Wireless-Network.html){ target="_blank" rel="noopener" }

---
*Õppematerjali koostaja: Priit Paap, 2026*
