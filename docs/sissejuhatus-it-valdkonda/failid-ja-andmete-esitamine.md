# Failid ja andmete esitamine

Dokument, foto, veebileht ja programm on kõik failid. Õpi eristama faili nime, tegelikku formaati ja sisu ning mõistma, kuidas samad baidid võivad eri programmides erinevalt paista.

## Teema tulemusena oskad

- eristada failinime, faililaiendit, failitüüpi ja failiformaati;
- seostada levinud faililaiendeid sobivate programmidega;
- selgitada tekstifaili ja binaarfaili erinevust;
- kirjeldada ASCII, Unicode’i ja UTF-8 rolli;
- selgitada, miks sama HTML-fail paistab tekstiredaktoris ja brauseris erinev;
- tuua näite, kuidas standardid aitavad andmeid eri programmides kasutada.

Täiendavad teemad ja näited leiad lõpus avatavatest lisalugemise plokkidest.

## Fail, nimi ja laiend

**Fail** on nimega andmekogum. Andmekandjale salvestatud fail võib sisaldada teksti, pilti, heli, videot, programmi või muid andmeid. Arvuti jaoks koosneb faili sisu bittidest ja baitidest.

**Failinimi** võib koosneda nime põhiosast ja laiendist. Laiend on tavaliselt viimase punkti järel olev osa, mis annab vihje faili formaadi kohta.

foto

Nime põhiosa

.jpg

Faililaiend

Täielik failinimi on `foto.jpg`. Laiendit esitatakse näidetes sageli koos punktiga, näiteks `.jpg`.

| Failinimi | Nime põhiosa | Laiend | Tõenäoline sisu |
| --- | --- | --- | --- |
| `referaat.docx` | referaat | .docx | Tekstidokument |
| `foto.jpg` | foto | .jpg | Pilt |
| `andmed.csv` | andmed | .csv | Tabelandmed |
| `index.html` | index | .html | Veebilehe HTML-kood |

!!! note "Pea meeles"

    **Laiend on vihje.** Fail võib olla ilma laiendita või vale laiendiga. Faili tegeliku formaadi määrab selle sisu ja struktuur.

## Failitüüp ja failiformaat

### Failitüüp

Kirjeldab siin materjalis faili üldist otstarvet või sisu liiki: näiteks pilt, dokument, heli või video.

### Failiformaat

Määrab reeglid, mille järgi andmed on failis korraldatud. Näiteks sama pildi võib salvestada JPEG-, PNG- või WebP-vormingus.

Faili avamiseks peab programm oskama selle formaati tõlgendada. Ühte formaati võib toetada mitu programmi ning üks programm võib avada paljusid formaate.

| Failitüüp | Levinud laiendid | Võimalik programm |
| --- | --- | --- |
| Lihttekst | .txt, .md | Tekstiredaktor; Markdowni vaatur |
| Dokument | .docx, .odt, .pdf | Word / Writer; PDF-lugeja |
| Arvutustabel | .xlsx, .ods | Excel / Calc |
| Pilt | .jpg, .png, .gif, .svg, .webp | Pildivaatur või sobiv graafikaprogramm |
| Heli | .mp3, .wav, .flac | VLC või muu helipleier |
| Video | .mp4, .mkv, .webm | VLC või muu videopleier |
| Arhiiv | .zip, .7z, .tar | 7-Zip või muu arhiivitööriist |
| Veebifail | .html, .css, .js | Tekstiredaktor; brauser kasutab neid veebilehe esitamisel |
| Andmefail | .csv, .json, .xml | Tekstiredaktor või vastavat vormingut toetav rakendus |
| Programm või paigaldaja | .exe, .msi | Windowsi käivituskeskkond või paigaldusteenus |

Need on tüüpilised seosed. Näiteks brauser oskab lisaks HTML-ile näidata ka paljusid pilte ja PDF-faile.

## Ümbernimetamine ja teisendamine

### Muudan ainult nime

Nimetan faili `foto.jpg` ümber failiks `foto.png`.

Faili sisu jääb JPEG-vormingusse. PNG-laiend annab nüüd eksitava vihje.

### Teisendan formaadi

Avan JPEG-pildi sobivas programmis ja ekspordin selle PNG-vormingusse.

Programm kirjutab uue faili PNG-formaadi reeglite järgi.

!!! note "Pea meeles"

    **Ümbernimetamine muudab nime. Teisendamine muudab andmete esitust failis.** Formaati saab muuta näiteks käsuga *Save As*, *Export* või *Convert*, kui valida ka soovitud väljundvorming.

Operatsioonisüsteem valib faili topeltklõpsamisel programmi sageli laiendi järgi. Mõni programm kontrollib ka faili sisu, mistõttu vale laiendiga fail võib siiski avaneda.

## Tekstifail ja binaarfail

**Kõik failid salvestatakse bittide ja baitidena.** Tekstifaili puhul tõlgendatakse baite kindla märgikodeeringu järgi tekstina. Binaarfailis on andmed korraldatud muu formaadi reeglite järgi ning nende mõistmiseks on vaja seda formaati tundvat programmi.

### Tekstifail

Õige kodeeringuga tekstiredaktoris näed loetavat teksti, märgendeid või programmikoodi.

`.txt` `.md` `.html` `.css` `.js` `.csv` `.json` `.xml`

```text
Tere õpilane!
See on tekstifail.
```

### Binaarfail

Tekstiredaktor võib näidata üksikuid loetavaid osi, kuid tavaliselt ei ole tervik inimesele arusaadav.

`.jpg` `.png` `.mp3` `.mp4` `.exe` `.docx`

Pildi nägemiseks kasutad pildivaaturit; heli kuulamiseks helipleierit.

!!! note "Pea meeles"

    **Sisu otstarve ei määra teksti ja binaari erinevust.** DOCX-dokument võib sisaldada teksti, kuid fail ise on ZIP-põhine pakett. SVG-pilt on tavaliselt tekstipõhine XML-fail.

Kui avad PNG-pildi tekstiredaktoris ja näed arusaamatuid sümboleid, ei tähenda see tingimata, et fail on katki. Programm püüab pildi baite tõlgendada tekstina.

## ASCII, Unicode ja UTF-8

Teksti salvestamiseks on vaja kokkulepet, kuidas märgid seostuvad arvuliste väärtustega ning kuidas need väärtused baitidena esitatakse.

### ASCII

7-bitine märgistik 128 koodiväärtusega. Sisaldab inglise tähestiku tähti, numbreid, kirjavahemärke ja juhtmärke. Eesti täpitähti selles ei ole.

### Unicode

Standard, mis annab väga paljude keelte märkidele ja sümbolitele ühised koodipunktid ehk arvulised tunnused.

### UTF-8

Unicode’i kodeering, mis määrab, kuidas koodipunktid baitideks muuta. Sobib nii eesti täpitähtede kui ka paljude muude märkide salvestamiseks.

!!! note "Pea meeles"

    **Unicode ütleb, millise märgiga on tegemist. UTF-8 määrab, kuidas see baitidena salvestada.**

### Üks märk ei tähenda alati ühte baiti

| Märk | Unicode’i koodipunkt | UTF-8 baidid hex-kujul | Maht |
| --- | --- | --- | --- |
| A | `U+0041` | `41` | 1 bait |
| õ | `U+00F5` | `C3 B5` | 2 baiti |
| 🙂 | `U+1F642` | `F0 9F 99 82` | 4 baiti |

`U+` tähistab Unicode’i koodipunkti, mille number on kirjutatud kuueteistkümnendsüsteemis. UTF-8 kodeerib ühe koodipunkti 1–4 baidina. ASCII märgid kasutavad UTF-8-s ühte baiti ning nende väärtused jäävad samaks.

??? info "Seos arvusüsteemidega"

    Tähe A ASCII kood on kümnendsüsteemis `65`, hex-kujul `41` ning ühe baidina kirjutatult binaarkujul `01000001`. Need on sama arvulise väärtuse erinevad esitused.

    Mõni nähtav märk või emotikon koosneb mitmest Unicode’i koodipunktist. Seepärast ei pruugi teksti nähtavate märkide arv võrduda koodipunktide ega baitide arvuga.

### Mis juhtub vale kodeeringuga?

Kui fail on salvestatud UTF-8-s, kuid programm loeb selle baite näiteks Windows-1252 kodeeringu järgi, võib tekst paista vigane.

| Õige UTF-8 tõlgendus | Samad baidid Windows-1252 järgi |
| --- | --- |
| Tere õpilane! | Tere Ãµpilane! |
| Pärnu | PÃ¤rnu |

!!! note "Pea meeles"

    **Vigane kuva ei tähenda alati rikutud faili.** Probleem võib olla selles, kuidas programm olemasolevaid baite tõlgendab. Vale tõlgenduse uuesti salvestamine võib aga sisu muuta.

## HTML kui märgistuskeel

**HTML** (*HyperText Markup Language*) on tekstipõhine märgistuskeel veebilehe sisu ja struktuuri kirjeldamiseks. Märgendid näitavad näiteks, milline tekst on pealkiri ja milline lõik.

### Tekstiredaktoris

Näed faili lähtekoodi koos märgenditega.

```html
<h1>Tere õpilane!</h1>
<p>See on veebileht.</p>
```

### Veebibrauseris

Brauser tõlgendab märgendeid ja kuvab tulemuse.

Tere õpilane!

See on veebileht.

`<h1>` tähistab esimese taseme pealkirja ning `<p>` tekstilõiku. Näidetes algab lõppmärgend kaldkriipsuga, näiteks `</p>`.

!!! note "Pea meeles"

    **Sama fail, erinev esitus.** HTML-fail ei muutu brauseris avamisel teiseks failiformaadiks. Brauser esitab failis kirjeldatud struktuuri ekraanil.

HTML-i roll on kirjeldada sisu ja struktuuri. Kujundust kirjeldatakse tavaliselt CSS-iga ning programmiloogikat lisatakse JavaScriptiga.

??? info "Kus näidatakse HTML-faili kodeeringut?"

    HTML-dokumendi päises kasutatakse tavaliselt märgendit:

    ```html
    <meta charset="utf-8">
    ```

    See ütleb brauserile, millise kodeeringuga tekst on esitatud. Fail tuleb ka tegelikult UTF-8-s salvestada: märgendi muutmine üksi ei teisenda olemasolevaid baite.

## Miks on standardeid vaja?

**Standard** on kokkulepitud reeglistik. Andmete esitamisel määravad ühised reeglid näiteks märkide koodid, kodeeringu või failiformaadi struktuuri. See aitab eri tootjate programmidel samu andmeid kasutada.

### Ühilduvus

Ühes programmis salvestatud faili saab avada teises, kui mõlemad toetavad sama formaati.

### Üheselt mõistetav esitus

Saatja ja vastuvõtja saavad kokku leppida, kuidas baite tõlgendada. Nii saab näiteks õ-täht jõuda ühest süsteemist teise õigesti.

### ISO ja IEC

**ISO** on Rahvusvaheline Standardiorganisatsioon ja **IEC** Rahvusvaheline Elektrotehnikakomisjon. Nad koostavad muu hulgas ühiseid IT-standardeid, mille tähises on **ISO/IEC**.

Siinse teemaga seostub **ISO/IEC 10646**, mis määratleb universaalse kodeeritud märgistiku. Selle märgikoodid ja kodeerimisvormid on Unicode’iga kooskõlastatud. Unicode’i standard sisaldab lisaks reegleid ja teavet märkide töötlemiseks.

!!! note "Pea meeles"

    **Kõiki IT-standardeid ei koosta ISO ja IEC.** Näiteks Unicode’i standardit arendab Unicode Consortium ning HTML-i kehtivat standardit WHATWG.

Oluline on mõista standardi eesmärki, mitte õppida standardinumbreid pähe. Standard toetab ühilduvust, kuid programm peab vastavat formaati või funktsiooni ka rakendama.

## Andmete salvestamine ja esitamine

Salvestamisel muudab programm sisu valitud formaadi reeglite järgi baitideks. Avamisel loeb programm baite ja tõlgendab neid, et kuvada näiteks tekst, pilt või veebileht.

Sisu → Formaat ja teksti kodeering → Baidid failis

Baidid failis → Programmi tõlgendus → Kuvatud sisu

Faililaiend aitab valida programmi. Failiformaat kirjeldab andmete struktuuri. Tekstikodeering määrab tekstibaite tõlgendades, milliseid märke näidatakse.

### Milline formaat valida?

| Olukord | Sobiv valik | Põhjendus |
| --- | --- | --- |
| Valmis dokumendi jagamine | .pdf | Säilitab lehekülgede kujunduse ja sobib lugemiseks. |
| Tekstidokumendi edasine muutmine | .docx / .odt | Säilitab redigeeritava dokumendistruktuuri. |
| Foto veebilehele | .jpg / .webp | Võimaldab sobivat kvaliteedi ja failimahu suhet. |
| Läbipaistva taustaga logo | .png / .svg | Toetab läbipaistvust; SVG sobib vektorgraafikaks. |
| Tabelandmete ülekanne | .csv | Lihtne tekstipõhine vorming, mida toetavad paljud programmid. |
| Veebilehe sisu ja struktuur | .html | Brauser oskab HTML-i tõlgendada. |

!!! note "Pea meeles"

    **Vali formaat kasutusotstarbe järgi.** Arvesta, kes faili avab, kas seda peab muutma, millist kvaliteeti on vaja ning milline failimaht on sobiv.

## Kokkuvõte

- Fail on andmekogum; faili tegeliku formaadi määravad selle sisu ja struktuur.
- Faililaiend on vihje. Laiendi muutmine ei teisenda formaati.
- Kõik failid koosnevad baitidest. Tekstifaili baite loetakse märgikodeeringu järgi.
- ASCII-st ei piisa eesti täpitähtede jaoks. Unicode määratleb märgid ning UTF-8 nende esituse baitidena.
- HTML on märgistuskeel. Brauser esitab märgenditega kirjeldatud sisu veebilehena.
- Standardid aitavad eri programmidel andmeid ühtemoodi tõlgendada ja vahetada.

### Mõtle õpitule

Milline uus teadmine failide kohta oli sulle kõige üllatavam? Too üks näide, kus vale failiformaat või kodeering võiks probleemi tekitada.

## Teema sõnastik

| Mõiste | Selgitus |
| --- | --- |
| Fail | Nimega andmekogum, mis võib sisaldada näiteks teksti, pilti või programmi. |
| Failinimi | Faili nimi koos võimaliku laiendiga, näiteks `foto.jpg`. |
| Faililaiend | Tavaliselt failinime viimase punkti järel olev osa, mis annab vihje formaadi kohta, näiteks `.jpg`. |
| Failitüüp | Siin materjalis faili üldine otstarve või sisu liik, näiteks dokument või pilt. |
| Failiformaat ehk failivorming | Reeglid, mille järgi on andmed faili sisse korraldatud. |
| Tekstifail | Fail, mille baite tõlgendatakse kindla märgikodeeringu järgi tekstina. |
| Binaarfail | Fail, mille terviksisu ei ole mõeldud tavalise tekstina lugemiseks; tõlgendamiseks kasutatakse vastava formaadi reegleid. |
| Märgistik | Kokkulepitud märkide kogum. Kodeeritud märgistik seob märgid arvuliste koodidega. |
| Koodipunkt | Märgile omistatud arvuline tunnus, näiteks Unicode’is õ-tähe `U+00F5`. |
| ASCII | 7-bitine kodeeritud märgistik 128 koodiväärtusega; ei sisalda eesti täpitähti. |
| Unicode | Standard, mis määratleb väga paljude keelte märkide ja sümbolite ühised koodipunktid ning nende töötlemise reeglid. |
| Kodeering | Reeglid, mille järgi märgid esitatakse baitidena ning baidid tõlgendatakse märkideks. |
| UTF-8 | Unicode’i kodeering, mis esitab ühe koodipunkti 1–4 baidina. |
| HTML | Tekstipõhine märgistuskeel veebilehe sisu ja struktuuri kirjeldamiseks. |
| Märgistuskeel | Keel, mis kasutab märgendeid sisu struktuuri ja tähenduse kirjeldamiseks. |
| Standard | Kokkulepitud reeglistik, mis toetab näiteks andmete üheselt mõistetavat esitust ja süsteemide ühilduvust. |
| ISO ja IEC | Rahvusvahelised standardiorganisatsioonid, mis koostavad muu hulgas ühiseid ISO/IEC tähisega IT-standardeid. |
| Teisendamine | Andmete esituse muutmine ühest formaadist teise vastava programmi abil. |

??? info "Lisalugemise mõisted"

    | Mõiste | Selgitus |
    | --- | --- |
    | CSV | Tekstipõhine tabelandmete vorming; väljad on eraldatud tavaliselt komadega, mõnes variandis muu eraldajaga. |
    | JSON | Tekstipõhine struktureeritud andmevahetusformaat, mis kasutab näiteks objekte, massiive ja võtme-väärtuse paare. |
    | XML | Tekstipõhine märgistuskeel struktureeritud andmete kirjeldamiseks. |
    | Metaandmed | Andmeid kirjeldav lisainfo, näiteks foto tegemise aeg või dokumendi autor. |
    | Arhiiv | Fail, millesse on koondatud teisi faile ja vajadusel kaustade struktuur. |
    | Pakkimine ehk tihendamine | Andmete esitamine väiksema mahuga. Arhiivi koondamine ja tihendamine on erinevad toimingud. |
    | Kadudeta pakkimine | Tihendamine, mille järel saab algsed andmed täpselt taastada. |
    | Kadudega pakkimine | Mahu vähendamine osa info eemaldamisega; algset infot ei saa täielikult taastada. |
    | Faili signatuur | Failiformaadile iseloomulik baitide jada, mis aitab formaati tuvastada. |


## Lisalugemine

Need teemad aitavad põhisisu laiendada. Ava plokk, mille kohta soovid rohkem teada.

??? info "CSV, JSON ja XML — sama info erineval kujul"

    Õppija nimi ja vanus võivad olla esitatud eri struktuuridega. Kõik kolm allolevat näidet on tekstipõhised.

    ### CSV

    ```text
    nimi,vanus
    Mari,16
    ```

    Sobib näiteks tabelandmete importimiseks ja eksportimiseks. CSV ei säilita arvutustabeli kujundust ega töövihiku mitut lehte.

    ### JSON

    ```json
    {"nimi": "Mari", "vanus": 16}
    ```

    Levinud rakenduste ja API-de andmevahetuses.

    ### XML

    ```xml
    <opilane>
      <nimi>Mari</nimi>
      <vanus>16</vanus>
    </opilane>
    ```

    Sobib struktureeritud andmete ja konfiguratsiooni kirjeldamiseks.

??? info "Failimaht ja pakkimine"

    Erinevad formaadid võivad esitada sama sisu erineva mahuga. Pildi, heli ja video puhul kasutatakse sageli tihendamist ehk pakkimist.

    | Pakkimine | Põhimõte | Tüüpilised näited |
    | --- | --- | --- |
    | Kadudega | Osa infot eemaldatakse; algset infot ei saa täielikult taastada. | JPEG-pilt, MP3-heli |
    | Kadudeta | Algsed andmed on täpselt taastatavad. | PNG-pilt, FLAC-heli, ZIP-pakkimine |

    **Arhiveerimine ja tihendamine ei ole sama.** TAR koondab failid ühte arhiivi. Tihendamiseks võib kasutada eraldi näiteks gzip’i; tulemuse tavapärane laiend on `.tar.gz`.

    Väiksem fail ei ole automaatselt parem: oluline on ka kvaliteet, ühilduvus ja kasutusotstarve.

??? info "Metaandmed"

    Fail võib sisaldada või sellega võib olla seotud lisainfot. Fotol võivad olla tegemise aeg, kaamera mudel ja GPS-asukoht. Dokumendil võivad olla autor ja muutmise aeg.

    Neid andmeid nimetatakse **metaandmeteks**. Enne avalikku jagamist tasub vaadata, millist lisainfot fail kaasa annab.

??? info "Faili tegeliku formaadi tuvastamine"

    Paljude formaatide puhul aitab tuvastamisel faili alguses olev tunnus ehk signatuur (*magic bytes*). Näiteks PNG-faili esimesed kaheksa baiti on hex-kujul:

    ```text
    89 50 4E 47 0D 0A 1A 0A
    ```

    Tööriistad võivad uurida nii tunnuseid kui ka sisu struktuuri. Linuxis saab faili tüüpi hinnata käsuga:

    ```bash
    file failinimi
    ```

    Signatuur on tuvastamise abivahend, mitte kinnitus faili terviklikkuse või ohutuse kohta.

## Allikad ja lisalugemine

- [Unicode Consortium — UTF-8, UTF-16, UTF-32 ja BOM](https://www.unicode.org/faq/utf_bom.html)
- [Unicode Consortium — Unicode’i ja ISO/IEC 10646 seos](https://www.unicode.org/faq/unicode_iso.html)
- [WHATWG — HTML Standard](https://html.spec.whatwg.org/multipage/)
- [GNU tar — arhiivide tihendamine](https://www.gnu.org/software/tar/manual/html_section/Compression.html)

