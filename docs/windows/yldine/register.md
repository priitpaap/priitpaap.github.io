# Windowsi register

**Windowsi register** (*Windows Registry*) on hierarhiline andmebaas, kuhu Windows, teenused, draiverid ja paljud rakendused salvestavad oma seadistusi. IT-spetsialist kasutab registrit näiteks seadistuse kontrollimiseks, tõrke põhjuse uurimiseks ja dokumenteeritud muudatuse tegemiseks.

Kõik seaded ei asu registris. Rakendused võivad kasutada ka seadistusfaile, andmebaase või pilveteenuseid. Registri olemasolu ei tähenda, et programmide seadistusfailid oleksid kadunud.

Materjali näited lähtuvad **Windows 11-st**. Läbiv näidisvõti `HKEY_CURRENT_USER\Software\OSLab\RegistriNaide` on mõeldud õppimiseks; selle väärtused ei juhi Windowsi tegelikke funktsioone.

!!! info "Õpieesmärgid"

    Pärast materjali läbimist oskad:

    - selgitada registri otstarvet ning eristada võtit, väärtust ja väärtuse andmeid;
    - eristada kasutaja ja arvuti tasemel seadistusi;
    - valida lihtsa näite jaoks sobiva andmetüübi;
    - leida ja muuta registrikirjet registriredaktoris ning kasutada lihtsaid käsureakäske;
    - eksportida registriharu ja selgitada importimise piiranguid;
    - kavandada muudatuse tagasivõtmist ja kontrollida tulemust.

## 1. Milleks registrit kasutatakse?

Registris võib olla näiteks rakenduste seadistusi, teenuste käivitusparameetreid, draiverite konfiguratsiooni ning kasutaja töökeskkonna eelistusi. Registri andmeid loevad ja muudavad ka Windows ning rakendused ise.

| Olukord | Mida registrist uurida? |
| --- | --- |
| Rakendus käitub eri kasutajatel erinevalt | Kas kasutajate seadistused erinevad? |
| Dokumenteeritud seadistus ei rakendu | Kas väärtus asub õiges harus ning kas nimi, tüüp ja andmed on õiged? |
| Seadistus muutub pärast sisselogimist tagasi | Kas seda haldab organisatsiooni poliitika või kirjutab rakendus selle üle? |

Registrisse suvalise nimega väärtuse lisamine ei loo uut Windowsi funktsiooni. Mõju tekib siis, kui mõni komponent oskab seda väärtust lugeda ja kasutada.

## 2. Registri ülesehitus

### 2.1. Võti, alamvõti ja väärtus

Registri struktuur meenutab kaustapuud. **Võti** (*key*) võib sisaldada alamvõtmeid ja **väärtusi** (*values*). Väärtusel on nimi, andmetüüp ja andmed.

Näide:

```text
HKEY_CURRENT_USER\Software\OSLab\RegistriNaide
```

Selles võtmes võiksid olla järgmised väärtused:

| Väärtuse nimi | Andmetüüp | Andmed |
| --- | --- | --- |
| `Teade` | `REG_SZ` | `Tere!` |
| `Lubatud` | `REG_DWORD` | `1` |

`RegistriNaide` on võtme nimi, `Lubatud` väärtuse nimi ning `1` selle andmed. Võtit ja selle all olevat väärtust ei tohi juhendis omavahel segi ajada.

### 2.2. Registri põhiharud

Registriredaktoris on tavaliselt näha viis põhiharu.

| Põhiharu | Lühend | Tähendus |
| --- | --- | --- |
| **HKEY_CURRENT_USER** | `HKCU` | Registrit kasutava konto seadistused. Tavakasutuses on see sisselogitud kasutaja haru. |
| **HKEY_LOCAL_MACHINE** | `HKLM` | Arvuti tasemel süsteemi, riistvara ja tarkvara seadistused. |
| **HKEY_USERS** | `HKU` | Parajasti laaditud kasutajaprofiilide ja mõne süsteemikonto registriharud. |
| **HKEY_CLASSES_ROOT** | `HKCR` | Muu hulgas failitüüpide ja tarkvarakomponentide registreerimisteave. |
| **HKEY_CURRENT_CONFIG** | `HKCC` | Vaade kasutusel oleva riistvaraprofiili seadetele. |

`HKCU` viitab vastava kasutaja harule `HKEY_USERS` all. Kõik arvutis olemasolevad kasutajakontod ei pea olema seal korraga nähtavad. `HKCR` ühendab kasutaja ja arvuti tasemel klasside registreerimisteavet.

**Näide:** ühe konto `HKCU\Software` all tehtud muudatus ei ilmu automaatselt teise kasutaja samasse harusse.

### 2.3. Kus registri andmed paiknevad?

**Registrisülem** (*hive*) on võtmete, alamvõtmete ja väärtuste loogiline kogum, mille andmeid toetavad registrifailid. Põhiharu ja sülem ei ole alati üks ja sama: osa põhiharusid on vaated teistele registriosadele.

| Näide | Tüüpiline fail |
| --- | --- |
| Süsteemi seadistused | `%SystemRoot%\System32\Config\SYSTEM` |
| Arvuti tarkvaraseadistused | `%SystemRoot%\System32\Config\SOFTWARE` |
| Kasutaja registri põhiandmed | `%USERPROFILE%\NTUSER.DAT` |

`%SystemRoot%` tähistab Windowsi kataloogi ning `%USERPROFILE%` kasutajaprofiili kataloogi. Register koosneb mitmest failist ja seda ei tule käsitleda ühe tervikuna korraga mällu loetava tekstifailina. Töötava süsteemi registrifailide käsitsi kopeerimine ei asenda sobivat varundusmeetodit.

## 3. Olulisemad andmetüübid

| Andmetüüp | Sisu | Näide või märkus |
| --- | --- | --- |
| **REG_SZ** | Tekstistring | `Tere!` |
| **REG_DWORD** | 32-bitine täisarv | Seadistus võib kasutada näiteks `0` ja `1`. |
| **REG_QWORD** | 64-bitine täisarv | Suuremat arvu vajav seadistus. |
| **REG_EXPAND_SZ** | Keskkonnamuutujate viiteid sisaldav string | `%USERPROFILE%\Documents`; laiendamine sõltub seda kasutavast programmist. |
| **REG_MULTI_SZ** | Mitmest stringist koosnev loend | Üksikud stringid on eraldatud nullmärkidega; registriredaktoris sisestatakse need tavaliselt eri ridadele. |
| **REG_BINARY** | Binaarandmed | Andmete tähendus sõltub neid kasutavast komponendist. |

Andmetüübi peab valima vastavalt seadistuse dokumentatsioonile. Tekst `"1"` ja DWORD-arv `1` ei ole sama tüüpi andmed.

!!! tip "64-bitine Windows võib vajada DWORD-väärtust"

    DWORD tähendab 32-bitist andmetüüpi. Seda kasutatakse ka 64-bitises Windowsis. Operatsioonisüsteemi arhitektuuri põhjal ei tohi juhendis nõutud DWORD-i QWORD-iks muuta.

Registriredaktoris saab arvväärtuse sisestamisel valida kümnend- või kuueteistkümnendsüsteemi. Näiteks kümnendarv `10` on kuueteistkümnendsüsteemis `A`. Valitud arvusüsteem mõjutab sisestatud teksti tõlgendamist.

## 4. Registriredaktori kasutamine

Windowsi graafiline tööriist on **Registry Editor**, mille saab avada klahvidega `Win+R`, sisestades `regedit.exe`. Vajaduse korral kinnita haldusõiguste küsimine.

Vasakul asub võtmete puu, paremal valitud võtme väärtused. Teekonna saab kleepida akna ülaosas olevale aadressiribale. Otsimiseks kasuta `Ctrl+F` ja järgmise vaste leidmiseks `F3`. Kontrolli alati vaste täielikku teekonda.

### 4.1. Näidisvõtme ja väärtuste loomine

1. Leia `HKEY_CURRENT_USER\Software`.
2. Loo selle alla **New → Key** abil võti `OSLab` ja selle alla `RegistriNaide`. Kui `OSLab` on olemas, kasuta olemasolevat võtit.
3. Loo `RegistriNaide` alla **New → String Value** abil väärtus `Teade` ning määra andmeteks `Tere!`.
4. Loo **New → DWORD (32-bit) Value** abil väärtus `Lubatud` ning määra andmeteks `1`.
5. Kontrolli mõlema väärtuse nime, tüüpi ja andmeid.

Väärtuse andmeid saab muuta topeltklõpsuga. Nime muutmiseks sobib **Rename**, eemaldamiseks **Delete**. Võtme kustutamisel eemaldatakse ka selle all olevad alamvõtmed ja väärtused.

Näidisväärtuse `Lubatud` muutmine ei lülita ühtegi Windowsi võimalust sisse ega välja: siin õpitakse registri andmetega töötamist.

## 5. Varundamine ja muudatuse tagasivõtmine

### 5.1. Valitud registriharu eksportimine

Enne olemasoleva seadistuse muutmist salvesta algne olukord. Näidisvõtme puhul:

1. Vali `HKEY_CURRENT_USER\Software\OSLab\RegistriNaide`.
2. Vali **File → Export**.
3. Kontrolli, et valitud oleks **Selected branch** ja õige teekond.
4. Salvesta fail näiteks nimega `registrinaide-enne.reg`.

Eksporditakse valitud võtme ja selle alamvõtmete väärtused. Faili saab importida käsuga **File → Import**.

!!! warning "Import ei taasta haru täpset varasemat seisu"

    .reg-faili import lisab või kirjutab üle failis kirjeldatud kirjed. Vahepeal juurde loodud väärtused ei kao lihtsalt selle tõttu, et neid ekspordifailis pole. Eksport ei salvesta ka võtmete juurdepääsuõigusi ega asenda kogu süsteemi varukoopiat.

**Näide:** ekspordid võtme, milles on `Teade` ja `Lubatud`. Seejärel lisad `UusVaartus`. Varasema ekspordi importimine taastab failis kirjeldatud andmed, kuid ei eemalda `UusVaartus`-t.

Kui väärtust alguses ei olnud, pane see enne muudatust kirja: algse olukorra taastamine võib nõuda loodud väärtuse eemaldamist.

### 5.2. Taastepunkt ja süsteemi taastamine

Ulatuslikuma muudatuse eel võib kasutada **süsteemikaitset** (*System Protection*) ja luua taastepunkti. Kontrolli esmalt, kas süsteemikaitse on lubatud.

1. Vajuta `Win+R` ja käivita `systempropertiesprotection.exe`.
2. Vali süsteemiketas ning vajaduse korral **Configure → Turn on system protection** ja sobiv ruumipiirang.
3. Vali **Create** ning anna taastepunktile arusaadav kirjeldus.
4. Kontrolli loomise õnnestumist.

Taastepunkti rakendamiseks ava samas aknas **System Restore**. See võib mõjutada pärast taastepunkti loodud süsteemiseadeid, paigaldatud programme ja draivereid. Taastepunkt ei asenda isiklike failide varundamist.

## 6. .reg-failid

.reg-fail on kokkulepitud vorminguga tekstifail registrimuudatuste kirjeldamiseks. Selle abil saab sama muudatuse korratavalt rakendada. Enne importimist ava fail tekstiredaktoris ja vaata sisu üle.

### 6.1. Väärtuste lisamine või muutmine

```reg
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\OSLab\RegistriNaide]
"Teade"="Tere!"
"Lubatud"=dword:00000001
```

Päis `Windows Registry Editor Version 5.00` sobib ka Windows 11-le: number tähistab failivormingut. Nurksulgudes on võtme teekond; järgmistel ridadel väärtuste nimed ja andmed. DWORD esitatakse siin kuueteistkümnendsüsteemis.

Käsitsi faili luues vali Notepadis **Save as type → All files**, nimeks näiteks `registrinaide.reg` ja kodeeringuks **UTF-16 LE**. Veendu, et tegelik laiend ei oleks `.reg.txt`. Alternatiivina ekspordi näidisvõti registriredaktorist ja uuri loodud faili.

### 6.2. Väärtuse või võtme kustutamine

Ainult väärtuse `Lubatud` eemaldamine:

```reg
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\OSLab\RegistriNaide]
"Lubatud"=-
```

Kogu näidisvõtme ja selle sisu eemaldamine:

```reg
Windows Registry Editor Version 5.00

[-HKEY_CURRENT_USER\Software\OSLab\RegistriNaide]
```

Miinusmärgi asukoht on oluline: väärtuse puhul asub see võrdusmärgi järel, võtme puhul teekonna ees nurksulgude sees. Üldise `OSLab` võtme asemel eemaldatakse ainult selle materjali näidisvõti.

## 7. Registri kasutamine käsurealt

`reg.exe` võimaldab registrit lugeda, muuta, eksportida ja importida. Järgmised käsud sobivad nii käsuviibale kui ka PowerShelli terminalile. Kasuta enda loodud näidisvõtit ning ekspordifailide jaoks kausta, kuhu sul on kirjutamisõigus. [7]

### 7.1. Lugemine ja muutmine

Võtme väärtuste vaatamine:

```powershell
reg.exe query "HKCU\Software\OSLab\RegistriNaide"
```

Ühe väärtuse vaatamine:

```powershell
reg.exe query "HKCU\Software\OSLab\RegistriNaide" /v Lubatud
```

DWORD-väärtuse lisamine või muutmine:

```powershell
reg.exe add "HKCU\Software\OSLab\RegistriNaide" /v Lubatud /t REG_DWORD /d 0
```

`/v` määrab väärtuse nime, `/t` tüübi ning `/d` andmed. Olemasoleva väärtuse ülekirjutamisel võib tööriist küsida kinnitust. Seejärel kontrolli tulemust uuesti `reg.exe query` abil.

### 7.2. Eksportimine ja importimine

```powershell
reg.exe export "HKCU\Software\OSLab\RegistriNaide" ".\registrinaide-enne.reg"
```

```powershell
reg.exe import ".\registrinaide-enne.reg"
```

`reg.exe export` kirjutab failina välja registriharu sisu; `reg.exe import` rakendab failis kirjeldatud muudatused. Failitee alguses olev `.\` viitab terminali töökaustale.

### 7.3. Ainult näidisvõtme eemaldamine

Kui näidet enam vaja ei ole:

```powershell
reg.exe delete "HKCU\Software\OSLab\RegistriNaide"
```

Kontrolli kustutamisküsimuses teekonda ning kinnita ainult selle näidisvõtme eemaldamine. Seejärel peaks sama võtme päring teatama, et võtit ei leitud.

## 8. Õigused ja erinevad registrivaated

### 8.1. Juurdepääsuõigused

Registrivõtmetel on juurdepääsuõigused, mis võivad päranduda ülemvõtmelt. Need määravad muu hulgas lugemise, väärtuste muutmise ja alamvõtmete loomise võimaluse. Väärtustele eraldi õigusi ei määrata: juurdepääsu juhib neid sisaldav võti.

Kasutaja saab tavaliselt muuta oma `HKCU\Software` seadeid; arvuti tasemel `HKLM` muudatused vajavad sageli kõrgendatud õigusi. Haldusõigustes töötamine ei anna siiski automaatselt kirjutamisõigust igale võtmele.

Kui tööriist käivitatakse teise administraatorikonto mandaadiga, võib ka `HKCU` tähistada selle konto haru. Seetõttu kontrolli lisaks teekonnale, kelle õigustes tööriist töötab.

### 8.2. 32- ja 64-bitiste rakenduste vaated

64-bitises Windowsis on osa registrist 32- ja 64-bitistele rakendustele eraldi vaadetena. Näiteks x64-süsteemis võib 32-bitise rakenduse tarkvarateave olla registriredaktoris nähtav `HKLM\SOFTWARE\WOW6432Node` all. Kõik registriosad ei ole eraldatud.

See selgitab, miks ühe tööriistaga nähtud väärtus ei pruugi teise arhitektuuriga rakenduse käitumist muuta.

**Registri virtualiseerimine** on teistsugune ühilduvusmehhanism: teatud vanemate 32-bitiste rakenduste kirjutamised võidakse suunata kaitstud asukohast kasutajapõhisesse asukohta. Selle eesmärk on vanema tarkvara ühilduvus, mitte registri mälumahu piiramine. See ei rakendu kõigile programmidele.

## 9. Registrimuudatuse tööjärjekord

1. **Leia dokumenteeritud seadistus.** Kontrolli juhendi sobivust Windowsi või rakenduse versiooniga.
2. **Täpsusta sihtkoht.** Vaata haru, kasutajat, väärtuse nime, tüüpi ja vajaduse korral registrivaadet.
3. **Salvesta algseis.** Ekspordi vajalik haru ja pane kirja ka seni puudunud väärtused.
4. **Tee üks põhjendatud muudatus.** Eelista võimaluse korral rakenduse enda seadistusvahendit või organisatsiooni halduslahendust.
5. **Kontrolli andmeid ja tegelikku mõju.** Registrikirje olemasolu ei tõenda veel seadistuse rakendumist. Rakendus võib vajada taaskäivitamist või kasutaja uut sisselogimist; järgi vastava seadistuse juhendit.
6. **Dokumenteeri ja vajaduse korral taasta.** Pane kirja põhjus, muudatus, tulemus ja tagasivõtmise viis.

Organisatsioonis võib rühmapoliitika või muu halduslahendus registriseade uuesti määrata. Sel juhul tuleb muudatust hinnata koos seadistuse haldamise viisiga.

## 10. Enesekontroll

Vasta kõigepealt ise. Seejärel ava vastus ja võrdle oma põhjendust.

### 1. Mille poolest erinevad registrivõti, väärtuse nimi ja andmed?

??? question "Vaata vastust"

    Võti määrab asukoha registripuus. Selle all oleva väärtuse nimi võib olla näiteks `Lubatud`, tüüp `REG_DWORD` ning andmed `1`. Võtme kustutamisel eemaldatakse ka selle all olevad väärtused ja alamvõtmed.

### 2. Miks ei muuda HKCU all tehtud seadistus automaatselt kõigi kasutajate seadeid?

??? question "Vaata vastust"

    HKCU näitab registrit kasutava konto seadistusi. Teise konto kasutajaharus võivad olla teised andmed. Kontrollida tuleb ka seda, kelle õigustes seadistusvahend töötab.

### 3. Kas 64-bitises Windowsis tuleb juhendis nõutud DWORD asendada QWORD-iga?

??? question "Vaata vastust"

    Ei. Andmetüüp tuleb valida vastavalt seadistuse dokumentatsioonile. DWORD-väärtusi kasutatakse ka 64-bitises Windowsis.

### 4. Pärast eksporti lisati võtmesse uus väärtus. Kas varasema .reg-faili import eemaldab selle?

??? question "Vaata vastust"

    Ei. Import lisab või kirjutab üle failis kirjeldatud kirjed. Uus väärtus tuleb vajaduse korral eraldi eemaldada, kui see on algse olukorra taastamiseks vajalik.

## Kokkuvõte

- Register hoiab Windowsi ja rakenduste seadistusi, kuid kõik seaded ei asu registris.
- Erista registrivõtit, väärtuse nime, andmetüüpi ja andmeid.
- HKCU on kasutaja ning HKLM arvuti tasemel seadistuste oluline asukoht.
- Ekspordi enne muudatust vajalik haru ja kavanda tagasivõtmine.
- .reg-faili import ei ole registriharu täielik tagasipööramine varasemasse seisu.
- Kontrolli lisaks kirjele ka rakenduse või süsteemi tegelikku käitumist.

## Teema sõnastik

| Mõiste | Selgitus |
| --- | --- |
| **Register — registry** | Hierarhiline andmebaas Windowsi ja rakenduste seadistuste hoidmiseks. |
| **Registrivõti — registry key** | Registripuu osa, mis võib sisaldada alamvõtmeid ja väärtusi. |
| **Registriväärtus — registry value** | Võtme all olev nime, andmetüübi ja andmetega kirje. |
| **Registrisülem — hive** | Registrivõtmete ja väärtuste loogiline kogum, mida toetavad registrifailid. |
| **Registriredaktor — Registry Editor** | Graafiline tööriist registri vaatamiseks ja muutmiseks; `regedit.exe`. |
| **.reg-fail** | Tekstifail registrikirjete lisamiseks, muutmiseks või eemaldamiseks. |
| **Taastepunkt — restore point** | Salvestatud süsteemiseis, mida saab kasutada süsteemiseadete ja registri taastamisel. |

## Allikad ja lisalugemine

1. [Microsoft Learn — Structure of the Registry](https://learn.microsoft.com/en-us/windows/win32/sysinfo/structure-of-the-registry).
2. [Microsoft Learn — Predefined Keys](https://learn.microsoft.com/en-us/windows/win32/sysinfo/predefined-keys).
3. [Microsoft Learn — Registry Hives](https://learn.microsoft.com/en-us/windows/win32/sysinfo/registry-hives).
4. [Microsoft Learn — Registry value types](https://learn.microsoft.com/en-us/windows/win32/sysinfo/registry-value-types).
5. [Microsoft Support — How to back up and restore the registry in Windows](https://support.microsoft.com/en-gb/windows/experience/backup-recovery/how-to-back-up-and-restore-the-registry-in-windows).
6. [Microsoft Support — System Protection](https://support.microsoft.com/en-gb/windows/experience/backup-recovery/system-protection).
7. [Microsoft Learn — reg commands](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/reg).
8. [Microsoft Learn — reg add](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/reg-add).
9. [Microsoft Learn — reg export](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/reg-export).
10. [Microsoft Learn — reg import](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/reg-import).
11. [Microsoft Learn — reg delete](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/reg-delete).
12. [Microsoft Learn — Registry Key Security and Access Rights](https://learn.microsoft.com/en-us/windows/win32/sysinfo/registry-key-security-and-access-rights).
13. [Microsoft Learn — Registry Redirector](https://learn.microsoft.com/en-us/windows/win32/winprog64/registry-redirector).
14. [Microsoft Learn — Registry Virtualization](https://learn.microsoft.com/en-us/windows/win32/sysinfo/registry-virtualization).

---

*Õppematerjali koostaja: Priit Paap, 2026*
