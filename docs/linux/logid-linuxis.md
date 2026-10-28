# Logid Linuxi operatsioonisüsteemis

**Logi** on süsteemi või rakenduse sündmuste kohta salvestatud kirjete kogum. Logidest saab uurida näiteks teenuse käivitumist, ebaõnnestunud sisselogimist, veebipäringut või seadistusviga. Need aitavad vastata küsimustele **mis juhtus, millal ja millises komponendis**.

!!! info "Õpieesmärgid"

    Pärast materjali läbimist oskad:

    - selgitada logide rolli veaotsingus ja süsteemihalduses;
    - eristada tekstilogifaili, systemd journal'it ja nende lugemise tööriistu;
    - leida teenuse ning kerneli logisid;
    - filtreerida kirjeid aja, teenuse, olulisuse ja otsingusõna järgi;
    - tõlgendada logikirje põhiandmeid ja seostada neid vea põhjusega;
    - selgitada logide roteerimise, säilitamise ja keskse kogumise vajadust;
    - koguda vea kohta asjakohased tõendid ning kontrollida paranduse tulemust.

## 1. Miks logisid vaja on?

Kui teenus ei käivitu, ei selgita olek `failed` üksinda põhjust. Rakenduse logis võib olla kirjas vigane parameeter, puuduv fail või port, mida teine programm juba kasutab.

| Eesmärk | Logide kasutamise näide |
| --- | --- |
| **Veaotsing — troubleshooting** | Leia, miks teenus pärast seadistuse muutmist ei käivitu. |
| **Töö jälgimine** | Kontrolli, kas ajastatud varundus jõudis lõpuni. |
| **Turvalisus** | Uuri ebaõnnestunud sisselogimiskatsete aega ja päritolu. |
| **Tegevuste kontrollimine** | Võrdle registreeritud seadistusmuudatust ja teenuse tõrke algust. |
| **Hiljem uurimine** | Taasta mitme teenuse kirjete põhjal sündmuste järjekord. |

**Logid ei salvesta automaatselt kõiki tegevusi.** Salvestatav teave sõltub rakenduse võimalustest, seadistusest ja logitasemest. Näiteks süsteemilogi ei ole kõigi kasutaja sisestatud käskude ega kogu võrguliikluse täielik salvestus.

Ka logikirje puudumine ei tõenda, et sündmust ei toimunud. Kirje võis jääda loomata, olla eemaldatud säilitamisreegli tõttu või puududa sinu valitud filtrist.

## 2. Logide liigid ja logikirje ülesehitus

### 2.1. Logi otstarve

Logisid saab liigitada nende sisu järgi. Rühmad kattuvad ning need ei määra üht kohustuslikku failinime.

| Logi liik | Tüüpiline sisu |
| --- | --- |
| **Süsteemilogi — system log** | OS-i ja süsteemiteenuste sündmused. |
| **Kerneli logi — kernel log** | Tuuma, riistvara ja draiveritega seotud teated. |
| **Autentimislogi — authentication log** | Sisselogimiskatsed ja kasutaja tuvastamisega seotud sündmused. |
| **Rakenduse logi — application log** | Rakenduse töö, vead ja seadistusega seotud teated. |
| **Ligipääsulogi — access log** | Näiteks veebiserverile saabunud päringud ja nende vastusekoodid. |
| **Auditilogi — audit log** | Seadistatud kontrolli jaoks registreeritud tegevused ja muudatused. |

**Sündmuste logi** (*event log*) on lai üldnimetus. Tarkvara **muudatustelogi** (*changelog*) kirjeldab sageli hoopis versioonide muudatusi; see ei ole sama mis süsteemis tehtud kasutajatoimingute auditilogi.

**Syslog** võib tähistada logiteadete edastamise viisi või protokolli. See ei ole iga Linuxi süsteemi ühe kindla logifaili nimi. `rsyslog` on üks logiteadete töötlemise ja edastamise tarkvara.

### 2.2. Mida logikirjest otsida?

Järgmine on lihtsustatud näidis, mitte ühe kohustusliku logivormingu kirjeldus:

```text
2026-10-16T12:10:04+03:00 lnxserver naidisteenus[1842]: ERROR: /etc/naidisteenus/app.conf: unknown parameter 'LogLevell'
```

| Osa | Näite tähendus |
| --- | --- |
| **Ajatempel — timestamp** | Sündmus registreeriti 16. oktoobril kell 12.10.04 ajavööndis UTC+3. |
| **Hostinimi — hostname** | Teade pärineb masinast `lnxserver`. |
| **Allikas** | Teate väljastas `naidisteenus`. |
| **PID — process ID** | Teatega seotud protsessi tunnus on `1842`. |
| **Tase — severity** | Rakendus tähistas teate veana: `ERROR`. |
| **Sõnum — message** | Seadistusfailis on tundmatu parameeter `LogLevell`. |

Mõni logi esitab samad andmed JSON-väljadena, mõni mitmel real ja mõni jätab osa välju välja. PID ei ole püsiv teenuse tunnus: pärast uut käivitamist võib see muutuda ja sama number võib hiljem kuuluda teisele protsessile.

### 2.3. Aeg ja logitasemed

Mitme masina logide võrdlemisel peavad kell ja ajavöönd olema arvesse võetud. Kell 12.00 UTC ja kell 15.00 UTC+3 tähistavad sama hetke. Vale kell võib jätta sündmustest vale järjekorra. Süsteemi aja ja sünkroonimise olekut saab uurida käsuga:

```bash
timedatectl
```

Täpne teave sõltub kasutatavast ajasünkroonimise teenusest.

Syslogi ja journal'i olulisustasemed on järgmised. Rakenduste endi nimetused võivad erineda.

| Arv | Nimi | Üldine tähendus |
| --- | --- | --- |
| **0** | `emerg` | Süsteem on kasutuskõlbmatu. |
| **1** | `alert` | Vaja on viivitamatut sekkumist. |
| **2** | `crit` | Kriitiline olukord. |
| **3** | `err` | Viga. |
| **4** | `warning` | Hoiatus. |
| **5** | `notice` | Oluline tavapärane sündmus. |
| **6** | `info` | Teavitav kirje. |
| **7** | `debug` | Üksikasjalik silumisteave. |

Väiksem arv tähendab suuremat olulisust. `INFO` ei tähenda automaatselt, et kõik on korras, ega `WARNING`, et teenus on kindlasti seiskunud. Loe sõnumit ja selle konteksti.

## 3. Kus logid Debian 13-s asuvad?

### 3.1. Tekstifailid ja journal

**Kõik logid ei ole tekstifailid.** Debian 13-s kogub paljusid sündmusi teenus **systemd-journald**, mis salvestab struktureeritud **journal'i**. Selle tavapärane lugemise tööriist on **journalctl**. `journalctl` ei ole logiteenuse ega logifaili nimi.

| Allikas või asukoht | Kuidas uurida? | Mida arvestada? |
| --- | --- | --- |
| **systemd journal** | `journalctl` | Sisaldab muu hulgas süsteemiteenuste ja kerneli kirjeid. |
| **`/var/log/syslog`** | Näiteks `less` ja `grep`. | Olemasolu sõltub logimistarkvarast ja seadistusest. |
| **`/var/log/auth.log`** | Näiteks `less` ja `grep`. | Traditsiooniline autentimisteadete fail; seda ei pruugi olla. |
| **`/var/log/apache2/`** | Veebiserveri logifailid. | Kui Apache on paigaldatud ja sinna logima seadistatud. |
| **`/var/log/nginx/`** | Veebiserveri logifailid. | Kui Nginx on paigaldatud ja sinna logima seadistatud. |
| **Rakenduse määratud asukoht** | Rakenduse juhendi järgi. | Logi võib paikneda mujal või minna journal'isse. |

Alates Debian 12-st ei paigaldata `rsyslog`-i baassüsteemi vaikimisi. Seetõttu ei saa ka Debian 13-s eeldada `/var/log/syslog` või `/var/log/auth.log` olemasolu. Nende puudumine ei tähenda üksinda, et süsteem ei logi.

### 3.2. Kerneli teated

Kerneli praeguse käivituse logi saab journal'ist vaadata nii:

```bash
sudo journalctl -k -b
```

Kerneli sõnumipuhvrit saab lugeda ka käsuga:

```bash
sudo dmesg
```

`dmesg` ei kuva kõigi rakenduste logisid ega ole ise ühe tekstilogifaili nimi. Puhvri maht on piiratud ning see ei ole kõigi varasemate käivituste püsiv arhiiv.

## 4. Tekstipõhiste logifailide lugemine

Järgmistes näidetes on `/var/log/rakendus.log` **näitlik failitee**. Asenda see oma süsteemis olemasoleva logifailiga. `sudo` on vajalik siis, kui tavakasutajal lugemisõigus puudub.

### 4.1. less: loe ja otsi

```bash
sudo less /var/log/rakendus.log
```

| Klahv | Tegevus |
| --- | --- |
| `Space` | Liigu ekraanitäis edasi. |
| `b` | Liigu ekraanitäis tagasi. |
| `g` | Mine faili algusse. |
| `G` | Mine faili lõppu. |
| `/error` ja `Enter` | Otsi edasisuunas sõna `error`. |
| `n` | Järgmine vaste. |
| `N` | Vaste vastassuunas. |
| `q` | Välju. |

Otsing on vaikimisi tõstutundlik. Suure faili uurimiseks on lehitseja sageli mugavam kui kogu faili terminali väljastamine.

### 4.2. tail: vaata lõppu ja uusi kirjeid

Viimased 30 rida:

```bash
sudo tail -n 30 /var/log/rakendus.log
```

Jooksvalt lisanduvad read:

```bash
sudo tail -f /var/log/rakendus.log
```

Jälgimise lõpetab `Ctrl+C`. Näiteks saab ühes terminalis logi jälgida ja teises teenuse käivitada, et näha just selle katse teateid.

**`tail -F`** jälgib failinime ja proovib faili uuesti avada ka selle asendamisel, näiteks logi roteerimise järel. Tavaline `-f` järgib vaikimisi avatud faili.

### 4.3. grep: filtreeri vajalikud read

```bash
sudo grep -in 'error' /var/log/rakendus.log
```

- `-i` eirab suur- ja väiketähtede erinevust;
- `-n` lisab faili reanumbrid;
- `'error'` on otsingumuster.

Vaste koos kahe eelneva ja järgneva reaga:

```bash
sudo grep -in -C 2 'error' /var/log/rakendus.log
```

Kindla teksti otsimiseks ilma regulaaravaldise erimärkide tõlgendamiseta saab kasutada `-F`:

```bash
sudo grep -nF 'app.conf' /var/log/rakendus.log
```

Sõna `error` puudumine ei tõenda vigade puudumist. Teade võib kasutada teist sõnastust; uuri ka teenuse nime, parameetrit, kellaaega ja ümbritsevaid ridu. [9]

## 5. Journal'i lugemine ja filtreerimine

### 5.1. Alusta uusimatest kirjetest

Lihtne käsk journal'i vaatamiseks on:

```bash
sudo journalctl -r
```

Valik **`-r`** (*reverse*) kuvab kirjed **uusimast vanimani**. Nii näed kõigepealt viimaseid sündmusi, mis on eriti mugav äsja tekkinud probleemi uurimisel. Ilma `-r` valikuta alustab `journalctl` vaikimisi vanimatest kirjetest.

Väljundit saab lehitsejas sirvida; väljumiseks vajuta **`q`**.

### 5.2. Vali teenus ja käivitus

Näidetes tähistab `naidis.service` **sinu uuritava teenuse tegelikku nime**.

```bash
sudo journalctl -u naidis.service -b
```

`-u` valib systemd üksuse, `-b` praeguse süsteemikäivituse. Ilma filtriteta võib logikirjeid olla väga palju. Väljund avatakse tavaliselt lehitsejas, millest väljub `q`.

| Käsk | Otstarve |
| --- | --- |
| `sudo journalctl -b` | Praeguse käivituse sündmused. |
| `sudo journalctl -u naidis.service -n 30 --no-pager` | Teenuse 30 viimast kirjet ilma lehitsejata. |
| `sudo journalctl -u naidis.service -f` | Teenuse uute kirjete jälgimine. |
| `sudo journalctl --list-boots` | Säilinud käivituste loend. |
| `sudo journalctl -b -1` | Eelmise käivituse logid, kui need on säilinud. |

`-f` jälgimise lõpetab `Ctrl+C`. Eelmise käivituse logide kättesaadavus sõltub säilitamise seadistusest ja sellest, kas vanad kirjed on veel alles.

### 5.3. Piira aega

Viimase kümne minuti sündmused ühe teenuse kohta:

```bash
sudo journalctl -u naidis.service --since "10 minutes ago"
```

Kindel ajavahemik:

```bash
sudo journalctl -u naidis.service --since "2026-10-16 12:00:00" --until "2026-10-16 12:15:00"
```

Asenda näite kuupäev uuritava sündmuse kuupäevaga. Ajavööndi eraldi määramata kasutatakse selle ajavormingu puhul süsteemi kohalikku aega.

### 5.4. Piira olulisust

```bash
sudo journalctl -b -p warning
```

See valib hoiatused **ja neist olulisemad kirjed** ehk tasemed 0–4. Ainult veataseme 3 jaoks saab kasutada vahemikku `-p err..err`.

!!! tip "Logitase ja sõnaotsing ei ole sama asi"

    journalctl-i valik -p kasutab kirje prioriteedivälja. grep otsib teksti. Kui rakendus edastab oma teated sama prioriteediga, võib sõnumis olla tekst ERROR, kuid prioriteedifilter seda kirjet mitte valida. Kontrolli vajaduse korral ka filtreerimata teenuselogi.

### 5.5. Kuidas oma prooviteadet leida?

Käsk `logger` võimaldab saata süsteemi logimistaristusse teate. See sobib logimise toimimise kontrollimiseks:

```bash
logger -t oppe-logi -p user.notice "Logimise prooviteade"
sudo journalctl -t oppe-logi --since "5 minutes ago" --no-pager
```

`logger -t` määrab sildi; `journalctl -t` valib selle sildiga kirjed. See proov ei kontrolli automaatselt kõigi teiste rakenduste logimist.

## 6. Teenuse vea uurimine logide abil

### 6.1. Olek ja põhjus

```bash
systemctl status naidis.service
```

Olek näitab, kas teenus töötab, ning kuvab sageli ka viimaseid teateid. Põhjuse uurimiseks ava teenuse täpsem logi. Kõigi ebaõnnestunud üksuste leidmiseks sobib `systemctl --failed`.

Tööjärjekord:

1. **Märgi sümptomi aeg.** Millal teenus viimati töötas ja millal viga ilmnes?
2. **Vali allikas.** Teenuse journal, rakenduse logifail või kerneli teated.
3. **Piira kirjeid.** Kasuta teenust, käivitust ja sobivat ajavahemikku.
4. **Leia konkreetne põhjus.** Näiteks vigane parameeter või ligipääsuõiguse puudumine.
5. **Kontrolli seadistust.** Võrdle veateadet faili tegeliku sisuga ja vajaduse korral dokumentatsiooniga.
6. **Paranda ja kontrolli.** Käivita teenus ning vaata uue katse logisid ja tegelikku tulemust.

### 6.2. Näidisjuhtum

Logis on:

```text
ERROR: /etc/naidisteenus/app.conf: unknown parameter 'LogLevell'
```

See ütleb rohkem kui üldine `Failed to start`:

- nimetatud on seadistusfail;
- vigane parameetrinimi on välja toodud;
- järgmiseks tuleb kontrollida õiget kirjapilti rakenduse juhendist.

Ära eelda, et iga punane rida on algpõhjus. Viimane teade võib kirjeldada ainult käivitamise ebaõnnestumist; konkreetne põhjus võib olla mõni rida varem.

Pärast rakenduse seadistuse parandamist võib olla vaja:

```bash
sudo systemctl restart naidis.service
```

Rakenduse tavalise `.conf` faili muutmine ei vaja üldjuhul `systemctl daemon-reload` käsku. See on vajalik systemd üksuse määrangute uuesti laadimiseks, kui muudeti üksusefaili. Mõni rakendus toetab ka seadistuse taaslaadimist ilma taaskäivituseta.

**Vana veateade võib pärast parandust logisse alles jääda.** Kontrolli viimase käivituskatse aega, uusi kirjeid ja teenuse tegelikku funktsiooni. Veateate kustutamine ei paranda teenust.

### 6.3. Salvesta asjakohased tõendid

```bash
sudo journalctl -u naidis.service --since "10 minutes ago" --no-pager > teenuse-logi.txt
```

Näide salvestab valitud kirjed praeguse kataloogi faili. `>` kirjutab olemasoleva faili üle. Lisa tõenditele enda selgitus: **mis oli põhjus, mida muutsid ja kuidas kontrollisid tulemust**.

## 7. Logide säilitamine ja roteerimine

### 7.1. Miks logid ei saa lõputult kasvada?

Logid kasutavad kettaruumi. Väga üksikasjalik silumine või pidevalt korduv viga võib kasvatada nende mahtu kiiresti. Säilitamise korralduses otsustatakse:

- mida ja millise detailsusega salvestada;
- kui kaua kirjeid vaja on;
- kui palju ruumi võib logidele kuluda;
- kellel on lugemise ja muutmise õigus;
- kas koopiaid kogutakse teise serverisse.

Logides võib olla kasutajanimesid, IP-aadresse ja rakenduse sisestatud andmeid. Jaga vea uurimiseks vajalikku osa ning väldi paroolide ja ligipääsutõendite sattumist logisse või esitatavasse väljavõttesse.

### 7.2. Tekstilogide roteerimine

**Roteerimine** (*log rotation*) tähendab logifailide haldamist nii, et aktiivse faili kõrvale tekivad varasemad logid, mida saab tihendada ja säilitamisreegli järgi eemaldada. Linuxis kasutatakse selleks sageli tööriista **logrotate**. Näitlikud nimed on `rakendus.log`, `rakendus.log.1` ja `rakendus.log.2.gz`; täpne nimetamine sõltub seadistusest. 

`logrotate`-i seadistused paiknevad tavaliselt failis `/etc/logrotate.conf` ja kataloogis `/etc/logrotate.d/`. Rakendus võib korraldada roteerimist ka ise. Seetõttu tuleb teada, kes konkreetset logifaili haldab.

### 7.3. Journal'i säilitamine

Journal'il on oma mahu- ja säilitamisreeglid. Selle andmeid ei hallata lihtsalt tekstilogidega samal viisil. Püsivad journal'i failid paiknevad tavaliselt `/var/log/journal/`, ajutised `/run/log/journal/`; kasutatav viis sõltub seadistusest. Ajutine salvestus ei säili taaskäivitamisel.

Journal'i ruumikasutuse vaatamine:

```bash
sudo journalctl --disk-usage
```

Roteerimine ega logide olemasolu kohalikul kettal ei taga iseenesest sõltumatut varukoopiat või kaitset masina rikke eest.

## 8. Keskne logihaldus

Kui servereid ja võrguseadmeid on palju, on igaühes eraldi otsimine ajakulukas. **Keskne logihaldus** kogub valitud kirjed ühisesse keskkonda, kus saab teha otsinguid, võrrelda eri seadmete sündmusi ja seadistada teavitusi.

Tüüpilised sammud on **kogumine → edastamine → töötlemine ja indekseerimine → otsing ning analüüs**. Indekseerimine valmistab andmed ette kiiremaks otsimiseks.

| Lahendus | Põhiidee |
| --- | --- |
| **Elastic Stack ehk ELK** | Elasticsearch otsib ja analüüsib andmeid, Logstash kogub ning töötleb neid, Kibana pakub veebiliidest uurimiseks ja visualiseerimiseks. ELK nimi tuleb nende algustähtedest; tervik võib sisaldada ka teisi komponente. |
| **Graylog** | Logihalduse keskkond keskseks kogumiseks, otsinguks ja analüüsiks.
| **Splunk** | Masinloodud andmete, sealhulgas logide kogumise, otsingu ja analüüsi platvorm. |
| **Loggly** | Pilvepõhine logide kogumise ja analüüsi teenus.  |

Need nimed on näited, mitte paigalduskohustus ega paremusjärjestus. Võimalused, väljaanded ja litsentsitingimused sõltuvad konkreetsest tootest ning versioonist. Alguses on tähtsam osata õigeid kirjeid leida ja mõista, millisele küsimusele need vastavad.

**Näide:** veebiteenuse tõrke uurimisel saab samasse ajavahemikku koondada veebiserveri ja andmebaasi teated. See aitab näha sündmuste seost; samaaegsus üksi ei tõenda veel põhjuslikkust.

## 9. Enesekontroll

Vasta kõigepealt ise. Seejärel ava vastus ja võrdle oma põhjendust.

### 1. Kas logid sisaldavad alati kõiki süsteemis tehtud tegevusi?

??? question "Vaata vastust"

    Ei. Sisu sõltub sellest, mida rakendus ja logimise seadistus salvestavad. Kirje võib ka puududa valitud filtrist või olla säilitamisreegli järgi eemaldatud.

### 2. Kas journalctl on logiteenuse nimi?

??? question "Vaata vastust"

    Ei. See on journal'i lugemise tööriist. Sündmusi kogub muu hulgas teenus systemd-journald.

### 3. Kas /var/log/syslog puudumine tähendab Debian 13-s logimise riket?

??? question "Vaata vastust"

    Mitte tingimata. Süsteem võib logida journal'isse ning traditsiooniliste tekstifailide loomiseks vajalikku rsyslog-it ei pruugi olla paigaldatud või vastavalt seadistatud.

### 4. Millist teavet otsiksid teenuse veakirjest?

??? question "Vaata vastust"

    Ajatemplit, allikat, konkreetset veasõnumit ning nimetatud faili, parameetrit või ressurssi. Kontrolliksin ka eelnevaid ja järgnevaid kirjeid.

### 5. Mis vahe on journalctl-i valikutel -u ja -b?

??? question "Vaata vastust"

    -u valib systemd üksuse, näiteks teenuse. -b piirab kirjed käivitusega; argumendita praeguse süsteemikäivitusega. Neid saab kasutada koos.

### 6. Kas journalctl -p warning näitab ainult hoiatusi?

??? question "Vaata vastust"

    Ei. See näitab hoiatusi ja neist olulisemaid kirjeid ehk tasemeid 0–4. Sõnaotsing grep-iga ja prioriteedivälja järgi filtreerimine on erinevad toimingud.

### 7. Kuidas jälgida tekstilogi uusi ridu ja kuidas jälgimine lõpetada?

??? question "Vaata vastust"

    Kasuta tail -f koos tegeliku logifaili teega. Lõpetamiseks vajuta Ctrl+C. Roteerimisel failinime järgi jätkamiseks võib sobida tail -F.

### 8. Teenus töötab pärast parandust, kuid logis on viga alles. Kas parandus ebaõnnestus?

??? question "Vaata vastust"

    Seda ei saa vana kirje põhjal järeldada. Vaata ajatemplit, viimase käivituskatse uusi kirjeid ja kontrolli teenuse tegelikku toimimist.

### 9. Milleks logisid roteeritakse?

??? question "Vaata vastust"

    Et hallata failide kasvu ja varasemate kirjete säilitamist. Vanemaid logisid saab tihendada ja eemaldada määratud reeglite järgi. See ei asenda automaatselt varundamist.

### 10. Mis kasu on logide kesksest kogumisest?

??? question "Vaata vastust"

    Mitme seadme sündmusi saab otsida ja võrrelda ühest keskkonnast. Nii on lihtsam uurida teenustevahelisi seoseid ja seadistada oluliste sündmuste teavitusi.

## Kokkuvõte

- Logid aitavad mõista sündmusi, leida vea põhjust ja kontrollida parandust.
- Logi sisu sõltub seadistusest; kõik sündmused ei pruugi olla salvestatud.
- Tekstifail ja struktureeritud journal vajavad erinevaid lugemisvahendeid.
- Debian 13-s alusta süsteemiteenuste uurimist sageli journalctl-ist.
- Tõlgenda ajatemplit, allikat, olulisust ja sõnumi konkreetset sisu.
- Piira otsingut teenuse ning ajaga ja loe ka vea ümbrust.
- less võimaldab lugeda, tail lõppu jälgida ja grep teksti filtreerida.
- Vana veakirje säilimine ei tähenda, et teenus on endiselt rikkis.
- Roteerimine, säilitamisreeglid ja keskne kogumine lahendavad eri ülesandeid.

### Mõtle õpitule

1. Teenus ei käivitu. Millises järjekorras kasutaksid olekuteavet, journal'it ja rakenduse seadistusfaili?
2. Kuidas võiks vale kellaaeg või liiga kitsas filter sind vea uurimisel eksitada?
3. Millised logikirjed esitaksid tõendina, et parandus aitas, ja mida lisaksid enda selgitusse?

## Teema sõnastik

| Mõiste | Selgitus |
| --- | --- |
| **Logi — log** | Salvestatud sündmuskirjete kogum. |
| **Logifail — log file** | Logikirjeid sisaldav fail; see ei pea olema tavaline tekstifail. |
| **Logikirje — log entry** | Üks logisse salvestatud sündmus või teade. |
| **Ajatempel — timestamp** | Kirjega seotud aja tähis. |
| **Logitase — severity level** | Teate olulisuse või detailsuse tähis. |
| **Silumine — debugging** | Vea põhjuste uurimine; debug-tase võib anda selleks üksikasjalikke teateid. |
| **Journal** | systemd struktureeritud logi. |
| **Roteerimine — log rotation** | Logifailide kasvu ja varasemate failide haldamine. |
| **Säilitamisreegel — retention policy** | Kord, kui kaua või millises mahus kirjeid hoitakse. |
| **Keskne logihaldus — centralized log management** | Mitme allika logide kogumine ja uurimine ühises keskkonnas. |
| **Indekseerimine — indexing** | Andmete ettevalmistamine tõhusamaks otsinguks. |


## Allikad ja lisalugemine

1. [RFC 5424 — The Syslog Protocol](https://www.rfc-editor.org/rfc/rfc5424.html).
2. [Debian manpages](https://manpages.debian.org/trixie/systemd/systemd-journald.service.8.en.html).
3. [The Elastic Stack](https://www.elastic.co/docs/get-started/the-stack).
4. [What is Graylog](https://go2docs.graylog.org/current/what_is_graylog/what_is_graylog.htm).
5. [Splunk Enterprise](https://www.splunk.com/en_us/products/splunk-enterprise.html).
6. [Product Overview](https://www.loggly.com/product/).

---

*Õppematerjali koostaja: Priit Paap, 2026*
