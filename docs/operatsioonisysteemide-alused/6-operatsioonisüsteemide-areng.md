# Operatsioonisüsteemide areng

Operatsioonisüsteemide areng on seotud sellega, kuidas inimesed arvuteid kasutavad. Algul oli arvuti kallis töövahend, millele anti ülesandeid järjekorras. Tänapäeval töötavad operatsioonisüsteemid sülearvutites, telefonides, serverites ja paljudes muudes seadmetes. Selles materjalis vaatame, millised muutused selle arenguni viisid ja miks need olulised olid.

!!! info "Õpieesmärgid"

    Pärast materjali läbimist oskad:

    - selgitada, miks pakktöötluse kõrval kujunes ajajaotuslik tööviis;
    - kirjeldada Unixi mõju operatsioonisüsteemide arengule;
    - selgitada personaalarvutite ja graafilise kasutajaliidese leviku tähendust;
    - eristada Unixi, BSD, GNU ja Linuxi ajaloolist rolli;
    - kirjeldada Windows NT ja Mac OS X tähtsust tänapäevaste süsteemide kujunemisel;
    - selgitada mobiilseadmete, virtualiseerimise ja pilveteenuste mõju operatsioonisüsteemidele.

## 1. Miks operatsioonisüsteemid muutuvad?

Operatsioonisüsteemi ülesanne on korraldada riistvara kasutamist ja pakkuda rakendustele vajalikke teenuseid. See ülesanne on püsinud, kuid arvutid ja nende kasutamine on muutunud.

Arengut on suunanud mitu vajadust:

- **Ressursside parem kasutamine.** Kallis arvuti ei peaks kasutajate tegevuse ootel tarbetult seisma.
- **Mugavam suhtlemine.** Arvutit peaks saama kasutada ka inimene, kes ei ole programmeerija.
- **Töökindlus ja turvalisus.** Ühe programmi viga ei peaks rikkuma teiste tööd ning kasutajate andmeid tuleb kaitsta.
- **Uued seadmed ja ühendused.** Süsteem peab toetama näiteks võrku, puuteekraani, kaamerat ja akutoitel töötamist.
- **Erinevad kasutusviisid.** Koduarvuti, telefon ja ettevõtte server vajavad osaliselt erinevaid võimalusi.

!!! tip "Kuidas ajalugu õppida?"

    Iga arenguetapi juures mõtle kahele küsimusele: **millist probleemi lahendati** ja **mis muutus kasutaja jaoks**. Aastaarvud aitavad sündmusi järjestada, kuid olulisem on mõista muutuste tähendust.

## 2. Varased arvutid: pakktöötlusest ajajaotuseni

**1950.** aastate arvutid olid kallid ja nende kasutamine nõudis palju ettevalmistust. **Pakktöötluse** levik võimaldas ettevalmistatud töid automaatselt käivitada ning vähendada nende vahele jäävat aega. Kasutaja pidi aga endiselt ootama, kuni tema töö täideti ja tulemus väljastati.

**1960.** aastatel kasvas vajadus arvutiga töö käigus suhelda. Näiteks programmi katsetamisel oli kasulik tulemust kiiresti näha ja selle põhjal järgmine muudatus teha. **Ajajaotuse** areng võimaldas mitmel inimesel kasutada sama kallist arvutit interaktiivselt, igaühel oma terminali kaudu.

**Muutuse tähtsus:** arvuti muutus etteantud tööde täitjast ka kasutaja töö käigus antavatele käskudele vastavaks töövahendiks. Pakktöötlus jäi samal ajal kasutusse. 

## 3. Unix: ideed, mis mõjutasid paljusid süsteeme

**Unixi** arendus algas **1969. aastal** Bell Labsis. Selle kujunemisel olid olulised Ken Thompson, Dennis Ritchie ja teised arendajad. Varasemast Multicsi projektist saadud kogemused aitasid luua väiksema ja lihtsama süsteemi.

Unixi juures kujunes oluliseks tööriistade koostöö: üks programm täidab kindlat ülesannet ning selle tulemust saab kasutada järgmise programmi sisendina. Näiteks saab ühe tööriistaga otsida tekstist vajalikke ridu ja teisega need sortida.

**1973. aastal** kirjutati suur osa Unixist ümber **C-programmeerimiskeeles**. See lihtsustas süsteemi kohandamist erinevale riistvarale. Sellist omadust nimetatakse **teisaldatavuseks** (*portability*). Kohandamine nõuab endiselt tööd: see ei tähenda, et sama valmis programmifail sobiks kõigile protsessoritele.

**Muutuse tähtsus:** Unixi ideed ja tarkvara levisid teadusasutustes ning mõjutasid paljude hilisemate süsteemide ülesehitust.

### BSD ja Unixi mõju

**1970.** aastate lõpus kujunes California ülikoolis Berkeleys **BSD** (*Berkeley Software Distribution*). Algul oli see Unixi juurde loodud lisade ja muudatuste kogum. Hiljem arenesid sellest iseseisvad süsteemid, näiteks FreeBSD, OpenBSD ja NetBSD.

Unixi mõju on näha ka tänapäevases macOS-is ja Linuxi süsteemides, kuid nende seos Unixiga on erinev.

!!! tip "Unix, BSD ja Linux ei ole sama asi"

    - **Unix** on ajalooline operatsioonisüsteem, mille ideed mõjutasid paljusid hilisemaid süsteeme.
    - **BSD** arenes ajalooliselt Unixi tarkvarast ja selle edasiarendustest.
    - **Linux** on eraldi kirjutatud Unixilaadne tuum. See ei ole Unixi järgmine versioon ega BSD haru.

## 4. Personaalarvutid ja graafiline kasutajaliides

### Personaalarvuti: arvuti ühe inimese töölaual

**1970.** ja **1980.** aastatel muutusid väiksemad arvutid kättesaadavaks ettevõtetele ja kodukasutajatele. **Personaalarvuti** (*personal computer*, PC) oli mõeldud eelkõige ühe inimese kasutusse.

**1981. aastal** tuli turule IBM PC. Sellega seostus Microsofti arendatud DOS-süsteem, mida IBM pakkus PC DOS-i nime all. MS-DOS levis ka IBM-iga ühilduvatel arvutitel.

**MS-DOS** (*Microsoft Disk Operating System*) pakkus tekstikäskudel põhinevat töökeskkonda. Kasutaja käivitas programme ja haldas faile käsurealt. Selle põhikasutus erines tänapäevasest töölauast, kus paljud rakendused töötavad korraga.

**Muutuse tähtsus:** arvuti muutus isiklikuks töövahendiks, millel sai näiteks kirjutada dokumente, teha tabelarvutusi ja mängida.

### Graafiline liides: aknad, ikoonid ja menüüd

Graafilise kasutajaliidese ideid arendati juba enne Windowsi ja Macintoshi. Oluline roll oli 1970. aastate **Xerox PARC-i** uurimistööl ja **Alto** arvutil.

**1984. aastal** esitletud Apple Macintosh aitas akende, ikoonide, menüüde ja hiirega juhitavat tööviisi laiema kasutajaskonnani tuua. **1985. aastal** ilmus Windows 1.0, mis pakkus MS-DOS-i peal töötavat graafilist keskkonda. Varane Windows erines tehniliselt tänapäevasest Windowsist.

**1995. aastal** ilmunud Windows 95 tõi Windowsi kasutajatele Start-menüü ja tegumiriba. Need aitasid programme käivitada ning avatud akende vahel liikuda.

**Muutuse tähtsus:** kasutaja sai paljusid toiminguid teha nähtavate valikute kaudu, ilma et peaks iga tegevuse jaoks käsku teadma.

!!! tip "Graafiline liides ei asendanud käsurida täielikult"

    Graafiline liides muutis arvuti kasutamist, kuid käsurida jäi oluliseks haldamisel ja automatiseerimisel. Tänapäevases operatsioonisüsteemis saab sageli kasutada mõlemat.

## 5. GNU ja Linux: koostöös arendatav tarkvara

**1983. aastal** kuulutas Richard Stallman välja **GNU projekti**. Tarkvara arendamine algas 1984. aastal. Eesmärk oli luua Unixiga ühilduv vaba operatsioonisüsteem, mille tarkvara saaks kasutada, uurida, muuta ja jagada.

**1991. aastal** alustas Linus Torvalds **Linuxi tuuma** arendamist. Linux oli Unixilaadne, kuid kirjutatud eraldi. Linuxi ühendamine GNU tööriistade ja teiste projektide tarkvaraga võimaldas luua terviklikke kasutatavaid operatsioonisüsteeme.

Distributsioonide kujunemine lihtsustas nende komponentide kasutuselevõttu: kasutaja sai paigaldada koostatud terviku, selle asemel et kõik osad ise kokku panna. Sellest arengusuunast pärinevad näiteks Debian ja Ubuntu.

**Muutuse tähtsus:** süsteeme said koos arendada eri organisatsioonid ja vabatahtlikud ning tarkvara sai kohandada erinevateks kasutusotstarveteks.

## 6. Tänapäevaste töölauasüsteemide tehniline alus

Kasutajaliidese kõrval arenes operatsioonisüsteemi sisemine töökorraldus. Tähtsad olid programmide mäluruumi kaitsmine, kasutajate õigused ja mitme programmi töö juhtimine. Need võimalused ei tekkinud alles personaalarvutites, kuid muutusid seal üha olulisemaks.

### Windows NT

**1993. aastal** ilmus Windows NT 3.1. See oli DOS-iga seotud Windowsi harust erineva tehnilise alusega süsteem, mis oli mõeldud muu hulgas ettevõtete tööjaamadele ja serveritele.

**2001. aastal** ilmunud Windows XP viis NT-põhise tehnoloogia ka Windowsi peamise kodukasutajatele mõeldud tooteperekonna aluseks. Tänapäevane Windows, sealhulgas Windows 11, kuulub NT-põhisesse arenguliini.

### Mac OS X

**2001. aastal** ilmus Apple’i Mac OS X. Selle tehniline alus erines varasemast klassikalisest Mac OS-ist ning kasutas NeXT-i tehnoloogiat ja BSD komponente. Mac OS X ühendas Unixil põhineva süsteemi graafilise töökeskkonnaga. Hiljem sai tooteperekonna nimeks macOS.

**Muutuse tähtsus:** töölauasüsteemides levisid tugevamad kaitsemehhanismid ja töökindlam mitme programmi töökorraldus. See vähendas võimalust, et ühe rakenduse viga rikub teiste tööd, kuid ei kõrvaldanud kõiki tõrkeid.

!!! example "Näide: rakendus lõpetab ootamatult töö"

    Kui üks rakendus jookseb kokku, saab kasutaja sageli jätkata tööd teistes rakendustes. Seda toetavad protsesside eraldamine ja mälu kaitse. Kõik vead ei piirdu siiski ühe rakendusega: näiteks tõrge tuumas või draiveris võib mõjutada tervet süsteemi.

## 7. Võrguühendused ja mobiilsed operatsioonisüsteemid

### Üksikust arvutist võrku ühendatud töövahendiks

Arvutivõrkude ja interneti levik muutis operatsioonisüsteemide rolli. Võrgu kaudu sai jagada faile ja printereid, kasutada serverite teenuseid ning hallata arvuteid kaugelt.

Operatsioonisüsteemides muutusid oluliseks võrguühenduste seadistamine, kaugligipääs ja võrgu kaudu levivate ohtude eest kaitsmine. Serveris võib tähtsaim olla teenuste töö, kasutaja arvutis aga mugav juurdepääs neile.

**Muutuse tähtsus:** arvuti ei olnud enam ainult kohalik töövahend, vaid osa teiste seadmete ja teenustega seotud keskkonnast.

### Telefonid ja puuteekraanid

Mobiilsed operatsioonisüsteemid olid olemas ka enne iPhone’i ja Androidi. **2007. aastal** turule tulnud iPhone ning **2008. aastal** turule tulnud esimene Android-telefon olid olulised pidepunktid tänapäevase nutitelefoni kujunemisel. iPhone’i süsteem kandis algul nime iPhone OS ja sai hiljem nimeks iOS.

Mobiilseade vajas töölauaarvutist erinevaid lahendusi:

- väikesel puuteekraanil kasutatavat liidest;
- aku säästmist ja taustatöö piiramist;
- kaamera, asukoha ja andurite kasutamist;
- mobiilside ja Wi-Fi ühenduste haldamist;
- rakenduste ligipääsuõiguste kontrollimist.

**Muutuse tähtsus:** operatsioonisüsteem pidi sobima kaasaskantavale seadmele ning ühendama suhtlemise, internetikasutuse ja rakendused ühte keskkonda.

!!! example "Näide: kaamera kasutamise luba"

    Rakendus võib vajada pildistamiseks kaamerat. Operatsioonisüsteem võimaldab kasutajal otsustada, kas sellele rakendusele ligipääs anda. Rakenduse paigaldamine ja kõigi õiguste lubamine on erinevad otsused.

## 8. Virtualiseerimine ja pilveteenused

**Virtualiseerimine** võimaldab luua füüsilise arvuti ressursside abil virtuaalseid arvuteid. **Virtuaalmasinas** saab töötada oma operatsioonisüsteem, näiteks Linux või Windows. Virtuaalmasinate tööks vajalikke ressursse korraldab **hüperviisor**.

Virtualiseerimine ei tekkinud alles pilveteenustega. Selle juured ulatuvad 1960. aastate suurarvutitesse; hiljem levis see serverites ja personaalarvutites.

**Muutuse tähtsus:** ühel füüsilisel arvutil sai käitada mitut eraldi süsteemi, kasutada riistvara paindlikumalt ning luua katsetamiseks eraldatud keskkondi.

!!! example "Näide: operatsioonisüsteemi õppimine laboris"

    Windowsi kasutav õppija saab virtuaalmasinas proovida Debin Linuxit. Debian töötab oma süsteemina virtuaalsel riistvaral. Nii saab paigaldamist ja seadistamist harjutada ilma põhiarvuti operatsioonisüsteemi asendamata.

**Pilveteenuste** puhul pakub teenuseosutaja võrgu kaudu näiteks arvutusressursse, salvestusruumi või valmis rakendusi. Osa teenuseid töötab virtuaalmasinates. Pilveteenus võib pakkuda ka kaugelt kasutatavat töölauda.

!!! tip "Pilv ei kaota operatsioonisüsteemi vajadust"

    Pilveteenuse taga töötavad samuti arvutid ja operatsioonisüsteemid. Kasutaja ei pruugi neid ise hallata. Näiteks veebirakenduse kasutamisel töötavad rakenduse osad kasutaja seadmes ja teenuseosutaja serverites.

Tänapäevane areng ühendab varasemaid ideid: ressursside jagamist, kasutusmugavust, võrguühendusi ja turvalisust. Uus lahendus ei muuda kõiki vanemaid tööviise tarbetuks.

## 9. Olulisemad pidepunktid ajajoonel

Ajajoon on valik olulisematest muutustest, mitte kõigi süsteemide ja versioonide loend. Mitu arengusuunda toimus samal ajal.

| Aeg | Pidepunkt | Miks see oluline oli? |
| --- | --- | --- |
| **1950.–1960. aastad** | Pakktöötluse ja ajajaotuse areng. | Tööde käivitamine muutus automaatsemaks ning mitu kasutajat said arvutiga interaktiivselt töötada. |
| **1969 ja 1970. aastad** | Unixi loomine, C-keele kasutuselevõtt ja BSD kujunemine. | Levisid teisaldatavus, tööriistade koostöö ja hilisemaid süsteeme mõjutanud ideed. |
| **1981** | IBM PC ja DOS-süsteemide levik. | Personaalarvuti muutus laiemalt kasutatavaks isiklikuks töövahendiks. |
| **1983–1991** | GNU projekti algus ja Linuxi tuuma loomine. | Kujunesid olulised osad vabadest Unixilaadsetest operatsioonisüsteemidest. |
| **1984–1985** | Macintosh ja Windows 1.0. | Graafiline tööviis jõudis rohkemate personaalarvutite kasutajateni. |
| **1993** | Windows NT 3.1. | Alguse sai tänapäevase Windowsi NT-põhine tooteperekond. |
| **1995** | Windows 95. | Windowsis levisid Start-menüü ja tegumiriba. |
| **2001** | Windows XP ja Mac OS X. | Kodukasutajate süsteemides levisid NT-põhine Windows ja Unixil põhinev Apple’i süsteem. |
| **2007–2008** | iPhone ja esimene Android-telefon. | Puuteekraan, mobiilne internet ja rakendused kujundasid nutitelefoni kasutamist. |
| **2000. aastatest edasi** | Virtualiseerimise laiem kasutamine ja pilveteenuste levik. | Eraldi süsteeme ja teenuseid sai pakkuda paindlikult jagatud riistvaral. Virtualiseerimise idee ise on vanem. |

## 10. Enesekontroll

Vasta kõigepealt ise. Seejärel ava vastus ja võrdle oma põhjendust.

### 1. Millist kasutajate vajadust lahendas ajajaotuse kasutuselevõtt?

??? success "Vaata vastust"

    Kasutajad vajasid võimalust arvutiga töö käigus suhelda, näiteks programmi katsetada ja tulemuse põhjal järgmine muudatus teha. Ajajaotus võimaldas sellist tööd mitmele inimesele sama arvuti kaudu. See ei muutnud pakktöötlust tarbetuks.

### 2. Miks oli Unixi ümberkirjutamine C-keeles oluline?

??? success "Vaata vastust"

    See lihtsustas Unixi kohandamist erinevale riistvarale. Nii sai süsteem levida rohkematele arvutitele. Teisaldatavus ei tähenda, et sama valmis programmifail töötaks muutmata kujul igal protsessoril.

### 3. Kas Windows ja Macintosh leiutasid graafilise kasutajaliidese?

??? success "Vaata vastust"

    Ei. Graafilise liidese ideid arendati varem, muu hulgas Xerox PARC-is. Macintosh ja Windows aitasid graafilist tööviisi laiema kasutajaskonnani tuua.

### 4. Kas Linux on Unixi järgmine versioon? Kuidas seostub sellega BSD?

??? success "Vaata vastust"

    Linux ei ole Unixi järgmine versioon. See on eraldi kirjutatud Unixilaadne tuum. BSD ajalooline areng on seotud Unixi tarkvara ja selle edasiarendustega. Linux ja BSD on erinevad arengusuunad.

### 5. Miks oli GNU tarkvara ja Linuxi tuuma ühendamine oluline arenguetapp?

??? success "Vaata vastust"

    Eraldi projektides loodud osad täiendasid üksteist. GNU tööriistade, Linuxi tuuma ja teiste projektide tarkvara ühendamine võimaldas koostada kasutatavaid vabu operatsioonisüsteeme. Nende arengusse said panustada paljud inimesed ja organisatsioonid.

### 6. Milline oli Windows NT tähtsus Windowsi arengus?

??? success "Vaata vastust"

    Windows NT tõi DOS-iga seotud Windowsi harust erineva tehnilise aluse. Windows XP viis NT-põhise tehnoloogia Windowsi peamise kodukasutajatele mõeldud tooteperekonna aluseks. Samasse arenguliini kuulub tänapäevane Windows.

### 7. Miks ei piisa telefonis ainult töölauaarvuti liidese väiksemaks muutmisest?

??? success "Vaata vastust"

    Telefon vajab puuteekraani jaoks sobivat suhtlemisviisi, aku säästmist, mobiilside haldamist ja andurite kasutamist. Lisaks peab süsteem korraldama rakenduste taustatööd ning õigusi kasutada näiteks kaamerat ja asukohta.

### 8. Miks aitas virtualiseerimine kaasa pilveteenuste levikule?

??? success "Vaata vastust"

    Virtualiseerimine võimaldas käitada ühel füüsilisel arvutil mitut eraldi süsteemi ja jagada riistvara ressursse paindlikumalt. Teenuseosutaja sai nii luua ja hallata klientidele vajalikke keskkondi. Virtualiseerimine on üks pilveteenuseid toetav tehnoloogia, mitte pilveteenuse sünonüüm.

## Kokkuvõte

- Operatsioonisüsteemide arengut suunavad ressursside kasutamine, kasutusmugavus, töökindlus, turvalisus ja uued seadmed.
- Varaste süsteemide arengus lisandus ettevalmistatud tööde automaatsele täitmisele võimalus arvutiga töö käigus suhelda.
- Unix mõjutas paljude hilisemate süsteemide ülesehitust. BSD arenes ajalooliselt Unixi tarkvarast, Linux on eraldi kirjutatud Unixilaadne tuum.
- Personaalarvutite ja graafiliste liideste levik tõi arvutid suurema kasutajaskonna igapäevatöösse.
- GNU, Linux ja teised projektid võimaldasid koostada vabu operatsioonisüsteeme.
- Windows NT ja Mac OS X aitasid kujundada tänapäevaste töölauasüsteemide tehnilist alust.
- Mobiilsüsteemid arvestavad puuteekraani, aku, andurite ja rakenduste õigustega.
- Virtualiseerimine võimaldab mitut eraldi süsteemi jagatud riistvaral; pilveteenused pakuvad ressursse ja rakendusi võrgu kaudu.

### Mõtle õpitule

1. Vali üks arenguetapp ja selgita oma sõnadega, millist probleemi see lahendas. Too näide selle mõjust tänapäevasele arvutikasutusele.
2. Miks kasutatakse ka praegu nii käsurida kui ka graafilist liidest? Too kummagi jaoks üks sobiv ülesanne.
3. Milline seos vajab veel selgitamist: Unix ja BSD, GNU ja Linux või virtualiseerimine ja pilveteenus? Sõnasta üks küsimus, millele soovid vastust leida.

## Teema sõnastik

| Mõiste | Selgitus |
| --- | --- |
| Teisaldatavus (*portability*) | Võimalus kohandada tarkvara eri riistvarale või keskkondadele. |
| Unix | Bell Labsis 1969. aastal alguse saanud operatsioonisüsteem, mis mõjutas paljusid hilisemaid süsteeme. |
| Unixilaadne süsteem (*Unix-like system*) | Süsteem, mille ülesehitus ja tööviis sarnanevad Unixiga; see ei pea pärinema Unixi lähtekoodist. |
| BSD (*Berkeley Software Distribution*) | Berkeleys Unixi lisade ja edasiarendustena kujunenud tarkvara, millest arenesid mitmed iseseisvad operatsioonisüsteemid. |
| Personaalarvuti; PC (*personal computer*) | Eelkõige ühe inimese kasutusse mõeldud arvuti. |
| MS-DOS (*Microsoft Disk Operating System*) | Microsofti tekstikäskudel põhinev personaalarvutite operatsioonisüsteem. |
| Graafiline kasutajaliides; GUI (*graphical user interface*) | Kasutajaliides, milles kasutatakse näiteks aknaid, ikoone ja menüüsid. |
| GNU | Vaba Unixiga ühilduva operatsioonisüsteemi loomise projekt ja selle raames arendatud tarkvara. |
| Linuxi tuum (*Linux kernel*) | Linus Torvaldsi algatatud ja paljude arendajate edasi arendatud Unixilaadne operatsioonisüsteemi tuum. |
| Distributsioon (*distribution*) | Koostatud tarkvarakogum, mis ühendab tuuma, süsteemitööriistad, paketihalduse ja valitud rakendused kasutatavaks tervikuks. |
| Windows NT | 1993. aastal esimese väljalaskeni jõudnud Windowsi tooteperekond ja tehniline arenguliin, millel põhineb tänapäevane Windows. |
| Virtualiseerimine (*virtualization*) | Füüsiliste ressursside abil virtuaalsete keskkondade, näiteks virtuaalmasinate loomine. |
| Virtuaalmasin; VM (*virtual machine*) | Tarkvaraliselt loodud arvutikeskkond, milles saab töötada oma operatsioonisüsteem. |
| Hüperviisor (*hypervisor*) | Tarkvara, mis korraldab virtuaalmasinate tööd ja nende ligipääsu füüsilistele ressurssidele. |
| Pilveteenus (*cloud service*) | Teenuseosutaja võrgu kaudu pakutav arvutusressurss, salvestusruum või rakendus. |

## Allikad ja lisalugemine

1. [Computer History Museum: Operating Systems](https://softwarepreservation.computerhistory.org/os/){ target="_blank" rel="noopener" }.
2. [Computer History Museum: Timesharing](https://www.computerhistory.org/pdp-1/timesharing/){ target="_blank" rel="noopener" }.
3. [The Open Group: History of UNIX](https://www.unix.org/unix_history.html){ target="_blank" rel="noopener" }.
4. [FreeBSD projekt: BSD süsteemide ajalooline sugupuu](https://cgit.freebsd.org/src/tree/share/misc/bsd-family-tree){ target="_blank" rel="noopener" }.
5. [Computer History Museum: Microsoft MS-DOS early source code](https://computerhistory.org/blog/microsoft-ms-dos-early-source-code/){ target="_blank" rel="noopener" }.
6. [Computer History Museum: Apple Timeline](https://computerhistory.org/apple-timeline/){ target="_blank" rel="noopener" }.
7. [Computer History Museum: Windows 1.0](https://www.computerhistory.org/revolution/personal-computers/17/305/1208){ target="_blank" rel="noopener" }
8. [GNU projekt: Overview of the GNU System](https://www.gnu.org/gnu/gnu-history.html){ target="_blank" rel="noopener" }.
9. [Microsoft: 30 years of Windows Server](https://techcommunity.microsoft.com/blog/sbs/30-years-of-windows-server/3884810){ target="_blank" rel="noopener" }.
10. [IBM: What Is a Virtual Machine?](https://www.ibm.com/think/topics/virtual-machines){ target="_blank" rel="noopener" }.

---
*Õppematerjali koostaja: Priit Paap, 2026*
