# Ülesanne: Serveri monitooring, logide uurimine ja võrguseadistus

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
- Debiani versioon.

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


## 2. Kõrge CPU kasutuse põhjuse leidmine ja parandamine

**5.** Leia protsess, mis põhjustab kõrget CPU kasutust. Pane kirja selle:

- PID;
- kasutaja;
- käsurida koos programmi või skripti asukohaga.

Uuri ka argumente: ainult tõlgendaja nimi ei ütle, milline skript töötab.

**6.** Uuri teenuse olekut, käivitusmääranguid ja logisid. Leia, kust loetakse selle seadistus. Ava konfiguratsioon ning selgita välja, **milline väärtus põhjustab liigset tööd**.

Pane aruandesse kirja faili asukoht, parameetri nimi ja algne väärtus. Lisa oma sõnadega selgitus, kuidas see seadistus CPU kasutust mõjutab.

**7.** Tee seadistusfailist koopia väljapoole aktiivseid konfiguratsioonikaustu. Paranda leitud väärtus põhjendatult ning rakenda muudatus teenusele.

Säilita teenuse põhifunktsioon. Teenuse seiskamine, keelamine või ainult prioriteedi vähendamine ei ole siin piisav lahendus.

**8.** Kontrolli teenuse olekut ja uusi logikirjeid. Tõenda, et teenus on aktiivne ning jätkab oma tööd pärast parandust.

**9.** Jälgi uuesti süsteemi umbes 30 sekundi jooksul samade tööriistade ja vaadetega nagu alguses. Täida mõõtmistabeli parandusejärgne veerg ning lisa koormuse muutust tõendav kuvatõmmis.

Selgita, milline näit muutus kohe ja kas koormuskeskmistes oli muutus juba näha. Märgi ka, kui tööriist pärast teenuse taaskäivitamist näitab uut PID-d.

## 3. Mittekäivituva teenuse parandamine logide abil

**10.** Uuri teenuse `oslab-andmekoguja.service` olekut. Leia selle logist **konkreetne veateade**, mis selgitab ebaõnnestumise põhjust.

Salvesta aruandesse veakirje koos ajatempliga. Vajaduse korral lisa ka sellele eelnevad või järgnevad read. Üldine teade `Failed to start` ei ole üksinda piisav põhjuse kirjeldus.

**11.** Leia veateates või teenuse määrangutes nimetatud konfiguratsioonifail. Pane kirja:

- faili asukoht;
- parameetri nimi ja vigane väärtus;
- millist tüüpi või vahemikus väärtust teenus ootab.

Põhjenda vastust logi ja konfiguratsiooni või programmi dokumentatsiooni abil.

**12.** Säilita algfail, paranda väärtus ning käivita teenus. Kontrolli, et teenus on aktiivne ja uusi mõõtmisi või muid ootuspäraseid tööteateid lisandub ka pärast käivitumist.

**13.** Lisa raportisse parandatud väärtus ja vähemalt üks **uuem** logikirje, mis tõendab teenuse tööd. 

## 4. Võrgu algseisu uurimine ja varundamine

**14.** Tuvasta kasutatav primaarne võrguliides ja praegune võrguseadistus. Tee muudetavatest võrguseadistuse failidest koopiad. Märgi aruandesse algfailide ja koopiate asukohad ning kirjelda, kuidas taastaksid nende abil senise võrguühenduse.


**15.** Määra käsitsi serverile uued võrguparameetrid:

- IP: 10.100.0.235
- Subnet mask: /24
- Gateway: 10.100.0.1

Säilita loopback-liidese seadistus.

**16.** Kontrolli, mis haldab liidese seadistust ja faili `/etc/resolv.conf`.

Lisa õigesse kohta järgmised nimeserverid: 1.1.1.1 ja 8.8.8.8

Pane raportisse kirja kuhu faili sa mingid parameetrid lisasid (võib teha kuvatõmmised).

Tee nii, et uued võrguseadistused jõustuksid. 

## 5. Uute võrguparameetrite testimine

**17.** Testi ühendust osade kaupa ja täida tulemuste tabel. Lisa iga kontrolli puhul **tegelik käsk, tulemus ja lühike järeldus**.

| Kontroll | Kasutatud käsk või tegevus | Tulemus ja järeldus |
| --- | --- | --- |
| Liidesel on määratud uus aadress ja prefiks. |  |  |
| Vaikimisi marsruut kasutab õiget lüüsi ja liidest. |  |  |
| Gateway on kättesaadav. |  |  |
| Üks DNS server on kättesaadav. |  |  |
| DNS-päring kasutab oodatud serverit ja saab vastuse. |  |  |
| Nimega pöördumine või süsteemi nimelahendus toimib. |  |  |

Kui ühenduse ei toimi, siis proovi tuvastada vea põhjus ja see parandada.

## 6. Püsimise kontroll pärast taaskäivitamist

**18.** Kui teenused ning võrk töötavad, taaskäivita virtuaalmasin.

**19.** Kontrolli pärast sisselogimist:

- kas uus aadress ja prefiks on alles;
- kas vaikimisi marsruut ja DNS-serverid vastavad uutele väärtustele;
- kas nimepäring ja ühendus kontrollsihtmärgiga toimivad;
- kas mõlemad õppeteenused käivituvad automaatselt ning töötavad;
- kas koormusteenuse CPU kasutus on endiselt vähenenud;
- kas andmekoguja uusi tööteateid lisandub.

Lisa taaskäivitusejärgsed kontrollide hinnang raportisse. Kui midagi ei püsinud, paranda põhjus ja kontrolli uuesti.

## 7. Töö esitamine

**20.** Esita Moodle'i ülesande alla **üks raport ühe pdf failina**. Lisa käsuväljundid ja vajalikud kuvatõmmised aruande vastava osa juurde.

Aruanne peab sisaldama:

- süsteemi põhiandmeid ja täidetud koormusmõõtmiste tabelit;
- mälu ja kettaruumi seisundi põhjendatud hinnangut;
- koormusteenuse probleemi põhjust, tehtud muudatust ja tulemust;
- andmekoguja konkreetset veateadet, parandust ja töö tõendit;
- uusi võrguparameetreid ning varukoopiate asukohti;
- võrgu kontrollide tulemusi ja järeldusi;
- taaskäivitusejärgset lõppkontrolli.

---

*Ülesande koostaja: Priit Paap, 2026*
