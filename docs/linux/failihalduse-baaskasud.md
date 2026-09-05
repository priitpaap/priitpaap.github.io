icon:material/debian

# Linuxi failihaldus ja baaskäsud

Linuxi süsteemi haldamisel kasutatakse väga palju käsurida. Graafiline kasutajaliides võib olla mugav, kuid käsurida võimaldab tegevusi teha kiiresti, täpselt ja korratavalt. Serverites ei pruugi graafilist kasutajaliidest üldse olla.

Selles õppematerjalis tutvume Linuxi käsurea kasutamise põhimõtete ja kõige olulisemate failihalduse käskudega.

## Õpieesmärgid

Pärast materjali läbimist oskad:

- liikuda Linuxi kataloogipuus;
- kasutada absoluutseid ja suhtelisi failiteid;
- vaadata kataloogide sisu;
- luua faile ja katalooge;
- kopeerida, teisaldada, ümber nimetada ja kustutada faile ning katalooge;
- vaadata ja muuta tekstifailide sisu;
- kasutada metamärke failide valimiseks;
- kasutada käsurea laiendusi mitme faili või kataloogi loomiseks;
- suunata käsu väljundit faili;
- kasutada käskude ajalugu ja käsurea automaattäitmist;
- leida käsu kohta abi.

---

## Terminal ja shell

**Terminal** on programm või keskkond, mille kaudu kasutaja sisestab käske.

**Shell ehk kest** on programm, mis tõlgendab kasutaja sisestatud käske ja käivitab vajalikud programmid.

Debianis ja paljudes teistes Linuxi distributsioonides kasutatakse sageli **Bash**-i (*Bourne Again Shell*).

Käsureal näed tavaliselt viipa, näiteks:

```console
student@debian:~$
```

Viip võib anda infot kasutaja, arvuti ja asukoha kohta:

- `student` – kasutajanimi;
- `debian` – arvuti nimi;
- `~` – kasutaja kodukataloog;
- `$` – tavakasutaja viip.

Juurkasutaja (*root*) viiba lõpus kasutatakse tavaliselt märki `#`.

!!! note "Viip ei ole käsu osa"
    Õppematerjalides võib näidetes olla käsu ees `$` või `#`. Neid märke ei kirjutata käsu sisestamisel kaasa.

---

## Käsu ülesehitus

Linuxi käsk koosneb tavaliselt käsu nimest ning vajadusel võtmetest ja argumentidest.

```text
käsk [võtmed] [argumendid]
```

Näiteks:

```bash
ls -l /etc
```

Siin on:

| Osa | Tähendus |
|---|---|
| `ls` | käsk |
| `-l` | võti ehk suvand (*option*) |
| `/etc` | argument |

Võtmed muudavad käsu käitumist. Levinud on:

```bash
-h
-l
-a
-r
```

Paljudel käskudel on ka pikad võtmed:

```bash
--help
--version
```

Mitmeid lühikesi võtmeid saab sageli ühendada. Näiteks:

```bash
ls -l -a -h
```

on tavaliselt sama mis:

```bash
ls -lah
```

!!! info
    Kõik käsud ei kasuta võtmeid täpselt ühtemoodi. Konkreetse käsu süntaksit kontrolli käsu abiinfost.

---

## Abi leidmine

Kõiki käsu võtmeid ei ole vaja pähe õppida.

### `--help`

Paljud programmid kuvavad lühikese abi võtmega `--help`:

```bash
ls --help
```

```bash
cp --help
```

### `man`

Põhjalikuma dokumentatsiooni jaoks kasutatakse manuaalilehti:

```bash
man ls
```

```bash
man cp
```

Manuaalist väljumiseks vajuta:

```text
q
```

Manuaalis otsimiseks vajuta `/`, kirjuta otsingusõna ja vajuta Enter.

---

## Liikumine kataloogipuus

### Hetkeasukoht – `pwd`

Käsk `pwd` (*print working directory*) näitab, millises kataloogis parajasti asud.

```bash
pwd
```

Näiteks:

```text
/home/student
```

---

### Kodukataloog

Tavakasutajate kodukataloogid asuvad tavaliselt `/home` all.

Kasutaja `student` kodukataloog on näiteks:

```text
/home/student
```

Kodukataloogi tähistab shellis ka märk:

```text
~
```

Seega:

```text
/home/student
```

ja kasutaja `student` jaoks:

```text
~
```

viitavad samale asukohale.

---

### Kataloogi vahetamine – `cd`

`cd` (*change directory*) muudab aktiivset kataloogi.

```bash
cd /etc
```

Kodukataloogi saab minna lihtsalt:

```bash
cd
```

või:

```bash
cd ~
```

Ühe taseme võrra kõrgemale:

```bash
cd ..
```

Eelmisesse asukohta:

```bash
cd -
```

#### Suhteline tee

Kui asud kataloogis `/home/student`, siis:

```bash
cd ajutine
```

viib kataloogi:

```text
/home/student/ajutine
```

#### Absoluutne tee

Absoluutne tee algab juurkataloogist `/`:

```bash
cd /var/log
```

!!! tip "Kontrolli asukohta"
    Kui pole kindel, kus parajasti asud, kasuta `pwd`.

---

## Kataloogide sisu vaatamine

### `ls`

Käsk `ls` kuvab kataloogi sisu.

```bash
ls
```

Mõned kasulikud võtmed:

```bash
ls -l
```

kuvab detailse nimekirja.

```bash
ls -a
```

kuvab ka peidetud failid.

```bash
ls -h
```

kasutab koos sobivate võtmetega inimesele lihtsamini loetavaid failisuurusi.

Neid saab ühendada:

```bash
ls -lah
```

Teise kataloogi sisu saab vaadata sinna liikumata:

```bash
ls -l /var/log
```

See on käsureal väga oluline töövõte – paljude toimingute jaoks ei ole vaja aktiivset kataloogi vahetada.

---

## Failide ja kataloogide loomine

### Kataloogi loomine – `mkdir`

Ühe kataloogi loomine:

```bash
mkdir ajutine
```

Mitme kataloogi loomine:

```bash
mkdir failid1 failid2 failid3
```

Alamkataloogi saab luua teed kasutades:

```bash
mkdir ajutine/failid1
```

#### Puuduvate ülemkataloogide loomine

Võti `-p` loob vajadusel ka puuduvad ülemkataloogid:

```bash
mkdir -p projekt/docs/images
```

Kui `projekt` või `docs` veel ei eksisteeri, luuakse ka need.

---

### Brace expansion – mitme nime kiire loomine

Bash võimaldab looksulgudega genereerida mitu nime.

Näiteks:

```bash
mkdir failid{1..3}
```

loob:

```text
failid1
failid2
failid3
```

Samamoodi:

```bash
mkdir sept{1..30}
```

loob kataloogid `sept1` kuni `sept30`.

Tähti saab kasutada samal viisil:

```bash
mkdir grupp{a..d}
```

Tulemuseks:

```text
gruppa
gruppb
gruppc
gruppd
```

!!! note
    Brace expansion on **Bashi omadus**, mitte `mkdir` käsu funktsioon. Shell tekitab esmalt nimede loendi ja annab selle seejärel `mkdir` käsule.

---

### Tühikuga nimed

Shell kasutab tühikut käskude osade eraldamiseks. Seetõttu tuleb tühikut sisaldav nimi kirjutada jutumärkidesse:

```bash
mkdir "olulised failid"
```

või varjestada tühik kaldkriipsuga:

```bash
mkdir olulised\ failid
```

Sama põhimõte kehtib teiste käskudega:

```bash
cd "olulised failid"
```

```bash
cp "minu fail.txt" /tmp/
```

Failinimedes on tühikud lubatud, kuid administraatori töös kasutatakse sageli mugavuse huvides sidekriipse või alakriipse:

```text
olulised-failid
olulised_failid
```

---

### Tühja faili loomine – `touch`

Tühja faili saab luua käsuga:

```bash
touch andmed1
```

Mitme faili loomine:

```bash
touch andmed1 andmed2
```

Kui fail on juba olemas, ei kustuta `touch` selle sisu. Tavaliselt uuendatakse faili ajatemplite väärtusi.

---

## Failide kopeerimine

### `cp`

Faili kopeerimine:

```bash
cp lähtefail sihtkoht
```

Näiteks:

```bash
cp /etc/group ~/ajutine/
```

Faili saab kopeerimisel anda sellele uue nime:

```bash
cp andmed.txt andmed-varukoopia.txt
```

#### Kataloogi kopeerimine

Kataloogi koos sisuga kopeerimiseks kasutatakse rekursiivset võtit `-r`:

```bash
cp -r skriptid ~/ajutine/
```

!!! tip "Kasulik ohutusvõti"
    Õppimise ajal võib kasutada võtit `-i`, mis küsib kinnitust enne olemasoleva faili ülekirjutamist:

    ```bash
    cp -i fail.txt sihtkoht/
    ```

---

## Failide teisaldamine ja ümbernimetamine

### `mv`

`mv` (*move*) teisaldab faili või kataloogi.

```bash
mv fail.txt ~/ajutine/
```

Kataloogi teisaldamine:

```bash
mv projekt ~/ajutine/
```

Erinevalt `cp` käsust ei ole kataloogi teisaldamiseks tavaliselt `-r` võtit vaja.

#### Faili ümbernimetamine

Linuxis kasutatakse sama käsku ka ümbernimetamiseks:

```bash
mv data1.txt ajutised_andmed
```

#### Teisaldamine ja ümbernimetamine korraga

```bash
mv /srv/ohoo ~/ajutine/failid4
```

Sel juhul viiakse objekt uude asukohta ja sellele antakse uus nimi.

!!! tip
    Ka `mv` toetab võtit `-i`, et enne olemasoleva sihtfaili ülekirjutamist kinnitust küsida.

---

## Failide kustutamine

### `rm`

Faili kustutamine:

```bash
rm junk
```

Mitme faili kustutamine:

```bash
rm fail1 fail2 fail3
```

!!! danger "Linuxis ei ole käsurea `rm`-il tavaliselt prügikasti"
    Käsuga `rm` kustutatud faili ei saa tavaliselt graafilise töölaua prügikastist taastada. Kontrolli enne Enteri vajutamist hoolikalt faili nime ja asukohta.

#### Kataloogi kustutamine

Kataloogi ja selle sisu kustutamiseks kasutatakse:

```bash
rm -r kataloog
```

Näiteks:

```bash
rm -r vana-projekt
```

Tühja kataloogi saab eemaldada ka käsuga:

```bash
rmdir kataloog
```

#### Kinnituse küsimine

```bash
rm -i fail.txt
```

Kataloogipuuga töötamisel:

```bash
rm -ri kataloog
```

!!! danger
    Ära kasuta `rm -rf` käsku harjumusest. Võtmed `-r` ja `-f` koos võivad suure hulga andmeid ilma kinnitust küsimata pöördumatult kustutada.

---

## Metamärgid

Shell võimaldab failinimede valimiseks kasutada metamärke (*wildcards*).

### `*` – suvaline märkide jada

```bash
ls *.txt
```

valib kõik `.txt` lõpuga failid.

Näiteks:

```bash
rm *.txt
```

kustutab aktiivsest kataloogist kõik `.txt` lõpuga failid.

!!! warning
    Enne metamärgiga kustutamist vaata võimalusel kõigepealt `ls` abil, millised failid valitakse:

    ```bash
    ls *.txt
    ```

    Alles siis:

    ```bash
    rm *.txt
    ```

### `?` – üks suvaline märk

```bash
ls fail?.txt
```

võib sobitada näiteks:

```text
fail1.txt
failA.txt
failx.txt
```

aga mitte:

```text
fail10.txt
```

---

## Tekstifailide sisu vaatamine

### `cat`

Lühikese tekstifaili sisu kuvamiseks:

```bash
cat fail.txt
```

Mitme faili sisu saab kuvada järjest:

```bash
cat fail1.txt fail2.txt
```

`cat` sobib eelkõige väikeste failide jaoks.

---

### `less`

Pikema faili vaatamiseks:

```bash
less /etc/passwd
```

Olulisemad klahvid:

| Klahv | Tegevus |
|---|---|
| `↑`, `↓` | liikumine |
| `Page Up`, `Page Down` | lehekülgede vahel liikumine |
| `/tekst` | otsimine |
| `n` | järgmine otsingutulemus |
| `g` | faili algusesse |
| `G` | faili lõppu |
| `q` | väljumine |

---

### `head`

Faili alguse kuvamiseks:

```bash
head /etc/passwd
```

Vaikimisi kuvatakse 10 rida.

Esimese viie rea kuvamiseks:

```bash
head -n 5 /etc/passwd
```

Lühivormina töötab GNU tööriistades ka:

```bash
head -5 /etc/passwd
```

Õppematerjalides eelistame selgemat kuju:

```bash
head -n 5 /etc/passwd
```

---

### `tail`

Faili lõpu kuvamiseks:

```bash
tail /etc/passwd
```

Viimase viie rea kuvamiseks:

```bash
tail -n 5 /etc/passwd
```

Logifaili uute ridade jälgimiseks kasutatakse sageli:

```bash
tail -f /var/log/fail.log
```

`Ctrl+C` lõpetab jälgimise.

---

## Tekstifailide muutmine

Linuxis on palju tekstiredaktoreid. Alustamiseks sobib hästi **nano**.

```bash
nano fail.txt
```

Nano põhilised käsud kuvatakse ekraani allosas. Märk `^` tähendab `Ctrl` klahvi.

Olulisemad:

| Klahvikombinatsioon | Tegevus |
|---|---|
| `Ctrl+O` | salvesta |
| `Enter` | kinnita failinimi |
| `Ctrl+X` | välju |
| `Ctrl+W` | otsi |

Näiteks:

```bash
nano ajutised_andmed
```

!!! note
    Tekstiredaktori kasutamine on teistsugune kui faili sisu vaatamine käsuga `cat` või `less`: redaktoriga saab faili sisu muuta.

---

## Väljundi suunamine faili

Linuxis saab programmi väljundi terminali asemel faili suunata.

### `>` – kirjuta väljund faili

```bash
hostnamectl > ajalugu.txt
```

Kui faili ei ole olemas, luuakse see.

Kui fail **on juba olemas**, asendatakse selle senine sisu uue väljundiga.

!!! warning
    `>` võib olemasoleva faili sisu üle kirjutada.

Näiteks:

```bash
head -n 5 /etc/passwd > esimesed.txt
```

---

### `>>` – lisa väljund faili lõppu

```bash
uptime >> ajalugu.txt
```

Kui fail on olemas, säilib vana sisu ja uus väljund lisatakse lõppu.

Näiteks:

```bash
history >> ajalugu.txt
```

Erinevus:

```text
>   kirjuta fail üle
>>  lisa faili lõppu
```

---

## Toru `|`

Toru (*pipe*) saadab ühe käsu väljundi teise käsu sisendiks.

```bash
käsk1 | käsk2
```

Näiteks pika kataloogiloendi saatmine `less` programmi:

```bash
ls -la /etc | less
```

Või käsuabi vaatamine lehekülgede kaupa:

```bash
ls --help | less
```

Torusid kasutatakse Linuxi käsureal väga palju, sest väikseid programme saab nende abil omavahel kombineerida.

---

## Käskude ajalugu

Bash säilitab varem sisestatud käske.

### `history`

```bash
history
```

kuvab varasemad käsud.

Ajaloo kustutamiseks aktiivses shellis:

```bash
history -c
```

!!! warning
    `history -c` kustutab aktiivse shelli käsuajaloo. Seda ei ole tavapärases töös põhjust sageli kasutada.

#### Varasema käsu leidmine

Vajuta:

```text
Ctrl+R
```

ja hakka kirjutama osa varem kasutatud käsust.

Nooleklahvidega `↑` ja `↓` saab samuti varasemate käskude vahel liikuda.

---

## TAB-automaattäitmine

`Tab` on Linuxi käsureal üks olulisemaid klahve.

Kui kirjutad näiteks:

```text
cd /etc/net
```

ja vajutad `Tab`, võib shell nime automaatselt lõpuni täiendada.

Kui võimalikke vasteid on mitu, aitab teistkordne `Tab` nende nimekirja kuvada.

TAB-i kasutamine:

- kiirendab töötamist;
- vähendab trükivigu;
- aitab leida olemasolevaid failinimesid ja katalooge.

!!! tip
    Pikki failiteid ei ole mõistlik alati käsitsi lõpuni kirjutada. Kasuta TAB-automaattäitmist.

---

## Failide allalaadimine käsurealt

### `wget`

`wget` võimaldab faili veebist alla laadida.

```bash
wget URL
```

Näiteks:

```bash
wget https://example.com/fail.txt
```

Vaikimisi salvestatakse fail aktiivsesse kataloogi.

Failile saab anda ka teise nime:

```bash
wget -O uusnimi.txt https://example.com/fail.txt
```

---

## Skripti käivitamine

Shelliskript on tekstifail, mis sisaldab käske.

Näiteks fail:

```text
setup.sh
```

### Käivitamine Bashiga

Skripti saab käivitada Bashile argumendina:

```bash
bash setup.sh
```

Sel juhul ei pea skriptil endal olema käivitusõigust.

### Käivitamine failina

Kui skriptil on sobiv *shebang* ja käivitusõigus, saab seda käivitada:

```bash
./setup.sh
```

`./` tähendab, et käivitatav fail asub aktiivses kataloogis.

Käivitusõiguse saab lisada näiteks:

```bash
chmod +x setup.sh
```

või kõigile kasutajaklassidele:

```bash
chmod a+x setup.sh
```

!!! info "Failiõigused tulevad eraldi teemana"
    `chmod` muudab faili pääsuõigusi. Siin on vaja teada ainult skripti käivitamise põhimõtet. Omaniku, grupi, `rwx` õiguste ja numbrilise kirjaviisi juurde tuleme failiõiguste teemas.

---

## `sudo`

Mõned süsteemi muutvad käsud vajavad administraatori õigusi.

Näiteks:

```bash
sudo käsk
```

`sudo` käivitab lubatud käsu kõrgendatud õigustega.

!!! warning
    `sudo` ei ole lihtsalt viis veateatest mööda pääseda. Enne selle kasutamist peab olema selge, miks käsk administraatori õigusi vajab.

Failitoiminguid oma kodukataloogis peaks tavakasutaja üldjuhul saama teha ilma `sudo` käsuta.

---

## Mõned kasulikud süsteemikäsud

Kuigi need ei ole otseselt failihalduskäsud, kasutatakse neid sageli koos failitoimingutega.

### `whoami`

Näitab aktiivse kasutaja nime:

```bash
whoami
```

### `hostnamectl`

Kuvab süsteemi ja arvuti nime kohta infot:

```bash
hostnamectl
```

### `uptime`

Näitab muu hulgas, kui kaua süsteem on töötanud:

```bash
uptime
```

Nende käskude väljundit saab samuti faili suunata:

```bash
hostnamectl > info.txt
uptime >> info.txt
```

---

## Mitme käsu ühendamine

### `;`

Käsud täidetakse järjest sõltumata sellest, kas eelmine õnnestus:

```bash
pwd ; ls
```

### `&&`

Järgmine käsk täidetakse ainult siis, kui eelmine õnnestus:

```bash
mkdir test && cd test
```

See on kasulik juhul, kui teisel käsul on mõtet ainult esimese õnnestumise korral.

### `||`

Järgmine käsk täidetakse ainult siis, kui eelmine ebaõnnestus:

```bash
cd projekt || echo "Kataloogi ei leitud"
```

---

## Olulisemad klahvikombinatsioonid

| Klahv | Tegevus |
|---|---|
| `Tab` | automaattäitmine |
| `↑` / `↓` | käskude ajaloos liikumine |
| `Ctrl+R` | varasemast ajaloost otsimine |
| `Ctrl+C` | aktiivse käsu katkestamine |
| `Ctrl+L` | terminali kuva puhastamine |
| `Ctrl+A` | kursori viimine rea algusesse |
| `Ctrl+E` | kursori viimine rea lõppu |
| `Ctrl+U` | kustuta kursorist vasakule |
| `Ctrl+K` | kustuta kursorist paremale |
| `Ctrl+W` | kustuta eelmine sõna |
| `Ctrl+D` | EOF; tühjal käsureal tavaliselt shellist väljumine |

!!! note
    `Ctrl+C` saadab töötavale protsessile tavaliselt katkestussignaali. See ei tähenda Linuxi terminalis „kopeeri“.

---

## Praktiline näide

Oletame, et asud kasutaja `student` kodukataloogis.

Kontrolli asukohta:

```bash
pwd
```

Loo töökaust ja kolm alamkataloogi:

```bash
mkdir -p harjutus/failid{1..3}
```

Loo kaks tühja faili:

```bash
touch andmed1 andmed2
```

Kopeeri üks neist alamkataloogi:

```bash
cp andmed1 harjutus/failid1/
```

Nimeta teine ümber:

```bash
mv andmed2 andmed-varu
```

Vaata tulemust:

```bash
ls -l
ls -l harjutus/failid1
```

Salvesta süsteemi info faili:

```bash
hostnamectl > info.txt
```

Lisa faili süsteemi tööaeg:

```bash
uptime >> info.txt
```

Vaata faili:

```bash
cat info.txt
```

---

## Enne kustutamist kontrolli

Linuxi administraatori üks kasulikumaid harjumusi on enne muutvat või kustutavat käsku kontrollida, mida käsk mõjutab.

Näiteks enne:

```bash
rm *.txt
```

kontrolli:

```bash
ls *.txt
```

Enne kataloogi kustutamist:

```bash
ls -la vana-kataloog
```

Alles siis:

```bash
rm -r vana-kataloog
```

!!! tip "Hea töövõte"
    **Vaata → kontrolli → muuda.**

    See vähendab eriti `rm`, `mv`, metamärkide ja `sudo` kasutamisel tehtavaid vigu.

---

## Kokkuvõte

Linuxi failihalduses kasutatakse kõige sagedamini järgmisi käske:

| Tegevus | Käsk |
|---|---|
| Hetkeasukoht | `pwd` |
| Kataloogi vahetamine | `cd` |
| Kataloogi sisu | `ls` |
| Kataloogi loomine | `mkdir` |
| Tühja faili loomine | `touch` |
| Faili/kataloogi kopeerimine | `cp` |
| Teisaldamine või ümbernimetamine | `mv` |
| Kustutamine | `rm`, `rmdir` |
| Faili sisu kuvamine | `cat` |
| Pikema faili vaatamine | `less` |
| Faili algus | `head` |
| Faili lõpp | `tail` |
| Tekstifaili muutmine | `nano` |
| Käsu väljund faili | `>` |
| Väljundi lisamine faili | `>>` |
| Käskude ühendamine | `\|`, `&&`, `;`, `\|\|` |
| Käsuajalugu | `history` |
| Faili allalaadimine | `wget` |
| Abi | `--help`, `man` |

Kõige olulisem ei ole käskude päheõppimine. Oluline on osata:

1. aru saada, **kus sa failisüsteemis asud**;
2. kasutada õiget **failiteed**;
3. mõista käsu **võtmeid ja argumente**;
4. kontrollida tegevuse tulemust;
5. kasutada abiinfot, kui käsu süntaks ei ole meeles.

---

### Kontrollküsimused

1. Mis vahe on terminalil ja shellil?
2. Mida näitab `pwd`?
3. Mis vahe on absoluutsel ja suhtelisel failiteel?
4. Mida tähistab `~`?
5. Mis vahe on käskudel `cp` ja `mv`?
6. Millal vajab `cp` võtit `-r`?
7. Kuidas luua ühe käsuga kataloogid `test1` kuni `test10`?
8. Miks tuleb tühikut sisaldav failinimi panna jutumärkidesse või tühik varjestada?
9. Mis vahe on `>` ja `>>` operaatoritel?
10. Mida tähendab metamärk `*`?
11. Miks tasub enne `rm *.txt` kasutamist käivitada `ls *.txt`?
12. Millal kasutada `cat` ja millal `less`?
13. Kuidas kuvada faili esimesed viis rida?
14. Kuidas kuvada faili viimased viis rida?
15. Milleks kasutatakse TAB-klahvi?
16. Kuidas leida varem kasutatud käsku ilma kogu `history` väljundit läbi vaatamata?
17. Mis vahe on `bash script.sh` ja `./script.sh` käivitamisel?
18. Miks ei tohiks `sudo` kasutada lihtsalt sellepärast, et tavakasutajana saadi veateade?

## Allikad ja lisalugemine

Linux man pages online: [https://man7.org/linux/man-pages/](https://man7.org/linux/man-pages/){ target="_blank" rel="noopener" }

