# Tänapäevased operatsioonisüsteemid ja nende kasutusvaldkonnad

Tänapäevased operatsioonisüsteemid pakuvad erinevaid võimalusi tööks, õppimiseks, teenuste käitamiseks ja nutiseadmete kasutamiseks. Windows, Linux, macOS ja mobiilsüsteemid võivad täita sarnaseid ülesandeid, kuid nende riistvaratugi, rakenduskeskkond ja haldusvõimalused erinevad.

!!! info "Õpieesmärgid"

    Pärast materjali läbimist oskad:

    - seostada levinud operatsioonisüsteeme konkreetsete kasutusvaldkondadega;
    - eristada operatsioonisüsteemi perekonda, toodet, versiooni ja väljaannet;
    - selgitada Windows Home'i ja Pro olulisemaid erinevusi;
    - võrrelda Linuxi distributsioonide rõhuasetusi;
    - eristada distributsiooni ja töölauakeskkonda;
    - kirjeldada macOS-i, Androidi ja iOS/iPadOS-i platvormide eripärasid;
    - eristada Linuxi ja BSD süsteeme ning ChromeOS-i ja ChromeOS Flexi;
    - leida oma seadme süsteemi täpse nimetuse ja versiooni.

## 1. Tänapäevaste platvormide kasutusnäited

Sama ülesande jaoks võib sobida mitu operatsioonisüsteemi. Näiteks veebilehte saab vaadata Windowsis, Linuxis ja macOS-is. Erinevused muutuvad oluliseks siis, kui vaja on kindlat rakendust, haldusvõimalust või seadmetuge.

| Platvorm | Kasutusnäited | Mis võib valikut mõjutada? |
| --- | --- | --- |
| **Windows 11** | Õppimine, kontoritöö, mängimine ja organisatsiooni tööarvutid. | Vajalikud Windowsi rakendused ning väljaande haldusvõimalused. |
| **Windows Server** | Failide jagamine, kasutajate keskne haldamine ja muud serveriteenused. | Vajalikud serverifunktsioonid ja organisatsiooni Windowsi keskkond. |
| **Linuxi distributsioonid** | Veebiserverid, tarkvaraarendus, töölauatöö ja eriseadmed. | Distributsiooni tarkvaravalik, hooldusmudel ning vajalikud tööriistad. |
| **macOS** | Kontoritöö, tarkvaraarendus, foto-, heli- ja videotöötlus Mac-arvutis. | Maci mudel ja kiip ning vajalike rakenduste saadavus. |
| **Android, iOS ja iPadOS** | Telefonide ja tahvelarvutite rakendused, side, mobiilne töö ja õppimine. | Seadmemudel, rakenduskeskkond ja tootja uuendustugi. |
| **BSD süsteemid** | Serverid ning võrgu- ja muud erilahendused. | Süsteemi tööriistad, seadmetugi ja haldaja teadmised. |
| **ChromeOS ja ChromeOS Flex** | Veebirakendustel põhinev õppimine ja kontoritöö. | Vajalikud rakendused ja konkreetse seadme toetatud võimalused. |

!!! example "Näide: sama õppetöö, erinevad keskkonnad"

    Õppija kirjutab veebipõhises tekstiredaktoris aruannet, teine kasutab kindlat Windowsi rakendust ja kolmas käitab Linuxi virtuaalmasinas veebiserverit. Kõik kasutavad arvutit õppetööks, kuid nende ülesanded vajavad erinevaid süsteemi ja rakenduste võimalusi.

Kasutusvaldkonnad ei ole jäigad piirid. Näiteks Linux sobib ka kontoritööks ja macOS tarkvaraarenduseks. Konkreetse ülesande sobivust tuleb hinnata vajalike võimaluste järgi.

## 2. Mida operatsioonisüsteemi nimi näitab?

Üks nimi võib sisaldada korraga mitut täpsustust. Näiteks **Windows 11 Pro, versioon 24H2** kirjeldab nii toodet, väljaannet kui ka selle väljalaset. **Debian 13 töölauaga Xfce** kirjeldab distributsiooni, versiooni ja valitud töölauakeskkonda.

| Mõiste | Tähendus | Näide |
| --- | --- | --- |
| **Perekond või platvorm** | Ühine nimetus omavahel seotud süsteemidele. | Windowsi perekonnas on tööjaamadele ja serveritele mõeldud tooted. |
| **Toode** | Konkreetne operatsioonisüsteem või tooteliin. | Windows 11 ja Windows Server on erinevad tooted. |
| **Versioon või väljalase** | Tähis, mis eristab süsteemi arengu ja uuendamise etappe. | Debian 13; Windows 11 versioon 24H2. |
| **Väljaanne** (*edition*) | Sama toote variant, mille võimalused või kasutustingimused võivad erineda. | Windows 11 Home ja Pro. |
| **Distributsioon** (*distribution*) | Koostatud operatsioonisüsteemi tervik koos valitud komponentide ja tarkvarahaldusega. | Debian, Ubuntu ja Fedora on Linuxi distributsioonid. |
| **Töölauakeskkond** (*desktop environment*) | Graafiline töökeskkond koos akende, menüüde, paneelide ja tööriistadega. | GNOME, KDE Plasma ja Xfce. |

Need nimetused ei ole kõigi tootjate juures täpselt ühesugused. Näiteks Linux Mint kasutab nimetust „väljaanne” ka eri töölauakeskkondadega variantide kohta.

!!! example "Näide: sama toode, erinev väljaanne"

    Kahel arvutil on Windows 11, kuid ühel on Home ja teisel Pro. Nende tavaline töölaud võib olla väga sarnane, kuigi osa haldusvõimalusi on erinev. Kui juhend eeldab Pro võimalust, ei piisa teadmisest, et arvutis on „Windows”.

Teoreetilistes näidetes kasutatud versiooninumbrid selgitavad nimetusi. Peatükkide lõpus olevad versioonitabelid kirjeldavad ametlikult avaldatud väljalaskeid **seisuga 13.10.2026**.

## 3. Windows: tööjaama tooted ja väljaanded

### Windows 11 Home ja Pro

Home ja Pro sobivad mõlemad tavapäraste rakenduste käitamiseks. Pro lisab eelkõige võimalusi, mis on kasulikud arvutite haldamisel organisatsioonis.

| Võimalus | Windows 11 Home | Windows 11 Pro |
| --- | --- | --- |
| Brauseri, kontoritarkvara ja mängude kasutamine | Jah, kui rakendus ja riistvara sobivad. | Jah, kui rakendus ja riistvara sobivad. |
| Liitumine domeeni ja Entra ID-ga | Ei toeta seadme liitmist domeeni ja Entra ID-ga. | Toetab seadme liitmist domeeni ja Entra ID-ga. |
| Windowsi sisseehitatud kaugtöölaua ühenduste vastuvõtmine | Ei toeta. | Toetab pärast vajalike seadistuste tegemist. |
| BitLockeri ketta krüpteerimise laiendatud haldus | Ei sisalda samu haldusvõimalusi nagu Pro. | Sisaldab BitLockeri ketta krüpteerimise haldusvõimalusi. |

**Microsoft Entra ID** on organisatsioonide pilvepõhine identiteediteenus. Seadme liitmine võimaldab kasutada organisatsiooni kontot seadmesse sisselogimisel ja siduda seadme organisatsiooni keskkonnaga. See on erinev sellest, kui kasutaja lihtsalt logib brauseris kooli Microsoft 365 kontoga sisse.

!!! tip "Kaugtöölaua ühenduse kaks poolt"

    Windows Home'i kasutavast arvutist saab ühenduda teise arvuti kaugtöölauaga. Piirang puudutab Windowsi sisseehitatud kaugtöölaua ühenduse **vastuvõtmist** Home'iga arvutis. Muude kaugjuhtimisrakenduste võimalused sõltuvad konkreetsest tarkvarast.

Home'il on samuti turvafunktsioonid, näiteks tulemüür ja pahavarakaitse. Sobivas seadmes võib olla saadaval **seadme krüpteerimine** (*Device Encryption*). Seetõttu ei ole õige öelda, et Home ei võimalda üldse andmeid krüpteerida.

**Enterprise** ja **Education** on organisatsioonidele mõeldud väljaanded, mille funktsioonid ja kasutusõigused sõltuvad konkreetsest tootest ning litsentsist. „Education” ei tähenda lihtsalt teistsugust töölauda või ainult õppimiseks sobivat süsteemi.

**Väljaande tähtsus:** tavalise rakenduse kasutamisel võib Home'i ja Pro vahe olla väike, kuid seadme organisatsiooni keskkonda liitmisel või haldamisel määrab väljaanne, kas vajalik võimalus on olemas.

### Windows 10 ja Windows 11 viimased funktsiooniväljalasked

**Seisuga 13.10.2026.** Tabelis on funktsiooniväljalasked, mitte igakuiste paranduste järgunumbrid.

| Operatsioonisüsteem | Viimane üldlevitatav funktsiooniväljalase | Märkus |
| --- | --- | --- |
| **Windows 11** | **26H2** — Windows 11 2026 Update | Avaldati 29.09.2026. Sobivatele seadmetele pakutakse uuendust järk-järgult. |
| **Windows 10** | **22H2** — Windows 10 2022 Update | Viimane Windows 10 funktsiooniväljalase. Home'i ja Pro tavapärane tugi lõppes 14.10.2025; täiendavad turvauuendused eeldavad ESU tingimuste täitmist. |

Windows 11 **26H1** on eraldi väljalase valitud uutele seadmetele. See ei ole kõigile olemasolevatele arvutitele pakutav funktsiooniuuendus. Windows 10 LTSC väljaannetel on eraldi toe elutsükkel.

### Windows Server

Windows Server on eraldi serveritoode. Selle ülesehitus ja funktsioonid on suunatud teenuste pakkumisele ja haldamisele. Windows Serveri väljaanded on näiteks **Standard** ja **Datacenter**; nende funktsioonid ja virtualiseerimise kasutusõigused erinevad.

Windows Serveri puhul tuleb eristada ka **installimisvalikut**:

- **Server Core** pakub süsteemi ilma tavapärase graafilise töölauata. Seda hallatakse käsurea või sobivate kaugvahenditega.
- **Desktop Experience** sisaldab graafilist töölauda ja kohalikke graafilisi haldusvahendeid.

Need ei ole sama liigitus mis Standard ja Datacenter. Server Core võib olla ühe väljaande paigaldusviis.

!!! example "Näide: serveri kirjeldus"

    „Windows Server 2025 Standard, Server Core” annab kolm täpsustust: serveritoote väljalase, väljaanne Standard ja installimisvalik Server Core. Sellest nimest ei selgu veel, millised teenused serveris on seadistatud.

### Windows Serveri kolm viimast LTSC põhiversiooni

**Seisuga 13.10.2026.** LTSC (*Long-Term Servicing Channel*) tähistab Windows Serveri pika hooldusperioodiga väljalaskeid.

| Põhiversioon | Esmase avaldamise aasta | Märkus |
| --- | --- | --- |
| **Windows Server 2025** | **2024** | Uusim Windows Serveri LTSC põhiversioon. Nimes olev aastaarv ei ole alati tegelik väljalaskeaasta. |
| **Windows Server 2022** | **2021** | Windows Server 2025-le eelnenud LTSC põhiversioon. |
| **Windows Server 2019** | **2018** | Windows Server 2022-le eelnenud LTSC põhiversioon. |

## 4. Linux: distributsioonid ja nende rõhuasetused

Linuxi distributsioonide ühine komponent on **Linuxi tuum**, kuid nende tarkvaravalik, tööriistad, uuendamise korraldus ja tugivõimalused võivad erineda.

| Distributsioon | Iseloomulik rõhuasetus | Mida algajal tähele panna? |
| --- | --- | --- |
| **Debian** | Kogukonna arendatav süsteem, mille stabiilne väljalase keskendub etteaimatavale tarkvarakomplektile. | Saab paigaldada eri töölaudadega või ilma graafilise töölauata. Näiteks Debian 13 kannab nime „trixie”. |
| **Ubuntu** | Debianil põhinev süsteem, millel on töölaua- ja serverivariandid ning pika toega väljalasked. | Ubuntu Desktop ja Ubuntu Server erinevad eelkõige esialgse tarkvaravaliku ja seadistuse poolest. |
| **Linux Mint** | Töölaua kasutusmugavusele keskenduv süsteem. Peamine tooteliin põhineb Ubuntul. | Cinnamon, MATE ja Xfce on töölauavariandid. LMDE on otse Debianil põhinev tooteliin. |
| **Fedora** | Kogukonna arendatav süsteem, mis toob uusi Linuxi tehnoloogiaid kiiresti kasutusse. | Fedora Workstation ja Fedora Server on eri vajadustele koostatud variandid. |
| **Red Hat Enterprise Linux; RHEL** | Ettevõtetele mõeldud platvorm, mille puhul on oluline pikk elutsükkel ja tootja tugiteenused. | Tarkvara kasutamine ja tugiteenused on seotud konkreetsete tellimuste ning tingimustega. |
| **CentOS Stream** | RHEL-i arendusega seotud distributsioon, mis paikneb järgmise RHEL-i väljalaske ees. | See ei ole ajaloolise CentOS Linuxi lihtsalt ümber nimetatud samaväärne jätk. |
| **Arch Linux** | Kasutaja enda koostatud süsteem ja pidev uuendamine. | Eeldab rohkem iseseisvat seadistamist ning dokumentatsiooni kasutamist. |

**Põhinemine teisel distributsioonil** tähendab, et kasutatakse osa selle tarkvaralisest alusest ja arendustööst. See ei muuda distributsioone samaks tooteks. Ubuntu ja Linux Mint teevad oma tarkvaravaliku, seadistused ning väljalasked.

!!! tip "Sarnane alus ei taga sama juhendi sobivust"

    Debianil, Ubuntul ja Linux Mintil on palju ühiseid tööriistu, kuid nende versioonid ja vaikeseadistused võivad erineda. Fedora ja Arch kasutavad muu hulgas teistsugust paketihaldust. Enne käsu kasutamist vaata, millisele distributsioonile ja versioonile juhend on mõeldud.

### Töölauakeskkond ei ole distributsioon

Distributsiooni saab sageli kasutada mitme erineva töölauakeskkonnaga. Töölaud mõjutab näiteks menüüde, akende, failihalduri ja seadistusvahendite välimust ning tööviisi.

Levinud töölauakeskkonnad on **GNOME**, **KDE Plasma**, **Xfce** ja **Cinnamon**. Valik võib mõjutada nii ressursikasutust kui ka seda, kust kasutaja vajaliku seadistuse leiab.

!!! example "Näide: kaks erineva välimusega Debianit"

    Ühes arvutis töötab Debian 13 GNOME'iga ja teises Debian 13 Xfcega. Menüüde paigutus ja seadistusaknad erinevad, kuid distributsioon ja põhiversioon on samad. Töölaua vahetamine ei tähenda automaatselt teise distributsiooni paigaldamist.

Samuti võib sama töölauakeskkond olla saadaval mitmes distributsioonis. Sarnane välimus ei tõesta, et nende süsteemitööriistad ja uuendamise korraldus on ühesugused.

### Mainitud Linuxi distributsioonide viimased väljalasked

**Seisuga 13.10.2026.** Tabelis on stabiilsed väljalasked; arendus- ja beetaversioone ei ole käsitletud valmis väljalasetena.

| Distributsioon | Viimane stabiilne versioon või paigalduspilt | Täpsustus |
| --- | --- | --- |
| [**Debian**](https://www.debian.org/releases/stable/){ target="_blank" rel="noopener" } | **13.7 „trixie”** | Debian 13 uusim hooldusväljalase. |
| [**Ubuntu**](https://releases.ubuntu.com/){ target="_blank" rel="noopener" } | **26.04.1 LTS „Resolute Raccoon”** | Uusim avaldatud stabiilne väljalase ja LTS. Ubuntu 26.10 ametlik väljalase on kavandatud 15.10.2026. |
| [**Linux Mint**](https://linuxmint.com/download.php){ target="_blank" rel="noopener" } | **22.3 „Zena”** | Ubuntu-põhine põhiliin; saadaval Cinnamoni, MATE ja Xfce töölaudadega. |
| [**LMDE**](https://linuxmint.com/download_lmde.php){ target="_blank" rel="noopener" } | **7 „Gigi”** | Linux Minti otse Debianil põhinev tooteliin. |
| [**Fedora Linux**](https://fedoraproject.org/workstation/download/){ target="_blank" rel="noopener" } | **44** | Põhiversioon, mida kasutatakse näiteks Workstationi ja Serveri variantides. |
| [**Red Hat Enterprise Linux**](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/10.2_release_notes/index){ target="_blank" rel="noopener" } | **10.2** | Uusim väljalase RHEL-i kõige uuemas põhiversioonis. Vanemaid põhiversioone hooldatakse eraldi. |
| [**CentOS Stream**](https://www.centos.org/download/){ target="_blank" rel="noopener" } | **10** | Põhiversiooni sees lisanduvad muudatused jooksvalt; RHEL 10.2-ga samasugust väljalaskenumbrit ei kasutata. |
| [**Arch Linux**](https://archlinux.org/download/){ target="_blank" rel="noopener" } | **Pidev uuendamine; paigalduspilt 2026.10.01** | Kuupäev tähistab ISO paigalduspilti, mitte eraldi Archi süsteemiversiooni. |

Debiani ja Ubuntu hooldusväljalasked koondavad parandusi ja uuendatud paigalduspilte. Toetatud süsteemi tavapärane uuendamine võib viia paigaldatud tarkvara samale tasemele, ilma et süsteemi peaks uuesti paigaldama.

## 5. Väljalasete ja uuendamise erinevad mudelid

Distributsioonid erinevad ka selle poolest, kuidas uus tarkvara kasutajani jõuab.

| Mudel | Põhimõte | Näide |
| --- | --- | --- |
| **Kindlate väljalasetega mudel** (*fixed release*) | Avaldatakse eristatavad süsteemiversioonid. Uuele põhiversioonile üleminek on eraldi tegevus. | Debian ja Ubuntu. |
| **Pika toega väljalase** (*Long-Term Support*, LTS) | Konkreetse väljalaske hooldust ja turvaparandusi pakutakse pikema perioodi jooksul. | Ubuntu LTS. |
| **Pidev uuendamine** (*rolling release*) | Süsteemi komponente uuendatakse jooksvalt ning tavapärane uuendamine viib süsteemi edasi. | Arch Linux. |

LTS on väljalaske toe omadus, mitte kindlate väljalasetega mudeli vastand. Ubuntu LTS on samuti kindel väljalase.

**Stabiilne väljalase ei tähenda uuendusteta süsteemi.** Tarkvara põhiversioonid võivad püsida, kuid süsteem saab turva- ja veaparandusi. Nii saab säilitada tuttava keskkonna ning samal ajal vigu parandada.

Ka pideva uuendamise mudel nõuab hooldamist. Suuremate muudatuste puhul võib olla vaja lugeda projekti teadaandeid ja teha täiendavaid toiminguid.

!!! tip "LTS ei tähenda igavest tuge"

    Toe kestus sõltub tootest, väljalaskest ja vahel ka tugipaketist. Sama lühendi kasutamine eri projektides ei tähenda sama arvu aastaid ega kõigile komponentidele võrdset tuge.

## 6. macOS: Apple'i arvutite platvorm

macOS on Apple'i Mac-arvutite operatsioonisüsteem. Apple kujundab nii süsteemi kui ka selle jaoks mõeldud riistvara. macOS-i väljalasetel on versiooninumbrid ja sageli nimed, näiteks **macOS Tahoe 26**. Kodukasutaja ei vali tavaliselt Home'i ja Pro laadsete väljaannete vahel.

Mac-arvuti mudel ja protsessor on olulised. Kasutusel on nii vanemad **Inteli protsessoriga Macid** kui ka **Apple silicon'i** ehk Apple'i oma kiipidega Macid. Kõik macOS-i väljalasked ei toeta kõiki neid arvuteid ning funktsioonid võivad sõltuda riistvarast.

macOS-i tuumas ja süsteemi aluses on avatud lähtekoodiga komponente, kuid kogu macOS ei ole vabalt muudetav ja levitatav avatud lähtekoodiga distributsioon. Ametlik süsteem on mõeldud toetatud Apple'i riistvarale. Seetõttu ei saa selle paigaldusvõimalusi samastada tavapärase Linuxi distributsiooni omadega.

!!! example "Näide: kaks Maci, erinev sobivus"

    Kahel õppijal võib olla MacBook, kuid nende arvutite mudelid ja protsessorid on erinevad. Enne uue macOS-i või rakenduse paigaldamist tuleb kontrollida täpset ühilduvust. Ainult nimetus „MacBook” ei anna selle kohta piisavalt teavet.

**Platvormi tähtsus:** süsteemi ja riistvara seos on tihe. Uuendamisel ning rakenduste kasutamisel tuleb vaadata lisaks süsteemiversioonile ka arvuti mudelit ja kiipi.

### macOS-i kolm viimast põhiversiooni

**Seisuga 13.10.2026.** Iga põhiversiooni juures on näidatud ka Apple'i avaldatud uusim hooldusversioon.

| Põhiversioon | Nimi | Uusim hooldusversioon |
| --- | --- | --- |
| **macOS 27** | **Golden Gate** | **27.0.1** |
| **macOS 26** | **Tahoe** | **26.7.1** |
| **macOS 15** | **Sequoia** | **15.8.1** |

Põhiversioonide numbrid ei pea olema järjestikused: macOS 15-le järgnes macOS 26. Konkreetsele Macile sobiva väljalaske määrab Apple'i mudelipõhine ühilduvusloend.


## 7. Android, iOS ja iPadOS

### Android ja tootjate erinevad süsteemid

Androidi avatud lähtekoodiga aluseks on **AOSP** (*Android Open Source Project*). Tootjad saavad selle põhjal koostada oma seadmele sobiva süsteemi ja lisada rakendusi ning kasutajaliidese muudatusi.

See aitab selgitada, miks kahe tootja Android-telefonidel võivad olla erinevad seadistusmenüüd, kaasas olevad rakendused ja uuenduste ajakavad. Sama Androidi põhiversioon ei tähenda tingimata täiesti samasugust süsteemi.

**AOSP ja Google'i teenused ei ole sama asi.** Google Play pood ning muud Google'i rakendused ja teenused ei kuulu kõik AOSP avatud lähtekoodiga alusesse. Seetõttu ei saa eeldada, et igas Androidil põhinevas seadmes on sama Google'i tarkvara.

### iOS ja iPadOS

**iOS** on iPhone'i ning **iPadOS** iPadi operatsioonisüsteem. Apple arendab need oma seadmete jaoks. Süsteemidel on palju ühiseid põhimõtteid, kuid iPadOS-i võimalused ja liides arvestavad tahvelarvuti kasutamisega.

Macis, iPhone'is ja iPadis kasutatakse seotud Apple'i teenuseid, kuid nende operatsioonisüsteemid ja rakendused ei ole automaatselt üksteisega asendatavad. Rakenduse saadavust tuleb vaadata vastava platvormi jaoks.

!!! tip "Telefoni uuendustugi sõltub mudelist"

    Androidi, iOS-i või iPadOS-i nimi üksi ei näita, kui kaua konkreetne seade uuendusi saab. Kontrolli tootja infot selle mudeli kohta ning erista uue süsteemiversiooni pakkumist turvaparanduste pakkumisest.

## 8. BSD ja ChromeOS: teised süsteemid

### BSD süsteemid

**FreeBSD**, **OpenBSD** ja **NetBSD** on iseseisvad Unixilaadsed operatsioonisüsteemid. Need ei ole Linuxi distributsioonid: neil on oma tuum ja süsteemi põhikomponendid.

BSD süsteemidega võib kokku puutuda näiteks serverites ja eriseadmetes. Paljud tuttavad avatud lähtekoodiga rakendused on saadaval nii Linuxi kui ka BSD jaoks, kuid süsteemide paketihaldus, seadistusfailid ja tööriistad võivad erineda.

**Pea meeles:** sarnased käsud ja ühine Unixilaadne tööviis ei tähenda sama operatsioonisüsteemi.

### ChromeOS ja ChromeOS Flex

**ChromeOS** on Chromebookides kasutatav operatsioonisüsteem, mille keskmes on veebirakendused ja Google'i teenuste kasutamine. **ChromeOS Flex** on eraldi variant toetatud PC- ja Mac-arvutitele.

ChromeOS-i ja ChromeOS Flexi võimalused ei ole täielikult samad. Näiteks ei paku ChromeOS Flex tavapärast Google Play poe ja Androidi rakenduste tuge nagu seda toetavad Chromebookid. Flexi puhul tuleb kontrollida ka konkreetse arvutimudeli toetust.

Süsteemi täpne nimi on siin oluline: ChromeOS-i rakenduse paigaldamise juhend ei pruugi sobida ChromeOS Flexile.

## 9. Kuidas oma süsteemi täpselt tuvastada?

Juhendi valimiseks või abi küsimiseks tasub üles leida vähemalt **süsteemi nimi, väljaanne või distributsioon ning versioon**. Töölauaga Linuxis võib vaja minna ka töölauakeskkonna nime.

| Süsteem | Kust infot leida? | Mida kirja panna? |
| --- | --- | --- |
| **Windows** | Settings → System → About. | Toode, väljaanne, versioon ning vajadusel järgunumber (*OS build*). |
| **Linux** | Süsteemi teabeaken või terminalis `cat /etc/os-release`. | Distributsiooni nimi ja versioon. Töölauakeskkond leia eraldi keskkonna teabeaknast. |
| **macOS** | Apple'i menüü → About This Mac. | macOS-i nimi ja versioon; vajadusel Maci mudel ning kiip. |
| **Android** | Settings → About phone või sarnase nimega jaotis. | Seadmemudel, Androidi versioon ja vajadusel tootja süsteemivariandi versioon. |
| **iOS/iPadOS** | Settings → General → About. | Seadmemudel ja süsteemi versioon. |

Menüüde nimed ja asukohad võivad süsteemiversiooni ning kasutajaliidese keele järgi erineda.

!!! tip "Linuxi tuuma versioon ei ole distributsiooni versioon"

    Käsk `uname -r` näitab tuuma versiooni. See ei vasta küsimusele, kas arvutis on näiteks Debian 13 või Ubuntu. Distributsiooni tuvastamiseks vaata `/etc/os-release` sisu.

## 10. Enesekontroll

Vasta kõigepealt ise. Seejärel ava vastus ja võrdle oma põhjendust.

### 1. Millist infot annavad nimetuses „Windows 11 Pro, versioon 24H2” sõnad ja tähised?

??? success "Vaata vastust"

    Windows 11 on toode, Pro on väljaanne ja 24H2 selle versioon ehk väljalase. Väljaanne aitab mõista funktsioonide olemasolu, versioon aga seda, millise väljalaske süsteemiga on tegemist.

### 2. Kas Windows Home'iga arvutist saab ühenduda Proga arvuti kaugtöölauaga?

??? success "Vaata vastust"

    Jah, kui ühendus ja ligipääs on nõuetekohaselt seadistatud. Home saab olla ühenduse klient. Home'i piirang puudutab Windowsi sisseehitatud kaugtöölaua ühenduse vastuvõtmist selles arvutis.

### 3. Kas Windows Server Standard ja Server Core tähendavad sama liigitust?

??? success "Vaata vastust"

    Ei. Standard on väljaanne, Server Core installimisvalik. Väljaanne määrab võimalused ja kasutusõigused, installimisvalik muu hulgas selle, kas süsteemil on tavapärane graafiline töölaud.

### 4. Ühel arvutil on Debian GNOME'iga ja teisel Debian Xfcega. Kas tegemist on eri distributsioonidega?

??? success "Vaata vastust"

    Ei. Distributsioon on mõlemal Debian, kuid töölauakeskkonnad erinevad. Menüüd ja seadistusvahendid võivad seetõttu olla erineva välimuse ning paigutusega.

### 5. Kas stabiilne väljalase tähendab, et süsteemi ei uuendata?

??? success "Vaata vastust"

    Ei. Tarkvara põhiversioonid võivad püsida, kuid toetatud süsteemile pakutakse turva- ja veaparandusi. Stabiilse keskkonna hoidmine ja turvauuenduste paigaldamine ei välista teineteist.

### 6. Koolis kasutatakse Windowsi tööarvuteid ja Linuxi veebiserverit. Kas see on vastuolu?

??? success "Vaata vastust"

    Ei. Tööarvuti peab sobima õppijate rakenduste ja kooli haldusvajadustega, veebiserver aga veebiteenuse käitamisega. Ühes organisatsioonis võib kasutada eri ülesannete jaoks erinevaid operatsioonisüsteeme.

### 7. Kas Linux Mint on Ubuntu teine nimi ning CentOS Stream RHEL-i täpne koopia?

??? success "Vaata vastust"

    Ei. Linux Minti peamine tooteliin põhineb Ubuntul, kuid sellel on oma tarkvaravalik, töölauad ja tööriistad. CentOS Stream on seotud RHEL-i arendusega ning paikneb järgmise RHEL-i väljalaske ees. Seotud süsteemid ei ole automaatselt samad tooted.

### 8. Kas sama Androidi versiooniga telefonidel peab olema sama liides ja samad Google'i rakendused?

??? success "Vaata vastust"

    Ei. Tootja saab süsteemi kohandada ning lisada oma liidese ja rakendused. Androidi avatud lähtekoodiga alus AOSP ja Google'i teenused on erinevad osad; kõigis Androidil põhinevates seadmetes ei pruugi olla sama Google'i tarkvara.

### 9. Kas FreeBSD on Linuxi distributsioon? Kas `uname -r` näitab Debianis Debiani versiooni?

??? success "Vaata vastust"

    FreeBSD ei ole Linuxi distributsioon: see on oma tuumaga BSD operatsioonisüsteem. Debianis näitab `uname -r` Linuxi tuuma versiooni. Debiani nime ja versiooni saab vaadata failist `/etc/os-release`.

## Kokkuvõte

- Sama kasutusvaldkonna jaoks võib sobida mitu süsteemi ning ühes organisatsioonis võivad eri ülesandeid täita erinevad operatsioonisüsteemid.
- Süsteemi perekond, toode, versioon ja väljaanne annavad erinevat infot. Täpne nimi aitab valida sobivat tarkvara ja juhendit.
- Windows Home'i ja Pro tavaline töölaud võib olla sarnane, kuid nende haldusvõimalused erinevad.
- Windows Serveri väljaanne ja installimisvalik on erinevad asjad: näiteks Standard ning Server Core.
- Linuxi distributsioonid erinevad tarkvaravaliku, tööriistade, tugivõimaluste ja uuendamise korralduse poolest.
- Töölauakeskkond mõjutab graafilist tööviisi, kuid ei määra üksi distributsiooni.
- Kindel väljalase, LTS ja pidev uuendamine kirjeldavad väljalasete ning hoolduse korraldust. Ka stabiilne süsteem vajab turvaparandusi.
- macOS-i puhul on tähtsad nii süsteemiversioon kui ka Maci mudel ja kiip. Mobiilsüsteemide puhul sõltub sobivus ja tugi konkreetsest seadmest.
- BSD ei ole Linux ning ChromeOS ja ChromeOS Flex ei paku kõigis olukordades samu võimalusi.

### Mõtle õpitule

1. Leia oma arvuti või telefoni süsteemi täpne nimetus ja versioon. Millist lisateavet peaksid veel teadma, et valida sobiv seadistusjuhend?
2. Too üks näide olukorrast, kus kaks sarnase välimusega süsteemi ei võimalda teha sama haldustoimingut.
3. Milline eristus vajab veel harjutamist: versioon ja väljaanne, distributsioon ja töölaud või Android ja AOSP? Selgita erinevust oma sõnadega.

## Teema sõnastik

| Mõiste | Selgitus |
| --- | --- |
| Süsteemiperekond (*OS family*) | Ühine nimetus seotud operatsioonisüsteemidele, näiteks Windowsi perekond. |
| Toode (*product*) | Konkreetne operatsioonisüsteem või tooteliin, näiteks Windows 11. |
| Versioon või väljalase (*version*, *release*) | Tähis, mis eristab tarkvara arengu ja uuendamise etappe. |
| Väljaanne (*edition*) | Sama toote variant, mille võimalused või kasutustingimused võivad erineda. |
| Järgunumber (*build number*) | Konkreetse tarkvarajärgu tähis, mis täpsustab kasutatavat süsteemi. |
| Distributsioon (*distribution*) | Tuumast, süsteemitööriistadest ja valitud tarkvarast koostatud operatsioonisüsteemi tervik. |
| Töölauakeskkond (*desktop environment*) | Graafiline töökeskkond koos akende, menüüde, paneelide ja tööriistadega. |
| Kindlate väljalasetega mudel (*fixed release*) | Arendusmudel, kus avaldatakse eristatavad süsteemiversioonid ja uuele põhiversioonile üleminek toimub eraldi. |
| Pika toega väljalase; LTS (*Long-Term Support*) | Väljalase, mille hooldust ja turvaparandusi pakutakse pikema kindlaksmääratud perioodi jooksul. |
| Pidev uuendamine (*rolling release*) | Arendusmudel, milles süsteemi komponente uuendatakse jooksvalt. |
| Microsoft Entra ID | Organisatsioonide pilvepõhine identiteediteenus, millega saab siduda kasutajaid ja seadmeid. |
| Kaugtöölaud (*Remote Desktop*) | Võimalus kasutada teise arvuti graafilist töökeskkonda võrgu kaudu. |
| BitLocker | Windowsi ketta krüpteerimise tehnoloogia, mille haldusvõimalused sõltuvad väljaandest. |
| Server Core | Windows Serveri installimisvalik ilma tavapärase graafilise töölauata. |
| Desktop Experience | Windows Serveri installimisvalik, mis sisaldab graafilist töölauda. |
| Apple silicon | Apple'i enda arendatud kiipide nimetus; Macides kasutatakse näiteks M-seeria kiipe. |
| AOSP (*Android Open Source Project*) | Androidi avatud lähtekoodiga alus ja selle arendamise projekt. |
| BSD süsteem | Oma tuuma ja põhikomponentidega Unixilaadne operatsioonisüsteem, näiteks FreeBSD. |
| ChromeOS Flex | ChromeOS-iga seotud operatsioonisüsteem toetatud PC- ja Mac-arvutitele, mille võimalused erinevad ChromeOS-ist. |

## Allikad ja lisalugemine

1. [Microsoft: Windows 11 Home'i ja Pro võrdlus](https://www.microsoft.com/en-us/windows/compare-windows-11-home-vs-pro-versions){ target="_blank" rel="noopener" }.
2. [Microsoft Learn: Microsoft Entra ID-ga liidetud seadmed](https://learn.microsoft.com/en-us/entra/identity/devices/concept-directory-join){ target="_blank" rel="noopener" }.
3. [Microsoft Learn: Windows Serveri väljaannete võrdlus](https://learn.microsoft.com/en-us/windows-server/get-started/editions-comparison){ target="_blank" rel="noopener" }.
4. [Debian: süsteemi olemus ja distributsioon](https://www.debian.org/doc/manuals/debian-faq/basic-defs.en.html){ target="_blank" rel="noopener" }.
5. [Ubuntu: väljalasete elutsükkel](https://ubuntu.com/about/release-cycle){ target="_blank" rel="noopener" }.
6. [Fedora projekt: süsteemivariandid](https://fedoraproject.org/){ target="_blank" rel="noopener" }.
7. [Apple: macOS-i versiooni ja ühilduvuse tuvastamine](https://support.apple.com/en-us/109033){ target="_blank" rel="noopener" }.
8. [Android Open Source Project: AOSP ülevaade](https://source.android.com/docs/setup/about){ target="_blank" rel="noopener" }.
9. [FreeBSD: Linuxi ja FreeBSD erinevused](https://docs.freebsd.org/en/books/faq/){ target="_blank" rel="noopener" }.
10. [Google: ChromeOS-i ja ChromeOS Flexi erinevused](https://support.google.com/chrome/a/answer/11542901){ target="_blank" rel="noopener" }.

---
*Õppematerjali koostaja: Priit Paap, 2026*
