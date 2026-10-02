# Serverid, pilv ja töökoht

Vaata, kus teenused töötavad, kuidas andmed on korraldatud ja kuidas süsteemi osade sõltuvused aitavad tõrget uurida.

## Teema tulemusena oskad

- Tuua näiteid serverirollidest.
- Eristada kohalikke teenuseid ja pilveteenuseid.
- Selgitada rakenduse ning selle andmete erinevust.
- Kirjeldada Moodle’i kasutamiseks vajalikke osi.
- Kitsendada lihtsa tõrke võimalikke põhjuseid.
- Seostada süsteemi osad töökoha vajadustega.

Eelnevas materjalis vaatasime kasutajat, klientseadet, OS-i, rakendust ja võrku. Nüüd vaatame teenuse poolt ning seda, kuidas osade sõltuvused mõjutavad kasutaja tööd.

## Server ja serverirollid

Server pakub teistele süsteemi osadele teenust. Sõnaga „server“ võidakse mõelda teenust pakkuvat tarkvara või arvutit, kus see töötab. Serveritarkvara võib töötada füüsilises arvutis või virtuaalmasinas, kohapeal või pilves.

**Virtuaalmasin** on tarkvara abil loodud arvutikeskkond oma operatsioonisüsteemiga. Üks füüsiline arvuti võib käitada mitut virtuaalmasinat.

| Roll | Mida see pakub? | Näide |
| --- | --- | --- |
| Veebiserver | Edastab veebisisu ning vahendab veebirakenduse päringuid | Moodle’i veebiliides |
| Failiserver | Jagab faile ja kaustu | Osakonna ühiskaust |
| Andmebaasiserver | Haldab struktureeritud andmeid ning vastab rakenduse päringutele | Kursuste ja hinnete andmed |
| Autentimisteenus | Kontrollib identiteeti | Keskne sisselogimine |
| Varundusserver | Haldab või säilitab taastamiseks vajalikke varukoopiaid | Failide ja andmebaaside koopiad |

Autentimine ja õiguste kontroll on eri ülesanded, kuigi sama identiteedihalduse lahendus võib mõlemat toetada. Üks server võib täita mitu rolli.

## Pilveteenus

Pilveteenusega kasutatakse võrgu kaudu arvutusressursse või rakendusi. Teenust saab tavaliselt tellida vajaduse järgi ning ressursse paindlikult suurendada või vähendada. Pilv kasutab endiselt füüsilisi servereid ja võrke.

Avaliku pilveteenuse infrastruktuuri haldab teenusepakkuja oma andmekeskustes. Olemas on ka **privaatpilv**, mis on mõeldud ühe organisatsiooni kasutuseks ja võib asuda tema enda infrastruktuuris.

!!! note "Pea meeles"

    **Teise asukoha server ei ole automaatselt pilv.** Pilve puhul on oluline ka ressursside ja teenuste pakkumise viis, mitte ainult seadmete aadress.

### Kes mida haldab?

| Lahendus | Teenusepakkuja roll | Organisatsiooni roll |
| --- | --- | --- |
| Oma server | Võib pakkuda kokkulepitud tugiteenuseid | Korraldab riistvara, OS-i, rakenduse ja andmete halduse |
| Pilve virtuaalserver | Haldab füüsilist infrastruktuuri ja virtualiseerimise alust | Haldab tavaliselt virtuaalmasina OS-i, rakendusi ja andmeid |
| SaaS ehk tarkvara teenusena | Haldab pakutavat rakendust ja selle alust | Haldab kontosid, õigusi, andmeid ja talle antud seadistusi |

Vastutuse täpne jaotus sõltub teenusest ja kokkuleppest. Pilv ei kaota vajadust hallata kasutajaid, ligipääse ja andmekaitset.

??? info "Näide: kooli pilvepost"

    Kui kool kasutab teenusepakkuja hallatavat pilveposti, ei pea e-posti server kooli serveriruumis olema. Kasutaja vajab endiselt seadet, ühendust, töötavat kontot ja õigusi. Teenusepakkuja ning kool jagavad haldusvastutust.

## Kohalik ja pilvepõhine IT-lahendus

| Lahendus | Näide | Mida tuleb tähele panna? |
| --- | --- | --- |
| Kohalik ehk on-premises | Kooli enda failiserver ja kasutajahaldus | Organisatsioon korraldab infrastruktuuri halduse |
| Pilvepõhised teenused | Pilvepost ja teenusena kasutatav koostöökeskkond | Ligipääs ja vastutus sõltuvad teenusest ning ühendusest |
| Segatud ehk hübriidne IT-lahendus | Kohalik failiserver koos pilveposti ja pilverakendustega | Vaja on hallata mõlema keskkonna sõltuvusi |

**Kasutaja arvuti ja printer võivad olla kohapeal ka pilvepõhise teenuse puhul.** Nende olemasolu üksi ei tee lahendust hübriidpilveks.

??? info "Mis on hübriidpilv täpsemas tähenduses?"

    Hübriidpilv ühendab erinevaid pilvekeskkondi, näiteks privaat- ja avalikku pilve, nii et nende vahel saab andmeid või rakendusi liigutada. See on täpsem mõiste kui üldine kohalike seadmete ja pilveteenuste kooskasutus. Selles tunnis piisab sellest, et oskad nimetada, kus teenused töötavad ja kes neid haldab.

## Rakendus ja andmed

Rakendus töötleb andmeid. Rakenduse uuesti paigaldamine ei taasta automaatselt kasutajate loodud sisu.

| Andmed | Näide | Miks neid vaja on? |
| --- | --- | --- |
| Kasutaja failid | Dokument, foto, esitatud ülesanne | Kasutaja loodud sisu |
| Andmebaas | Kursused, hinded, tellimused | Rakenduse struktureeritud info |
| Konfiguratsioon | Teenuse seaded ja ühendusinfo | Süsteemi toimimise taastamine |
| Logid | Sisselogimise ja vigade kirjed | Sündmuste uurimine ja veaotsing |
| Identiteedid ja õigused | Kontod, rollid ja ligipääsud | Kasutajate eristamine ja andmete kaitse |

!!! note "Pea meeles"

    **Moodle’i näide:** programmikood, andmebaas ja üles laaditud failid on eri osad. Kursuse täielikuks taastamiseks ei piisa ainult Moodle’i programmi uuesti paigaldamisest.

### Varundamine ja taastamine

Varukoopia aitab taastada andmeid pärast kustutamist, riknemist või muud probleemi. Varundus peab hõlmama olulisi andmeid ja seadeid; vajalik võib olla ka kohandatud rakenduskood. Kontrolli, et koopiast saab päriselt taastada.

**Sünkroonimine ei ole sama mis varundus.** Kui kustutus kandub automaatselt kõigisse sünkroonitud kohtadesse, ei pruugi sinna jääda taastatavat koopiat. Versiooniajalugu ja taastamisvõimalused sõltuvad kasutatavast teenusest.

## Mis juhtub Moodle’i materjali avamisel?

Järgnev on lihtsustatud näide. Moodle võib töötada kohalikus serveris või teenusepakkuja juures. Osa andmeid võib olla juba brauseri vahemälus ning sisselogimine võib olla varasemast kehtiv.

| Samm | Mis toimub? | Vajalik osa |
| --- | --- | --- |
| 1 | Õppija käivitab brauseri | Klientseade, OS ja brauser |
| 2 | Seade saab teenuseni viiva võrguühenduse | Kohtvõrk; vajadusel Internet |
| 3 | Vajadusel leitakse teenuse nimele vastav IP-aadress | DNS ehk nimelahendus |
| 4 | Brauser loob ühenduse ja saadab päringu | Klient-server suhtlus |
| 5 | Rakendus kontrollib sisselogimise olekut ja ligipääsu | Autentimine ja õigused |
| 6 | Rakendus loeb kursuseinfot ning vajalikke faile | Andmebaas ja failid |
| 7 | Vastus jõuab brauserisse ja materjal kuvatakse | Server, võrk ja klient |

**DNS** aitab seostada teenuse nime võrgu aadressiga. Siin piisab selle rolli mõistmisest; seadistamist ja veebiprotokolle õpime võrgu teemas.

## „Moodle ei tööta“ on sümptom

Kasutaja kirjeldus näitab, mida ta märkas. See ei ütle veel, milline osa on katki. Alusta lihtsatest kontrollidest ja võrdle olukordi.

1. **Mis täpselt ebaõnnestub?** Kas leht ei avane, sisselogimine ei õnnestu või puudub ligipääs ühele kursusele?
2. **Kui ulatuslik probleem on?** Kas sama juhtub teistel kasutajatel ja teistes seadmetes?
3. **Mis veel töötab?** Kas avanevad teised veebilehed ja teenused? Kas sama probleem esineb teises brauseris?
4. **Mis muutus?** Millal probleem algas ja milline veateade täpselt kuvatakse?

| Tähelepanek | Mida see aitab uurida? | Mida see veel ei tõesta? |
| --- | --- | --- |
| Probleem on ainult ühes brauseris | Brauseri seaded, vahemälu või seanss | Et kõik muud süsteemi osad on kindlasti korras |
| Sisselogimine õnnestub, üks kursus ei avane | Kursusele registreerimine, roll ja õigused | Et põhjus on kindlasti õigustes |
| Ükski väline veebileht ei avane | Ühendus, DNS või ligipääsupiirang | Et Internet kogu koolis on maas |
| Sama viga esineb mitmel kasutajal | Ühine teenus või ühine võrgutee | Et veebiserver on kindlasti rikki läinud |
| Leht kuvab andmete laadimise vea | Rakendus, andmebaas, failid ja serverilogid | Et andmebaas on ainus võimalik põhjus |

Haldur saab vajadusel kontrollida teenuste olekut, nimelahendust ja logisid. Kogu veateade ja probleemi ulatus on kasulikum info kui juhuslik seadistuste muutmine.

??? info "Miks ei pruugi serveri IP-aadressi brauserisse kirjutamine aidata?"

    HTTPS ja veebiserveri seadistus võivad vajada õiget teenusenime. IP-aadressi kaudu saadud viga ei tõesta, et teenus on maas või et DNS töötab valesti. Nimelahendust kontrollitakse sobiva vahendiga eraldi.

## Sõltuvused ja töökindlus

Teenuse kasutamine sõltub mitmest osast. Rikke mõju oleneb sellest, millised osad kasutavad rikkis komponenti ja kas olemas on asendus.

| Probleemne osa | Võimalik mõju |
| --- | --- |
| Kasutaja seade | Üks kasutaja ei saa töötada |
| Ühine võrguühendus | Mitu kasutajat ei pääse vajalikele teenustele ligi |
| Autentimisteenus | Uued sisselogimised võivad ebaõnnestuda |
| Andmebaas | Rakendus ei saa vajalikke andmeid lugeda või muuta |
| Server või pilveteenus | Teenus võib olla paljudele kättesaamatu |
| Varundus | Igapäevatöö võib jätkuda, kuid hilisem taastamine on ohus |

Varundus toetab taastamist, kuid ei taga ise katkematut teenust. Töökindlust võivad parandada dubleerimine ja asenduslahendused; nende seadistamist selles tunnis ei käsitleta.

## Töökoht on rohkem kui arvuti

Töökohta kavandades alusta kasutaja ülesannetest. Vajalikud on ka kontod, rakendused, ühendused, ligipääsud ja andmete kaitse.

| Kasutaja vajadus | Mida tuleb kontrollida? |
| --- | --- |
| Saab sisse logida | Konto, autentimisviis ja vajalikud õigused |
| Saab tööülesannet teha | Seade, OS ja sobiv rakendus |
| Pääseb ühiskaustale ligi | Ühendus, failiteenus ja õigused |
| Saab kasutada e-posti | Klientrakendus või brauser, konto, ühendus ja postiteenus |
| Saab printida | Printer, ühendus, sobiv tugi OS-is ja vajadusel printimisteenus |
| Saab andmeid taastada | Varunduse ulatus, säilitus ja kontrollitud taastamine |

### Lühike mõtlemisülesanne

Vali üks tegevus, näiteks ühiskausta faili avamine. Nimeta vähemalt viis vajalikku osa ja üks võimalik tõrge. Selgita, millise võrdluse või kontrolliga alustaksid. Tehnilist seadistamist pole vaja.

## Kokkuvõte

- Serverit määrab pakutav teenus, mitte tema välimus või asukoht.
- Pilves jaguneb haldusvastutus teenusepakkuja ja kliendi vahel.
- Rakenduse paigaldamine ja andmete taastamine on eri ülesanded.
- Veaotsing kitsendab võimalikke põhjuseid tähelepanekute abil.
- Töökorras arvuti üksi ei taga kogu töökoha toimimist.

### Mõtle õpitule

Milline osa oli varem ebaselge? Nimeta ühe igapäevase teenuse sõltuvused. Millise kolme küsimusega alustaksid, kui kasutaja ütleb „teenus ei tööta“?

## Teema sõnastik

| Mõiste | Selgitus |
| --- | --- |
| Serveriroll | Serveri pakutav teenus või ülesanne. |
| Virtuaalmasin | Tarkvara abil loodud arvutikeskkond oma OS-iga. |
| Pilveteenus | Võrgu kaudu kasutatav, vajaduse järgi pakutav arvutusressurss või rakendusteenus. |
| On-premises | Organisatsiooni enda infrastruktuuris töötav lahendus. |
| Hübriidne IT-lahendus | Kohalike teenuste ja pilveteenuste kooskasutus. |
| Andmebaas | Korraldatud andmekogum, mida rakendused saavad lugeda ja muuta. |
| Konfiguratsioon | Süsteemi toimimist määravad seaded. |
| Logi | Sündmuste ja tegevuste kirjed. |
| Varundus | Taastamiseks mõeldud koopiate tegemine ja säilitamine. |
| DNS ehk nimelahendus | Seostab nimesid muu hulgas IP-aadressidega. |
| Sõltuvus | Ühe osa toimimise vajadus teise osa järele. |
| Sümptom | Kasutaja või halduri märgatud tõrke ilming. |

## Allikad ja lisalugemine

- [MDN — veebiteenuse osad ja nimelahendus](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Web_standards/How_the_web_works)
- [NIST — pilvandmetöötluse määratlus](https://csrc.nist.gov/pubs/sp/800/145/final)
- [Microsoft Learn — jagatud vastutus pilves](https://learn.microsoft.com/en-us/azure/security/fundamentals/shared-responsibility)
- [MoodleDocs — Moodle’i andmed ja varundus](https://docs.moodle.org/en/Site_backup)

