icon:material/debian

# Linuxi baaskäskude spikker

See spikker koondab Linuxi käsurea põhilised käsud tegevuste kaupa. Näited sobivad eelkõige **Debiani ja Ubuntu** laadsetele süsteemidele, kuid enamik käske töötab ka teistes Linuxi distributsioonides.

!!! info "Käsu abi"
    Kui käsu süntaks või võtmed ei ole meeles, kasuta:

    ```bash
    käsk --help
    man käsk
    ```

    Näiteks `man ls` avab käsu `ls` manuaalilehe. Manuaalist väljumiseks vajuta `q`.

---

## 1. Kasutaja ja administraatori õigused

### Kes ma olen?

```bash
whoami
```

Kuvab aktiivse kasutaja kasutajanime.

```bash
id
```

Kuvab kasutaja UID, primaarse GID ja grupid.

Teise kasutaja info:

```bash
id student
```

### `sudo`

`sudo` käivitab käsu teise kasutaja õigustes, vaikimisi **root**-kasutajana.

```bash
sudo apt update
```

!!! note
    `sudo` ei tähenda sõna-sõnalt *super user do*. Nimi pärineb ajalooliselt sellest kasutusest, kuid tänapäeval võib `sudo` käivitada käsu ka muu kasutajana.

Root-keskkonna avamine:

```bash
sudo -i
```

Väljumine:

```bash
exit
```

### `su`

`su` vahetab kasutajat.

```bash
su - kasutajanimi
```

`-` laadib sihtkasutaja sisselogimiskeskkonna.

---

## 2. Liikumine failisüsteemis

### Hetkeasukoht

```bash
pwd
```

`pwd` (*print working directory*) näitab aktiivse kataloogi täielikku teed.

### Kataloogi vahetamine

```bash
cd /var/log
```

Kodukataloogi liikumiseks:

```bash
cd
```

või

```bash
cd ~
```

Eelmisesse kataloogi:

```bash
cd -
```

Ühe taseme võrra üles:

```bash
cd ..
```

Kahe taseme võrra üles:

```bash
cd ../..
```

!!! tip "Tab-klahv"
    Faili- ja katalooginimede sisestamisel kasuta **Tab**-klahvi automaatseks lõpetamiseks. See vähendab kirjavigu ja muudab pikkade nimedega töötamise kiiremaks.

---

## 3. Kataloogide sisu vaatamine

```bash
ls
```

Pikem nimekiri koos õiguste, omaniku, suuruse ja muutmisajaga:

```bash
ls -l
```

Koos peidetud failidega:

```bash
ls -la
```

Inimloetavate failisuurustega:

```bash
ls -lh
```

Aja järgi, vanemad ees:

```bash
ls -ltr
```

Kataloogi enda, mitte selle sisu info:

```bash
ls -ld kataloog
```

---

## 4. Failide ja kataloogide loomine

### Tühja faili loomine

```bash
touch fail.txt
```

Olemasoleva faili puhul muudab `touch` vaikimisi faili ajatemplid praegusele ajale, kuid ei kustuta faili sisu.

### Kataloogi loomine

```bash
mkdir projekt
```

Mitmetasandiline kataloogistruktuur:

```bash
mkdir -p projekt/docs/images
```

Bash brace expansion võimaldab luua mitu kataloogi korraga:

```bash
mkdir kaust{1..5}
```

Tulemuseks on `kaust1` kuni `kaust5`.

Tühikut sisaldava nime korral kasuta jutumärke:

```bash
mkdir "Minu kataloog"
```

või escape'i:

```bash
mkdir Minu\ kataloog
```

---

## 5. Failide kopeerimine, liigutamine ja kustutamine

### Kopeerimine

```bash
cp fail1.txt fail2.txt
```

Kataloogi rekursiivseks kopeerimiseks:

```bash
cp -r kataloog1 kataloog2
```

Kasulik variant, mis säilitab võimalikult palju atribuute:

```bash
cp -a kataloog1 kataloog2
```

### Liigutamine ja ümbernimetamine

```bash
mv fail.txt /tmp/
```

`mv`-ga saab faili ka ümber nimetada:

```bash
mv vana.txt uus.txt
```

!!! warning
    `mv vana.txt uus.txt` **ei loo vana faili koopiat**. Fail nimetatakse ümber või liigutatakse.

### Kustutamine

Faili kustutamine:

```bash
rm fail.txt
```

Kataloogi ja selle sisu rekursiivne kustutamine:

```bash
rm -r kataloog
```

Tühja kataloogi kustutamine:

```bash
rmdir kataloog
```

!!! danger "`rm` ei kasuta prügikasti"
    Käsureal `rm`-iga kustutatud fail ei lähe tavaliselt töölaua prügikasti. Eriti ettevaatlik tuleb olla käskudega `rm -r` ja `rm -rf`.

---

## 6. Failide sisu vaatamine

### Lühike tekstifail

```bash
cat fail.txt
```

Failide ühendamine väljundisse:

```bash
cat fail1.txt fail2.txt
```

### Pikem tekst või logifail

```bash
less /var/log/syslog
```

Olulisemad `less` klahvid:

| Klahv | Tegevus |
|---|---|
| `q` | välju |
| `g` | faili algusesse |
| `G` | faili lõppu |
| `/tekst` | otsi edasi |
| `?tekst` | otsi tagasi |
| `n` | järgmine vaste |
| `N` | eelmine vaste |
| `F` | jälgi faili lisanduvaid ridu |

### Faili algus ja lõpp

```bash
head fail.txt
```

```bash
tail fail.txt
```

Viimased 50 rida:

```bash
tail -n 50 fail.txt
```

Logi reaalajas jälgimine:

```bash
tail -f /var/log/fail.log
```

---

## 7. Tekstiredaktorid

Algajale sobiv terminaliredaktor:

```bash
nano fail.txt
```

Teised levinud redaktorid:

```bash
vi fail.txt
vim fail.txt
```

!!! note
    `vi`/`vim` kasutavad erinevaid töörežiime ja vajavad eraldi õppimist. Lihtsateks muudatusteks on `nano` algajale tavaliselt mugavam.

---

## 8. Käsurea väljund, torud ja ümbersuunamine

### Teksti väljastamine

```bash
echo "Tere Linux"
```

### Väljundi suunamine faili

Fail kirjutatakse üle:

```bash
echo "Tere" > fail.txt
```

Faili lõppu lisamine:

```bash
echo "Uus rida" >> fail.txt
```

### Toru ehk pipe

Ühe käsu väljund antakse järgmise käsu sisendiks:

```bash
apt search nginx | less
```

Näiteks protsesside filtreerimine:

```bash
ps aux | grep nginx
```

### Käskude tingimuslik ühendamine

Teine käsk käivitub ainult siis, kui esimene õnnestus:

```bash
sudo apt update && sudo apt upgrade
```

Teine käsk käivitub ainult siis, kui esimene ebaõnnestus:

```bash
käsk1 || käsk2
```

---

## 9. Käsuajalugu ja terminali juhtimine

Käsuajalugu:

```bash
history
```

Ajaloo kustutamine aktiivsest shellist:

```bash
history -c
```

Ekraani puhastamine:

```bash
clear
```

või klahvikombinatsioon:

```text
Ctrl+L
```

### Olulised klahvikombinatsioonid

| Klahv | Tähendus |
|---|---|
| `Ctrl+C` | katkesta aktiivne protsess (`SIGINT`) |
| `Ctrl+D` | sisendi lõpp ehk EOF; tühjal shellireal tavaliselt väljub shellist |
| `Ctrl+Z` | peata protsess ja vii see job control'i alla |
| `Ctrl+L` | puhasta terminalivaade |

!!! warning
    `Ctrl+X` ei ole Linuxi shellis üldine „katkesta” klahvikombinatsioon. Näiteks `nano` redaktoris tähendab see väljumist.

Taustal/peatatud tööde vaatamine:

```bash
jobs
```

Viimase peatatud töö esiplaanile toomine:

```bash
fg
```

Konkreetne töö:

```bash
fg %3
```

---

## 10. Käsu õnnestumise kontrollimine

Viimase käsu exit status:

```bash
echo $?
```

Üldreegel:

- `0` – käsk õnnestus;
- muu väärtus – tekkis viga või eritingimus.

Näide:

```bash
ls /etc
 echo $?
```

!!! note
    Erinevate programmide veakoodide tähendus võib olla erinev. Täpse tähenduse leiab vastava programmi dokumentatsioonist.

---

## 11. Failide ja teksti otsimine

### `grep` – sisu otsimine

Failist:

```bash
grep "student" /var/log/auth.log
```

Tõstutundetu otsing:

```bash
grep -i "student" fail.txt
```

Rekursiivselt kataloogist:

```bash
grep -R "student" /etc/
```

Ainult sobivate failide nimed:

```bash
grep -Rl "student" /etc/
```

Reanumbrid:

```bash
grep -n "student" fail.txt
```

### `find` – failide otsimine

Kõik `.log` failid:

```bash
find /var/log -type f -name "*.log"
```

Tõstutundetu nimeotsing:

```bash
find /var/log -type f -iname "*.LOG"
```

Alla 1 MiB failid:

```bash
find /var/log -type f -size -1M
```

Root-kasutajale kuuluvad failid:

```bash
find /var/log -type f -user root
```

Katkised sümboolsed lingid:

```bash
find -L . -type l
```

!!! note
    `find -size 1M` ei tähenda intuitiivselt „täpselt 1 000 000 baiti”; `find` kasutab suurusühikute ja plokkide ümardamise reegleid. Vahemike puhul on sageli selgem kasutada `-size -1M`, `-size +10M` jne.

---

## 12. Kasutajate ja gruppide haldus

### Olulised failid

| Fail | Sisu |
|---|---|
| `/etc/passwd` | kasutajakontode põhiandmed |
| `/etc/shadow` | parooliräsid ja parooli aegumise info |
| `/etc/group` | gruppide andmed |
| `/etc/gshadow` | gruppide turbeandmed |

Kasutaja kirje vaatamine süsteemi kasutajaandmebaasist:

```bash
getent passwd student
```

Grupi info:

```bash
getent group sudo
```

!!! note
    `getent` on sageli parem kui ainult `/etc/passwd` lugemine, sest süsteem võib saada kasutajaid ka LDAP-ist või muudest NSS allikatest.

### UID ja GID

- UID – *user ID*;
- GID – *group ID*;
- UID `0` kuulub root-kasutajale;
- süsteemi- ja tavakasutajate UID vahemikud sõltuvad distributsioonist ja konfiguratsioonist.

!!! warning
    Reegel „UID 1–999 = süsteemikasutaja ja UID 1000+ = tavakasutaja” on levinud vaikeseadistus, kuid seda ei tasu käsitleda universaalse Linuxi reeglina.

### Kasutaja loomine Debianis

```bash
sudo adduser student
```

Debianis on `adduser` kasutajasõbralikum kõrgema taseme tööriist. Madalama taseme tööriist on:

```bash
sudo useradd student
```

### Kasutaja muutmine

Sisselogimisnime muutmine:

```bash
sudo usermod -l uusnimi vananimi
```

Kodukataloogi muutmine ja sisu teisaldamine:

```bash
sudo usermod -d /home/uusnimi -m uusnimi
```

Login-shelli muutmine:

```bash
sudo usermod -s /bin/bash student
```

Interaktiivse sisselogimise keelamiseks kasutatakse tänapäeval sageli:

```bash
sudo usermod -s /usr/sbin/nologin student
```

Konto lukustamine:

```bash
sudo usermod -L student
```

Konto lahtilukustamine:

```bash
sudo usermod -U student
```

### Parool

Enda parooli muutmine:

```bash
passwd
```

Teise kasutaja parooli muutmine administraatorina:

```bash
sudo passwd student
```

### Kasutaja kustutamine

Debianis:

```bash
sudo deluser student
```

Koos kodukataloogiga:

```bash
sudo deluser --remove-home student
```

Madalama taseme alternatiiv:

```bash
sudo userdel -r student
```

### Gruppide haldus

Kasutaja grupid:

```bash
groups
```

```bash
groups student
```

Uue grupi loomine:

```bash
sudo addgroup projekt
```

Kasutaja lisamine gruppi Debianis:

```bash
sudo adduser student projekt
```

Levinud distributsioonist sõltumatum variant:

```bash
sudo usermod -aG projekt student
```

!!! danger
    `usermod -G` ilma `-a` võtmeta võib kasutaja olemasolevate lisagruppide nimekirja üle kirjutada.

Kasutaja eemaldamine grupist Debianis:

```bash
sudo deluser student projekt
```

Grupi kustutamine:

```bash
sudo delgroup projekt
```

---

## 13. Failiõigused ja omanikud

Näiteks:

```text
-rwxr-x--- 1 student projekt 1234 Aug 31 12:00 skript.sh
```

Esimene märk näitab objekti tüüpi:

- `-` – tavaline fail;
- `d` – kataloog;
- `l` – sümboolne link.

Järgmised üheksa märki jagunevad kolmeks:

```text
rwx | r-x | ---
 u      g      o
```

- `u` – omanik (*user*);
- `g` – grupp;
- `o` – teised (*others*);
- `a` – kõik (*all*).

Õigused:

| Õigus | Väärtus | Faili puhul | Kataloogi puhul |
|---|---:|---|---|
| `r` | 4 | faili sisu lugemine | nimede loendi lugemine |
| `w` | 2 | faili muutmine | kirjete loomine/kustutamine, kui muud õigused lubavad |
| `x` | 1 | faili käivitamine | kataloogi läbimine / sealsete objektide poole pöördumine |

### `chmod`

Sümboolselt:

```bash
chmod u+rwx,g+rx,o-rwx fail
```

Numbriliselt:

```bash
chmod 750 fail
```

Levinud väärtused:

- `644` – omanik saab kirjutada, kõik saavad lugeda;
- `600` – ainult omanik saab lugeda ja kirjutada;
- `755` – omanik `rwx`, teised `r-x`;
- `700` – õigused ainult omanikule.

!!! warning
    `chmod 777` annab kõigile lugemis-, kirjutamis- ja käivitusõiguse. Seda ei tohiks kasutada „õiguste probleemi kiire parandamise” tavalahendusena.

### Omaniku ja grupi muutmine

```bash
sudo chown student fail.txt
```

Omanik ja grupp:

```bash
sudo chown student:projekt fail.txt
```

Ainult grupp:

```bash
sudo chown :projekt fail.txt
```

Rekursiivselt:

```bash
sudo chown -R student:projekt kataloog/
```

### Eribittide põhimõte

**Sticky bit** kataloogil:

```bash
chmod +t jagatud_kataloog
```

Tüüpiline näide on `/tmp`: kasutajad võivad kataloogi kirjutada, kuid ei saa tavaliselt kustutada teiste kasutajate faile.

**setgid** kataloogil:

```bash
chmod g+s projekt/
```

Uued failid ja alamkataloogid pärivad kataloogi grupi. See on kasulik jagatud töökataloogides.

**setuid** käivitataval failil tähendab, et programm käivitub faili omaniku efektiivse kasutaja-ID-ga. Seda kasutatakse turvakaalutlustel ettevaatlikult.

!!! note
    Root-kasutaja suudab klassikalisi UNIX-i DAC failiõigusi enamasti eirata, kuid väide „rootile õigused ei kehti” on liiga absoluutne. Rooti võivad endiselt piirata näiteks failisüsteemi omadused, immutable atribuut, read-only mount, Linux capabilities, SELinux/AppArmor jm.

### Failiatribuudid

Atribuutide vaatamine:

```bash
lsattr fail.txt
```

Faili immutable-atribuudi lisamine:

```bash
sudo chattr +i fail.txt
```

Eemaldamine:

```bash
sudo chattr -i fail.txt
```

---

## 14. Protsessid ja tööde haldus

Kõik protsessid:

```bash
ps aux
```

Protsessipuu:

```bash
ps -ef --forest
```

Interaktiivne ülevaade:

```bash
top
```

Kui paigaldatud:

```bash
htop
```

Protsessi lõpetamine PID järgi:

```bash
kill PID
```

Vajadusel jõuline lõpetamine:

```bash
kill -9 PID
```

!!! warning
    `kill -9` saadab `SIGKILL` signaali ega anna protsessile võimalust korrektselt sulguda. Kasuta seda siis, kui tavalisest `kill` käsust ei piisa.

---

## 15. systemd teenuste haldus

Tänapäevases Debianis kasutatakse teenuste haldamiseks enamasti `systemctl` käsku.

Teenuse olek:

```bash
systemctl status ssh
```

Teenuse käivitamine:

```bash
sudo systemctl start ssh
```

Peatamine:

```bash
sudo systemctl stop ssh
```

Taaskäivitamine:

```bash
sudo systemctl restart ssh
```

Automaatne käivitamine bootimisel:

```bash
sudo systemctl enable ssh
```

Kohe käivitamine ja ühtlasi enable:

```bash
sudo systemctl enable --now ssh
```

Bootimisel käivitamise keelamine:

```bash
sudo systemctl disable ssh
```

Ebaõnnestunud teenused:

```bash
systemctl --failed
```

---

## 16. Logid ja `journalctl`

Kogu systemd journal:

```bash
journalctl
```

Praeguse boodi logid:

```bash
journalctl -b
```

Teenuse logid:

```bash
journalctl -u ssh
```

Logi reaalajas jälgimine:

```bash
journalctl -u ssh -f
```

Viimased vead:

```bash
journalctl -p err -b
```

Kernelilogid:

```bash
journalctl -k
```

---

## 17. Võrguinfo ja diagnostika

### Võrguliidesed

Tänapäevane põhikäsk:

```bash
ip addr
```

Lühem variant:

```bash
ip a
```

Liideste olek:

```bash
ip link
```

Marsruudid:

```bash
ip route
```

või

```bash
ip r
```

!!! note "`ifconfig`"
    `ifconfig` on vana `net-tools` paketi tööriist. Seda võib vanades materjalides ja süsteemides endiselt kohata, kuid uues õppematerjalis tasub eelistada `iproute2` tööriistu (`ip addr`, `ip link`, `ip route`).

### Võrguühenduse kontroll

```bash
ping 1.1.1.1
```

DNS-i kontrollimiseks:

```bash
ping debian.org
```

DNS-päringute jaoks on sobivamad tööriistad näiteks:

```bash
getent hosts debian.org
```

või `dnsutils` paketi korral:

```bash
dig debian.org
```

### Kuulavad pordid ja ühendused

```bash
ss -tulpen
```

Levinud lihtsam variant:

```bash
ss -tulpn
```

!!! note
    Vanemates materjalides kasutatakse sageli `netstat` käsku. Tänapäevases Linuxis on selle asemel tavaliselt eelistatud `ss`.

### Võrgukonfiguratsiooni asukoht

Fail

```text
/etc/network/interfaces
```

on kasutusel eelkõige siis, kui süsteem haldab võrku **ifupdown** abil. See ei ole universaalne kõigi Linuxi süsteemide võrgukonfiguratsiooni asukoht. Töölauasüsteemides kasutatakse sageli NetworkManagerit ning serverites võib kasutusel olla ka `systemd-networkd` või muu lahendus.

Kui `ifupdown` on kasutusel:

```bash
sudo ifup enp1s0
sudo ifdown enp1s0
```

---

## 18. Tarkvarahaldus Debianis ja Ubuntus

### Paketiloendi värskendamine

```bash
sudo apt update
```

### Paigaldatud pakettide uuendamine

```bash
sudo apt upgrade
```

Täielik sõltuvuste lahendamine, vajadusel pakette lisades või eemaldades:

```bash
sudo apt full-upgrade
```

### Paketi paigaldamine

```bash
sudo apt install nginx
```

### Paketi otsimine

```bash
apt search nginx
```

### Paketi info

```bash
apt show nginx
```

Installitud ja saadaoleva versiooni kontroll:

```bash
apt policy nginx
```

### Eemaldamine

Pakett eemaldatakse, kuid süsteemsed konfiguratsioonifailid võivad alles jääda:

```bash
sudo apt remove nginx
```

Pakett koos selle paketihalduri hallatavate konfiguratsioonifailidega:

```bash
sudo apt purge nginx
```

Enam mittevajalike automaatselt paigaldatud sõltuvuste eemaldamine:

```bash
sudo apt autoremove
```

APT allalaaditud paketifailide puhvri tühjendamine:

```bash
sudo apt clean
```

!!! note "`apt` või `apt-get`?"
    Interaktiivses terminalitöös on tänapäeval mugav kasutada `apt` käsku. Skriptides soovitab Debian kasutada stabiilsema käsurealiidesega `apt-get` käsku.

### APT repositooriumid

Vanemates süsteemides paiknes põhiline nimekiri sageli failis:

```text
/etc/apt/sources.list
```

Tänapäeval võivad repositooriumid paikneda ka kataloogis:

```text
/etc/apt/sources.list.d/
```

ning kasutada `.sources` ehk deb822 vormingut.

### `.deb` paketi paigaldamine

Tänapäeval on mugav kasutada:

```bash
sudo apt install ./pakett.deb
```

Madalama taseme tööriist:

```bash
sudo dpkg -i pakett.deb
```

Kui `dpkg` paigalduse järel on sõltuvused katki:

```bash
sudo apt --fix-broken install
```

Kasulikud `dpkg` käsud:

```bash
dpkg -l
dpkg -s nano
dpkg -L nano
dpkg -S /bin/mkdir
```

`.deb` faili sisu enne paigaldamist:

```bash
dpkg-deb -c pakett.deb
```

---

## 19. Kettaruum, failisüsteemid ja plokiseadmed

### Failisüsteemide vaba ruum

```bash
df -h
```

### Kataloogi kettakasutus

```bash
du -sh /var/cache/apt/
```

Alamkataloogid:

```bash
du -h --max-depth=1 /var
```

### Kettad ja partitsioonid

```bash
lsblk
```

Koos failisüsteemide infoga:

```bash
lsblk -f
```

UUID ja failisüsteemi tüüp:

```bash
blkid
```

Partitsioonitabelid:

```bash
sudo fdisk -l
```

Ketta muutmine:

```bash
sudo fdisk /dev/sdb
```

!!! danger
    Partitsioonitabeli muutmine võib andmeid hävitada. Enne muudatusi kontrolli alati seadme nime (`lsblk`) ja varunda vajalikud andmed.

!!! note "MBR vs GPT"
    Vana MBR/DOS partitsioonitabeli puhul kehtivad primary/extended/logical partitsioonide piirangud. Tänapäevastes süsteemides kasutatakse enamasti **GPT-d**, kus sellist jaotust ei ole.

### Failisüsteemi loomine

```bash
sudo mkfs.ext4 /dev/sdb1
```

või üldkujul:

```bash
sudo mkfs -t ext4 /dev/sdb1
```

!!! danger
    `mkfs` loob failisüsteemi ja hävitab valitud partitsioonil oleva varasema failisüsteemi sisu.

---

## 20. Failisüsteemi mountimine

Mount-point'i loomine:

```bash
sudo mkdir -p /mnt/andmed
```

Mountimine:

```bash
sudo mount /dev/sdb1 /mnt/andmed
```

Kontrollimiseks:

```bash
findmnt
```

Konkreetse tee kontroll:

```bash
findmnt /mnt/andmed
```

Lahtiühendamine:

```bash
sudo umount /mnt/andmed
```

!!! note
    Kui failisüsteem mountida kataloogi peale, mille sees on juba faile, muutuvad need mountimise ajaks peidetuks. Need ilmuvad uuesti pärast `umount` käsku.

### `/etc/fstab`

Püsivad mountimised kirjeldatakse failis:

```text
/etc/fstab
```

Soovitatav on seadmenime `/dev/sdb1` asemel kasutada **UUID-d**, sest kettaseadme nimi võib muutuda.

UUID leidmine:

```bash
blkid /dev/sdb1
```

Näide:

```text
UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx /mnt/andmed ext4 defaults 0 2
```

Konfiguratsiooni testimine:

```bash
sudo mount -a
```

!!! warning
    Enne taaskäivitamist kontrolli `fstab` muudatused `mount -a` abil. Vigane kirje võib põhjustada bootimisel probleeme.

---

## 21. Saaleala ehk swap

Aktiivse swapi vaatamine:

```bash
swapon --show
```

Mälu ülevaade:

```bash
free -h
```

### Swap-partitsioon

```bash
sudo mkswap /dev/sdb2
sudo swapon /dev/sdb2
```

Väljalülitamine:

```bash
sudo swapoff /dev/sdb2
```

Kõigi `/etc/fstab` swap-kirjete aktiveerimine:

```bash
sudo swapon -a
```

Kõigi aktiivsete swap-alade väljalülitamine:

```bash
sudo swapoff -a
```

### Swap-fail

Näiteks 1 GiB swap-fail:

```bash
sudo fallocate -l 1G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

Kui `fallocate` ei sobi kasutatava failisüsteemiga, saab faili luua näiteks `dd` abil:

```bash
sudo dd if=/dev/zero of=/swapfile bs=1M count=1024 status=progress
```

`/etc/fstab` kirje:

```text
/swapfile none swap sw 0 0
```

---

## 22. Sümboolsed ja hard link'id

### Sümboolne link

```bash
ln -s /tegelik/fail link
```

Sümboolne link:

- viitab failinimele/teele;
- võib viidata failile või kataloogile;
- võib ületada failisüsteemi piire;
- võib muutuda katkiseks, kui sihtmärk eemaldatakse või teisaldatakse.

### Hard link

```bash
ln fail.txt teine_nimi.txt
```

Hard link:

- viitab samale inode'ile ja failiandmetele;
- tavaliselt ei saa teha kataloogidele;
- ei saa tavakasutuses ületada failisüsteemi piire;
- faili sisu jääb alles seni, kuni vähemalt üks hard link sellele inode'ile eksisteerib.

Katkiste sümboolsete linkide otsimine:

```bash
find -L . -type l
```

---

## 23. Skriptide käivitamine

Skript:

```bash
#!/bin/bash
echo "Tere"
```

Käivitusõiguse lisamine:

```bash
chmod +x skript.sh
```

Käivitamine aktiivsest kataloogist:

```bash
./skript.sh
```

Täieliku teega:

```bash
/home/student/skript.sh
```

Bashi kaudu:

```bash
bash skript.sh
```

!!! note
    `./skript.sh` kasutab skripti shebang-rida (`#!...`) ja nõuab käivitusõigust. `bash skript.sh` annab faili otse Bashile ning faili `x` õigus ei ole selleks vajalik.

---

## 24. Süsteemiinfo

Kernel ja süsteem:

```bash
uname -a
```

Operatsioonisüsteemi info:

```bash
cat /etc/os-release
```

Hostname:

```bash
hostname
```

Detailsem hostname/systemd info:

```bash
hostnamectl
```

Tööaeg ja koormus:

```bash
uptime
```

Mälu:

```bash
free -h
```

CPU info:

```bash
lscpu
```

Plokiseadmed:

```bash
lsblk
```

---

## 25. Arhiivid

### `tar`

`.tar` arhiivi loomine:

```bash
tar -cvf arhiiv.tar kataloog/
```

`.tar.gz` loomine:

```bash
tar -czvf arhiiv.tar.gz kataloog/
```

Lahtipakkimine:

```bash
tar -xzvf arhiiv.tar.gz
```

Sisu vaatamine ilma lahti pakkimata:

```bash
tar -tzvf arhiiv.tar.gz
```

!!! tip
    `tar` võtmetes tähendab tavaliselt `c` create, `x` extract, `t` list, `v` verbose, `f` file ja `z` gzip.

---

## 26. Käskude kiire meelespea

| Eesmärk | Käsk |
|---|---|
| asukoht | `pwd` |
| kataloogi vahetamine | `cd` |
| failide nimekiri | `ls -lah` |
| faili loomine | `touch` |
| kataloogi loomine | `mkdir -p` |
| kopeerimine | `cp`, `cp -r`, `cp -a` |
| liigutamine / ümbernimetamine | `mv` |
| kustutamine | `rm`, `rm -r` |
| faili lugemine | `less`, `cat` |
| faili lõpp | `tail` |
| teksti otsimine | `grep` |
| failide otsimine | `find` |
| kasutaja info | `id`, `getent passwd` |
| õiguste muutmine | `chmod` |
| omaniku muutmine | `chown` |
| protsessid | `ps`, `top`, `htop` |
| teenused | `systemctl` |
| logid | `journalctl` |
| võrk | `ip`, `ss`, `ping` |
| paketid | `apt` |
| kettaruum | `df -h`, `du -sh` |
| kettad/partitsioonid | `lsblk`, `blkid`, `fdisk` |
| mountimine | `mount`, `umount`, `findmnt` |
| mälu | `free -h` |
| käsu abi | `man`, `--help` |

---

## 27. Mõned olulised põhimõtted

1. **Loe veateadet.** Linuxi käsurea veateated annavad sageli üsna täpselt teada, mis valesti läks.
2. **Kasuta `man` ja `--help` abi.** Kõiki käsuvõtmeid ei ole vaja pähe õppida.
3. **Kasuta Tab-klahvi.** See vähendab kirjavigu failiteedes ja käsunimedes.
4. **Kontrolli enne administraatoriõigustes käivitamist.** Eriti `rm`, `chmod`, `chown`, `dd`, `mkfs`, `fdisk` ja paketihalduse käsud võivad süsteemi oluliselt muuta.
5. **Linux on tõstutundlik.** `Fail.txt` ja `fail.txt` on erinevad nimed.
6. **Absoluutne tee algab `/` märgiga.** Suhteline tee lähtestatakse aktiivsest kataloogist.
7. **Ära lahenda failiõiguste probleeme automaatselt `chmod 777` abil.** Leia esmalt, millist õigust ja kellele tegelikult vaja on.

