# Ülesanne: Debian 13 serveri monitooring, logide uurimine ja võrguseadistus

Oled saanud haldamiseks Debian 13 serveri, mille töös esineb probleeme. Serveri ressursikasutus on kõrge, üks teenus ei käivitu ning varasem dhcp põhine võrguseadistus vajab uuendamist.

Sinu ülesanne on **uurida algseisu, leida probleemide põhjused, teha vajalikud parandused ja tõendada kontrollidega, et server töötab nõutud viisil**.

!!! info "Õpieesmärgid"

    Ülesande sooritamisel harjutad:

    - süsteemi ressursikasutuse mõõtmist ja tulemuste tõlgendamist;
    - protsessi seostamist teenuse ning selle seadistusega;
    - logidest vea põhjuse leidmist;
    - püsiva IP- ja DNS-seadistuse lisamist;
    - ühenduse kontrollimist osade kaupa;
    - muudatuste dokumenteerimist ja paranduse mõju põhjendamist.

!!! warning "Enne alustamist"

    - Tee ülesanne õpetaja ettevalmistatud Debian 13 virtuaalmasinas kasutaja `student` õigustes.
    - Mitmed toimingud vajavad `sudo` kasutamist.
    - Tee võrgumuudatused konsoolilt, sest senine SSH-ühendus võib katkeda, muud toimingu võid sooritada SSH kaudu.
    - Säilita algne konfiguratsioon enne selle muutmist ehk kasuta seadistusfailide varundust.
    - Mõlemad õppeteenused peavad pärast parandamist töötama edasi. Ainult koormava protsessi lõpetamine või teenuse keelamine ei lahenda ülesannet.
    - Ära kustuta vea peitmiseks logisid ega muuda teenuste ressursipiiranguid.
    - Tee tegevused esitatud järjekorras ja kogu tõendid töö käigus.

## 1. Ettevalmistamine ja süsteemi algseis

**1.** Veendu, et kasutad õiget virtuaalmasinat. Pane raportisse kirja:

- oma nimi ja töö tegemise kuupäev;
- serveri hostinimi;
- Debiani versioon;

**2.** Uuri serveri algset koormust. Kasuta sobivaid vahendeid, näiteks `htop`, `top`, `uptime` ja `free -h`.

Jälgi CPU kasutust umbes **30 sekundi jooksul**. Pane kirja tüüpiline näit või vahemik; ära vali järelduse aluseks ainult üht juhuslikku hetke. Märgi mõõtmise aeg ja kasutatud tööriist.

Täida tabeli algseisu veerg. Parandusejärgse veeru täidad hiljem.

| Näitaja | Enne parandamist | Pärast parandamist |
| --- | --- | --- |
| Mõõtmise kellaaeg |  |  |
| Kogu süsteemi CPU kasutus või vaadeldud vahemik |  |  |
| Enim CPU-d kasutava protsessi nimi ja PID |  |  |
| Selle protsessi CPU kasutus või vahemik |  |  |
| Koormuskeskmised: 1, 5 ja 15 minutit |  |  |
| RAM-i kogumaht |  |  |
| Rakendustele saadaval RAM: `available` |  |  |
| Kasutatud swap |  |  |


**3.** Uuri kettaruumi kasutust. Pane vähemalt juurfailisüsteemi `/` kohta kirja:

- failisüsteemi allikas ja ühenduspunkt;
- kogumaht;
- kasutatud ja saadaval maht;
- kasutuse protsent.


**4.** Uuri käsu `du` abil kataloogi `/var/log` ruumikasutust. Leia seal üks suurema ruumikasutusega alamkataloog või fail ning pane selle nimi ja maht aruandesse. **Faile ei ole vaja kustutada.**

Anna lühihinnang: kas mälu ja kettaruumi näitude põhjal on põhjust kahtlustada lisaprobleemi? Põhjenda oma mõõtmistega. Ära eelda, et kõik suured näidud tähendavad viga.

## 2. Kõrge CPU kasutuse põhjuse leidmine ja parandamine

**6.** Leia protsess, mis põhjustab kõrget CPU kasutust. Pane kirja selle:

- PID;
- kasutaja;
- käsurida koos programmi või skripti asukohaga;
- seos teenusega `oslab-tootleja.service`.

Kui protsessi nimi on näiteks `python3`, uuri ka argumente: ainult tõlgendaja nimi ei ütle, milline skript töötab.

**7.** Uuri teenuse olekut, käivitusmääranguid ja logisid. Leia, kust loetakse selle seadistus. Ava konfiguratsioon ning selgita välja, **milline väärtus põhjustab liigset tööd**.

Pane aruandesse kirja faili asukoht, parameetri nimi ja algne väärtus. Lisa oma sõnadega selgitus, kuidas see seadistus CPU kasutust mõjutab.

**8.** Tee seadistusfailist koopia väljapoole aktiivseid konfiguratsioonikaustu. Paranda leitud väärtus põhjendatult ning rakenda muudatus teenusele.

Säilita teenuse põhifunktsioon. Teenuse seiskamine, keelamine või ainult prioriteedi vähendamine ei ole siin piisav lahendus.

**9.** Kontrolli teenuse olekut ja uusi logikirjeid. Tõenda, et teenus on aktiivne ning jätkab oma tööd pärast parandust.

**10.** Jälgi uuesti süsteemi umbes 30 sekundi jooksul samade tööriistade ja vaadetega nagu alguses. Täida mõõtmistabeli parandusejärgne veerg ning lisa koormuse muutust tõendav kuvatõmmis või käsuväljund.

Selgita, milline näit muutus kohe ja kas koormuskeskmistes oli muutus juba näha. Märgi ka, kui tööriist pärast teenuse taaskäivitamist näitab uut PID-d.

## 3. Mittekäivituva teenuse parandamine logide abil

**11.** Uuri teenuse `oslab-andmekoguja.service` olekut. Leia selle logist **konkreetne veateade**, mis selgitab ebaõnnestumise põhjust.

Salvesta aruandesse veakirje koos ajatempliga. Vajaduse korral lisa ka sellele eelnevad või järgnevad read. Üldine teade `Failed to start` ei ole üksinda piisav põhjuse kirjeldus.

**12.** Leia veateates või teenuse määrangutes nimetatud konfiguratsioonifail. Pane kirja:

- faili asukoht;
- parameetri nimi ja vigane väärtus;
- millist tüüpi või vahemikus väärtust teenus ootab.

Põhjenda vastust logi ja konfiguratsiooni või programmi dokumentatsiooni abil.

**13.** Säilita algfail, paranda väärtus ning käivita teenus. Kontrolli, et teenus on aktiivne ja uusi mõõtmisi või muid ootuspäraseid tööteateid lisandub ka pärast käivitumist.

**14.** Lisa aruandesse parandatud väärtus ja vähemalt üks **uuem** logikirje, mis tõendab teenuse tööd. Selgita, kuidas eristasid vana ebaõnnestunud katse kirjeid parandusejärgsetest kirjetest.

Leia ka selle õppeteenuse tekstipõhine logifail. Vaata faili viimaseid ridu ja leia `grep`-i abil varasem veateade. Pane aruandesse kasutatud käsud ja asjakohane väljavõte.

## 4. Võrgu algseisu uurimine ja varundamine

**15.** Tuvasta kasutatav võrguliides ja salvesta aruandesse senised võrguparameetrid. Täida tabeli algseisu ja uue seadistuse veerud. **Uued väärtused annab õpetaja; aadresse ei valita vabalt.**

| Parameeter | Senine väärtus | Õpetaja antud uus väärtus |
| --- | --- | --- |
| Võrguliidese nimi |  |  |
| IPv4-aadress |  |  |
| Prefiksi pikkus või alamvõrgumask |  |  |
| Vaikimisi lüüs |  |  |
| DNS-server 1 |  |  |
| DNS-server 2, kui määratud |  |  |

Pane lisaks kirja õpetaja määratud **kontrollsihtmärk**: teine laboriseade või väline IP-aadress ning nimi, mille lahendumist saad kontrollida. Välisühendust kontrolli siis, kui see on laborivõrgus lubatud.

**16.** Kontrolli, mis haldab liidese seadistust ja faili `/etc/resolv.conf`.

Selle labori võrguseadistuse muutmise näide lähtub **ifupdown-ist**, failist `/etc/network/interfaces` või sealt kaasatud failist ning **käsitsi hallatavast `/etc/resolv.conf`-ist**. Vaata tegelikku konfiguratsiooni ja seda, kas `resolv.conf` on tavaline fail või sümbollink.

Kui masinas kasutatakse teist haldurit või genereeritavat DNS-faili, täpsusta õpetajaga selle masina seadistusviis enne muutmist. Ära vaheta võrguhaldurit üksnes selleks, et ülesande failinimedega sobituda.

**17.** Tee muudetavatest võrguseadistuse failidest koopiad. Märgi aruandesse algfailide ja koopiate asukohad ning kirjelda, kuidas taastaksid nende abil senise võrguühenduse.

Kui liidese seadistus paikneb kaasatud failis, peab koopia hõlmama ka seda faili. Enne järgmise osa alustamist kontrolli veel kord konsoolile ligipääsu.

## 5. Uute võrguparameetrite seadistamine ja testimine

**18.** Määra õpetaja antud **püsiv IPv4-aadress, prefiks ja vaikimisi lüüs**. Muuda ka käsitsi hallatavas `/etc/resolv.conf` failis DNS-serverite aadressid.

Rakenda muudatused kasutatava võrguhalduri kaudu. Kui pead liidese senise konfiguratsiooni lõpetama, tee seda konsoolilt **enne vana seadistuse asendamist**. Säilita teiste liideste ja loopback-liidese vajalik seadistus.

Ainult `ip`-käsuga aadressi lisamine ei täida püsiseadistuse nõuet.

**19.** Testi ühendust osade kaupa ja täida tulemuste tabel. Lisa iga kontrolli puhul **tegelik käsk, tulemus ja lühike järeldus**.

| Kontroll | Kasutatud käsk või tegevus | Tulemus ja järeldus |
| --- | --- | --- |
| Liidesel on määratud uus aadress ja prefiks. |  |  |
| Vaikimisi marsruut kasutab õiget lüüsi ja liidest. |  |  |
| Lüüs või õpetaja määratud kohalik kontrollsihtmärk on kättesaadav. |  |  |
| Õpetaja määratud teine IP-aadress on kättesaadav. |  |  |
| DNS-päring kasutab oodatud serverit ja saab vastuse. |  |  |
| Nimega pöördumine või süsteemi nimelahendus toimib. |  |  |

Nimelahenduse kontrollimisel erista DNS-päringut ja süsteemi üldist nimeotsingut: näiteks `/etc/hosts` kirje võib nime lahendada ilma DNS-päringuta. Sobivad vahendid on muu hulgas `nslookup` või `dig` ning `getent`.

Kui `ping` vastust ei saa, uuri põhjust. Üksnes vastuse puudumine ei tõenda alati võrguühenduse katkemist, sest ICMP võib olla piiratud. Vajaduse korral kontrolli õpetaja määratud sihtteenust.

## 6. Püsimise kontroll pärast taaskäivitamist

**20.** Salvesta pooleliolev aruanne ja tõendid. Kui teenused ning võrk töötavad, taaskäivita virtuaalmasin.

**21.** Kontrolli pärast sisselogimist:

- kas uus aadress ja prefiks on alles;
- kas vaikimisi marsruut ja DNS-serverid vastavad uutele väärtustele;
- kas nimepäring ja ühendus kontrollsihtmärgiga toimivad;
- kas mõlemad õppeteenused käivituvad automaatselt ning töötavad;
- kas koormusteenuse CPU kasutus on endiselt vähenenud;
- kas andmekoguja uusi tööteateid lisandub.

Lisa aruandesse taaskäivitusejärgsed kontrollid ja nende tulemused. Kui midagi ei püsinud, paranda põhjus ja kontrolli uuesti.

## 7. Analüüsiküsimused

**22.** Vasta järgmistele küsimustele oma aruandes. Toetu võimaluse korral enda kogutud näitudele ja logikirjetele; ainult mõiste definitsioonist ei piisa.

**A1. CPU kasutus ja koormuskeskmine.** Miks ei pruugi ühe protsessi CPU näit olla sama suur kui kogu virtuaalmasina CPU kasutus? Seosta vastus oma masina vCPU-de arvuga. Miks ei pruugi 5 ja 15 minuti koormuskeskmised kohe pärast parandust madalad olla?

**A2. Mälu ja kettaruum.** Kas sinu serveris ilmnes nende ressursside puudujäägi tunnuseid? Põhjenda vähemalt kahe mõõdetud väärtusega. Selgita ka, miks vähene täiesti vaba RAM ei tähenda alati mälupuudust.

**A3. Põhjuse kõrvaldamine.** Miks pole koormava protsessi lõpetamine või teenuse keelamine piisav lahendus? Milliste tõenditega näitasid, et sinu parandus vähendas koormust ja säilitas vajaliku funktsiooni?

**A4. Logi tõlgendamine.** Milline konkreetne logikirje aitas sul andmekoguja vea leida? Mida see ütles, mida üldine `failed`-olek ei öelnud? Miks võib varasem veateade pärast parandust endiselt logis alles olla?

**A5. Võrgu vea eristamine.** Oletame, et teise seadmega saab IP-aadressi abil ühendust, kuid nime abil mitte. Mida kontrolliksid ja millises järjekorras? Kuidas eristaksid DNS-i probleemi valest IP-aadressist või lüüsist?

**A6. Püsivus ja taastamine.** Miks ei piisa edukast ühenduse testist enne taaskäivitamist? Kirjelda lühidalt, kuidas taastaksid algseisu, kui võrgumuudatuse järel kaoks kaugühendus.

## 8. Töö esitamine

**23.** Esita Moodle'i ülesande alla **üks aruanne** õpetajaga kokkulepitud vormingus. Lisa käsuväljundid ja vajalikud kuvatõmmised aruande vastava osa juurde.

Aruanne peab sisaldama:

- süsteemi põhiandmeid ja täidetud koormusmõõtmiste tabelit;
- mälu ja kettaruumi seisundi põhjendatud hinnangut;
- koormusteenuse probleemi põhjust, tehtud muudatust ja tulemust;
- andmekoguja konkreetset veateadet, parandust ja töö tõendit;
- seniseid ja uusi võrguparameetreid ning varukoopiate asukohti;
- võrgu kontrollide tulemusi ja järeldusi;
- taaskäivitusejärgset lõppkontrolli;
- vastuseid kuuele analüüsiküsimusele.

Esita asjakohased logiväljavõtted, mitte kogu journal'i sisu. Tõendist peab olema arusaadav, millist kontrolli see näitab ja kas see tehti enne või pärast parandust.

## Ülesande lõppkontroll

Enne esitamist veendu, et:

- [ ] `oslab-tootleja.service` töötab ja selle liigne CPU kasutus on kõrvaldatud.
- [ ] `oslab-andmekoguja.service` töötab ning lisab uusi tööteateid.
- [ ] Mälu ja kettaruumi kasutus on uuritud ning hinnang põhjendatud.
- [ ] Uued võrguparameetrid ja DNS-seadistus on salvestatud ning testitud.
- [ ] Teenused ja võrk töötavad ka pärast taaskäivitamist.
- [ ] Aruandes on vajalikud tõendid, muudatuste selgitused ja analüüsivastused.

---

*Ülesande koostaja: Priit Paap, 2026*
