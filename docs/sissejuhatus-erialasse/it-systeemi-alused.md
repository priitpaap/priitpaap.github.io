# IT süsteemi alused

Kasutajast rakenduseni: mõista seadme, tarkvara, konto ja võrgu rolli ning kliendi ja serveri koostööd.

## Teema tulemusena oskad

- Kirjeldada IT-süsteemi kui seotud osade tervikut.
- Selgitada kasutaja, klientseadme, operatsioonisüsteemi ja rakenduse rolli.
- Eristada autentimist ja ligipääsuõiguste kontrolli.
- Selgitada võrgu ning kliendi ja serveri ülesandeid.

Alustame kasutaja tegevusest: mida peab süsteem tegema, et õppija saaks Moodle’is materjali avada? Servereid, pilve ja veaotsingut vaatame põhjalikumalt teises osas.

## IT-süsteem on koostöötav tervik

IT-süsteem koosneb kasutajatest, seadmetest, tarkvarast, võrgust, teenustest ja andmetest. Selle eesmärk on toetada mõnd tegevust, näiteks õppimist, dokumentide koostamist või kaupade müüki.

Sülearvuti võib olla töökorras, kuid õppija ei saa kursuse materjali avada, kui vajalik teenus ei vasta, võrguühendus puudub või kursusele ligipääsu pole.

!!! note "Pea meeles"

    **Alusta vajadusest:** kes tahab mida teha? Seejärel vaata, millised osad peavad selle tegevuse jaoks koostööd tegema.

## IT-süsteemi lihtne mudel

### Kasutaja ja klientseade

Õppija kasutab sülearvutit. Seadmes töötavad operatsioonisüsteem ja brauser.

### Võrguühendus

Ühendus võimaldab brauseril teenusega suhelda. Vajadusel läbib liiklus ka Interneti.

### Teenus ja andmed

Moodle’i rakendus töötleb päringuid ning kasutab kursuseandmeid ja faile.

Need on rollid terviksüsteemis, mitte kõigi andmete kohustuslik järjestikune teekond. Kohalik tekstiredaktor saab töötada ka ilma võrguta. Veebiteenus võib kasutada mitut serverit.

| Komponent | Põhiküsimus | Näide |
| --- | --- | --- |
| Kasutaja | Kes ja milleks süsteemi kasutab? | Õppija avab materjali |
| Klientseade | Millise seadme kaudu? | Sülearvuti või telefon |
| Operatsioonisüsteem | Mis haldab seadme ressursse? | Windows, Linux, Android |
| Rakendus | Milline tarkvara täidab ülesannet? | Brauser, tekstiredaktor, Moodle |
| Võrk | Kuidas osad suhtlevad? | Kohtvõrk ja Internetiühendus |
| Server või teenus | Mis pakub kliendile vajalikku teenust? | Veebiserver või pilvepost |
| Andmed | Millist infot kasutatakse ja säilitatakse? | Kursused, hinded ja failid |

## Konto, autentimine ja õigused

**Kasutajakonto** esindab süsteemis inimese või teenuse identiteeti. Isiklikud kontod aitavad eristada kasutajaid ja siduda tegevusi õige inimesega.

### Autentimine: kes sa oled?

Süsteem kontrollib, kas oled see, kellena end esitled. Näiteks kasutad parooli ja täiendavat kinnitust.

### Autoriseerimine: mida tohid teha?

Süsteem kontrollib õigusi: kas võid kursust vaadata, faili muuta või teisi kasutajaid hallata?

**Roll** koondab sarnase ülesandega kasutajate õigusi. Moodle’i õppija ja õpetaja võivad mõlemad sisse logida, kuid neil on erinevad võimalused.

!!! note "Pea meeles"

    **Edukas sisselogimine ei anna kõiki õigusi.** Õppija võib olla sisse logitud, kuid tal võib puududa konkreetsele kursusele juurdepääs.

??? info "Näide: konto või kursusele ligipääs?"

    Kui sisselogimine õnnestub, kuid kursus ei ole kättesaadav, kontrolli näiteks kursusele registreerimist ja rolli. Kui sisselogimine ise ebaõnnestub, kontrolli konto olekut ning kasutatud autentimisviisi. Need on lähtekohad, mitte lõplik diagnoos.

## Klientseade

Klientseade on seade, mille kaudu kasutaja teenust kasutab: lauaarvuti, sülearvuti, telefon, tahvelarvuti või iseteenindusterminal.

**Õhuke klient** on seade, mis kasutab suure osa töö tegemiseks serveris töötavat keskkonda. Tavalises arvutis võivad osa rakendusi töötada kohapeal ja osa olla kasutatavad võrgu kaudu.

??? info "Kas printimisterminal on klientseade?"

    Jah, kui kasutaja saab selle kaudu printimisteenusele ligi ja valida oma töö. Klientseadet määrab tema roll, mitte see, kas tal on tavapärase arvuti välimus.

Protsessori, mälu ja teiste riistvaraosade valikut käsitleme eraldi teemas. Praegu on oluline seadme roll süsteemis.

## Operatsioonisüsteem

Operatsioonisüsteem ehk OS haldab riistvararessursse ja pakub rakendustele võimalusi neid kasutada. Näited on Windows, Linux, macOS, Android ja iOS.

- Jagab rakenduste vahel protsessori aega ja mälu.
- Haldab faile ning ligipääsu seadmetele.
- Käivitab rakendusi ja haldab nende tööd.
- Pakub võrgusuhtluse võimalusi ning rakendab turvameetmeid.

!!! note "Pea meeles"

    **Lihtne näide:** tekstiredaktor tahab faili salvestada. Operatsioonisüsteem vahendab ligipääsu salvestusseadmele ja kontrollib vajalikke kohalikke õigusi.

Operatsioonisüsteemi konto ja veebiteenuse konto võivad olla erinevad. Arvutisse sisselogimine ei tähenda automaatselt, et igasse veebiteenusesse on samuti sisse logitud.

## Rakendus: kus see töötab ja kuidas seda pakutakse?

Rakendus on tarkvara konkreetse ülesande tegemiseks. Näiteks VLC mängib videot, tekstiredaktor aitab dokumenti koostada ning Moodle toetab õppimist.

| Mõiste | Mida see kirjeldab? | Näide |
| --- | --- | --- |
| Kohalik rakendus | Töötab kasutaja seadmes; osa funktsioone võib kasutada võrku | VLC või paigaldatud tekstiredaktor |
| Veebirakendus | Kasutatakse brauseriga; töö jaguneb brauseri ja serveri vahel | Moodle, e-pood või veebipost |
| SaaS: tarkvara teenusena | Teenusepakkuja haldab rakendust ja pakub seda kasutamiseks | Microsoft 365 või teenusena pakutav õppekeskkond |

**Need pole kolm üksteist välistavat liiki.** Veebirakendust võib pakkuda SaaS-teenusena. SaaS-il võib olla ka paigaldatav klientrakendus.

??? info "Kuidas kirjeldada Moodle’it?"

    Moodle on veebirakendus: õppija kasutab seda brauseris. Kool võib Moodle’it ise hallata või kasutada teenusepakkuja hallatavat lahendust. SaaS kirjeldab teenuse pakkumise ja haldamise viisi, mitte ainult brauseris avamist.

Brauser on klientseadmes töötav rakendus. Veebirakenduse kasutamisel suhtleb see serveritega. Seepärast ei taga korras brauser veel, et veebiteenus on kättesaadav.

## Võrk ühendab süsteemi osad

Võrk võimaldab seadmetel andmeid vahetada. Kohtvõrgus võivad suhelda arvutid, printerid ja kohalikud serverid. Internet ühendab võrke ning võimaldab kasutada väljaspool organisatsiooni asuvaid teenuseid.

| Mõiste | Roll |
| --- | --- |
| LAN ehk kohtvõrk | Ühendab seadmed piiratud alal, näiteks koolis |
| Wi-Fi | Võimaldab traadita ligipääsu võrgule |
| Kommutaator (switch) | Ühendab seadmeid kohalikus võrgus |
| Ruuter | Edastab liiklust erinevate võrkude vahel |
| Tulemüür | Lubab või piirab võrguliiklust reeglite järgi |

!!! note "Pea meeles"

    **Wi-Fi-ühendus ja Internetiühendus on eri asjad.** Seade võib olla Wi-Fi-ga ühendatud, kuid Internetile ligipääs puudub. Kohalik printer võib samal ajal edasi töötada.

Kui teenus on kohalikus võrgus, ei pruugi selle kasutamiseks Internetti vaja olla. Kõik sõltub konkreetse teenuse asukohast ja sõltuvustest.

## Klient-server mudel

**Klient** algatab päringu. **Server** pakub teenust, töötleb päringu ja saadab vastuse. Veebilehe avamisel on brauser kliendi rollis ning veebiserver vastab päringule.

1. Õppija valib brauseris kursuse materjali.
2. Brauser saadab teenusele päringu.
3. Serveris töötav rakendus leiab vajaliku info ja kontrollib ligipääsu.
4. Vastus jõuab brauserisse ning brauser kuvab tulemuse.

Server võib omakorda olla klient: näiteks Moodle’i rakendus küsib andmeid andmebaasiserverilt. Üks seade võib täita mitut rolli ning üks teenus võib kasutada mitut serverit.

!!! note "Pea meeles"

    **Roll on olulisem kui välimus.** Server ei pea olema suur kapp. Serveritarkvara võib töötada füüsilises arvutis või virtuaalmasinas.

## Kokkuvõte

- IT-süsteemi osad peavad kasutaja eesmärgi nimel koostööd tegema.
- Klientseade annab ligipääsu, OS haldab ressursse ja rakendus täidab ülesannet.
- Autentimine kontrollib identiteeti, autoriseerimine õigusi.
- Veebirakendus ja SaaS kirjeldavad eri vaatenurki ning võivad kattuda.
- Võrk ühendab osad; klient küsib ja server pakub teenust.

### Mõtle õpitule

Miks ei piisa Moodle’i kasutamiseks ainult töökorras arvutist? Milline rakendus töötab sinu seadmes ja millist teenust see kasutab? Too näide olukorrast, kus konto töötab, kuid õigusi pole.

## Teema sõnastik

| Mõiste | Selgitus |
| --- | --- |
| IT-süsteem | Seotud kasutajate, seadmete, tarkvara, võrgu, teenuste ja andmete tervik. |
| Kasutajakonto | Inimese või teenuse identiteet süsteemis. |
| Autentimine | Identiteedi kontrollimine. |
| Autoriseerimine | Kontroll, kas soovitud tegevuseks on õigus. |
| Roll | Ülesannetega seotud õiguste kogum. |
| Klientseade | Seade, mille kaudu teenust kasutatakse. |
| Operatsioonisüsteem (OS) | Riistvararessursse haldav ja rakendustele teenuseid pakkuv põhitarkvara. |
| Rakendus | Tarkvara konkreetse ülesande täitmiseks. |
| Veebirakendus | Brauseri kaudu kasutatav rakendus. |
| SaaS | Tarkvara teenusena, mida haldab teenusepakkuja. |
| LAN | Piiratud ala seadmeid ühendav kohtvõrk. |
| Klient-server mudel | Suhtlus, milles klient algatab päringu ja server pakub teenust. |

## Allikad ja lisalugemine

- [MDN — kuidas veeb ning kliendi ja serveri suhtlus toimib](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Web_standards/How_the_web_works)
- [Microsoft Learn — pilveteenuste mudelid ja vastutuse jaotus](https://learn.microsoft.com/en-us/azure/security/fundamentals/shared-responsibility)

