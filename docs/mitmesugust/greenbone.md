# Praktiline töö: Greenbone Community Edition (OpenVAS)

## Eesmärk

Praktilise töö eesmärk on õppida paigaldama ja kasutama Greenbone
Community Editioni haavatavuste skannerit, skaneerima üksikut
võrguseadet ja alamvõrku ning analüüsima leitud turvanõrkusi.

Töö käigus õpid:

-   paigaldama Docker Engine'i Debian Linuxile;
-   käivitama Greenbone Community Editioni konteinerites;
-   kontrollima konteinerite ja Greenbone'i teenuste olekut;
-   kasutama Greenbone Security Assistant veebiliidest;
-   skaneerima üksikut IP-aadressi ja tervet alamvõrku;
-   hindama skannimise tulemusi ja haavatavuste tõsidust;
-   leidma haavatavuse kirjelduse ja soovitatud parandusmeetmed.

!!! warning "Oluline" Skaneeri ainult süsteeme ja võrke, mille
skaneerimiseks on sul luba. Selles praktilises töös kasuta ainult kooli
selleks ette nähtud laborivõrgu virtuaalmasinaid.

------------------------------------------------------------------------

## 1. Debian virtuaalmasina ettevalmistamine

Greenbone Community Edition käivitatakse selles töös Dockeri
konteinerites.

Kasuta kooli laborikeskkonda paigaldatud **Debian 13** virtuaalmasinat,
millel on võrgukaardiks "Internet".

Soovituslikud ressursid:

  Ressurss                      Soovitus
  --------------------- ----------------
  CPU                             4 vCPU
  Mälu                          8 GB RAM
  Kõvaketas               vähemalt 80 GB
  Operatsioonisüsteem          Debian 13

!!! info "Miks on vaja nii palju kettaruumi?"
    Greenbone'i konteinerid,
    haavatavuste andmebaasid ehk *feed'id* ja skannimise tulemused vajavad
    märkimisväärselt kettaruumi. Varasemast 16 GB kettast ei piisa.


### 1.1. Uuenda Debian

``` bash
sudo apt update
sudo apt upgrade -y
```

------------------------------------------------------------------------

## 2. Docker Engine'i paigaldamine

### 2.1. Paigalda vajalikud paketid

``` bash
sudo apt install ca-certificates curl
```

### 2.2. Eemalda võimalikud konfliktseid paketid

Kui masinasse on varem paigaldatud mõni teine Dockeri versioon või
sellega konfliktne pakett, eemalda need:

``` bash
sudo apt remove $(dpkg --get-selections \
  docker.io docker-compose docker-doc docker-buildx \
  podman-docker containerd runc | cut -f1)
```

Kui mõnda paketti pole paigaldatud, ei ole see probleem.

### 2.3. Lisa Dockeri ametlik repositoorium

Loo võtmete kataloog:

``` bash
sudo install -m 0755 -d /etc/apt/keyrings
```

Laadi alla Dockeri allkirjastamisvõti:

``` bash
sudo curl -fsSL https://download.docker.com/linux/debian/gpg \
  -o /etc/apt/keyrings/docker.asc
```

Anna võtmele lugemisõigus:

``` bash
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

Lisa Dockeri repositoorium:

``` bash
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF
```

Uuenda pakettide nimekirja:

``` bash
sudo apt update
```

### 2.4. Paigalda Docker

``` bash
sudo apt install docker-ce docker-ce-cli containerd.io \
  docker-buildx-plugin docker-compose-plugin
```

Kontrolli Dockeri versiooni:

``` bash
docker --version
```

Kontrolli Docker Compose'i versiooni:

``` bash
docker compose version
```

### 2.5. Lisa kasutaja gruppi `docker`

``` bash
sudo usermod -aG docker $USER
```

Muudatuse rakendamiseks **logi Debianist välja ja uuesti sisse**.

Pärast uuesti sisselogimist kontrolli, kas Docker töötab:

``` bash
docker run hello-world
```

Kui näed teadet **Hello from Docker!**, töötab Docker korrektselt.

------------------------------------------------------------------------

## 3. Greenbone Community Editioni paigaldamine

Greenbone Community Edition koosneb mitmest teenusest, mis käivitatakse
eraldi Dockeri konteinerites. Nende haldamiseks kasutatakse Docker
Compose'i.

### 3.1. Loo Greenbone'i kataloog

``` bash
export DOWNLOAD_DIR=$HOME/greenbone-community-edition
mkdir -p "$DOWNLOAD_DIR"
```

### 3.2. Laadi alla Greenbone'i Compose-fail

``` bash
curl -f -O -L \
  https://greenbone.github.io/docs/latest/_static/compose.yaml \
  --output-dir "$DOWNLOAD_DIR"
```

Kontrolli, kas fail on olemas:

``` bash
ls -l "$DOWNLOAD_DIR"
```

Kataloogis peab olema fail:

``` text
compose.yaml
```

------------------------------------------------------------------------

## 4. Greenbone'i konteinerite allalaadimine

Liigu Greenbone'i kataloogi:

``` bash
cd "$DOWNLOAD_DIR"
```

Laadi konteinerid alla:

``` bash
docker compose -f compose.yaml pull
```

!!! info 
    Allalaadimine võib võtta mitu minutit ja vajada mitu gigabaiti
    kettaruumi.

------------------------------------------------------------------------

## 5. Greenbone'i käivitamine

Käivita Greenbone:

``` bash
docker compose -f compose.yaml up -d
```

Kontrolli konteinerite olekut:

``` bash
docker compose -f compose.yaml ps
```

Greenbone koosneb mitmest konteinerist, seega on normaalne, et
nimekirjas kuvatakse palju teenuseid.

Probleemide korral vaata logisid:

``` bash
docker compose -f compose.yaml logs -f
```

Logide jälgimise lõpetamiseks vajuta:

``` text
Ctrl+C
```

!!! tip "Kasulikud käsud" 
    Greenbone'i saab hiljem käivitada käsuga:
    ```bash
    cd "$HOME/greenbone-community-edition"
    docker compose up -d
    ```

    Peatamiseks kasuta:

    ```bash
    docker compose down
    ```

------------------------------------------------------------------------

## 6. Administraatori parooli muutmine

Greenbone'i administraatori kasutajanimi on:

``` text
admin
```

Määra administraatorile uus parool:

``` bash
docker compose -f "$DOWNLOAD_DIR/compose.yaml" \
  exec -u gvmd gvmd gvmd \
  --user=admin --new-password='SINU-TURVALINE-PAROOL'
```

Asenda `SINU-TURVALINE-PAROOL` enda valitud parooliga.

!!! warning 
    Ära kasuta näidisparooli `Passw0rd`. Vali labori jaoks
    piisavalt tugev parool ja jäta see endale meelde.

------------------------------------------------------------------------

## 7. Greenbone'i veebiliidese avamine

Vaata Greenbone'i virtuaalmasina IP-aadressi:

``` bash
ip addr
```

Ava oma tööarvuti veebibrauseris Greenbone'i HTTPS-aadress vastavalt
laboris seadistatud pordile, näiteks:

``` text
https://GREENBONE-IP
```

Logi sisse:

-   **kasutaja:** `admin`
-   **parool:** eelmises sammus määratud parool

!!! warning "Sertifikaadihoiatus" 
    Laboris võib Greenbone kasutada ise allkirjastatud TLS-sertifikaati. Seetõttu võib brauser kuvada
    sertifikaadihoiatuse. Kontrolli enne jätkamist, et avasid kindlasti enda
    Greenbone'i virtuaalmasina õige IP-aadressi.

------------------------------------------------------------------------

## 8. Greenbone'i feed'ide kontrollimine

Greenbone vajab haavatavuste tuvastamiseks ajakohaseid andmebaase ehk
*feed'e*.

Veebiliideses ava:

**Administration → Feed Status**

Pärast Greenbone'i esmakordset käivitamist toimub andmete laadimine ja
importimine automaatselt.

!!! danger "Ära alusta skannimist liiga vara" 
    Enne esimese skanni tegemist oota, kuni vajalikud feed'id on alla laaditud ja töödeldud.
    Esmane sünkroniseerimine võib võtta kaua aega.

Puuduliku feed'i korral ei ole Greenbone'i haavatavuste andmebaas
täielik ning skannimise tulemus ei pruugi olla usaldusväärne.

------------------------------------------------------------------------

# 9. Ühe kohtvõrgu masina skaneerimine

Nüüd skaneerid ühte kooli laborivõrgu virtuaalmasinat.

Sobivaks sihtmärgiks võib olla näiteks sinu enda Zabbixi või mõni muu
õpetaja lubatud virtuaalmasin.

!!! danger "Skaneeri ainult lubatud süsteeme" 
    Ära sisesta sihtmärgiks
    suvalist Internetist leitud IP-aadressi või domeeninime.

### 9.1. Loo skann

1.  Ava Greenbone'i veebiliides.
2.  Navigeeri **Scans → Tasks**.
3.  Ava **Task Wizard**.
4.  Sisesta skaneeritava laborimasina IP-aadress.
5.  Käivita skann.
6.  Oota, kuni skann on lõpetanud.

Skannimine võib sõltuvalt sihtmärgist võtta mitu minutit.

------------------------------------------------------------------------

## 10. Ühe masina skanni tulemuste analüüsimine

Ava lõpetatud skanni raport.

Tutvu leitud tulemustega ning vasta järgmistele küsimustele:

1.  Millist IP-aadressi skaneerisid?
2.  Mitu turvaleidu Greenbone tuvastas?
3.  Milliseid **Severity** tasemeid tulemustes esines?
4.  Milline oli kõige kõrgema **CVSS/severity** väärtusega leid?
5.  Mis oli selle leiu nimi?
6.  Kirjelda oma sõnadega, milles probleem seisneb.
7.  Millist lahendust või parandusmeedet (**Solution**) Greenbone
    soovitab?

!!! info "Severity ja CVSS" 
    Greenbone kasutab leidude tõsiduse
    hindamisel CVSS-põhist skoori. Mida suurem skoor, seda tõsisemaks
    hinnatakse võimalikku turvariski. Kõrge skoor ei tähenda siiski
    automaatselt, et süsteemi saab kindlasti rünnata -- tulemusi tuleb alati
    analüüsida kontekstis.

------------------------------------------------------------------------

# 11. Alamvõrgu skaneerimine

Järgmisena skaneerid ühe IP-aadressi asemel tervet kooli laborivõrgu
alamvõrku.

Selles ülesandes kasutatav alamvõrk on:

``` text
172.16.200.0/24
```

### 11.1. Käivita skann

1.  Ava **Scans → Tasks**.

2.  Ava **Task Wizard**.

3.  Sisesta sihtmärgiks:

    ``` text
    172.16.200.0/24
    ```

4.  Käivita skann.

5.  Oota, kuni skann on lõpetanud.

!!! info 
    Terve `/24` alamvõrgu skaneerimine võtab märgatavalt rohkem
    aega kui ühe IP-aadressi skaneerimine.

------------------------------------------------------------------------

## 12. Alamvõrgu skanni tulemuste analüüsimine

Ava lõpetatud skanni raport ja vasta:

1.  Mitu aktiivset hosti Greenbone leidis?
2.  Milliste hostide juures tuvastati turvaleide?
3.  Millise hosti juures tuvastati kõige rohkem turvaleide?
4.  Kas erinevate hostide juures esines samu haavatavusi või probleeme?
    Too üks näide.
5.  Milline oli kogu alamvõrgu kõige kõrgema severity-väärtusega leid?

------------------------------------------------------------------------

# 13. Mõtle ja võrdle

Vasta oma töö lõpus lühidalt järgmistele küsimustele.

### Üks host vs alamvõrk

Mis erinevus oli ühe IP-aadressi ja terve `/24` alamvõrgu skaneerimisel?

### Haavatavuse skanneri kasulikkus

Milleks võiks süsteemiadministraator Greenbone'i organisatsiooni võrgus
kasutada?

### Tulemuste tõlgendamine

Kas Greenbone'i leitud kõrge severity-väärtusega probleem tähendab
alati, et süsteemi on võimalik kohe edukalt rünnata? Põhjenda lühidalt.

------------------------------------------------------------------------

# Avalike teenuste skaneerimine

Haavatavuste skaneerimine tekitab sihtsüsteemile aktiivset võrguliiklust
ja võib olla käsitletav turvatestimisena.

!!! danger "Ära skaneeri suvalisi Interneti-teenuseid" 
    **Skaneeri ainult
    süsteeme ja võrke, mille skaneerimiseks on sul selge luba.**

    Selles praktilises töös skaneerime ainult kooli selleks ette nähtud laborivõrgu virtuaalmasinaid.

Ära sisesta Greenbone'i proovimiseks suvalisi:

-   avalikke IP-aadresse;
-   veebiservereid;
-   ettevõtete servereid;
-   kooliväliseid võrke;
-   domeeninimesid.

Turvatestimise õppimiseks saab kasutada spetsiaalselt selleks loodud
harjutuskeskkondi ja haavatavaid virtuaalmasinaid, näiteks Hack The Boxi
või VulnHubi keskkondi. Ka nende puhul tuleb järgida konkreetse
platvormi kasutustingimusi ja lubatud testimise piire.

------------------------------------------------------------------------

# Töö esitamine

Esita töö **ühe PDF-failina**.

PDF peab sisaldama järgmisi kuvatõmmiseid:

1.  Greenbone'i veebiliides, kuhu oled administraatorina sisse loginud.
2.  **Administration → Feed Status** vaade, millest on näha feed'ide
    olek.
3.  Ühe IP-aadressi lõpetatud skanni tulemus.
4.  Valitud turvaleiu detailvaade, kus on näha vähemalt leiu nimi,
    severity ja soovitatud lahendus.
5.  Alamvõrgu `172.16.200.0/24` lõpetatud skanni tulemus.

Lisaks peavad PDF-is olema vastused peatükkide:

-   **Ühe masina skanni tulemuste analüüsimine**;
-   **Alamvõrgu skanni tulemuste analüüsimine**;
-   **Mõtle ja võrdle**

küsimustele.

------------------------------------------------------------------------

## Kasulikud käsud

  Tegevus                     Käsk
  --------------------------- --------------------------
  Konteinerite käivitamine    `docker compose up -d`
  Konteinerite olek           `docker compose ps`
  Logide vaatamine            `docker compose logs -f`
  Konteinerite peatamine      `docker compose down`
  Dockeri versioon            `docker --version`
  Docker Compose'i versioon   `docker compose version`
  IP-aadresside vaatamine     `ip addr`

!!! success "Töö tulemus" 
    Kui oled ülesande lõpetanud, oskad paigaldada
    Greenbone Community Editioni, käivitada haavatavuste skanne ning teha
    skannimise tulemustest esmase turvaanalüüsi.
