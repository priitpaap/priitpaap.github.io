
# Moodulid ja ad-hoc käsud

## Eesmärk

Selles peatükis õpid:

- Mis on Ansible moodulid ja miks neid kasutatakse  
- Kuidas käivitada ad-hoc käske Ansible abil  
- Levinumad moodulid ja nende kasutamine (Linux ja Windows)  
- Kuidas testida ja harjutada moodulite kasutamist  

---

## Mis on moodulid?

- **Moodul** on Ansible põhikomponent, mis täidab konkreetset ülesannet hallataval hostil.  
- Moodulid on nagu **tööriistad** – igal ühel on kindel funktsioon (nt failide kopeerimine, pakettide paigaldamine, kasutajate haldus).  
- Ansible ei logi lihtsalt hosti sisse ja ei käivita käske otse – ta kasutab mooduleid, mis tagavad idempotentse ja standardiseeritud tulemuse.  

!!! info
    **Idempotentsus** tähendab seda, et sama playbook’i või mooduli käivitamine mitu korda järjest ei muuda süsteemi seisundit pärast esimest edukat käivitust.

### Näited moodulitest
- **ping** – kontrollib, kas hostiga saab ühenduda (Linux);  
- **win_ping** – kontrollib ühendust Windows hostiga;  
- **copy** – kopeerib faile;  
- **package / apt / yum / dnf** – paigaldab või eemaldab tarkvara;  
- **service** – käivitab või peatab teenuseid;  
- **user** – haldab kasutajakontosid;
- **win_feature** - paigaldab või eemaldab Windowsi Serveri rollid ja funktsioonid. 

### Mooduli nime kasutamine (lühike vs täisnimi)

Ansible’is saab mooduleid kutsuda kas **lühinimega** või **täisnimega**:

```bash
# Lühinimi (builtin moodul)
ansible all -m ping

# Täisnimi (täpne nimeruum)
ansible all -m ansible.builtin.ping
```

- Lühivorm (ping, copy, service) on mugavam ja töötab siis, kui moodul on Ansible enda sisseehitatud kogust (*builtin*).
- Täisvorm (n: ansible.builtin.ping) muutus oluliseks alates Ansible Collections süsteemist (versioon 2.9+). See väldib segadust, kui sama nimega moodul on saadaval mitmes kogus.

**Kogu** (collection) on moodulite, plugin’ite ja rollide kogumik.
Ansible jagas moodulid kogudesse, et neid oleks lihtsam hallata ja uuendada.

Näiteks:

- ansible.builtin – Ansible enda sisseehitatud moodulid (nt ping, copy).
- ansible.windows – Microsoft Windowsi moodulid.
- community.general – kogukonna loodud moodulid, mida pole vaikimisi Ansible sees.

!!! info
    Enamasti piisab lühinimest, aga dokumentatsioonis näed sageli ka täisnimekujusid. Kui võtad mooduli väljastpoolt Ansible sisseehitatud kogu, tuleb kasutada täisnime.

---

## Mis on ad-hoc käsud?

- **Ad-hoc käsud** on ühekordsed Ansible käsud, millega saab kiiresti mooduleid kasutada **ilma playbooki kirjutamata**.  
- Need sobivad katsetamiseks, kiireks muudatuseks või kontrollimiseks.  
- Käivitatakse käsurealt `ansible` käsu abil.  

### Süntaks

```bash
ansible <hostid> -m <moodul> -a "<argumendid>" -i <inventory>
```

- `<hostid>` – hosti alias või grupi nimi inventory failist.  
- `-m` – määrab mooduli.  
- `-a` – annab moodulile argumendid.  
- `-i` – määrab inventory faili (ilma selleta kasutatakse vaikimisi faili).  

---

## Ad-hoc käsu näited

### Linux hostidel

**Kontrolli ühendust (ping moodul):**
```bash
ansible linux -m ping -i hosts.ini
```

**Paigalda Apache (kasutades apt moodulit):**
```bash
ansible webservers -m apt -a "name=apache2 state=present update_cache=yes" -b -i hosts.ini
```

**Kopeeri fail kohalikust masinast hostile:**
```bash
ansible linux -m copy -a "src=./test.txt dest=/tmp/test.txt" -i hosts.ini
```

**Käivita teenus:**
```bash
ansible webservers -m service -a "name=apache2 state=started enabled=yes" -b -i hosts.ini
```

### Windows hostidel

**Kontrolli ühendust (win_ping):**
```bash
ansible windows -m win_ping -i hosts.ini
```

**Paigalda Windowsi funktsioon (nt IIS):**
```bash
ansible winservers -m win_feature -a "name=Web-Server state=present" -i hosts.ini
```

**Kopeeri fail Windowsi hostile:**
```bash
ansible winservers -m win_copy -a "src=./test.txt dest=C:\\Temp\\test.txt" -i hosts.ini
```

**Käivita teenus Windowsis:**
```bash
ansible winservers -m win_service -a "name=Spooler state=restarted" -i hosts.ini
```

---

## Levinumad moodulid (õppuritele kasulikud)

### Linux
- `ping` – ühenduse kontroll.  
- `command` / `shell` – käskude käivitamine (ettevaatusega!).  
- `copy` – failide kopeerimine.  
- `file` – failide/kaustade õiguste ja oleku haldamine.  
- `package` – universaalne pakettide paigaldamise moodul.  
- `service` – teenuste haldamine.  
- `user` – kasutajate loomine ja haldamine.  

### Windows
- `win_ping` – ühenduse kontroll.  
- `win_command` / `win_shell` – käskude käivitamine.  
- `win_copy` – failide kopeerimine.  
- `win_file` – failide/kaustade haldamine.  
- `win_package` – tarkvara paigaldamine MSI/exe abil.  
- `win_service` – teenuste haldamine.  
- `win_user` – kasutajate haldamine.  


---

## Head tavad

- Kasuta **ad-hoc käske ainult kiireks testimiseks** – päris konfiguratsiooni jaoks kirjuta playbookid.  
- Kasuta mooduleid, mitte käske `command` või `shell`, sest moodulid on idempotentsed.  
- Alusta alati ühenduse testimisest moodulitega `ping` või `win_ping`.  
- Kasuta `-b` (become), kui vajad root õigusi Linuxi hostides.  

---

## Harjutus

1. Kontrolli, et saad ühenduse Linuxi ja Windowsi hostidega (`ping`, `win_ping`).  
2. Loo oma kodukataloogi fail `test.txt` ja kopeeri see kõikidesse Linux hostidesse `/tmp` alla.  
3. Paigalda Linux hostidesse pakett `htop` (või Apache, kui kasutad webservereid).  
4. Luba ja käivita Windows hostides teenus `Spooler`.  
5. Proovi eemaldada mõni pakett Linuxist ad-hoc käsuga.  
6. Katseta `command` või `win_command` moodulit ja võrdle tulemusi `shell` mooduliga.  

---

## Rohkem infot

- [Ansible modules](https://docs.ansible.com/ansible/latest/collections/index_module.html){:target="_blank"}   
- [Intro to ad-hoc commands](https://docs.ansible.com/ansible/latest/cli/ansible.html){:target="_blank"}   
