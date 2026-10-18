# WiFi levi ja ühenduse kiiruse uurimine telefoniga

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
    - seostada tulemusi kauguse, takistuste ja muude mõõtmistingimustega.

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
    - Band, ehk sagedusala (2,4 või 5 GHz);
    - kui näed dBm-näitu, kirjuta see üles;
3. Mine **"Speed"** lehele ja leia interneti kiirusetesti nupp **Start Speed Test**.
4. Käivita **esimene test** ja oota selle lõpuni.
5. Kirjuta tekstifaili pealkirja **„Koht A — katse 1”** alla:
    - allalaadimiskiirus ehk **Download**, ühik **Mbps ehk Mbit/s**;
    - üleslaadimiskiirus ehk **Upload**, ühik **Mbps ehk Mbit/s**;
    - viivitus ehk **Latency**, ühik **ms**.
6. Käivita samas kohas **teine test**. Oota selle lõpuni ja kirjuta tulemused pealkirja **„Koht A — katse 2”** alla.

!!! info "Info"
    
    Iga võrgu puhul ei pruugi näha kõike. Puuduva näidu tõttu ei pea otsima teist rakendust. Lisa selgitus, kui mõnda näitu valitud võrk ei kuvanud.


### Mõõtmised kohas B

7. Mine **kohta B** ja oota, et ühendus stabiliseeruks.
8. Teosta samad toimingudmis esimese koha juures ehk kirjuta üles signaalinäit, sagedus ja kahe kiirustesti tulemus.
    
### Mõõtmised kohas C

9. Mine **kohta C** ja oota, et ühendus stabiliseeruks.
10. Teosta samad toimingudmis esimese koha juures ehk kirjuta üles signaalinäit, sagedus ja kahe kiirustesti tulemus.

### Kontrolli, kas kõik tulemused on kirjas

11. Veendu, et tekstifailis on **kolme koha kirjeldused, signaalinäidud ja kuue testi tulemused**.
12. Lisa tähelepanekud: kas ühendus katkes, sagedusala või pääsupunkt vahetus või mõni test ebaõnnestus.
13. Lülita mobiilne andmeside tagasi sisse, kui seda tavaliselt kasutad, paigaldatud rakenduse võib soovi korral eemaldada.

## 5. Töö esitamine

Tulemused esita tekstidokumendina (n: Google docs või MS Word) ehk esita fail Moodlesse.

Veendu, et dokumendis on olemas: 

- **Õppija nimi**
- **Mõõtmise kuupäev ja ajavahemik**
- **Telefoni mudel ning Androidi või iOS**
- **Mõõtmiskohtade kirjeldus**
- **Kajastatud on kõik mõõtmistulemused koos vajalike andmetega.**

*Mõõtmistulemuste näidis. Järgnev ei pea, aga võib olla esitatud tabelina. Tähtis, et kõik vajalik oleks kirjas.*

| Koht | Katse | Alla Mbit/s | Üles Mbit/s | Signaalitugevus | Viivitus ms | Sagedus | Märkused | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| A | 1 | | | | | | |
| A | 2 | | | | | | |
| B | 1 | | | | | | |
| B | 2 | | | | | | |
| C | 1 | | | | | | |
| C | 2 | | | | | | |


**Lisa veel faili kirjalikud vastused järgmistele küsimustele:**

1. Millises kohas toimis ühendus kõige paremini? Millest sa seda järeldad?
2. Millises kohas toimis ühendus kõige halvemini? Millest sa seda järeldad?
3. Kas signaalinäit, kiirus ja viivitus muutusid samas suunas? 
4. Millised kauguse, takistuste või võrgu kasutamisega seotud asjaolud võisid tulemusi mõjutada? 


## 6. Hindamine

Töö on arvestatud, kui:

- uuritud on kolme kohta ning igas kohas on tehtud kaks mõõtmiskatset;
- mõõtmiskohad, kasutatud vahendid ja tingimused on arusaadavalt kirjeldatud;
- tulemused on esitatud arusaadavalt õigete mõõtühikutega;
- võrdluses kasutatakse enda andmeid;
- vastatud on neljale kokkuvõtvale küsimusele.

---
*Ülesande koostaja: Priit Paap, 2026*
