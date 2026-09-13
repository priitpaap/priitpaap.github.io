icon: material/magnify

# Linuxi otsinguvahendid

Linuxi halduses tuleb sageli leida faile, katalooge, tekstiridu, programme ja süsteemilogidest vajalikku infot. Selleks on Linuxis mitu otsinguvahendit, millest igaüks sobib veidi erineva ülesande jaoks.

Selles materjalis käsitletavad põhivahendid töötavad nii **Debiani laadsetes** kui ka **Red Hati laadsetes** Linuxi distributsioonides. Mõned erinevused esinevad näiteks traditsiooniliste logifailide asukohtades ja `locate` käsu taustal kasutatavas tarkvaras.

!!! info "Seos varasemate teemadega"
    Failihalduse teemas õppisid Linuxi kataloogipuus liikuma ning faile ja katalooge haldama. Otsinguvahendid võimaldavad vajalikke objekte leida ka siis, kui nende täpne asukoht ei ole teada.

---

## Õpieesmärgid

Pärast materjali läbimist oskad:

- valida otsinguülesande jaoks sobiva Linuxi tööriista;
- kasutada `grep`-i teksti otsimiseks ja tulemuste filtreerimiseks;
- kasutada toru `|` ühe käsu väljundi suunamiseks teisele käsule;
- kasutada `find`-i failide ja kataloogide otsimiseks nime, tüübi, omaniku, suuruse ja muutmisaja järgi;
- kasutada metamärke otsingumustrites;
- selgitada `find`-i ja `locate`-i erinevust;
- kasutada `locate`-i kiireks failinime järgi otsimiseks;
- leida käsu asukohta käsuga `command -v`;
- kasutada `journalctl`-i systemd logide vaatamiseks ja otsimiseks;
- arvestada olulisemate erinevustega Debiani ja Red Hati laadsete süsteemide logides.

---

## Miks Linuxis otsingut vaja on?

Süsteemiadministraator ei tea alati vajaliku faili täpset asukohta ega seda, millises logikirjes probleem kajastub.

Näiteks võib olla vaja:

- leida konfiguratsioonifail;
- otsida logidest veateadet;
- leida kõik kindlale kasutajale kuuluvad failid;
- leida kettaruumi kasutavad suured failid;
- leida hiljuti muudetud failid;
- kontrollida, kus asub mingi käivitatav programm.

Erinevate ülesannete jaoks kasutatakse erinevaid vahendeid.

| Mida soovid leida? | Sobiv vahend |
|---|---|
| teksti faili või käsu väljundi seest | `grep` |
| faili või kataloogi omaduste järgi | `find` |
| faili kiiresti nime järgi | `locate` |
| käsu või programmi asukohta | `command -v` |
| systemd süsteemi- ja teenuslogisid | `journalctl` |

!!! tip "Vali tööriist vastavalt sellele, mida otsid"
    Enne käsu kirjutamist mõtle, kas otsid **teksti**, **faili**, **programmi** või **logikirjet**. Õige tööriista valimine muudab otsingu lihtsamaks.

---

## `grep` - teksti otsimine

`grep` otsib sisendist ridu, mis vastavad etteantud otsingumustrile. Sisendiks võib olla fail või mõne teise käsu väljund.

Üldkuju:

```bash
grep [võtmed] muster fail
```

Näiteks failis `kasutajad.txt` sõna `student` sisaldavate ridade leidmiseks:

```bash
grep "student" kasutajad.txt
```

Mitmest sõnast koosnev otsingumuster pannakse jutumärkidesse:

```bash
grep "login failed" log.txt
```

### Kasulikud `grep` võtmed

| Võti | Tähendus |
|---|---|
| `-i` | ei erista suuri ja väikseid tähti |
| `-n` | näitab leitud rea numbrit |
| `-v` | näitab ridu, mis mustrile **ei vasta** |
| `-w` | otsib tervet sõna |
| `-r` | otsib kataloogist ja selle alamkataloogidest rekursiivselt |
| `-l` | kuvab ainult nende failide nimed, milles vaste leiti |

Näited:

```bash
grep -i "error" log.txt
grep -n "student" kasutajad.txt
grep -v "student" kasutajad.txt
grep -w "root" kasutajad.txt
grep -r "PermitRootLogin" /etc/ssh/
grep -rl "student" /home/
```

!!! note "`-r` ja `-R`"
    Rekursiivseks otsimiseks piisab tavaliselt võtmest `-r`. `-R` töötab samuti rekursiivselt, kuid järgib lisaks otsingu käigus kohatud sümbollinke. Alustuseks kasuta enamasti `-r`.

### `grep` ja toru

Linuxi käsureal saab ühe käsu väljundi suunata teise käsu sisendiks. Selleks kasutatakse **toru** ehk märki `|`.

```text
käsk 1 → | → käsk 2
```

Näiteks kui kataloogis on palju faile ja soovid `ls -l` väljundist leida faili, mille nimes esineb `raport`:

```bash
ls -l | grep "raport"
```

Esimene käsk:

```bash
ls -l
```

kuvab kataloogi sisu. Toru `|` suunab selle väljundi `grep`-ile, mis jätab alles ainult read, kus esineb sõna `raport`.

Näiteks võib tulemus olla:

```text
-rw-r--r-- 1 student student 2450 Sep 13 10:15 raport.txt
-rw-r--r-- 1 student student 1830 Sep 12 14:20 raport_vana.txt
```

Samal viisil saab filtreerida ka teiste käskude väljundit. Näiteks töötavate protsesside hulgast SSH-ga seotud ridade otsimiseks:

```bash
ps aux | grep "ssh"
```

Siin:

```text
ps aux → kuvab töötavad protsessid
|      → suunab väljundi järgmisele käsule
grep   → jätab alles otsingumustrile vastavad read
```

!!! tip "Toru muudab käsud kombineeritavaks"
    `grep` ei pea otsima ainult faili sisust. Seda kasutatakse väga sageli mõne teise käsu väljundi filtreerimiseks. See on Linuxi käsureal üks olulisemaid töövõtteid.

!!! note "`grep` filtreerib teksti"
    Käsus `ls -l | grep "raport"` ei otsi `grep` otse failisüsteemist. `ls -l` loob tekstilise väljundi ja `grep` otsib sellest tekstist sobivaid ridu. Failide omaduste järgi otsimiseks sobib paremini `find`.

---

## `find` - failide ja kataloogide otsimine

`find` otsib otse failisüsteemist ning võimaldab määrata, **kust otsida** ja **millistele tingimustele objekt peab vastama**.

Lihtsustatud üldkuju:

```bash
find [asukoht] [tingimused]
```

Näiteks:

```bash
find /home -name "*.txt"
```

Selle käsu saab lugeda järgmiselt:

```text
find /home -name "*.txt"
     │     │      │
     │     │      └── otsitav nimi või muster
     │     └───────── otsimine nime järgi
     └─────────────── otsingu alguskoht
```

Ilma täiendava tegevuseta kuvab `find` leitud objektide teed.

---

## Nime järgi otsimine

Täpselt nime järgi otsimiseks kasutatakse `-name` tingimust:

```bash
find /home -name "kola.txt"
```

See otsib kataloogist `/home` ja selle alamkataloogidest objekte nimega `kola.txt`.

`-name` eristab suuri ja väikseid tähti. Kui tähesuurust ei soovita eristada, kasuta `-iname`:

```bash
find /home -iname "kola.txt"
```

Sellisel juhul võivad vasteks olla näiteks:

```text
kola.txt
Kola.txt
KOLA.TXT
```

---

## Metamärgid otsingus

Failinimede otsimisel kasutatakse sageli metamärke ehk *wildcard'e*.

| Märk | Tähendus |
|---|---|
| `*` | suvaline arv märke |
| `?` | täpselt üks suvaline märk |

Kõigi `.txt` lõpuga failide otsimine:

```bash
find /home -name "*.txt"
```

Näiteks muster:

```text
test?.txt
```

võib sobida nimedega:

```text
test1.txt
testA.txt
testx.txt
```

!!! note "Miks kasutatakse jutumärke?"
    Muster `"*.txt"` pannakse jutumärkidesse, et shell ei laiendaks `*` märki enne `find` käsu käivitamist. Nii töötleb mustrit `find` ise.

---

## Faili tüübi järgi otsimine

`-type` võimaldab määrata otsitava objekti tüübi.

| Tingimus | Objekti tüüp |
|---|---|
| `-type f` | tavaline fail |
| `-type d` | kataloog |
| `-type l` | sümbollink |

Kõigi tavaliste `.txt` failide leidmine:

```bash
find /home -type f -name "*.txt"
```

Kõigi kataloogide leidmine nimega `projekt`:

```bash
find /home -type d -name "projekt"
```

Kõigi sümbollinkide leidmine:

```bash
find /home -type l
```

---

## Omaniku järgi otsimine

Kasutajale `student` kuuluvate failide otsimiseks:

```bash
find /home -type f -user student
```

Kui otsida kogu failisüsteemist:

```bash
sudo find / -user student
```

!!! note "Permission denied"
    Tavakasutajal ei ole õigust kõiki süsteemi katalooge lugeda. Seetõttu võib kogu failisüsteemist otsides ilmuda `Permission denied` teateid. Administraatoriõigustega otsimiseks võib kasutada `sudo`-t, kuid seda tuleks teha ainult siis, kui kogu süsteemi läbivaatamine on päriselt vajalik.

---

## Suuruse järgi otsimine

Faili suuruse järgi otsimiseks kasutatakse `-size` tingimust.

Üle 50 MiB suurused failid:

```bash
find /home -type f -size +50M
```

Alla 10 MiB suurused failid:

```bash
find /home -type f -size -10M
```

Failid suurusega üle 50 MiB, kuid alla 100 MiB:

```bash
find /home -type f -size +50M -size -100M
```

Siin tähendab:

```text
+50M → suurem kui 50 MiB
-100M → väiksem kui 100 MiB
```

!!! note "Mida tähendab `M`?"
    GNU `find` kasutab `-size` juures sufiksit `M` 1 048 576-baidiste ühikute jaoks. Igapäevases kõnes nimetatakse neid sageli megabaitideks, täpsemalt on tegemist MiB-suurusjärguga.

---

## Muutmisaja järgi otsimine

Faili sisu viimase muutmise aja järgi saab otsida `-mtime` abil.

Viimase seitsme ööpäeva jooksul muudetud failid:

```bash
find /home -type f -mtime -7
```

Vanemad kui 30 päeva:

```bash
find /home -type f -mtime +30
```

Minutites saab otsida `-mmin` abil.

Viimase 60 minuti jooksul muudetud failid:

```bash
find /home -type f -mmin -60
```

!!! note "`mtime` ja `ctime` ei tähenda sama asja"
    `mtime` kirjeldab faili **sisu muutmise aega**. `ctime` kirjeldab inode'i oleku muutmise aega, näiteks õiguste või omaniku muutmist. `ctime` ei tähenda Linuxis faili loomise aega.

---

## Otsingu sügavuse piiramine

Vaikimisi liigub `find` läbi kõigi otsingu alguskoha all olevate alamkataloogide.

`-maxdepth` võimaldab otsingu sügavust piirata.

Ainult `/home/student` kataloogis olevate objektide vaatamine:

```bash
find /home/student -maxdepth 1
```

Kuni kahe taseme sügavuseni:

```bash
find /home/student -maxdepth 2
```

See on kasulik, kui kogu kataloogipuu läbimine pole vajalik.

---

## Mitme tingimuse kasutamine

`find` võimaldab tingimusi kombineerida.

Näiteks:

```bash
find /home -type f -name "*.log" -size +10M
```

leiab objektid, mis on korraga:

- tavalised failid;
- `.log` lõpuga;
- suuremad kui 10 MiB.

Tingimuste kõrvuti kirjutamisel rakendatakse tavaliselt loogilist **JA** seost.

---

## Otsingutulemusega tegutsemine

Kõige turvalisem on kõigepealt otsing käivitada ja kontrollida, millised objektid leitakse:

```bash
find /home/student -type f -name "*.tmp"
```

`-print` väljendab tegevuse selgelt:

```bash
find /home/student -type f -name "*.tmp" -print
```

Leitud objektidega saab teha ka tegevusi. Näiteks `-delete` kustutab leitud objektid:

```bash
find /home/student -type f -name "*.tmp" -delete
```

!!! danger "Kontrolli enne kustutamist"
    Ära lisa `-delete` valikut enne, kui oled sama otsingu ilma kustutamiseta käivitanud ja tulemused üle kontrollinud. Valesti koostatud otsing võib mõjutada suurt hulka faile.

### `-exec`

`find` saab iga leitud objekti jaoks käivitada ka teise käsu. Selleks kasutatakse `-exec` valikut.

Üldkuju:

```bash
find asukoht tingimused -exec käsk {} \;
```

Siin:

```text
{}  → asendatakse leitud objekti nimega
\;  → lõpetab -exec käsu
```

Praktiline näide – WordPressi failiõigused:

Veebiserveri kataloogis võivad failid ja kataloogid vajada erinevaid õigusi. Näiteks WordPressi failid asuvad kataloogis:

```text
/var/www/wordpress
```

Kõigi selle kataloogi ja selle all olevate **kataloogide** leidmiseks:

```bash
find /var/www/wordpress -type d
```

Kui oled tulemused üle kontrollinud, saad määrata kõigile leitud kataloogidele õigused `755`:

```bash
sudo find /var/www/wordpress -type d -exec chmod 755 {} \;
```

Kõigi **tavaliste failide** leidmiseks:

```bash
find /var/www/wordpress -type f
```

Pärast tulemuste kontrollimist saad määrata kõigile leitud failidele õigused `644`:

```bash
sudo find /var/www/wordpress -type f -exec chmod 644 {} \;
```

Tulemuseks on:

```text
kataloogid → 755 → rwxr-xr-x
failid     → 644 → rw-r--r--
```

See on parem kui näiteks:

```bash
sudo chmod -R 755 /var/www/wordpress
```

sest rekursiivne `chmod -R 755` annaks `755` õigused ka tavalistele failidele ning muudaks need käivitatavaks.

`find` võimaldab failid ja kataloogid eraldi valida:

```text
find ... -type d → ainult kataloogid → chmod 755
find ... -type f → ainult failid     → chmod 644
```

!!! tip "Kõigepealt otsi, seejärel muuda"
    Enne `-exec chmod` kasutamist käivita sama `find` käsk ilma `-exec` osata. Nii saad kontrollida, milliste objektide õigusi muudetakse.

!!! warning "Õigused sõltuvad rakendusest"
    `755` kataloogidele ja `644` failidele on levinud lähtekoht veebirakenduse failide puhul, kuid see ei ole universaalne reegel. Rakendus võib vajada mõnele failile või kataloogile teistsuguseid õigusi ning oluline on ka õige omanik ja grupp.
---

## `locate` - kiire otsing nime järgi

`locate` otsib failinimesid eelnevalt koostatud andmebaasist. Seetõttu on see suure failisüsteemi puhul tavaliselt väga kiire.

Näiteks:

```bash
locate passwd
```

```bash
locate "*.conf"
```

Tähesuurust eirav otsing:

```bash
locate -i arved.odt
```

Oluline erinevus:

```text
find    → vaatab otsingu ajal failisüsteemi
locate  → otsib eelnevalt koostatud andmebaasist
```

Seetõttu võib `locate` anda tulemuse faili kohta, mis on juba kustutatud, või mitte leida äsja loodud faili, kui andmebaasi pole veel uuendatud.

Andmebaasi saab administraatoriõigustega värskendada käsuga:

```bash
sudo updatedb
```

!!! info "`plocate` tänapäeva distributsioonides"
    Tänapäevastes Debianis ja RHEL-is võib `locate` käsu taga olla **plocate**. Vanemates juhendites kohtab sageli nime **mlocate**. Kasutaja jaoks jääb tavaliseks otsingukäsuks siiski `locate`.

!!! note "`locate` ei pruugi olla vaikimisi paigaldatud"
    `find` kuulub tavaliselt Linuxi põhivahendite hulka, kuid `locate`/`plocate` võib vajada eraldi paketi paigaldamist. Seetõttu peab administraator oskama kasutada `find`-i ka ilma `locate`-ita.

---

## Käsu asukoha leidmine

Kui soovid teada, millise käsu shell vastava nimega leiab, kasuta:

```bash
command -v käsunimi
```

Näiteks:

```bash
command -v bash
command -v ssh
command -v python3
```

Väljund võib olla näiteks:

```text
/usr/bin/ssh
```

`command -v` võib näidata ka seda, kui nimi viitab shelli sisseehitatud käsule, alias'ele või funktsioonile.

!!! note "Aga `which`?"
    Programmi asukoha leidmiseks kasutatakse sageli ka `which` käsku. Shelli sisseehitatud `command -v` on üldine viis kontrollida, kuidas shell antud käsunime lahendab. Administraatorina tasub mõlemad käske tunda.

---

## `journalctl` - systemd logide otsimine

Tänapäevased Debian ja RHEL kasutavad systemd-d ning süsteemi journali saab vaadata käsuga `journalctl`. Käsku kasutades võiks meeles pidada, et `sudo` õigustes näeb rohkem infot kui tavakasutajana.

Kõigi kasutajale nähtavate journalikirjete vaatamine:

```bash
journalctl
```

### Uusimad kirjed esimesena

Praktiliseks tõrkeotsinguks on väga kasulik võti `-r` (*reverse*), mis kuvab journalikirjed **uuemast vanemani**:

```bash
journalctl -r
```

Nii näed kohe kõige hiljutisemaid sündmusi. See on kasulik näiteks siis, kui teenus andis äsja veateate ja sa ei mäleta parasjagu ühtegi täpsemat `journalctl` filtrit.

!!! tip "Kui muud meeles ei ole, proovi `journalctl -r`"
    Tõrkeotsingu alustamiseks on `journalctl -r` mugav käsk, sest kõige värskemad logikirjed kuvatakse kohe esimesena. Kui oled probleemi kohta rohkem teada saanud, saad otsingut täpsustada näiteks teenuse, prioriteedi või aja järgi.

### Praeguse alglaadimise kirjed

```bash
journalctl -b
```

### Veatasemega kirjed

```bash
journalctl -p err
```

### Tänased kirjed

```bash
journalctl --since today
```

Võtmeid saab ka kombineerida. Näiteks praeguse alglaadimise veateated, uusimad ees:

```bash
journalctl -b -p err -r
```

### Teenuse logid

Konkreetse systemd unit'i logide vaatamiseks kasutatakse `-u` võtit:

```bash
journalctl -u ssh
```

või näiteks:

```bash
journalctl -u NetworkManager
```

Unit'i nimi võib distributsiooniti või paigaldatud tarkvarast sõltuvalt erineda. Vajaduse korral kontrolli teenuse nime:

```bash
systemctl status teenus
```

### `journalctl` ja `grep`

`journalctl` väljundit saab omakorda `grep`-iga filtreerida.

Näiteks:

```bash
journalctl | grep -i "error"
```

või:

```bash
journalctl -u ssh | grep -i "failed"
```

Siin täidavad tööriistad eri ülesandeid:

```text
journalctl → valib süsteemi või teenuse logikirjed
grep       → otsib saadud tekstist sobivaid ridu
```

!!! tip "Eelista võimalusel `journalctl` enda filtreid"
    Kui `journalctl` oskab vajaliku valiku ise teha, kasuta esmalt selle võtmeid, näiteks `-u`, `-p`, `-b` või `--since`. `grep` sobib hästi siis, kui soovid saadud tekstist veel kindlat sõna või mustrit otsida.

---

## Debian ja Red Hat - olulised erinevused

`grep`, `find`, `command -v` ja `journalctl` põhikasutus on Debiani ja Red Hati laadsetes süsteemides üldiselt sama.

Erinevusi kohtab eelkõige distributsioonispetsiifilistes failides, teenusenimedes ja paigaldatud pakettides.

### Traditsioonilised autentimislogid

Kui süsteemis kasutatakse traditsioonilisi tekstipõhiseid syslogi logifaile, erinevad nende nimed distributsiooniti.

| Süsteemiperekond | Levinud autentimislogi |
|---|---|
| Debian-laadne | `/var/log/auth.log` |
| Red Hat-laadne | `/var/log/secure` |

Seetõttu ei ole näiteks:

```bash
grep "student" /var/log/auth.log
```

universaalne Linuxi käsk.

Systemd journaliga saab paljusid logisid uurida distributsioonist sõltumatumalt:

```bash
journalctl
```

!!! note "Kõiki logisid ei pea leiduma `/var/log` tekstifailidena"
    Logimise seadistus sõltub distributsioonist ja süsteemi konfiguratsioonist. Mõnes süsteemis säilitatakse vajalik info journalis, mõnes kirjutab syslogi teenus lisaks traditsioonilisi tekstifaile.

### `locate` ja `plocate`

Tänapäevastes Debianis ja RHEL-is on kasutusel `plocate`, kuid paketi olemasolu ja vaikimisi paigaldatus võivad erineda. Õppimise seisukohalt on tähtis mõista `locate` käsu põhimõtet: otsing toimub indekseeritud andmebaasist. Käsu kasutus on ikka sama. 

---

## Otsinguvahendite võrdlus

| Tööriist | Mida otsib? | Tüüpiline kasutus |
|---|---|---|
| `grep` | tekstiridu | konfiguratsiooni või käsu väljundi filtreerimine |
| `find` | failisüsteemi objekte ja nende omadusi | nime, tüübi, omaniku, suuruse või aja järgi otsimine |
| `locate` | failinimesid andmebaasist | kiire nimeotsing |
| `command -v` | shellile kättesaadavat käsku | programmi või käsu asukoha kontrollimine |
| `journalctl` | systemd journali kirjeid | süsteemi ja teenuste logide uurimine |

---

## Hea töövõte: otsusta → otsi → kontrolli → tegutse

Otsingut kasutades on kasulik järgida kindlat tööjärjekorda:

```text
Mida ma otsin?
      ↓
Milline tööriist sobib?
      ↓
Kui lai peab otsing olema?
      ↓
Käivita otsing
      ↓
Kontrolli tulemusi
      ↓
Vajaduse korral tegutse
```

Näiteks failide kustutamise korral:

1. koosta `find` otsing;
2. käivita see ilma kustutamiseta;
3. kontrolli leitud objektide nimekirja;
4. täpsusta vajaduse korral tingimusi;
5. alles seejärel kasuta muutvat või kustutavat tegevust.

!!! tip "Administraatori eesmärk ei ole otsida võimalikult laialt"
    Alusta võimalikult täpsest asukohast ja tingimustest. Näiteks `/home/student` on sageli parem otsingu alguskoht kui kogu failisüsteem `/`.

---

## Käskude spikker

| Käsk | Eesmärk |
|---|---|
| `grep muster fail` | otsib failist mustrile vastavaid ridu |
| `grep -i muster fail` | otsib tähesuurust eristamata |
| `grep -n muster fail` | näitab ka rea numbrit |
| `grep -v muster fail` | näitab mittevastavaid ridu |
| `grep -w muster fail` | otsib tervet sõna |
| `grep -r muster kataloog` | otsib rekursiivselt kataloogipuust |
| `käsk \| grep muster` | filtreerib teise käsu väljundit |
| `find asukoht -name nimi` | otsib nime järgi |
| `find asukoht -iname nimi` | otsib nime järgi tähesuurust eristamata |
| `find asukoht -type f` | otsib tavalisi faile |
| `find asukoht -type d` | otsib katalooge |
| `find asukoht -user kasutaja` | otsib omaniku järgi |
| `find asukoht -size +50M` | otsib suuruse järgi |
| `find asukoht -mtime -7` | otsib muutmisaja järgi |
| `find asukoht -mmin -60` | otsib minutites määratud muutmisaja järgi |
| `find asukoht -maxdepth 1` | piirab otsingu sügavust |
| `locate nimi` | otsib failinime andmebaasist |
| `sudo updatedb` | uuendab `locate` andmebaasi |
| `command -v käsk` | näitab, kuidas shell käsu leiab |
| `journalctl` | kuvab journali kirjeid |
| `journalctl -b` | kuvab praeguse alglaadimise kirjeid |
| `journalctl -u teenus` | kuvab systemd unit'i logisid |
| `journalctl -p err` | kuvab veatasemega kirjeid |
| `journalctl --since today` | kuvab tänased kirjed |

---

## Olulised mõisted

| Mõiste | Selgitus |
|---|---|
| **otsingumuster** | tekst või muster, millele otsitav tulemus peab vastama |
| **rekursiivne otsing** | otsing, mis läbib ka alamkatalooge |
| **metamärk** | erilise tähendusega märk otsingumustris, näiteks `*` või `?` |
| **toru `\|`** | suunab ühe käsu standardväljundi järgmise käsu standardsisendiks |
| **`grep`** | tekstiridade otsimise ja filtreerimise vahend |
| **`find`** | failisüsteemi objektide otsimise vahend |
| **`locate`** | failinimede andmebaasist otsimise vahend |
| **`plocate`** | tänapäevane `locate`-i teostus mitmes Linuxi distributsioonis |
| **`updatedb`** | uuendab `locate` otsinguandmebaasi |
| **`command -v`** | kontrollib, kuidas shell etteantud käsunime leiab |
| **journal** | systemd hallatav struktureeritud logi |
| **`journalctl`** | systemd journali vaatamise ja filtreerimise tööriist |

---

## Kontrollküsimused

1. Miks ei ole Linuxis kõigi otsinguülesannete jaoks ainult ühte otsingukäsku?
2. Milleks kasutatakse `grep` käsku?
3. Mida muudab `grep -i`?
4. Milleks kasutatakse `grep -n` võtit?
5. Mida teeb `grep -v`?
6. Mis vahe on `grep -r` ja tavalisel ühe faili otsingul?
7. Milleks kasutatakse Linuxi käsureal toru `|`?
8. Mida teeb käsk `ps aux | grep ssh`?
9. Milleks kasutatakse `find` käsku?
10. Mida tähendavad käsus `find /home -name "*.txt"` osad `/home`, `-name` ja `"*.txt"`?
11. Mis vahe on `-name` ja `-iname` tingimustel?
12. Mida tähendavad metamärgid `*` ja `?`?
13. Mida tähendavad `find` tingimused `-type f`, `-type d` ja `-type l`?
14. Kuidas leiad kasutajale `student` kuuluvad failid?
15. Mida tähendab `find` käsus `-size +50M`?
16. Milleks kasutatakse `-mtime` ja `-mmin` tingimusi?
17. Mis vahe on faili `mtime` ja `ctime` väärtustel?
18. Milleks kasutatakse `-maxdepth` tingimust?
19. Miks tuleb `find` otsingu tulemust enne `-delete` või muutva `-exec` käsu kasutamist kontrollida?
20. Mis vahe on `find` ja `locate` tööpõhimõttel?
21. Miks võib `locate` mitte leida äsja loodud faili?
22. Milleks kasutatakse `updatedb` käsku?
23. Milleks kasutatakse `journalctl` käsku?
24. Kuidas saad `journalctl` väljundist `grep` abil kindlat teksti otsida?


---

## Kokkuvõte

Linuxis kasutatakse erinevate otsinguülesannete jaoks erinevaid tööriistu.

`grep` otsib ja filtreerib tekstiridu. Seda saab kasutada nii failidega kui ka teiste käskude väljundiga.

`find` otsib otse failisüsteemist ning võimaldab objekte valida näiteks nime, tüübi, omaniku, suuruse ja muutmisaja järgi. See on üks olulisemaid Linuxi administraatori otsinguvahendeid.

`locate` otsib failinimesid eelnevalt koostatud andmebaasist ning on seetõttu kiire, kuid selle tulemused sõltuvad andmebaasi ajakohasusest. Tänapäevastes distributsioonides kasutatakse selleks sageli `plocate`-i.

`command -v` aitab kontrollida, kuidas shell käsu leiab.

`journalctl` võimaldab uurida systemd süsteemi- ja teenuseloge. Traditsiooniliste logifailide nimed võivad Debiani ja Red Hati laadsetes süsteemides erineda.

Kõige olulisem põhimõte on:

> **Mõtle kõigepealt, mida otsid, vali selle järgi sobiv tööriist ja kontrolli tulemusi enne nende põhjal muudatuste tegemist.**

---

## Allikad ja lisalugemine

- [Debian Manpages: grep(1)](https://manpages.debian.org/stable/grep/grep.1.en.html){ target="_blank" rel="noopener" }
- [Debian Manpages: find(1)](https://manpages.debian.org/stable/findutils/find.1.en.html){ target="_blank" rel="noopener" }
- [Debian Manpages: plocate(1)](https://manpages.debian.org/stable/plocate/plocate.1.en.html){ target="_blank" rel="noopener" }
- [Debian Manpages: updatedb(8)](https://manpages.debian.org/stable/plocate/updatedb.8.en.html){ target="_blank" rel="noopener" }
- [Debian Manpages: journalctl(1)](https://manpages.debian.org/stable/systemd/journalctl.1.en.html){ target="_blank" rel="noopener" }
