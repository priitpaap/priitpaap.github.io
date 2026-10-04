# Arvusüsteemid

Õpi lugema kahend- ja kuueteistkümnendarve, tegema lihtsaid teisendusi ning mõistma, kuidas need seostuvad bittide, baitide, võrguaadresside ja värvikoodidega.

## Teema tulemusena oskad

- eristada kümnend-, kahend- ja kuueteistkümnendsüsteemi;
- selgitada arvusüsteemi alust ja kohaväärtust;
- teisendada lihtsaid kahend- ja kümnendarve omavahel;
- teisendada lihtsaid hex- ja kümnendarve omavahel;
- seostada ühe hex-märgi nelja bitiga;
- selgitada biti ja baidi erinevust;
- tuua IT-st kahend- ja hex-esituse näiteid;
- kontrollida teisenduse tulemust kohaväärtuste abil.

Alusta kohaväärtuste mõistmisest ja väikestest arvudest. Jagamismeetodid on avatavates lisaselgitustes. Näidetes käsitleme märgita ehk mittenegatiivseid täisarve.

## Miks vajame erinevaid arvusüsteeme?

Igapäevaelus kasutame enamasti kümnendsüsteemi. Digitaalses elektroonikas eristatakse kahte loogilist olekut, mida tähistatakse 0 ja 1-ga. Need võivad vastata näiteks madalale ja kõrgele signaalitasemele. Seetõttu sobib andmete esitamiseks kahendsüsteem.

Pikki kahendarve on inimesel keeruline lugeda. **Kuueteistkümnendsüsteem** ehk *hexadecimal* võimaldab samu väärtusi kirjutada lühemalt. Seda kasutatakse näiteks võrguaadresside ja värvikoodide esitamisel.

Arvu järel olev väike alaindeks näitab arvusüsteemi alust: näiteks 11001<sub>2</sub> on kahendarv. Arvutustes ilma alaindeksita arvud on siin kümnendsüsteemis.

| Arvusüsteem | Alus | Numbrimärgid | Sama väärtuse näide |
| --- | --- | --- | --- |
| Kümnendsüsteem (*decimal*) | 10 | 0–9 | 25<sub>10</sub> |
| Kahendsüsteem (*binary*) | 2 | 0 ja 1 | 11001<sub>2</sub> |
| Kuueteistkümnendsüsteem (*hexadecimal*, hex) | 16 | 0–9 ja A–F | 19<sub>16</sub> |

!!! note "Pea meeles"

    **Arvu väärtus jääb samaks.** 25<sub>10</sub> = 11001<sub>2</sub> = 19<sub>16</sub>. Muutub see, kuidas väärtus kirjutatakse.

**Arvusüsteemi alus** näitab kasutatavate numbrimärkide arvu ja kohaväärtuste kasvamise kordajat. Alust nimetatakse ka *base* või *radix*.

??? info "Kuidas arvusüsteemi tähistatakse?"

    Arvu järel olev väike alaindeks tähistab alust: 1010<sub>2</sub> on kahendarv ja 10<sub>10</sub> kümnendarv. Kui alaindeksit pole, on selle materjali arvutustes arvud kümnendsüsteemis.

    Tehnilistes tekstides ja näiteks Pythonis kohtad ka eesliiteid `0b` ja `0x`:

    `0b1010` = 1010<sub>2</sub> = 10__MD_LINEBREAK__`0xFF` = FF<sub>16</sub> = 255

    Eesliide ei ole arvu numbrimärkide osa; see näitab, kuidas järgnevat numbrijada lugeda.

## Sama number võib eri kohal tähendada erinevat väärtust

**Kohaväärtus** määrab, kui suure väärtuse annab number arvu selles asukohas. Parempoolseima koha väärtus on 1. Iga sammuga vasakule korrutatakse kohaväärtus arvusüsteemi alusega.

### Kümnendsüsteem

Paremalt vasakule: 1, 10, 100, 1000 …

### Kahendsüsteem

Paremalt vasakule: 1, 2, 4, 8, 16 …

### Hex-süsteem

Paremalt vasakule: 1, 16, 256, 4096 …

### Tuttav kümnendsüsteemi näide

| Koht | Sajalised | Kümnelised | Ühelised |
| --- | --- | --- | --- |
| Kohaväärtus | 100 | 10 | 1 |
| Number | 3 | 5 | 2 |
| Panus arvu väärtusse | 300 | 50 | 2 |

352 = 3 × 100 + 5 × 10 + 2 × 1__MD_LINEBREAK__352 = 3 × 10<sup>2</sup> + 5 × 10<sup>1</sup> + 2 × 10<sup>0</sup>

Sama põhimõte töötab ka kahend- ja kuueteistkümnendsüsteemis. Muutuvad kohaväärtused.

??? info "Mida tähendavad astmed?"

    `2³` tähendab 2 × 2 × 2 = 8. Arvusüsteemi alus tõstetakse astmesse, mis sõltub koha kaugusest paremast servast. Parempoolseima koha aste on 0 ning `2⁰ = 10⁰ = 16⁰ = 1`.

## Kahendsüsteem, bitt ja bait

Kahendsüsteemis kasutatakse ainult 0 ja 1. Üks kahendnumber vastab ühele **bitile**. Kaheksa bitti moodustavad ühe **baidi**.

| Aste | 2<sup>7</sup> | 2<sup>6</sup> | 2<sup>5</sup> | 2<sup>4</sup> | 2<sup>3</sup> | 2<sup>2</sup> | 2<sup>1</sup> | 2<sup>0</sup> |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Kohaväärtus | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |

| Bittide arv | Erinevaid bitikombinatsioone | Märgita täisarvu vahemik |
| --- | --- | --- |
| 1 | 2 | 0–1 |
| 2 | 4 | 0–3 |
| 4 | 16 | 0–15 |
| 8 | 256 | 0–255 |

!!! note "Pea meeles"

    **n bitiga saab esitada 2<sup>n</sup> erinevat bitikombinatsiooni.** Märgita täisarvude puhul on väärtused 0 kuni 2<sup>n</sup> − 1. Kaheksa bitti annavad 256 väärtust; suurim väärtus on 255, sest ka null kuulub vahemikku.

??? info "Näide: baidi bittide väärtused"

    Kokku liidetakse ainult nende kohtade väärtused, mille bitt on 1.

    | Kohaväärtus | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
    | --- | --- | --- | --- | --- | --- | --- | --- | --- |
    | Bitt | 0 | 0 | 0 | 0 | 1 | 1 | 0 | 1 |

    8 + 4 + 1 = 13

    00001101<sub>2</sub> = 13<sub>10</sub> = 0D<sub>16</sub>

    Kõik bitid 0: 00000000<sub>2</sub> = 0<sub>10</sub>. Kõik bitid 1: 11111111<sub>2</sub> = 255<sub>10</sub>.

    Proovi paberil: muuda kohaväärtusega 2 bitt üheks. Saad 8 + 4 + 2 + 1 = 15 ehk 00001111<sub>2</sub> = 15<sub>10</sub> = 0F<sub>16</sub>.

??? info "Kas baidi sisu on alati arv 0–255?"

    Baidil on 256 võimalikku bitimustrit. Tähendus sõltub tõlgendusest: muster võib tähistada märgita arvu, osa tekstikodeeringust, värvikomponenti või muud infot. Märgiga täisarvude esitamiseks kasutatakse eraldi kokkuleppeid.

## Kahendarvust kümnendarvuks

Kirjuta kahendarvu alla kohaväärtused ja liida kokku need, mille kohal on 1.

### Näide: kahendarv 1101

| Kohaväärtus | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- |
| Bitt | 1 | 1 | 0 | 1 |
| Panus | 8 | 4 | 0 | 1 |

1101<sub>2</sub> = 1 × 8 + 1 × 4 + 0 × 2 + 1 × 1 = **13**

### Teine näide: kahendarv 101010

Kuue koha väärtused on vasakult paremale 32, 16, 8, 4, 2 ja 1.

101010<sub>2</sub> = 32 + 8 + 2 = **42**

!!! note "Pea meeles"

    **Kontrolli:** kui kahendarvu viimane bitt on 0, on märgita täisarv paaris; kui viimane bitt on 1, on see paaritu.

## Kümnendarvust kahendarvuks

Alustamiseks sobib kohaväärtuste meetod. Leia suurim kahe aste, mis arvu sisse mahub. Liigu seejärel väiksemate kohaväärtuste poole.

1. Kui kohaväärtus mahub allesjäänud arvu sisse, kirjuta 1 ja lahuta see väärtus.
2. Kui ei mahu, kirjuta 0 ning jäta allesjäänud arv samaks.
3. Jätka kuni kohaväärtuseni 1.

### Näide: kümnendarv 13

| Kohaväärtus | Kas mahub? | Bitt | Alles jääb |
| --- | --- | --- | --- |
| 8 | Jah: 13 − 8 | 1 | 5 |
| 4 | Jah: 5 − 4 | 1 | 1 |
| 2 | Ei mahu 1 sisse | 0 | 1 |
| 1 | Jah: 1 − 1 | 1 | 0 |

13<sub>10</sub> = 1101<sub>2</sub>__MD_LINEBREAK__Ühe baidina: 00001101<sub>2</sub>

Algusesse lisatud nullid arvu väärtust ei muuda. Neid kasutatakse näiteks selleks, et esitada arv täpselt kaheksa bitiga.

??? info "Lisameetod: jaga kahega ja kirjuta jäägid"

    Jagamisel kasutatakse täisarvulist jagatist. Jätka jagatise jagamist kahega, kuni see on 0. Loe jäägid **alt üles**.

    | Jagamine | Jagatis | Jääk |
    | --- | --- | --- |
    | 25 ÷ 2 | 12 | 1 |
    | 12 ÷ 2 | 6 | 0 |
    | 6 ÷ 2 | 3 | 0 |
    | 3 ÷ 2 | 1 | 1 |
    | 1 ÷ 2 | 0 | 1 |

    25<sub>10</sub> = 11001<sub>2</sub>

    Arv 0 kirjutatakse kahendsüsteemis samuti 0.

## Kuueteistkümnendsüsteem

Kuueteistkümnendsüsteemis on 16 numbrimärki. Märgid 0–9 tähistavad samu väärtusi nagu kümnendsüsteemis, kuid väärtusi 10–15 tähistatakse tähtedega A–F.

| Kümnend | Hex | 4 bitti | Kümnend | Hex | 4 bitti |
| --- | --- | --- | --- | --- | --- |
| 0 | `0` | `0000` | 8 | `8` | `1000` |
| 1 | `1` | `0001` | 9 | `9` | `1001` |
| 2 | `2` | `0010` | 10 | `A` | `1010` |
| 3 | `3` | `0011` | 11 | `B` | `1011` |
| 4 | `4` | `0100` | 12 | `C` | `1100` |
| 5 | `5` | `0101` | 13 | `D` | `1101` |
| 6 | `6` | `0110` | 14 | `E` | `1110` |
| 7 | `7` | `0111` | 15 | `F` | `1111` |

!!! note "Pea meeles"

    **Pärast F-i tuleb 10.** F<sub>16</sub> = 15<sub>10</sub>, aga 10<sub>16</sub> = 16<sub>10</sub>. Arvusüsteemi alust tuleb alati tähele panna.

Suured ja väikesed tähed tähistavad samu väärtusi: näiteks `FF` ja `ff` on mõlemad hex-kujud arvu 255 jaoks.

## Kahendarv ja hex-arv

**Üks hex-märk vastab täpselt neljale bitile**, sest 2<sup>4</sup> = 16. Seega saab ühe baidi kirjutada kahe hex-märgiga.

### Hex → kahendarv

Asenda iga hex-märk nelja bitiga. Näiteks A = 1010 ja F = 1111.

AF<sub>16</sub> = 1010 1111<sub>2</sub>

### Kahendarv → hex

Jaga bitid **paremalt alustades** neljakaupa rühmadesse. Asenda iga rühm hex-märgiga.

1100 1010<sub>2</sub> = CA<sub>16</sub>

### Kui bitte pole neljaga jaguv arv

Lisa vasakule vajadusel nulle, et iga rühm sisaldaks nelja bitti.

11001<sub>2</sub> → 0001 1001<sub>2</sub> → 19<sub>16</sub>

Rühmade vahel olev tühik aitab arvu lugeda. See ei lisa ühtegi bitti ega muuda arvu väärtust.

!!! note "Pea meeles"

    **Ühe baidi näide:** 0000 1101<sub>2</sub> = 0D<sub>16</sub> = 13<sub>10</sub>. Arvu väärtuse puhul on 0D<sub>16</sub> ja D<sub>16</sub> võrdsed; baidi esituses kasutatakse sageli kahte hex-märki.

## Hex-arvu ja kümnendarvu teisendamine

### Hex → kümnend: kasuta kohaväärtusi

Kahe hex-märgi puhul on vasakpoolse koha väärtus 16 ja parempoolse koha väärtus 1. F-i arvuline väärtus on 15.

2F<sub>16</sub> = 2 × 16 + 15 × 1 = **47**__MD_LINEBREAK__FF<sub>16</sub> = 15 × 16 + 15 × 1 = **255**

### Kümnend → hex: kasuta kahendsüsteemi vaheastmena

Teisenda arv kahendsüsteemi ja jaga bitid neljastesse rühmadesse.

60<sub>10</sub> = 0011 1100<sub>2</sub> = 3C<sub>16</sub>

### Kahekohalise hex-arvu saab leida ka jagamisega

Arvude 0–255 korral jaga arv 16-ga. Täisarvuline jagatis annab esimese hex-märgi, jääk teise. Väärtused 10–15 asenda tähtedega A–F.

60 = 3 × 16 + 12__MD_LINEBREAK__Jagatis 3 → `3`, jääk 12 → `C`__MD_LINEBREAK__60<sub>10</sub> = 3C<sub>16</sub>

Väiksema kui 16 arvu korral on esimene märk 0: näiteks 13<sub>10</sub> = 0D<sub>16</sub>.

??? info "Lisameetod suurema arvu jaoks: korduv jagamine 16-ga"

    Jaga järjest 16-ga ning kirjuta jäägid hex-märkidena. Loe jäägid **alt üles**.

    | Jagamine | Jagatis | Jääk | Hex-märk |
    | --- | --- | --- | --- |
    | 1000 ÷ 16 | 62 | 8 | 8 |
    | 62 ÷ 16 | 3 | 14 | E |
    | 3 ÷ 16 | 0 | 3 | 3 |

    1000<sub>10</sub> = 3E8<sub>16</sub>__MD_LINEBREAK__Kontroll: 3 × 256 + 14 × 16 + 8 = 1000

## Kus neid teadmisi IT-s kasutatakse?

Bitid ja baidid on andmete salvestamise ning edastamise alus. Üksikud bitid võivad tähistada sisse/välja olekuid; IP-aadresside ja võrgumaskide käsitlemisel kasutatakse bitipositsioone. Hex-kuju aitab neid andmeid inimesele kompaktsemalt näidata.

| Näide | Mida see näitab? |
| --- | --- |
| MAC-aadress `00:1A:2B:3C:4D:5E` | Tüüpiline 48-bitine MAC-aadress koosneb 6 baidist. Iga bait on esitatud kahe hex-märgiga. |
| IPv6-aadress `2001:db8::1` | IPv6-aadress on 128-bitine. Hex-kuju ja nullide lühendamine muudavad selle loetavamaks. |
| RGB-värv `#FF8800` | Kuus hex-märki kirjeldavad kolme värvikomponenti: punast, rohelist ja sinist. |
| Mäluaadress `0x7FFE` | Eesliide 0x näitab, et aadressi arvuline väärtus on kirjutatud hex-kujul. |
| Hex-redaktor | Faili baite saab vaadata kahe hex-märgi kaupa, näiteks `41 C3 B5`. |

??? info "Mida tähendab IPv6-aadressi sees ::?"

    IPv6 tavalises hex-esituses on kaheksa 16-bitist rühma. Näites `2001:db8::1` on järjestikused nullirühmad lühendatud märgiga `::`. Täiskuju on:

    ```text
    2001:0db8:0000:0000:0000:0000:0000:0001
    ```

    Igas rühmas on neli hex-märki ehk 16 bitti. Kaheksa rühma annavad kokku 128 bitti.

### RGB-värv kujul #RRGGBB

Selles kuuekohalises värvikoodis tähistab RR punast, GG rohelist ja BB sinist komponenti. Iga komponent on vahemikus 00<sub>16</sub> kuni FF<sub>16</sub>, kümnendsüsteemis 0–255.

| Värvikood | R | G | B | Värv |
| --- | --- | --- | --- | --- |
| `#FF0000` | 255 | 0 | 0 | Punane |
| `#00FF00` | 0 | 255 | 0 | Roheline |
| `#0000FF` | 0 | 0 | 255 | Sinine |
| `#FFFFFF` | 255 | 255 | 255 | Valge |
| `#000000` | 0 | 0 | 0 | Must |

Näiteks `#FF0000` tähendab maksimaalset punast ning rohelist ja sinist väärtusega 0. Koodi `#FF8800` komponendid on kümnendsüsteemis 255, 136 ja 0.

??? info "Näide: RGB-komponendid kümnend- ja hex-kujul"

    Iga värvikomponent on kümnendsüsteemis 0–255 ja esitatakse kahe hex-märgiga.

    | Komponent | Kümnendkuju | Hex-kuju |
    | --- | --- | --- |
    | Punane (R) | 255 | FF |
    | Roheline (G) | 136 | 88 |
    | Sinine (B) | 0 | 00 |

    Komponentide hex-kujud annavad kokku värvikoodi `#FF8800`.

    Proovi paberil: muuda roheline komponent väärtuseks 0. Uus kood on `#FF0000` ja värv on punane.

## Levinud vead

| Näide | Hinnang | Põhjendus |
| --- | --- | --- |
| 102<sub>2</sub> | Vigane | Kahendsüsteemis on lubatud ainult numbrimärgid 0 ja 1. |
| 1G<sub>16</sub> | Vigane | Hex kasutab märke 0–9 ja A–F; G ei ole hex-märk. |
| 10<sub>2</sub> = 10<sub>10</sub> | Vale | 10<sub>2</sub> väärtus on kümnendsüsteemis 2. |
| FF<sub>16</sub> = 255<sub>10</sub> | Õige | 15 × 16 + 15 = 255. |

!!! note "Pea meeles"

    **Sama numbrijada ei tähenda alati sama väärtust.** 10<sub>2</sub> tähendab 2, 10<sub>10</sub> tähendab 10 ja 10<sub>16</sub> tähendab 16.

Teisendamisel märgi üles lähte- ja sihtarvusüsteem. Kontrolli tulemust, teisendades arvu kohaväärtuste abil tagasi.

## Enesekontroll koos lahenduskäikudega

Proovi kõigepealt ise. Ava seejärel vastava ülesande lahendus ja võrdle arvutuskäiku.

### Teisenda kahendarv 1110 kümnendarvuks

??? info "Vaata lahendust"

    1110<sub>2</sub> = 8 + 4 + 2 = **14**

### Teisenda kümnendarv 25 kahendarvuks

??? info "Vaata lahendust"

    25 = 16 + 8 + 1. Kohaväärtuste 16, 8, 4, 2 ja 1 all on seega bitid 1, 1, 0, 0 ja 1.

    25<sub>10</sub> = 11001<sub>2</sub>

### Teisenda hex-arv 3C kümnendarvuks

??? info "Vaata lahendust"

    C arvuline väärtus on 12.

    3C<sub>16</sub> = 3 × 16 + 12 = **60**

### Teisenda kahendarv 11111111 hex-kujule

??? info "Vaata lahendust"

    Moodusta kaks neljabitist rühma: 1111 1111. Mõlemale vastab hex-märk F.

    11111111<sub>2</sub> = FF<sub>16</sub>

### Mis on suurim märgita 8-bitine täisarv?

??? info "Vaata lahendust"

    Kaheksa bitti annavad 2<sup>8</sup> = 256 erinevat väärtust. Vahemik algab nullist, seega suurim väärtus on 256 − 1 = 255.

    11111111<sub>2</sub> = 255<sub>10</sub> = FF<sub>16</sub>

## Kokkuvõte

- Kümnendsüsteemi alus on 10, kahendsüsteemi alus 2 ja hex-süsteemi alus 16.
- Kohaväärtused kasvavad paremalt vasakule iga sammuga aluse korda.
- Üks bitt saab olla 0 või 1; kaheksa bitti moodustavad ühe baidi.
- Märgita 8-bitise täisarvu vahemik on 0–255: kokku 256 erinevat väärtust.
- Üks hex-märk vastab neljale bitile ning üks bait kahele hex-märgile.
- Teisendus muudab arvu esitust, kuid jätab selle arvulise väärtuse samaks.
- Hex-esitust kasutatakse näiteks MAC- ja IPv6-aadressides, RGB-värvikoodides ning diagnostikas.

??? info "Mõtlemisküsimused"

    1. Miks sobib kahendesitus digitaalse elektroonika jaoks?
    2. Miks on hex-kuju inimesele sageli mugavam kui pikk kahendarv?
    3. Miks on suurim märgita 8-bitine väärtus 255, kuigi kombinatsioone on 256?
    4. Miks ei ole 102 korrektne kahendarv?
    5. Mida tähendab värvikoodis #FF0000 osa FF?
    6. Mitu hex-märki on vaja ühe baidi esitamiseks?
    7. Kus oled varem näinud 0x-algusega väärtusi?

## Teema sõnastik

| Mõiste | Selgitus |
| --- | --- |
| Arvusüsteem | Reeglid arvude esitamiseks. Siin käsitleme positsioonilisi süsteeme, kus väärtus sõltub ka numbrimärgi asukohast. |
| Arvusüsteemi alus (base, radix) | Numbrimärkide arv ja kohaväärtuste kasvamise kordaja. Kahendsüsteemi alus on 2. |
| Kohaväärtus | Väärtus, mille annab numbrimärgi asukoht. Näiteks kümnendsüsteemi arvu 352 keskmise koha väärtus on 10. |
| Kümnendsüsteem (decimal) | Alusega 10 arvusüsteem, mis kasutab numbrimärke 0–9. |
| Kahendsüsteem (binary) | Alusega 2 arvusüsteem, mis kasutab numbrimärke 0 ja 1. |
| Kuueteistkümnendsüsteem (hexadecimal, hex) | Alusega 16 arvusüsteem, mis kasutab märke 0–9 ja A–F. |
| Bitt (bit) | Kahendnumber, mille väärtus saab olla 0 või 1. |
| Bait (byte) | Kaheksast bitist koosnev andmeühik. |
| Märgita täisarv (unsigned integer) | Täisarv, mille esitus ei sisalda negatiivseid väärtusi. Märgita 8-bitise arvu vahemik on 0–255. |
| 0b | Näiteks Pythonis kahendarvu tähistav eesliide: `0b1010` on kümnendarv 10. |
| 0x | Hex-arvu tähistav eesliide paljudes tehnilistes tekstides ja programmeerimiskeeltes: `0xFF` on kümnendarv 255. |
| MAC-aadress | Võrguliidese linkkihi aadress. Tüüpilist 48-bitist MAC-aadressi esitatakse kuue hex-paarina. |
| IPv6 | Interneti protokolli versioon, mille aadressid on 128-bitised ning mida esitatakse sageli hex-rühmadena. |
| RGB | Värvimudel, milles kasutatakse punase (red), rohelise (green) ja sinise (blue) komponente. |

## Allikad ja lisalugemine

- [MDN Web Docs — kuueteistkümnendesitusega CSS-värvid](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/hex-color)
- [Python — kahend- ja hex-täisarvude kirjapilt](https://docs.python.org/3/reference/lexical_analysis.html#integer-literals)
- [RFC 4291 — IPv6-aadresside tekstiline esitus](https://www.rfc-editor.org/rfc/rfc4291.html#section-2.2)

