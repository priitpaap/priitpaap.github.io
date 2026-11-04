# Windowsi litsentsimine ja aktiveerimine

Windowsi kasutamiseks on vaja sobivat **litsentsi** ehk kasutusõigust. IT-spetsialist peab oskama selgitada, kuidas see õigus saadakse, millisele seadmele või kasutajale see kehtib ning kuidas kontrollida Windowsi aktiveerimist. Neid teadmisi läheb vaja arvuti ostmisel, Windowsi paigaldamisel ja organisatsiooni arvutite haldamisel.

Materjali näited lähtuvad **Windows 11-st**. Konkreetsed kasutusõigused määravad kasutatava toote litsentsitingimused ja organisatsiooni puhul ka sõlmitud leping.

!!! info "Õpieesmärgid"

    Pärast materjali läbimist oskad:

    - eristada litsentsi, tootevõtit ja aktiveerimist;
    - võrrelda OEM- ja jaemüügilitsentsi ning selgitada organisatsiooni litsentsimise põhimõtet;
    - eristada Windowsi väljaannet ja litsentsi soetamise viisi;
    - kontrollida aktiveerimisolekut ning selgitada taaspaigalduse ja riistvaravahetuse mõju;
    - leida kasutusõiguse kontrollimiseks sobivad tingimused ja dokumendid.

## 1. Litsents, tootevõti ja aktiveerimine

Need kolm mõistet on omavahel seotud, kuid vastavad erinevatele küsimustele.

| Mõiste | Millisele küsimusele vastab? |
| --- | --- |
| **Litsents** | Millistel tingimustel tohin tarkvara kasutada? |
| **Tootevõti** | Millist koodi saab kasutada Windowsi aktiveerimiseks? |
| **Aktiveerimine** | Kas Windowsi aktiveerimise tehniline kontroll on selles seadmes õnnestunud? |

Tootevõti on tavaliselt 25 märgist koosnev kood, näiteks `XXXXX-XXXXX-XXXXX-XXXXX-XXXXX`. Windows võib aktiveeruda ka **digitaallitsentsiga**, ilma et kasutaja peaks tootevõtit sisestama.

!!! warning "Aktiveerimine ei asenda kasutusõigust"

    Teade „Windows on aktiveeritud” ei tõenda üksinda, et kogu litsentsikasutus vastab tingimustele. Kontrollida tuleb ka litsentsi päritolu, lubatud kasutusviisi ja kasutajate või seadmete arvu.

Näiteks organisatsiooni tootevõtme toimimine koduarvutis ei tähenda automaatselt, et organisatsioon on andnud õiguse seda seal kasutada.

## 2. Kuidas Windowsi litsents saadakse?

### 2.1. OEM: Windows koos arvutiga

**OEM-litsents** (*Original Equipment Manufacturer*) on tavaliselt uue arvutiga kaasa saadud Windowsi kasutusõigus. Arvuti tootja või paigaldaja on Windowsi seadmele ette valmistanud.

Microsofti tavatingimustes on eelpaigaldatud Windows seotud algse seadmega ning selle teisele kasutajale üleandmine toimub koos seadmega. Edasimüügi ja üleandmise puhul tuleb arvestada ka kohaldatava õigusega; tingimustes on riigipõhiseid erisusi.

**Näide:** ostad sülearvuti, mille hinnas sisaldub Windows 11 Pro. Selle arvuti kasutamiseks ei ole tavaliselt vaja eraldi Windowsi jaemüügilitsentsi osta.

### 2.2. Jaemüügilitsents: Windows eraldi ostuna

**Jaemüügilitsents** (*Retail*, vahel ka *FPP — Full Packaged Product*) ostetakse Windowsi eraldi tootena. See võib olla digitaalne ost või füüsiline pakend; litsentsi liik ei sõltu sellest, kas kaasas on USB-mälupulk.

Microsofti eraldi ostetud Windowsi tavatingimused lubavad tarkvara teise seadmesse üle viia, järgides tingimusi ja eemaldades selle eelmisest seadmest. Üleviimine ei anna õigust sama litsentsi mitmes seadmes jagada.

### 2.3. Organisatsiooni litsentsid ja tellimused

Ettevõte või õppeasutus võib hankida Windowsi kasutusõigusi organisatsioonile mõeldud lepingute ja tellimuste kaudu. Litsents võib olla seotud **seadme** või **kasutajaga**. Tähtajalise tellimuse kasutusõigus sõltub tellimuse kehtimisest.

Organisatsiooni puhul tuleb teada, millist toodet ja kasutusõigust leping sisaldab. Näiteks Windows Enterprise E3/E5 tellimus ja arvutiga kaasa tulnud Windows Pro litsents täidavad erinevat rolli.

!!! example "Litsentsi valimine algab vajadusest"

    Väikese ettevõtte uute tööarvutite ostul kontrollitakse esmalt, kas Windows Pro on hinnas. Seejärel hinnatakse, kas halduse ja turbe jaoks on vaja täiendavaid kasutusõigusi või teenuseid. Ainult arvutite arvu teadmisest sobiva lahenduse valimiseks ei piisa.

## 3. Windowsi väljaanne ja litsentsi liik

**Väljaanne** (*edition*) näitab Windowsi funktsioonide komplekti. **Soetamisviis** näitab, kuidas kasutusõigus saadi. Näiteks Windows 11 Pro võib olla arvutisse eelpaigaldatud või eraldi ostetud.

| Väljaanne | Tüüpiline kasutus |
| --- | --- |
| **Windows 11 Home** | Igapäevased kodused toimingud ja rakendused. |
| **Windows 11 Pro** | Tööarvutid, mis vajavad täiendavaid haldus- ja turbevõimalusi. |
| **Windows 11 Enterprise** | Organisatsioonid, mis vajavad ulatuslikumaid haldus- ja turbefunktsioone ning vastavaid kasutusõigusi. |
| **Windows 11 Education** | Õppeasutuste jaoks mõeldud väljaanne ja litsentsimisvõimalused. |

Näiteks Pro toetab liitumist Active Directory domeeniga ning kaugtöölauaühenduse vastuvõtmist Windowsi Remote Desktopi abil. Home saab olla kaugtöölaua klient, kuid ei paku sama vastuvõtufunktsiooni.

**Home'i litsents ei anna automaatselt Pro kasutusõigust.** Samuti ei muuda ainuüksi Enterprise'i paigaldusfaili olemasolu arvutit Enterprise'i kasutamiseks litsentsituks.

### 3.1. Täislitsents ja täienduslitsents

Täislitsents annab vastava toote kasutusõiguse ilma nõudeta omada varasemat sobivat Windowsi litsentsi. Täienduslitsents (*upgrade license*) eeldab sobivat aluslitsentsi.

Windowsi töölauaversioonide kohalikuks kasutamiseks organisatsiooni kommertslitsentsimise kaudu hangitavad õigused on üldjuhul **täiendusõigused**. Sobiva aluslitsentsi saab näiteks arvutiga kaasa tulnud OEM-litsentsina või jaemüügist. Nõutav alus sõltub konkreetsest pakkumisest.

**Näide:** ettevõttel on Windows 11 Pro litsentsidega arvutid. Sobiva Enterprise'i tellimuse ja tehniliste eelduste täitmisel saab kasutada Enterprise'i väljaannet. Ainult Enterprise'i tellimuse olemasolust ei saa järeldada, et iga operatsioonisüsteemita arvuti on kohe kohaliku Windowsi kasutamiseks litsentsitud.

## 4. Aktiveerimisoleku kontrollimine

Windows 11 aktiveerimisoleku leiad asukohast **Sätted → Süsteem → Aktiveerimine** (*Settings → System → Activation*). Vaata paigaldatud väljaannet ja aktiveerimisolekut.

| Teade või olukord | Kuidas seda mõista? |
| --- | --- |
| Windows on aktiveeritud digitaallitsentsiga | Seadmel on digitaalne aktiveerimisõigus. |
| Digitaallitsents on seotud Microsofti kontoga | Kontoseos võib aidata Windowsi pärast olulist riistvaramuudatust uuesti aktiveerida. |
| Windows on aktiveeritud organisatsiooni aktiveerimisteenuse abil | Aktiveerimine toimub organisatsiooni lahenduse kaudu. |
| Windows ei ole aktiveeritud | Uurida tuleb veateadet, litsentsi olemasolu ja paigaldatud väljaande sobivust. |

Isikliku Microsofti kontoga sidumine ei muuda OEM-litsentsi jaemüügilitsentsiks ega anna lisakasutusõigusi. Kooli või tööandja arvutis järgitakse organisatsiooni kontode ja aktiveerimise korraldust.

### 4.1. Aktiveerimine organisatsioonis

Organisatsioonis võib aktiveerimine toimuda automaatselt. Levinud lahendused on järgmised:

| Lahendus | Põhimõte |
| --- | --- |
| **KMS — Key Management Service** | Seadmed pöörduvad organisatsiooni aktiveerimisteenuse poole ning uuendavad aktiveerimist perioodiliselt. |
| **MAK — Multiple Activation Key** | Organisatsiooni võtit kasutatakse lubatud arvu aktiveerimiste tegemiseks Microsofti aktiveerimisteenuse kaudu. |
| **Tellimuspõhine aktiveerimine** | Sobiva litsentsiga kasutaja ja seadme puhul võimaldab tellimus kasutusele võtta näiteks Enterprise'i väljaande. |

Kasutusel võib olla ka Active Directory põhine aktiveerimine. Kõik need on aktiveerimise korraldamise viisid; kasutusõigus peab tulema sobivast litsentsist.

## 5. Taaspaigaldus, arvutivahetus ja virtuaalmasinad

### 5.1. Windowsi taaspaigaldamine samasse arvutisse

Kui seadmel on digitaallitsents, saab samasse seadmesse sama Windowsi väljaande tavaliselt uuesti paigaldada ilma tootevõtit sisestamata. Võrguühenduse tekkimisel aktiveerub see üldjuhul automaatselt.

Enne paigaldamist pane kirja kasutatav väljaanne ja aktiveerimisolek ning varunda andmed. Kui seadmel on Home'i kasutusõigus, vali taaspaigaldamisel Home; Pro valimine ei anna Pro litsentsi.

### 5.2. Riistvara või kogu arvuti vahetamine

Oluline riistvaramuudatus, eriti emaplaadi vahetus, võib põhjustada vajaduse Windows uuesti aktiveerida. Sobival juhul aitab aktiveerimise tõrkeotsing ja varem seotud Microsofti konto. Tootja tehtud garantiiremondi puhul võib abi saada arvuti tootjalt.

**Taasaktiveerimine ja litsentsi üleviimine on erinevad toimingud.** Esimene lahendab aktiveerimise tehnilise seisundi; teise lubatavus sõltub kasutusõigusest.

### 5.3. Windows virtuaalmasinas

Virtuaalmasin on litsentsimise seisukohalt samuti arvestatav Windowsi kasutuskoht. Hostarvuti Windowsi litsents ei anna automaatselt piiramatut õigust käivitada täiendavaid Windowsi virtuaalmasinaid. Õigused sõltuvad litsentsist ja lepingust.

!!! example "Windows kooli laboris"

    Õppija loob VMware'i laboris Windowsi virtuaalmasina. Enne selle kasutamist tuleb teada, milline kooli leping, hindamisversioon või muu kasutusõigus seda katab. Labori tehniline võimalus masinaid kloonida ei määra lubatud Windowsi eksemplaride arvu.

## 6. Varasema versiooni kasutamine

**Varasema versiooni kasutamise õigus** (*downgrade rights*) võib lubada litsentsitud Windowsi asemel kasutada varasemat versiooni. See sõltub väljaandest ja litsentsitingimustest. Organisatsiooni kommertslitsentsidel on sellised õigused, jaemüügi Windowsi litsentsidel üldjuhul mitte; OEM-i puhul tuleb vaadata vastava toote tingimusi.

Allaviimisõigus ei tähenda õigust kasutada sama litsentsi alusel mõlemat versiooni korraga. Samuti tuleb eraldi hinnata varasema versiooni turvatoe olemasolu: kasutusõigus ja turvauuenduste saamine on erinevad küsimused.

## 7. Kasutusõiguse kontrollimise tööjärjekord

1. **Selgita vajadus.** Millist väljaannet on vaja ning kas Windows töötab füüsilises arvutis või virtuaalmasinas?
2. **Leia litsentsi päritolu.** Kas see saadi arvutiga, osteti eraldi või anti organisatsiooni kaudu?
3. **Kontrolli tingimusi.** Millisele seadmele või kasutajale õigus kehtib ja kas kavandatud kasutus on lubatud?
4. **Kontrolli aktiveerimist.** Vaata väljaannet, aktiveerimisolekut ja võimalikke veateateid.
5. **Säilita tõendid.** Hoia alles ostudokument või organisatsiooni litsentsi määramist tõendav teave.

**Näide:** kasutatud arvuti kuulutuses on kirjas „Windows aktiveeritud”. Enne ostmist tasub küsida, milline litsents seadmega kaasa tuleb ja millised dokumendid seda tõendavad. Aktiveerimisakna kuvatõmmis vastab ainult osale vajalikest küsimustest.

## 8. Enesekontroll

Vasta kõigepealt ise. Seejärel ava vastus ja võrdle oma põhjendust.

### 1. Kas aktiveeritud Windows tõendab üksinda nõuetekohast kasutusõigust?

??? question "Vaata vastust"

    Ei. Lisaks aktiveerimisolekule tuleb kontrollida litsentsi päritolu ja seda, kas konkreetne kasutus vastab tingimustele. Näiteks organisatsiooni võti ei anna automaatselt õigust seda isiklikus arvutis kasutada.

### 2. Arvutil on Windows 11 Home'i digitaallitsents. Kas taaspaigaldamisel Pro valimine annab Pro kasutusõiguse?

??? question "Vaata vastust"

    Ei. Taaspaigaldamisel tuleb valida olemasolevale õigusele vastav väljaanne. Pro kasutamiseks on vaja sobivat Pro litsentsi või täiendusõigust.

### 3. Mida kontrollid enne olemasoleva Windowsi litsentsi uude arvutisse üleviimist?

??? question "Vaata vastust"

    Selgitan välja litsentsi päritolu ning loen üleviimise tingimusi. Eelpaigaldatud ja eraldi ostetud Windowsi õigused võivad erineda. Samuti kontrollin, et kasutus ei jääks lubamatult mõlemas seadmes jätkuma.

### 4. Kas litsentsitud Windowsi hostarvutis võib piiramatult Windowsi virtuaalmasinaid käivitada?

??? question "Vaata vastust"

    Ei. Virtuaalmasinate kasutusõigused tuleb eraldi välja selgitada. Organisatsiooni leping võib anda täiendavaid õigusi, kuid neid ei saa eeldada üksnes hostarvuti litsentsi olemasolust.

## Kokkuvõte

- Litsents määrab kasutusõiguse; aktiveerimine on tehniline toiming.
- OEM, jaemüük ja organisatsiooni litsentsimine on erinevad viisid kasutusõiguse saamiseks.
- Windowsi väljaanne ja litsentsi soetamisviis tuleb teineteisest eristada.
- Taaspaigalduse, arvutivahetuse ja virtuaalmasinate puhul kontrolli olemasolevate õiguste sobivust.
- Kasutusõiguse hindamiseks on vaja nii litsentsi puudutavat teavet kui ka aktiveerimisolekut.

## Teema sõnastik

| Mõiste | Selgitus |
| --- | --- |
| **Litsents — license** | Tarkvara kasutusõigus kindlaksmääratud tingimustel. |
| **OEM-litsents** | Tavaliselt seadmega koos saadud, selle seadmega seotud kasutusõigus. |
| **Jaemüügilitsents — retail / FPP** | Eraldi tootena ostetud litsents, mille kasutamise ja üleviimise määravad tingimused. |
| **Tootevõti — product key** | Windowsi aktiveerimiseks kasutatav 25-märgiline kood. |
| **Aktiveerimine — activation** | Windowsi tootevõtme või digitaalse aktiveerimisõiguse tehniline kontroll seadmes. |
| **Digitaallitsents — digital license** | Seadmega seotud aktiveerimisviis, mis ei nõua tavaliselt tootevõtme käsitsi sisestamist. |
| **Täienduslitsents — upgrade license** | Kasutusõigus, mis eeldab sobiva aluslitsentsi olemasolu. |

## Allikad ja lisalugemine

1. [Microsoft Support — Activate Windows](https://support.microsoft.com/en-us/windows/activation/activate-windows).
2. [Microsoft — Windows 11 litsentsitingimused (PDF)](https://www.microsoft.com/content/dam/microsoft/usetm/documents/windows/11/oem-%28pre-installed%29/Useterms_OEM_Windows_11_EnglishGreatBritain.pdf). 
3. [Microsoft Learn — Windows subscription activation](https://learn.microsoft.com/en-us/windows/deployment/windows-subscription-activation).
4. [Microsoft — Windows 11 Home'i ja Pro võrdlus](https://www.microsoft.com/en-us/windows/compare-windows-11-home-vs-pro-versions).
5. [Microsoft — Windows 11 Qualifying Operating System Licensing Guidance](https://www.microsoft.com/licensing/guidance/Windows-11-Qualifying-Operating-System).
6. [Microsoft Learn — Activate clients running Windows](https://learn.microsoft.com/en-us/windows/deployment/volume-activation/activate-windows-clients-vamt).
7. [Microsoft Support — Reactivating Windows after a hardware change](https://support.microsoft.com/en-us/windows/activation/reactivating-windows-after-a-hardware-change).
8. [Microsoft — Downgrade Rights Licensing Guidance](https://www.microsoft.com/licensing/guidance/Downgrade-rights).

---

*Õppematerjali koostaja: Priit Paap, 2026*
