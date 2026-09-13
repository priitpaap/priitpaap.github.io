## Kolmanda osapoole hoidla lisamine

Mõnda programmi Debiani põhihoidlates ei ole või on seal selle vanem versioon. Sellisel juhul võib tarkvara tootja pakkuda oma hoidlat.

Kolmanda osapoole hoidla lisamine tähendab, et lubad APT-il laadida pakette ja uuendusi ka Debiani-välisest allikast.

!!! warning
    Lisa ainult usaldusväärseid hoidlaid. Kontrolli, et juhend pärineks tarkvara tootja ametlikult veebilehelt ning sobiks sinu Debiani versiooniga.

### Näide: Zabbixi hoidla lisamine Debian 13 süsteemi

Zabbix pakub hoidla lisamiseks paketti `zabbix-release`. See pakett ei paigalda veel Zabbixi programmi, vaid lisab süsteemi:

- Zabbixi hoidla seadistuse;
- hoidla pakettide kontrollimiseks vajaliku GPG-võtme.

#### 1. Kontrolli Debiani versiooni

```bash
cat /etc/os-release
```

Näites kasutatav pakett on mõeldud Debian 13 jaoks. Teise Debiani versiooni korral tuleb valida sellele vastav pakett.

#### 2. Paigalda allalaadimiseks vajalik vahend

```bash
sudo apt update
sudo apt install wget
```

#### 3. Laadi hoidla seadistuspakett alla

```bash
wget https://repo.zabbix.com/zabbix/7.4/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.4+debian13_all.deb
```

#### 4. Uuri paketti enne paigaldamist

Paketi üldinfo vaatamiseks:

```bash
dpkg-deb --info zabbix-release_latest_7.4+debian13_all.deb
```

Paketis sisalduvate failide vaatamiseks:

```bash
dpkg-deb --contents zabbix-release_latest_7.4+debian13_all.deb
```

Kontrolli, et paketi väljastaja ja kirjeldus viitaksid Zabbixi ametlikule hoidlale.

#### 5. Paigalda hoidla seadistuspakett

```bash
sudo apt install ./zabbix-release_latest_7.4+debian13_all.deb
```

`./` näitab APT-ile, et paigaldatakse praeguses kataloogis olev fail, mitte ei otsita sama nimega paketti hoidlast.

#### 6. Uuenda pakettide nimekiri

```bash
sudo apt update
```

Alles pärast seda tunneb APT uues hoidlas olevaid pakette.

#### 7. Kontrolli lisatud hoidlat

Vaata, millised failid paigaldas `zabbix-release`:

```bash
dpkg -L zabbix-release
```

Otsi APT-i seadistusest Zabbixi hoidla aadressi:

```bash
grep -R "repo.zabbix.com" /etc/apt/sources.list.d/
```

Kontrolli, kas Zabbixi pakett on nüüd leitav ja millisest hoidlast see pärineb:

```bash
apt policy zabbix-agent2
```

Väljundis peaks olema näha Zabbixi ametliku hoidla aadress `repo.zabbix.com`.

#### 8. Paigalda hoidlast näidispakett

```bash
sudo apt install zabbix-agent2
```

Pärast paigaldamist kontrolli eraldi paketi ja teenuse olekut:

```bash
dpkg -s zabbix-agent2
systemctl status zabbix-agent2
```

### Hoidla eemaldamine

Kui hoidlat pole enam vaja, eemalda selle seadistuspakett:

```bash
sudo apt purge zabbix-release
sudo apt update
```

Kontrolli, kas hoidla kirje eemaldati:

```bash
grep -R "repo.zabbix.com" /etc/apt/sources.list.d/
```

Kui `grep` midagi ei väljasta, ei leitud sellest kataloogist enam Zabbixi hoidla kirjet.

!!! note
    Hoidla eemaldamine ei eemalda automaatselt sellest varem paigaldatud programme. Need tuleb vajaduse korral eraldi eemaldada.

### Kuidas näeb välja tänapäevane APT-i hoidla seadistus?

Debian eelistab uute hoidlate kirjeldamiseks laiendiga `.sources` faile. Need paiknevad tavaliselt kataloogis:

```text
/etc/apt/sources.list.d/
```

Deb822-vormingus hoidla kirjelduse üldkuju on järgmine:

```text
Types: deb
URIs: https://hoidla.example.org/debian
Suites: trixie
Components: main
Signed-By: /etc/apt/keyrings/hoidla.gpg
```

| Väli | Tähendus |
|---|---|
| `Types` | kasutatava hoidla tüüp, tavaliselt `deb` |
| `URIs` | hoidla veebiaadress |
| `Suites` | distributsiooni versioon või koodnimi |
| `Components` | hoidla kasutatav osa, näiteks `main` |
| `Signed-By` | GPG-võti, millega selle hoidla allkirju kontrollitakse |

Tootja pakutav seadistuspakett võib kasutada ka vanemat `.list` vormingut. Administraator ei pea seda kohe käsitsi ümber tegema, kuid peaks oskama leida, millise seadistusfaili pakett lisas.

!!! note
    Vanades juhendites võib kohata käsku `apt-key`. Uue hoidla lisamisel tuleks kasutada hoidlapõhist `Signed-By` seadistust. Nii ei anta ühe hoidla võtmele õigust kinnitada kõigi teiste hoidlate pakette.

### Mida sellest näitest õppida?

Hoidla lisamisel tuleb:

1. kontrollida operatsioonisüsteemi versiooni;
2. kasutada tootja ametlikku allikat;
3. uurida allalaaditud seadistuspaketti;
4. lisada hoidla;
5. käivitada `apt update`;
6. kontrollida käsuga `apt policy`, millisest hoidlast pakett pärineb;
7. osata hoidla hiljem eemaldada.

Allikad:

- [Zabbixi ametlik allalaadimisleht](https://www.zabbix.com/download)
- [Zabbixi ametlik paketihoidla](https://repo.zabbix.com/)
- [Debiani `sources.list(5)` käsiraamat](https://manpages.debian.org/stable/apt/sources.list.5.en.html)