# Võrgukaabli valmistamine ja kontrollimine

WiFi pääsupunkt, lauaarvuti või võrguprinter võib vajada kaabliga ühendust. Korralikult valmistatud kaabel loob füüsilise ühenduse seadmete vahel. Selles materjalis uurime keerdpaarkaabli ehitust, soonte ühendamise skeemi ning kaabli kontrollimist. Praktilises töös valmistad koos kaaslasega ühe ühenduskaabli.

!!! info "Õpieesmärgid"

    Pärast materjali läbimist oskad:

    - kirjeldada keerdpaarkaabli ehitust;
    - eristada ühenduskaabli ja püsikaabelduse ülesandeid;
    - kasutada T568B ühendusskeemi ja kontrollida soonte järjekorda;
    - valida õpetaja abiga kaablile sobiva pistiku;
    - valmistada juhendamisel ühenduskaabli ning kasutada testrit;
    - selgitada lihtsa kaablitestri võimalusi ja piiranguid.

## 1. Millest võrgukaabel koosneb?

Kursuses kasutatav **keerdpaarkaabel** (*twisted-pair cable*) sisaldab nelja juhtmepaari ehk kaheksat isoleeritud soont. Igas paaris on tavaliselt üks ühevärviline ja üks sama värvi valgekirju soon.

| Paar | Soonte värvid |
| --- | --- |
| Oranž paar | Valge-oranž ja oranž. |
| Roheline paar | Valge-roheline ja roheline. |
| Sinine paar | Valge-sinine ja sinine. |
| Pruun paar | Valge-pruun ja pruun. |

Paaris olevad sooned on omavahel keerutatud. Keerdumine aitab vähendada häirete mõju ning paaridevahelist häirimist. Seepärast ei ole soonte paigutus pistikus juhuslik ja paaride keerde ei avata rohkem, kui ühendamiseks vaja.

Kaabli väliskest kaitseb sooni mehaaniliste vigastuste eest. Kaablil võib olla ka varjestus või muid konstruktsiooniosi. Kaabli kategooria, näiteks **Cat 5e** või **Cat 6**, kirjeldab kindlaid omaduste nõudeid. Ainult pistiku paigaldamine ei muuda suvalist kaablit sobivaks igale ühenduskiirusele.

### Ühenduskaabel ja püsikaabeldus

**Ühenduskaabel** ehk **patch-kaabel** ühendab näiteks arvuti seinapesaga või patchpaneeli kommutaatoriga. Seda liigutatakse ja vahetatakse vajaduse järgi.

**Püsikaabeldus** paigaldatakse hoonesse ning ühendatakse tavaliselt seinapesa ja patchpaneeliga. Patchpaneel koondab eri kohtadest tulevate kaablite ühendused; see ise ei tee kommutaatori tööd.

| Kaabelduse osa | Näide |
| --- | --- |
| Ühenduskaabel | Arvuti ja seinapesa vaheline kaabel. |
| Püsikaabel | Seinapesa ja võrgukapis oleva patchpaneeli vaheline kaabel. |
| Ühenduskaabel võrgukapis | Patchpaneeli porti kommutaatoriga ühendav kaabel. |

Püsikaablil on sageli ühesoonelised metalljuhid, painduval ühenduskaablil peenkiududest juhid. Pistik peab sobima konkreetse kaabli juhtide, mõõtmete ja konstruktsiooniga. Õppekaabli ja pistikud annab õpetaja sobiva komplektina.

## 2. Pistik ja töövahendid

Etherneti keerdpaarkaabliga kasutatakse tavaliselt kaheksa kontaktiga pistikut, mida kutsutakse **RJ45-pistikuks**. Täpsemalt on levinud pistiku kuju 8P8C. Pistikul olev lukustuskeel aitab selle pesas paigal hoida.

Kaheksa kontakti ühendatakse kaabli kaheksa soonega. **Otsastamine** tähendab kaabli otsa ühendamiseks ettevalmistamist ja pistiku või muu ühendusdetaili paigaldamist.

| Vahend | Milleks seda kasutatakse? |
| --- | --- |
| Kaabel ja sobivad pistikud | Ühenduskaabli valmistamiseks. |
| Kaablikoorija | Väliskesta eemaldamiseks sooni kahjustamata. |
| Lõikur | Kaabli ja soonte sobivaks lõikamiseks. |
| Otsastamistangid | Pistiku kontaktide ja tõmbetõkise kinnitamiseks vastava pistikutüübi puhul. |
| Kaablitester koos vastuvõtja või kaugotsaga | Kaabli ühenduste kontrollimiseks. |

Läbiviiguga ja kinnise otsaga pistikute paigaldus erineb. Näiteks läbiviiguga pistiku puhul peab töövahend sobima väljaulatuvate soonte õigeks lõikamiseks. Järgi õpetaja näidatud töövõtet ja kasutatava pistiku juhendit.

!!! warning "Tööohutus"

    - Otsasta kaablit, mille mõlemad otsad on võrguseadmetest lahti ühendatud.
    - Hoia sõrmed lõiketerast ja tangide liikuvate osade vahelt eemal.
    - Kasuta töövahendeid õpetaja näidatud viisil ja vajaduse korral ettenähtud kaitsevahendeid.
    - Korista lõigatud sooned ja muud jäägid kohe, et need ei satuks seadmetesse ega põrandale.
    - Ära ühenda koolivõrku ega muuda selle kaabeldust ilma õpetaja juhiseta.

## 3. T568B ühendusskeem

Soontel peab olema kindel järjekord. Levinud ühendusskeemid on **T568A** ja **T568B**. Mõlemad on kasutatavad; nende erinevus on oranži ja rohelise paari paigutuses.

Selles praktikumis valmistame otseühendusega kaabli: **mõlemas otsas kasutatakse T568B skeemi**. Soonte järjekorra päheõppimisest tähtsam on oskus skeemi õigesti lugeda ja tulemust kontrollida.

### Kuidas pistikut vaadata?

Aseta pistik nii, et **lukustuskeel on allpool ja metallkontaktid on ülalt nähtavad**. Ühendatav ots osutab sinust eemale, kaabli sisenemiskoht on sinu pool. Selles ülaltvaates on kontakt **1 vasakul** ja kontakt **8 paremal**.

Enne soonte sisestamist võrdle pistiku asendit õpetaja näidisega. Pistikku vastasküljelt vaadates näib järjekord peegelpildis.

| Kontakt vasakult paremale | Soone värv T568B skeemis |
| --- | --- |
| 1 | Valge-oranž |
| 2 | Oranž |
| 3 | Valge-roheline |
| 4 | Sinine |
| 5 | Valge-sinine |
| 6 | Roheline |
| 7 | Valge-pruun |
| 8 | Pruun |

!!! tip "Rohelise paari sooned ei ole kõrvuti"

    T568B skeemis paikneb roheline paar kontaktidel 3 ja 6. Sinine paar on nende vahel kontaktidel 4 ja 5. Ära paiguta kõiki sama paari sooni lihtsalt üksteise kõrvale.

Seinapesa ja patchpaneeli kontaktide füüsiline paigutus võib pistikust erineda. Nende otsastamisel tuleb järgida detailil olevat valitud skeemi värvimärgistust. Seda harjutame eraldi õppepäeval.

## 4. Kaabli valmistamise tööjärjekord

Õpetaja näitab kõigepealt kogu töövõtte ette. Seejärel valmistate paaris ühe kaabli: kumbki õppija otsastab ühe otsa.

1. **Vali sobiv komplekt.** Kontrolli õpetajaga kaabli, pistiku ja tangide sobivust. Lõika kaabel ülesande jaoks sobivasse pikkusesse.
2. **Eemalda väliskest.** Kasuta koorijat ja kasutatavale pistikule sobivat ettevalmistuspikkust. Ära lõika soonte isolatsiooni. Vigastatud osa lõika ära ja alusta uuesti.
3. **Valmista sooned ette.** Ava paaride keerud ainult vajalikus ulatuses. Paiguta sooned T568B järjekorda ning silu need ühendamiseks sobivalt.
4. **Valmista soonte otsad ette.** Kinnise otsaga pistiku jaoks lõika need ühtlaselt sobivasse pikkusesse. Läbiviiguga pistiku puhul järgi selle pistiku töövõtet.
5. **Sisesta sooned pistikusse.** Kontrolli, et kõik kaheksa jõuavad oma kanalisse ja nõutud sügavuseni. Väliskest peab ulatuma pistiku tõmbetõkise alla.
6. **Kontrolli enne kinnitamist.** Vaata õiges pistiku asendis värvide järjekorda, soonte paiknemist ja väliskesta asukohta. Vajaduse korral paranda enne tangidega kinnitamist.
7. **Kinnita pistik.** Kasuta õiget tangide pesa ja õpetaja näidatud võtet. Kontrolli kinnitatud otsa. Tavapärast presspistikut üldjuhul uuesti lahti ei võeta; parandamisel kasutatakse uut pistikut.
8. **Valmista teine ots ja testi.** Teises otsas kasutatakse sama T568B skeemi. Kontrollige kaablit koos testriga ning märkige tulemus üles.

!!! example "Miks peab väliskest pistikusse ulatuma?"

    Pistiku tõmbetõkis peab hoidma kaabli väliskesta. Kui kest jääb pistikust välja, võib kaabli liigutamine koormata üksikuid sooni ja kontakte. Kaabel võib alguses testriga korras näida, kuid ühendus ei ole mehaaniliselt korralik.

## 5. Kaabli kontrollimine testriga

Enne testimist veendu, et kaabel pole ühendatud ruuteri, kommutaatori ega muu aktiivseadmega. Kasuta testrit ja selle kaugotsa õpetaja näidatud viisil.

Lihtne tester võib kontrollida, kas ühe otsa kontakt on ühendatud teise otsa õige kontaktiga. Otseühendusega kaheksa soonega õppekaablil ootame seoseid **1 → 1, 2 → 2, …, 8 → 8**. Täpne märgutulede või ekraani lugemise viis sõltub testrist.

| Tulemus või tähelepanek | Võimalik põhjus ja järgmine tegevus |
| --- | --- |
| Ühe soone ühendus puudub | Soon võib olla katkenud, liiga lühike või halvasti kontaktiga ühendatud. Vaata mõlemat otsa. |
| Kontaktide vastavus on vale | Sooned võivad olla vahetuses või skeemid erinevad. Kontrolli järjekorda mõlemas otsas. |
| Tester näitab lühist, kui seade seda tuvastab | Soonte või kontaktide vahel võib olla soovimatu ühendus. Uuri pistikuid ja kaablit. |
| Ühendused näivad korras, kuid kest ei ulatu tõmbetõkise alla | Kaabel vajab mehaanilise kinnituse parandamist. Test ei asenda vaatlust. |
| Kaabel näib ühenduste järgi korras, kuid töötab ebakindlalt | Vajalik võib olla paaride paigutuse, kaabli omaduste või muude ühenduse osade täiendav kontroll. |

### Mida lihtne tester ei tõenda?

Lihtne ühenduste tester ei pruugi tuvastada **valesti moodustatud paare** (*split pair*). Sellisel juhul võivad kontaktid olla otsast otsani õige numbriga ühendatud, kuid signaali kandvad sooned ei kuulu ettenähtud keerdpaaridesse. Häirekindlus võib halveneda.

Samuti ei kinnita lihtne ühenduste kontroll kõiki kaabli kategooria või andmeedastuskiiruse nõudeid. Selleks kasutatakse sobivaid mõõtevahendeid. Seetõttu tuleb kontrollida nii värviskeemi, töövõtet, mehaanilist kinnitust kui ka testri tulemust.

!!! tip "Korras kaabel ja töötav internet on erinevad tulemused"

    Kaablitester kontrollib kaablit oma võimaluste piires. Interneti toimimine sõltub lisaks võrguseadmetest, seadistusest ja teenusepakkuja ühendusest. Isegi täiesti korras kaabel ei tõenda interneti olemasolu.

## 6. Praktilise töö tulemus

Pärast testimist näidake kaablit õpetajale. Õpetaja juhisel kasutage seda õppevõrgus, kontrollige ühendust ja selgitage tulemust.

Pange kirja:

- millist skeemi kasutasite mõlemas otsas;
- kes otsastas kummagi otsa;
- mida tester näitas;
- kas leidsite vea ja kuidas selle parandasite;
- mida kaabli testitulemus kinnitab ja mida see ei kinnita.

## 7. Enesekontroll

### 1. Mitu paari ja soont on selles tunnis kasutatavas kaablis?

??? success "Vaata vastust"

    Neli paari ja kaheksa soont. Iga paar koosneb kahest omavahel keerutatud soonest.

### 2. Kas T568B skeemis on rohelise paari sooned kõrvuti?

??? success "Vaata vastust"

    Ei. Need on kontaktidel 3 ja 6. Sinine paar paikneb kontaktidel 4 ja 5.

### 3. Kuidas väldid soonte järjekorra peegelpildis lugemist?

??? success "Vaata vastust"

    Hoia pistikut kokkulepitud asendis: lukustuskeel all, metallkontaktid ülalt nähtavad, ühendatav ots sinust eemal ja kaabli sisenemiskoht sinu pool. Selles vaates on kontakt 1 vasakul. Võrdle asendit õpetaja näidisega.

### 4. Kas mõlemas otsas sama juhuslik värvijärjekord on piisav?

??? success "Vaata vastust"

    Ei. Juhuslik järjekord võib rikkuda paaride paigutuse. Lihtne tester võib näidata kontaktide vastavust, kuid kaabel võib olla häiretele vastuvõtlik. Kasuta etteantud T568B skeemi.

### 5. Miks peab kaabli väliskest jääma tõmbetõkise alla?

??? success "Vaata vastust"

    Tõmbetõkis hoiab kaablit paigal ning vähendab tõmbe mõju üksikutele soontele ja kontaktidele.

### 6. Kas kõik kaheksa ühendust läbiv test tõendab, et kaabel toetab igat võrgukiirust?

??? success "Vaata vastust"

    Ei. Lihtne tester ei kontrolli kõiki elektrilisi omadusi ega pruugi leida valesti moodustatud paare. Tulemuse tähendus sõltub testri võimalustest.

### 7. Kas seinapesa otsastamisel kopeerid pistiku soonte füüsilise järjestuse?

??? success "Vaata vastust"

    Ei. Järgi seinapesal olevat valitud skeemi värvimärgistust. Selle kontaktide paigutus võib pistikust erineda.

## Kokkuvõte

- Keerdpaarkaablis on neli paari ja kaheksa soont; keerdumine aitab vähendada häirete mõju.
- Kaabel, pistik ja töövahend peavad omavahel sobima.
- Õppekaabli mõlemas otsas kasutame T568B skeemi.
- Kontrolli pistiku vaatamissuunda, soonte järjekorda ja väliskesta kinnitust.
- Testi valmistatud kaablit ning selgita tulemust testri tegelike võimaluste järgi.
- Ühenduste kontroll ei asenda kõikide kaabli tööomaduste mõõtmist ega internetiühenduse kontrolli.

### Mõtle õpitule

1. Millises tööetapis tekkis kõige rohkem küsimusi ja kuidas need lahendasid?
2. Kuidas märkaksid kaabli viga, mida sinu kasutatav tester ei näita?
3. Millises kodu või töökoha olukorras võiks ühenduskaabli valmistamise või kontrollimise oskus kasulik olla?

## Teema sõnastik

| Mõiste | Selgitus |
| --- | --- |
| Keerdpaarkaabel (*twisted-pair cable*) | Kaabel, mille juhtmed on häirete mõju vähendamiseks paarikaupa keerutatud. |
| Soon | Üks isoleeritud elektrijuht kaabli sees. |
| Ühenduskaabel ehk patch-kaabel (*patch cord*) | Näiteks seadme ja seinapesa või patchpaneeli ja kommutaatori vahel kasutatav kaabel. |
| Püsikaabeldus (*permanent cabling*) | Hoonesse püsivalt paigaldatud kaabeldus, tavaliselt seinapesade ja patchpaneeli vahel. |
| Patchpaneel (*patch panel*) | Ühenduspaneel, kuhu koondatakse kaablite otsad. |
| RJ45-pistik | Etherneti keerdpaarkaabliga levinud kaheksa kontaktiga pistiku tavanimetus. |
| Otsastamine (*termination*) | Kaabli otsa ettevalmistamine ja ühendusdetailiga ühendamine. |
| T568B | Ühendusskeem, mis määrab soonte paigutuse kontaktidel. |
| Tõmbetõkis (*strain relief*) | Kinnitus, mis vähendab tõmbe mõju soontele ja kontaktidele. |
| Kaablitester (*cable tester*) | Kaablit kontrolliv seade; kontrollide ulatus sõltub mudelist. |
| Ühenduste skeem (*wire map*) | Näitab, milline ühe kaabliotsa kontakt on ühendatud teise otsa kontaktiga. |
| Valesti moodustatud paar (*split pair*) | Viga, kus kontaktide vastavus võib olla õige, kuid keerdpaaride paigutus on vale. |

## Allikad ja lisalugemine

1. [Fluke Networks: Outlet Configurations](https://www.flukenetworks.com/knowledge-base/dsx-cableanalyzer-series/color-codes-dsx-cableanalyzer){ target="_blank" rel="noopener" } — T568A ja T568B ühendusskeemid.
2. [Fluke Networks: Split Pair Identification](https://www.flukenetworks.com/support/knowledge-base/linkiq/split-pair-identification){ target="_blank" rel="noopener" } — valesti moodustatud paarid ja nende tuvastamine.
3. [Fluke Networks: Verification Qualification and Certification](https://www.flukenetworks.com/knowledge-base/cableiq/verification-qualification-and-certification){ target="_blank" rel="noopener" } — kaabli ühenduste kontrollimise ja tööomaduste mõõtmise erinevused.

**Seotud materjalid:** [Võrgu toimimine](01_vorgu_toimimine.md) ja [Oma võrguühenduse uurimine](02_oma_vorguuhenduse_uurimine.md).

---
*Õppematerjali koostaja: Priit Paap, 2026*
