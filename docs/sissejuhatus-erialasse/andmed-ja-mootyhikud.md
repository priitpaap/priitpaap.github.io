# Andmed ja mõõtühikud

Õpi lugema seadmete ja ühenduste näitajaid ning arvutama andmemahtu, kiirust, aega, sagedust ja pikslite arvu.

## Teema tulemusena oskad

- Eristada bitti ja baiti ning tähiseid b ja B.
- Teisendada andmemahtu ning eristada 1000- ja 1024-põhiseid ühikuid.
- Eristada andmemahtu ja andmeedastuskiirust.
- Hinnata faili allalaadimisaega.
- Teisendada sagedust ning arvutada pikslite arvu.
- Kontrollida ühikut ja tulemuse suurusjärku.

Alusta ühiku tähendusest, seejärel vali teisendamiseks õige tehe. Kalkulaatorit võib kasutada. 1024-põhised näited on eraldi välja toodud.

## Mida ühik kirjeldab?

Arvuti, telefoni ja võrgu kirjelduses kohtad eri mõõtühikuid. Number muutub tähenduslikuks alles koos ühiku ja kontekstiga.

| Näitaja | Mida see kirjeldab? | Näide |
| --- | --- | --- |
| 256 GB | Andmemaht või salvestusruum | Telefoni salvestusruum |
| 500 Mbps | Andmeedastuskiirus | Võrguühendus |
| 4,2 GHz | Sagedus | Protsessori taktsagedus |
| 1920 × 1080 px | Laius ja kõrgus pikslites | Pilt või ekraan |
| 12 MP | Pikslite arv miljonites | Foto pikslite arv |

## Bitt ja bait

**Bitt** saab olla 0 või 1. **Bait** koosneb kaheksast bitist. Biti lühitähis on väike **b**, baidi tähis suur **B**.

!!! note "Pea meeles"

    **1 B = 8 b.** Baitidest bittideks teisendades korruta kaheksaga; bittidest baitideks teisendades jaga kaheksaga.

Faili suurust näidatakse tavaliselt baitides ja nende kordühikutes. Võrguühenduse kiirust näidatakse enamasti bittides sekundis.

```text
16 B = 16 × 8 = 128 b
80 b = 80 ÷ 8 = 10 B
```

## Andmemahu kaks ühikurida

**Kümnendpõhised ühikud** kasvavad iga sammuga 1000 korda. **Binaarsed ühikud** kasvavad iga sammuga 1024 korda ja nende tähises on i. Need on kaks eri rida, mida ei tohi arvutustes segamini ajada.

| 1000-põhine ühik | Seos | 1024-põhine ühik | Seos |
| --- | --- | --- | --- |
| kilobait (kB) | 1000 B | kibibait (KiB) | 1024 B |
| megabait (MB) | 1000 kB | mebibait (MiB) | 1024 KiB |
| gigabait (GB) | 1000 MB | gibibait (GiB) | 1024 MiB |
| terabait (TB) | 1000 GB | tebibait (TiB) | 1024 GiB |

!!! note "Pea meeles"

    **Selles materjalis on tähised täpsed:** 1 GB = 1000 MB, aga 1 GiB = 1024 MiB. Väiksema ühiku poole liikudes korruta; suurema ühiku poole liikudes jaga. Kasuta oma ühikurea kordajat.

```text
3 GB = 3 × 1000 = 3000 MB
7500 MB = 7500 ÷ 1000 = 7,5 GB
3 GiB = 3 × 1024 = 3072 MiB
```

??? info "Miks võib seadme või programmi kuvatud maht erineda?"

    Mõni programm kasutab binaarset arvutust, kuid näitab tähist GB või MB. Kontrolli programmi selgitust. Samuti võivad kasutatavat ruumi vähendada süsteemifailid, reserveeritud alad ja vormindamisega seotud andmed.

    ```text
    1 GB = 1 000 000 000 B
    1 GiB = 1 073 741 824 B
    1 GB ≈ 0,931 GiB
    ```

    Sama baitide arv võib eri ühikutes olla erinev number. Ainult teisendamine ei kaota andmeid ega kettaruumi.

## Faili suurus ja vaba ruum

Faili suurus näitab, kui palju ruumi fail vajab. Vaba ruum näitab, kui palju on veel kasutada. Võrdle mahtusid samas ühikus.

```text
Vaba ruum: 120 GB. Mäng vajab: 85 GB.
120 − 85 = 35 GB jääb teoreetiliselt alles.

40 GB ÷ 4 GB = 10 videofaili.
```

Mahutavuse arvutuses loe ainult terveid faile: 50 GB ÷ 4 GB = 12,5, seega mahub 12 tervet faili ja jääb 2 GB. Paigaldamisel võib olla vaja lisaruumi ajutiste failide ja uuenduste jaoks.

## Andmemaht ja andmeedastuskiirus

**Andmemaht** vastab küsimusele „Kui palju andmeid?“. **Andmeedastuskiirus** vastab küsimusele „Kui palju andmeid sekundis?“. Kiiruse tähises on sekund: näiteks bit/s või MB/s.

| Tähis | Tähendus |
| --- | --- |
| kbit/s ehk kbps | 1000 bitti sekundis |
| Mbit/s ehk Mbps | 1 000 000 bitti sekundis |
| Gbit/s ehk Gbps | 1 000 000 000 bitti sekundis |
| MB/s | 1 000 000 baiti sekundis |
| MiB/s | 1 048 576 baiti sekundis |

### Mbps ja MB/s

Mõlemad eesliited on kümnendpõhised. Bittide ja baitide vahel on täpne kordaja 8.

```text
100 Mbps ÷ 8 = 12,5 MB/s
50 MB/s × 8 = 400 Mbps
```

See on ühikute teisendus. Tegelik faili allalaadimiskiirus võib olla väiksem kui ühenduse nimikiiruse põhjal saadud tulemus.

??? info "Kui programm näitab MiB/s"

    MiB/s ja MB/s on eri ühikud. 100 Mbps = 12,5 MB/s ≈ 11,92 MiB/s. MiB/s saamiseks tuleb baitide arv sekundis jagada 1 048 576-ga.

## Allalaadimisaja hindamine

Vii faili maht ja kiirus samasse ühikusse. Seejärel jaga maht kiirusega. Kiirusega MB/s arvutades peab faili suurus olema MB-des.

!!! note "Pea meeles"

    **Aeg = andmemaht ÷ andmeedastuskiirus.** Kui kiirus on sekundis, saad aja sekundites.

### Näide: 1 GB fail ja 100 Mbps ühendus

1. Faili maht: 1 GB = 1000 MB.
2. Kiirus: 100 Mbps ÷ 8 = 12,5 MB/s.
3. Aeg: 1000 MB ÷ 12,5 MB/s = 80 s ehk 1 min 20 s.

Tulemus eeldab, et kogu nimetatud kiirus on faili andmete jaoks pidevalt kasutada. See on **ideaaltingimuste hinnang**; päriselus võib aega kuluda rohkem.

??? info "Võrdlus: sama ühendus, aga 1 GiB fail"

    ```text
    1 GiB = 1 073 741 824 B
    100 Mbps = 100 000 000 b/s
    Aeg = 1 073 741 824 × 8 ÷ 100 000 000
    ≈ 85,9 s
    ```

    1 GiB fail on suurem kui 1 GB fail. Seetõttu on ka aeg pikem. See ei ole erinev allalaadimisvalem.

### Miks tegelik aeg erineb?

Kiirust võivad piirata Wi-Fi häired ja signaal, ruuter, võrgukaart, server, sama ühendust kasutavad inimesed ning protokollide lisainfo. Väikeste päringute puhul mõjutab kasutuskogemust ka latentsus ehk viivitus.

1 Gbps ühendus ei tee iga tegevust automaatselt kümme korda kiiremaks kui 100 Mbps ühendus: tulemus sõltub sellest, mis tegevust parasjagu piirab.

## Sagedus: Hz, MHz ja GHz

Herts (Hz) kirjeldab perioodilise nähtuse tsüklite arvu sekundis. Protsessori taktsagedus näitab taktitsüklite sagedust; see ei ole sama mis tehtud käskude arv sekundis.

```text
1 kHz = 1000 Hz
1 MHz = 1000 kHz
1 GHz = 1000 MHz
3,6 GHz = 3600 MHz
```

**Suurem GHz ei taga kiiremat protsessorit.** Jõudlust mõjutavad ka arhitektuur, tuumad, vahemälu ja töökoormus. Kõige mõistlikum on võrrelda konkreetse ülesande tulemusi.

## Pikslid ja megapikslid

Piksel on rasterpildi pildipunkt. Pildi mõõtmed, näiteks 1920 × 1080, näitavad laiust ja kõrgust pikslites. Pikslite koguarvu saamiseks korruta need omavahel.

!!! note "Pea meeles"

    **1 MP = 1 000 000 pikslit.** Megapikslid = laius × kõrgus ÷ 1 000 000.

```text
1920 × 1080 = 2 073 600 pikslit
2 073 600 ÷ 1 000 000 = 2,0736 MP ≈ 2,07 MP

3840 × 2160 = 8 294 400 pikslit ≈ 8,29 MP
```

Suurem pikslite arv võib võimaldada rohkem detaili, kuid foto kvaliteeti mõjutavad ka sensor, optika, valgustus ja pakkimine. Pikslite arvust üksi ei saa teada faili mahtu: vaja on arvestada näiteks värviinfot ja pakkimisviisi.

## Kontrolli vastuse mõistlikkust

- Kas arvutasid mahtu, kiirust, aega, sagedust või pikslite arvu?
- Kas eristasid bitti ja baiti ning MB-d ja MiB-d?
- Kas väiksemasse ühikusse teisendades arv kasvas?
- Kas vastus on õiges ühikus ja ümardatud alles lõpus?

Näiteks 100 GB mängu allalaadimine 100 Mbps ühendusega kestab ideaaltingimustes 8000 sekundit ehk umbes 2 tundi ja 13 minutit. Mõnesekundiline tulemus viitaks ühikute või tehte veale.

### Proovi enne lahenduse avamist

??? info "4 GB = mitu MB?"

    4 × 1000 = 4000 MB.

??? info "800 Mbps = mitu MB/s?"

    800 ÷ 8 = 100 MB/s.

??? info "4,1 GHz = mitu MHz?"

    4,1 × 1000 = 4100 MHz.

??? info "2560 × 1440 pikslit = mitu MP?"

    2560 × 1440 = 3 686 400 pikslit. Jagades miljoniga saad 3,6864 MP ≈ 3,69 MP.

??? info "5 GB fail ja 200 Mbps ühendus: mitu sekundit?"

    5 GB = 5000 MB. 200 Mbps = 25 MB/s. 5000 ÷ 25 = 200 s.

## Kokkuvõte

- 1 B = 8 b.
- GB ja MB on 1000-põhised; GiB ja MiB on 1024-põhised.
- Mbps ÷ 8 = MB/s ja MB/s × 8 = Mbps.
- Aeg = maht ÷ kiirus, kui ühikud sobivad.
- GHz → MHz: korruta 1000-ga.
- MP = laius × kõrgus ÷ 1 000 000.
- Üks näitaja ei määra seadme või teenuse kvaliteeti.

### Mõtle õpitule

Milline ühik tekitas kõige rohkem segadust? Too näide, kus b ja B või MB ja MiB segiajamine annaks vale tulemuse. Selgita üht igapäevaselt nähtud seadmenäitajat oma sõnadega.

## Teema sõnastik

| Mõiste | Selgitus |
| --- | --- |
| Bitt (b) | Andmeühik, mille väärtus on 0 või 1. |
| Bait (B) | Kaheksa bitti. |
| Andmemaht | Andmete kogus, näiteks faili suurus baitides. |
| kB, MB, GB, TB | 1000-põhised baidi kordühikud: kilo-, mega-, giga- ja terabait. |
| KiB, MiB, GiB, TiB | 1024-põhised baidi kordühikud: kibi-, mebi-, gibi- ja tebibait. |
| Andmeedastuskiirus | Ajaühikus liikuv andmemaht. |
| bit/s; Mbps; Gbps | Bitti, miljon bitti ja miljard bitti sekundis. |
| MB/s; MiB/s | Miljon baiti ja 1 048 576 baiti sekundis. |
| Sagedus; Hz, MHz, GHz | Tsüklite arv sekundis; herts, miljon hertsi ja miljard hertsi. |
| Piksel (px) | Rasterpildi pildipunkt. |
| Eraldusvõime | Selles materjalis pildi või ekraani laius ja kõrgus pikslites. |
| Megapiksel (MP) | Miljon pikslit. |
| Teoreetiline allalaadimisaeg | Arvutuslik aeg, mis eeldab püsivat kiirust ja jätab lisakulud arvestamata. |
| Tegelik kiirus | Praktikas saavutatud andmeedastuskiirus. |
| Latentsus | Andmete liikumise või vastuse saabumise viivitus. |

## Allikad ja lisalugemine

- [NIST — kümnend- ja binaarsete andmeühikute võrdlus](https://physics.nist.gov/cuu/Units/binary.html)

