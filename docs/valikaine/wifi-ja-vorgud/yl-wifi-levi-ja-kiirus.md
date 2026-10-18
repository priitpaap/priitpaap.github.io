# Ülesanne 2 — WiFi levi ja ühenduse kiiruse uurimine telefoniga

## Sissejuhatus

Uuri oma Android-telefoni või iPhone’iga, kuidas muutub WiFi ühendus eri kohtades. Mõõda internetiühenduse kiirust, võrdle tulemusi ja paku välja võimalusi ühenduse parandamiseks. Ülesanne tehakse individuaalselt kodus, koolis või muus kohas, kus sul on lubatud WiFi võrku kasutada.

**Õppevorm:** iseseisev e-õpe.  
**Hindamine:** arvestatud / mittearvestatud.  
**Esitamine:** Moodle’i ülesandesse seal märgitud tähtajaks.

!!! info "Ülesande eesmärk"

    Ülesande läbimisel oskad:

    - kasutada telefonis ühenduse kiiruse mõõtmise rakendust;
    - võrrelda WiFi levi ja ühenduse toimimist eri asukohtades;
    - eristada signaalitugevust, alla- ja üleslaadimiskiirust ning viivitust;
    - seostada tulemusi kauguse, takistuste ja muude mõõtmistingimustega;
    - esitada põhjendatud parendusettepanekuid ning valida ühendusprobleemi korral kontrollisamme.

## 1. Vahendid ja rakendused

Vajad oma telefoni, ligipääsu lubatud WiFi võrgule ning kohta tulemuste kirjapanemiseks. 

### Soovitatav rakendus mõlemale telefonile

Kasuta **Ubiquiti WiFiman** rakendust. See on tasuta ning võimaldab mõõta alla- ja üleslaadimiskiirust ja vaadata testi kuvatavat viivitust.

| Telefon | Rakendus ja allalaadimine | Kuidas hinnata levi? |
| --- | --- | --- |
| Android | [WiFiman Google Plays](https://play.google.com/store/apps/details?id=com.ubnt.usurvey){ target="_blank" rel="noopener" } | Vaata võimaluse korral rakenduse võrguinfost signaalitugevust ehk RSSI-d dBm-des. Selle puudumisel kasuta telefoni WiFi kaari ja ühenduse püsimise vaatlust. |
| Apple iPhone ehk iOS | [Ubiquiti WiFiman App Store’is](https://apps.apple.com/us/app/ubiquiti-wifiman/id1385561119){ target="_blank" rel="noopener" } | Vaata telefoni WiFi kaari ja ühenduse püsimist. Täpne dBm-näit ei ole nõutud. |

Veendu poes, et rakenduse arendaja on **Ubiquiti Inc.** Ülesande jaoks kasutatakse interneti kiirusetesti, mitte UniFi seadmete haldamist või Teleport VPN-i.

!!! info "Androidi ja iPhone’i võimalused võivad erineda"

    WiFimanil on ka võimalusi, mis vajavad UniFi võrguseadmeid või lisariistvara. Eelkõige iPhone’is võib tavalises koduvõrgus puududa täpne signaaliinfo. Sul ei ole vaja selliseid funktsioone avada ega lisaseadmeid hankida.

    dBm-näidu puudumine ei vähenda hinnet. Sel juhul märgi WiFi kaarte arv ja nende maksimaalne arv, näiteks „2 kaart 3-st”, või kirjuta „näit pole nähtav”. Lisa, kas ühendus püsis, katkes või puudus.

## 2. Ettevalmistus

Ülesandes uurid WiFi ühendust **kolmes kohas** ja teed igas kohas **kaks kiirusetesti**. Kokku tuleb kuus testi.

1. Paigalda telefoni **WiFiman**.
2. Loo tekstifail, kuhu hakkad tulemusi kirjutama. Lisa faili algusesse oma nimi, telefoni mudel, kasutatav rakendus ja mõõtmise kuupäev.
3. Ühenda telefon uuritava WiFi võrguga.
4. Lülita mõõtmiste ajaks **mobiilne andmeside välja**, et telefon ei asendaks WiFi ühendust mobiilsega. WiFi peab jääma sisse.
5. Ära lae samal ajal telefoniga midagi alla.

Kasuta kõigi mõõtmiste ajal sama telefoni ja sama rakendust. Hoia telefoni sarnasel kõrgusel ning sarnases asendis.

Kui rakendus küsib WiFi info vaatamiseks õigusi, võivad Androidi versioonist sõltuvalt olla vajalikud läheduses olevate seadmete või asukoha õigused. Kui sa neid anda ei saa, kasuta telefonis nähtavat levihinnangut.

## 3. Vali ja kirjelda mõõtmiskohad

Enne testide alustamist vali kolm kohta:

- **A:** pääsupunktiga samas ruumis, mõne meetri kaugusel;
- **B:** näiteks kõrvaltoas või ühe seina taga;
- **C:** kaugemas kasutuskohas või kohas, kus ühendus tundub nõrgem.

Kui ruumijaotus on teistsugune, vali kolm mõistlikult erinevat kohta.

Kirjuta tekstifaili:

1. Pääsupunkti või WiFi ruuteri ligikaudne asukoht.
2. Kohtade A, B ja C kirjeldused.
3. Iga koha ligikaudne kaugus pääsupunktist.
4. Pääsupunkti ja iga mõõtmiskoha vahele jäävad seinad või muud takistused.

Näiteks: **Koht B:** magamistoas, pääsupunktist umbes 7 meetri kaugusel. Vahele jäävad üks sein ja riidekapp.

Kui pääsupunkti asukoht pole teada, märgi see tekstis ning kirjelda mõõtmiskohtade asukohti üksteise suhtes.

## 4. Tee mõõtmised järjekorras A, B ja C

### Mõõtmised kohas A

1. Mine **kohta A**, jää paigale ja oota hetk, et ühendus stabiliseeruks.
2. Ava **WiFiman**. Vaata WiFi signaalinäitu **"Signal"** lehelt (rakenduse akna all on vasatav nupp) ja kirjuta see tekstifaili koha A juurde:
    - Band, ehk sagedusala (2,4 vüu 5 GHz);
    - kui näed dBm-näitu, kirjuta see üles;
    - muul juhul märgi WiFi kaarte arv, näiteks „2 kaart 3-st”;
    - kui signaalinäitu pole näha, märgi „ei kuvata”.
3. Mine **"Speed"** lehele ja leia interneti kiirusetesti nupp **Start Speed Test**.
4. Käivita **esimene test** ja oota selle lõpuni.
5. Kirjuta tekstifaili pealkirja **„Koht A — katse 1”** alla:
    - allalaadimiskiirus ehk **Download**, ühik **Mbps ehk Mbit/s**;
    - üleslaadimiskiirus ehk **Upload**, ühik **Mbps ehk Mbit/s**;
    - viivitus ehk **Latency**, ühik **ms**.
6. Käivita samas kohas **teine test**. Oota selle lõpuni ja kirjuta tulemused pealkirja **„Koht A — katse 2”** alla.

### Mõõtmised kohas B

7. Mine **kohta B** ja oota, et ühendus stabiliseeruks.
8. Teosta samad toimingudmis esimese koha juures ehk kirjuta üles signaalinäit, sagedus ja kahe kiirustesti tulemus.
    
### Mõõtmised kohas C

9. Mine **kohta C** ja oota, et ühendus stabiliseeruks.
10. Teosta samad toimingudmis esimese koha juures ehk kirjuta üles signaalinäit, sagedus ja kahe kiirustesti tulemus.

### Kontrolli, kas kõik tulemused on kirjas

11. Veendu, et tekstifailis on **kolme koha kirjeldused, signaalinäidud ja kuue testi tulemused**.
12. Lisa tähelepanekud: kas ühendus katkes, sagedusala või pääsupunkt vahetus või mõni test ebaõnnestus.
13. Lülita mobiilne andmeside tagasi sisse, kui seda tavaliselt kasutad, paigaldstud rakenduse võib soovi korral eemaldada.

## 5. Töölehe täidetav osa

Tulemused võid esitada tekstidokumendina (n: Google docs või MS Word). Kasuta järgmisi alapealkirju ja tabeleid.

### Üldandmed

- **Õppija nimi:**
- **Mõõtmise kuupäev ja ajavahemik:**
- **Telefoni mudel ning Androidi või iOS-i versioon:**
- **Kasutatud rakendus või brauseritest:**
- **Võrgu tinglik nimetus:** näiteks „Koduvõrk”; tegelikku võrgunime pole vaja esitada.
- **Signaali hindamise viis:** dBm, kaarte arv või ühenduse püsimise kirjeldus.
- **Mõõdetud viivituse välja nimi:** või „ei kuvata”.
- **Teadaolevad muud tingimused:** näiteks samaaegne video vaatamine, VPN või mitu pääsupunkti.

### Mõõtmiskohad ja levi

| Koht | Asukoha kirjeldus ja kaugus pääsupunktist | Takistused | Signaalinäit või kaarte arv | Kas ühendus püsis? |
| --- | --- | --- | --- | --- |
| A | | | | |
| B | | | | |
| C | | | | |

### Kiirusetestide tulemused

| Koht | Katse | Alla Mbit/s | Üles Mbit/s | Viivitus ms | Märkused |
| --- | --- | --- | --- | --- | --- |
| A | 1 | | | | |
| A | 2 | | | | |
| B | 1 | | | | |
| B | 2 | | | | |
| C | 1 | | | | |
| C | 2 | | | | |

### Tulemusi kinnitavad pildid

Lisa **vähemalt üks kiirusetesti tulemust näitav ekraanipilt igast mõõtmiskohast** ehk kokku vähemalt kolm pilti. Märgi pildi juurde koht ja katse number. Kui mõnes kohas ei saa testi teha, lisa võimalusel pilt ebaõnnestumisest ning kirjelda juhtunut.

Lõika piltidelt välja paroolid, avalik IP-aadress, täpne asukoht ja muu ülesande jaoks mittevajalik isiklik info. WiFi signaalinäit ja kiirusetesti tulemus võivad olla eri vaadetes; soovi korral võid lisada ka levihinnangu pildi.

## 6. Võrdle tulemusi ja tee ettepanekud

**Orienteeruv aeg: koos järgmise peatüki juhtumitega 45 minutit.**

Vasta oma tulemuste põhjal:

1. Millises kohas toimis ühendus kõige paremini? Millised näitajad seda toetavad?
2. Millises kohas toimis ühendus kõige halvemini? Kui erinevus oli väike, ütle seda.
3. Kas signaalinäit, kiirus ja viivitus muutusid ühesuguses suunas? Too oma tabelist vähemalt kaks konkreetset näidet.
4. Millised kauguse, takistuste või võrgu kasutamisega seotud asjaolud võisid tulemusi mõjutada? Erista tähelepanekut oletusest.
5. Esita **kaks põhjendatud parendusettepanekut**. Selgita igaühe puhul, millist probleemi see võiks leevendada ja kuidas mõju kontrolliksid. Muudatusi ei pea päriselt tegema.

Kui ühendus oli kõigis kohtades hea, võid pakkuda, kuidas säilitada hea ühendus või mida teha kasutusala laienemisel. Väiksem mõõdetud kiirus ei tähenda automaatselt, et ühendus oleks igapäevaseks kasutuseks ebapiisav.

!!! example "Põhjenduse näide"

    „Kohas C oli esimese katse allalaadimiskiirus 8 Mbit/s ja teises 6 Mbit/s, kohas A vastavalt 90 ja 86 Mbit/s. Kohas C oli telefonil ka nõrgem WiFi näit. Vahele jäi kaks seina. Soovitaksin proovida pääsupunktile avatumat ja kesksemat asukohta ning korrata mõõtmisi samades kohtades.”

    Kasuta vastuses enda mõõtmistulemusi. Näites olevad arvud ei ole sinu töö andmed.

## 7. Kolm lühikest ühendusprobleemi

Kirjuta iga juhtumi kohta **kolm kontrollisammu loogilises järjekorras**. Põhjenda lühidalt iga sammu valikut. Sa ei pea vigu enda võrgus tekitama ega päriselt parandama.

### Juhtum 1 — WiFi on olemas, internet puudub

Telefon ja arvuti on WiFi võrgus, kuid veebilehed ei avane kummaski seadmes. Signaal on tugev. Mida kontrolliksid?

### Juhtum 2 — ühendus katkeb ühes toas

Pääsupunktiga samas ruumis videokõne töötab. Kaugemas toas hakkab heli katkema ja vahel kaob WiFi ühendus. Mida kontrolliksid?

### Juhtum 3 — probleem on ainult ühes telefonis

Samas kohas töötab kaaslase telefonis ühendus, sinu telefonis mitte. Mõlemad peaksid kasutama sama WiFi võrku. Mida kontrolliksid?

## 8. Esitamine ja hindamine

**Orienteeruv aeg: 30 minutit tulemuste kontrollimiseks ja esitamiseks.**

Esita Moodle’isse **üks dokument**, milles on:

1. üldandmed ja mõõtmistingimuste kirjeldus;
2. skeem koos pääsupunkti ning kolme mõõtmiskohaga;
3. täidetud levitabel ja kuue kiirusetesti tulemused;
4. vähemalt kolm mõõtmiskohtadega seostatud ekraanipilti;
5. tulemuste võrdlus ja kaks põhjendatud ettepanekut;
6. kolme ühendusprobleemi kontrollisammud.

Paiguta pildid dokumendi sisse ja anna failile selge nimi, näiteks `eesnimi_perenimi_wifi_uuring.pdf`. PDF-iks teisendamine ei ole kohustuslik. Eraldi esitlust või videot ei nõuta.

!!! success "Töö on arvestatud, kui"

    - uuritud on kolme kohta ning igas kohas on tehtud kaks mõõtmiskatset või selgitatud nende ebaõnnestumist;
    - mõõtmiskohad, kasutatud vahend ja tingimused on arusaadavalt kirjeldatud;
    - tulemused on esitatud õigete mõõtühikutega ja neid toetavad pildid;
    - võrdluses kasutatakse enda andmeid ning ettepanekud on põhjendatud;
    - kõigi kolme juhtumi kohta on esitatud loogilised kontrollisammud;
    - puuduvate näitude või tehniliste piirangute kohta on antud aus selgitus.

    Hinnatakse uurimise ja põhjendamise oskust. Aeglane ühendus, hea levi kõigis kohtades või telefoni piiratud näidud ei vähenda hinnet. Vajaduse korral saad õpetaja tagasiside põhjal tööd täiendada.

Pärast mõõtmisi lülita mobiilne andmeside tagasi sisse, kui seda tavaliselt kasutad.

## 9. Kui enda telefoni või WiFi võrku kasutada ei saa

Kui sul ei ole sobivat telefoni, mõõtmiseks lubatud WiFi võrku või piisavat andmemahtu, kasuta alternatiivi.

- Kui võimalik, tee individuaalne uuring omaniku loal laenatud telefoniga. Märgi vahend töösse.
- Kui mõõtmisi teha ei saa, kasuta allpool olevat ruumiplaani ja mõõteandmestikku. Koosta nende põhjal tulemuste võrdlus ja kaks ettepanekut ning vasta kõigile kolmele ühendusprobleemile. Märgi selgelt, et analüüsisid etteantud andmeid. Sel juhul asendavad andmestik ja selle skeem sinu mõõtmised ning ekraanipildid.

### Alternatiivne andmestik

Järgmine olukord ja tulemused on **väljamõeldud õppeandmed**. Neid ei esitata enda tehtud mõõtmistena.

Korteri ruumide lihtsustatud paigutus on järgmine. Pääsupunkt paikneb esikus madalal riiulil, selle kõrval on metallist nagi. Ruumide vahel on seinad ja uksed.

| Korteri vasak pool | Korteri parem pool |
| --- | --- |
| Esik — pääsupunkt ja mõõtmiskoht A. | Elutuba — mõõtmiskoht B. |
| Magamistuba — siin ei mõõdetud. | Töötuba — mõõtmiskoht C. |

Kõik kuus katset on tehtud sama Android-telefoni ja sama kiirusetestiga 15 minuti jooksul. Kasutusel oli 5 GHz ühendus sama pääsupunktiga, mobiilne andmeside oli välja lülitatud. Internetipaketi kiiruse ülempiir ja teiste seadmete andmekasutus ei ole teada. Viivituse näit on kõigis katsetes mõõdetud jõudeolekus.

| Koht | Kaugus ja takistused | Signaal dBm | Katse | Alla Mbit/s | Üles Mbit/s | Viivitus ms |
| --- | --- | --- | --- | --- | --- | --- |
| A | 3 m, samas ruumis. | −45 | 1 | 94 | 39 | 12 |
| A | 3 m, samas ruumis. | −46 | 2 | 91 | 38 | 13 |
| B | 7 m, üks sein. | −63 | 1 | 58 | 25 | 19 |
| B | 7 m, üks sein. | −64 | 2 | 52 | 23 | 22 |
| C | 12 m, kaks seina ja suur kapp ühenduse teel. | −79 | 1 | 9 | 4 | 56 |
| C | 12 m, kaks seina ja suur kapp ühenduse teel. | −81 | 2 | 5 | 2 | 85 |

Töötoa ühendus katkes enne teist katset ühe korra ja see taastati sama võrku ühendudes. Kohtades A ja B katkestusi ei täheldatud. Alternatiivse töö üldandmetes märgi telefoni mudeli ja mõõtmiskuupäeva asemel „etteantud õppeandmed”.

## Viited

- [Õppematerjal: WiFi levi, ühenduse kiirus ja turvaline kasutamine](04_wifi_levi_kiirus_ja_turvalisus.md).
- [Ubiquiti: Using WiFiman](https://help.ui.com/hc/en-us/articles/205204150-Using-WiFiman){ target="_blank" rel="noopener" } — kiirusetest ja lisafunktsioonide eeldused.
- [WiFiman Androidile](https://play.google.com/store/apps/details?id=com.ubnt.usurvey){ target="_blank" rel="noopener" } ja [WiFiman iPhone’ile](https://apps.apple.com/us/app/ubiquiti-wifiman/id1385561119){ target="_blank" rel="noopener" }.

---
*Ülesande koostaja: Priit Paap, 2026*
