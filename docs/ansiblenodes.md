# Hallatavate hostide ühendamine

## Eesmärk

Selles peatükis õpid:

- Kuidas Ansible *control node* ühendub hallatavate masinatega (Linux ja Windows)  
- Millised eeldused peavad olema täidetud  
- Kuidas seadistada SSH ühendus Linux hostidega  
- Kuidas seadistada SSH ja WinRM ühendus Windows hostidega  
- Kuidas kontrollida, et ühendus toimib  

---

## Kuidas Ansible ühendub hostidega?

- **Linux hostid** – Ansible kasutab **SSH**-ühendust, mis põhineb tavaliselt OpenSSH-l.  
- **Windows hostid** – Ansible kasutab **SSH** (OpenSSH) või **WinRM**-ühendust (Windows Remote Management).  

Control node ei vaja agenttarkvara. Piisab, kui hallatavatel masinatel on avatud ja õigesti seadistatud **SSH** või **WinRM** teenus.  

---

## Eeldused

### Linuxi host
- **SSH server** peab olema paigaldatud ja töötama.  
- Ansible control node peab omama ligipääsu kas **parooliga** või **SSH võtmega**.  
- Lisaks peab Linuxi masinas olema **Python** (vaikimisi olemas enamikes distributsioonides)

### Windowsi host
- Windowsi masin peab olema seadistatud kasutama **OpenSSH-serverit** või lubama **WinRM-i**.
- **Kui kasutatakse OpenSSH-d**:
    - OpenSSH-server peab olema paigaldatud ja töötav.
    - Tulemüür peab lubama SSH liikluse (port 22).
- **Kui kasutatakse WinRM-i**:
    - WinRM peab olema lubatud ja konfigureeritud (pole vaikimisi sees).
    - Windowsi tulemüür peab lubama vajalikud pordid (vaikimisi 5985 HTTP, 5986 HTTPS).
- Ansible peab teadma, et tegemist on Windows hostiga - vajalikud *inventory* muutujad (nt ansible_connection, ansible_winrm_transport, ansible_shell_type jne).  

---

## Linux hosti ühendamine Ansible'iga

1. Loo SSH võti (kui seda veel pole loodud)
    ```bash
    ssh-keygen -t ed25519
    ```
    Kui jätkad lihtsalt Enteriga, siis vaikimisi salvestatakse privaatvõti `~/.ssh/id_ed25519` ja avalik võti `~/.ssh/id_ed25519.pub`.


2. Kopeeri võti hallatavasse masinasse
    ```bash
    ssh-copy-id kasutaja@host
    ```
    Näiteks:
    ```bash
    ssh-copy-id student@192.168.1.50
    ```

3. Testi SSH ühendust
    ```bash
    ssh student@192.168.1.50
    ```
    Kui saad ilma parooli küsimata sisse, on kõik korras.


4. Lisa host *inventory* faili

    Näited:

    `ini`:
    ```ini
    [webservers]
    web1 ansible_host=192.168.1.10 ansible_user=student
    ```

    `yaml`:    
    ```yaml
    all:
      children:
        webservers:
          hosts:
            web1:
              ansible_host: 192.168.1.10
              ansible_user: student
    ```

    !!! info
        Kasutajanime peab määrama ainult siis kui see erineb control node’i kasutajanimest.

5. Kontrolli ühendust Ansible’iga

    Proovime kontrolliks Ansible abil käivitada ping moodulit:

    Näide: 
    ```bash
    ansible -i inventory/hosts.yml web1 -m ping
    ```
    või (kui inventory failile pole eraldi vaja viidata)
    ```bash
    ansible -i web1 -m ping
    ```

    Kui kõik töötab, saad tulemuseks midagi sellist ehk "ping"-ile tuleb vastuseks "pong":  
    ```json
    web1 | SUCCESS => {
      "changed": false,
      "ping": "pong"
    }
    ```

    !!! info
        Ansible ping moodul ei tee ICMP ping’i, vaid testib reaalselt mooduli täitmist Pythoniga.

---

## Windows hosti ühendamine Ansible'iga

Nagu eelnevalt mainitud, on Windowsi hosti võimalik tänapäeval ühendada Ansible'iga kas OpenSSH või WinRM kaudu. Viimastel aastatel on selgelt näha trendi, kus SSH muutub üha populaarsemaks alternatiiviks WinRM-ile.

### Windowsi ühendusmeetodite võrdlus

| Omadus / Protokoll | **WinRM** | **OpenSSH** |
|--------------------|-----------|-------------|
| **Toetus**         | Windowsi ametlik vaikimisi haldusprotokoll | Alates Windows 10 / Server 2019 ametlikult toetatud |
| **Ühendusviis**    | HTTP(S) portidel 5985 / 5986 | SSH (tavaliselt port 22) |
| **Turvalisus**     | Vajab täiendavat seadistust krüpteeritud ühenduse jaoks | Kasutab kohe turvalist SSH protokolli |
| **Kasutus Ansible'is** | `ansible_connection=winrm` | `ansible_connection=ssh` |
| **Shell**          | PowerShell | CMD või PowerShell (määratav `ansible_shell_type`) |
| **Plussid**        | Sügav Windowsi integratsioon, lai haldusvõimekus | Lihtne, Linuxiga ühtne töövoog, vähem seadistust |
| **Miinused**       | Võib olla keeruline seadistada (eriti krüpteering ja tulemüürid) | Kõik Windowsi käsud ei tööta OpenSSH shellis samamoodi kui PowerShellis |


### Windows hosti ühendamine Ansible'iga OpenSSH abil

Windowsi masinatele saab Ansible'iga ligi ka OpenSSH-serveri kaudu, mis on tänapäeval paljudes Windowsi versioonides (nt Windows 10, 11 ja Server 2019+) juba sisseehitatud või hõlpsasti paigaldatav. Järgnevalt vaatame samm-sammult selle seadistamist.

1. Ava Windowsi hostis PowerShell/Terminal administraatorina.
2. Kontrolli, kas OpenSSH Server on juba paigaldatud:
    ```powershell
    Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH.Server*'
    ```
3. Kui ei ole paigaldatud, paigalda:
    ```powershell
    Add-WindowsCapability -Online -Name OpenSSH.Server
    ```
4. Käivita ja luba teenus automaatselt:
    ```powershell
    Set-Service -Name sshd -StartupType 'Automatic'
    Start-Service sshd
    ```
5. Ava port 22 (SSH) Windowsi tulemüüris:
    ```powershell
    New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
    ```
6. Võtmetega autenimise jaoks luba ja käivita `ssh-agent` teenus:
    ```powershell
    Set-Service -Name ssh-agent -StartupType 'Automatic'
    Start-Service ssh-agent
    ```
7. Windowsis on võtmega autentimine natuke teistsugune kui Linuxis. Administrator konto puhul soovitab Microsoft kasutada `authorized_keys` jaoks spetsiaalset faili: `C:\ProgramData\ssh\administrators_authorized_keys`, mille sisse tuleb lisada Ansible CN masina avalik võti. 
    Selleks leia Ansible CN masinast üles eelnevalt avalik võti, mida soovid kasutada.

    Näiteks:
    ```bash
    cat ~/.ssh/id_ed25519.pub
    ```
    Kopeeri võti täielikult, et saaksid seda kasutada.

    !!! info
        Tavakasutaja jaoks on ka võtmeid võimalik luua kui selleks on vajadust. Siis on Windowsil kasutusel teekond `C:\Users\kasutaja\.ssh\authorized_keys`.

8.  Loo parooliga ühendus Ansible CN masinast Windows Serverisse (eeldame, et Windows Serveril kasutatakse kontot Administrator):
    ```bash
    ssh Administrator@windows_host
    ```
    Sisse logides käivita käsureal poweshell:
    ```powershell
    powershell
    ```
    Nüüd lisa Ansible CN võti õigesse kohta, käsus asenda `<avalik_ssh_võti>` kopeeritud avaliku SSH võtmega: 
    ```powershell
    Add-Content -Path C:\ProgramData\ssh\administrators_authorized_keys -Value "<avalik_ssh_võti>"
    ```
9. `administrators_authorized_keys`fail peab olema ainult **Administrators** ja **SYSTEM** kontrolli all, vastasel juhul OpenSSH keeldub seda kasutamast. Seega sea nüüd ka vajalikud õigused:
    ```powershell
    icacls.exe "C:\ProgramData\ssh\administrators_authorized_keys" /inheritance:r /grant "Administrators:F" /grant "SYSTEM:F"
    ```
10. Taaskäivita SSH teenus:
    ```powershell
    Restart-Service sshd
    ```
11. Katkesta SSH ühenduse ja proovi uuesti ühenduda:
    ```bash
    ssh Administrator@windows_host
    ```
    Kui eelnevad tegevused said korrektselt teostatud, siis toimub ühendus nüüd ilma parooli küsimata.

12. Ansible peab teadma, kuidas Windowsi hostiga ühendust võtta.

    Inventory failis tuleb kirjeldada:

    - `ansible_host` - Windowsi hosti IP
    - `ansible_user` - kasutaja, millega ühendutakse
    - `ansible_connection=ssh` - käsib Ansible’il kasutada OpenSSH-d (mitte WinRM-i)
    - `ansible_ssh_private_key_file` - ütleb, millist võtit kasutada (siin vaikimisi ~/.ssh/id_ed25519)
    - `ansible_shell_type: powershell` – ütleb Ansible'ile, et käsud tuleb käivitada PowerShellis. On vajalik, muidu Ansible eeldab CMD-d.
    - `ansible_shell_executable: powershell.exe` – määrab täpse shelli käivitusfaili, mis on vajalik mõnes Windowsi versioonis, et vältida vigu nagu command not found.

    Inventrory näited:
    
    `ini`:
    ```ini
    [winservers]
    win1 ansible_host=192.168.56.101 ansible_user=Administrator ansible_connection=ssh ansible_ssh_private_key_file=~/.ssh/id_ed25519 ansible_shell_type=powershell ansible_shell_executable=powershell.exe
    ```

    `yaml`:
    ```yaml
    all:
      children:
        winservers:
          hosts:
            win1:
              ansible_host: 192.168.56.101
              ansible_user: Administrator
              ansible_connection: ssh
              ansible_ssh_private_key_file: ~/.ssh/id_ed25519
              ansible_shell_type: powershell
              ansible_shell_executable: powershell.exe
    ```

    !!! info
        Ansible'i *inventory* muutujad soovitatakse paigutada eraldi failidesse nagu group_vars ja host_vars, et hoida konfiguratsioon puhtana, korduvkasutatavana ja skaleeritavana. Seda vaatame täpsemalt aga hiljem.

13. Kontrolli ühendust Ansible’iga

    Proovime kontrolliks Ansible abil käivitada **ping** moodulit (pane tähele, et moodul, mida kasutame on teine kui Linuxil):

    Näide: 
    ```bash
    ansible -i inventory/hosts.yml win1 -m win_ping
    ```
    või (kui inventory failile pole eraldi vaja viidata)
    ```bash
    ansible win1 -m win_ping
    ```

    Kui kõik töötab, saad "ping"-ile vastuseks "pong":  
    ```json
    win1 | SUCCESS => {
      "changed": false,
      "ping": "pong"
    }
    ```

### Windows hosti ühendamine Ansible'iga WinRM abil

Ansible toetab Windowsi haldamist **WinRM (Windows Remote Management)** kaudu, mis on PowerShell Remotingu alusprotokoll. See on Ansible’i ametlikult soovitatud meetod Windowsi masinate haldamiseks. Vaikimisi töötab WinRM krüpteerimata HTTP kaudu üle pordi 5985 – seda ei tohiks kasutada production-keskkonnas. Turvaliseks kasutamiseks tuleb seadistada HTTPS listener (port 5986) koos sertifikaadiga. Vaatamegi nüüd turvalist ühendust:

1. Luba PowerShell Remoting Windowsi hostis

    Ava PowerShell administraatorina ja käivitage:  

    ```powershell
    Enable-PSRemoting -Force
    ```

    See loob vajalikud WinRM-i listenerid ja lubab tulemüüri reeglid. 

2. Kontrolli WinRM-i olekut

    ```powershell
    winrm quickconfig
    ```

    `Do you want to Configure LocalAccountTokenFilterPolicy to grant administrative rights remotely to local users` - See küsimus ilmub, sest winrm quickconfig soovib lubada kohalikul administraatorikontol säilitada täisõigused ka kaugühenduse korral. Windowsi UAC (User Account Control) rakendab vaikimisi token filtering-it, mis eemaldab osa administraatori õigusi, kui ühendus tuleb võrgu kaudu. 

    - **Nõustu muutusega, kui sul domeeni pole veel loodud.**
    - See muudatus ei ole vajalik, kui kasutad domeenikontot (nt Active Directory), sest UAC token filtering ei mõjuta domeenikontosid.

3. Loo sertifikaat (self-signed)
    
    Production keskkonnas soovitatakse kasutada CA poolt väljastatud sertifikaati. Õppimiseks sobib self-signed sertifikaat.

    Näide sertifikaadi loomisest:

    ```powershell
    $cert = New-SelfSignedCertificate -DnsName "win1.local" -CertStoreLocation "Cert:\LocalMachine\My"
    ```

    - `DnsName` väärtuseks pane oma serveri hostname või IP (nt 192.168.56.101).
    - `Cert:\LocalMachine\My`: Sertifikaat salvestatakse masinasse sertifikaadihoidlasse (LocalMachine\My). `LocalMachine` - sertifikaat paigaldatakse arvuti tasemel (mitte ainult konkreetsele kasutajale). `My` - see on sertifikaadihoidla nimi ("Personal" store). Seal hoitakse arvuti/teenuse jaoks vajalikke isiklikke sertifikaate, mida saab kasutada nt krüpteeritud ühenduste loomiseks.

4. HTTPS listeneri loomine WinRM-ile

    Leia sertifikaadi thumbprint:
    ```powershell
    $cert.Thumbprint
    ```

    Seejärel loo uus HTTPS listener (muuda hosname selliseks nagu kasutasid sertifikaadi loomisel):
    ```powershell
    winrm create winrm/config/Listener?Address=*+Transport=HTTPS "@{Hostname=`"win1.local`"; CertificateThumbprint=`"$($cert.Thumbprint)`"}"
    ```

    Kontrolli, et listener lisandus:
    ```powershell
    winrm enumerate winrm/config/Listener
    ```    

    Nüüd kuulab WinRM porti 5986 (HTTPS).

5. Tulemüüri seadistus

    Lisa reegel, mis lubab WinRM HTTPS liikluse (port 5986):
    ```powershell
    New-NetFirewallRule -Name "WinRM-HTTPS" -DisplayName "WinRM over HTTPS" -Protocol TCP -LocalPort 5986 -Action Allow -Direction Inbound
    ```    

6. Self-signed sertifikaadi lisamine Trusted Root Certification Authorities hoidlasse

    Otsi loodud sertifikaat My store'ist:
    ```powershell
    $cert = Get-ChildItem -Path Cert:\LocalMachine\My | Where-Object { $_.DnsNameList -match "win1.local" }
    ```
    Ekspordi see failiks:
    ```powershell
    Export-Certificate -Cert $cert -FilePath C:\winrm-cert.cer
    ```
    Impordi see Trusted Root Certification Authorities alla:
    ```powershell
    Import-Certificate -FilePath C:\winrm-cert.cer -CertStoreLocation "Cert:\LocalMachine\Root"
    ```
    Testi ühendust iseendaga (localhost), et kontrollida kas listener vastab:
    ```powershell
    Test-WSMan -ComputerName "win1.local" -Port 5986 -UseSSL
    ```    

7. Ansible inventory seadistamine

    Kui WinRM on seadistatud, tuleb Ansible’is määrata õiged muutujad: 

    - `ansible_connection=winrm` – määrab, et kasutatakse WinRM-i.
    - `ansible_winrm_transport` – autentimisprotokolli, tavaliselt ntlm, kuid võib olla ka credssp või kerberos (vt. tabelit).
    - `ansible_winrm_server_cert_validation=ignore` – õppetöös mugav, sest self-signed sert ei ole CA poolt usaldatud. Production’is tuleks see panna validate ja kasutada CA sertifikaate.
    - `ansible_port=5986` – HTTPS port.
    
    Näiteks:
    
    `ini`:
    ```ini
    [winservers]
    win1 ansible_host=192.168.56.101 ansible_user=Administrator ansible_password=SalajaneParool ansible_connection=winrm ansible_winrm_transport=ntlm ansible_winrm_server_cert_validation=ignore ansible_port=5986
    ```

    `yaml`:
    ```
    all:
      children:
        winservers:
          hosts:
            win1:
              ansible_host: 192.168.56.101
              ansible_user: Administrator
              ansible_password: "SalajaneParool"
              ansible_connection: winrm
              ansible_winrm_transport: ntlm
              ansible_winrm_server_cert_validation: ignore
              ansible_port: 5986

    ```

    !!! warning
        Turvalisuse mõttes tuleks paroolid hoida **Ansible Vault** abil, mitte otse *inventory* failis. Seda käsitleme hiljem turvalisuse materjalis.  


    Autentimiprotokolli valik sõltub keskkonnast:

    | Protokoll    | Kus kasutatakse                              | Kirjeldus                                                                 |
    |--------------|----------------------------------------------|---------------------------------------------------------------------------|
    | **ntlm**     | Tavaline kohalik või domeenikonto            | Kõige levinum meetod. Lihtne seadistada, sobib enamasti labori jaoks.     |
    | **credssp**  | Kui vaja volitada mandaate edasi             | Võimaldab edastada mandaadid ka *teistele masinatele*. Vajab lisaseadistust. |
    | **kerberos** | Active Directory domeenikeskkond             | Kõige turvalisem lahendus. Kasutab domeeni SSO-d, ei pea parooli käsitsi sisestama. |


8. Kontrolli ühendust Ansible’iga

    ```bash
    ansible -i hosts.ini windows -m win_ping
    ```

    Kui kõik on korras, näed väljundit:  
    ```json
    win1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
    }
    ```

---

## Hostide lisamise head tavad

- Kasuta **SSH võtmeid** SSH ühenduste puhul hostidega – ära kasuta paroole inventuuri failides.  
- Kasuta **Ansible Vault**’i paroolide turvaliseks hoidmiseks.  
- Kontrolli alati ühendust `ping` mooduliga enne, kui jooksutad keerukamaid playbooke.  
- Hoia Linuxi ja Windowsi hostid eraldi gruppides.  
- Grupeeri hostid loogiliselt (nt webservers, dbservers, test, prod) – see lihtsustab playbookide haldamist.
- Kasuta alias-nimesid hostide jaoks, et vältida IP-aadresside kasutamist otse playbookides. See muudab konfiguratsiooni loetavamaks ja paremini hallatavaks.

---

## Harjutus

1. Loo oma inventuuri faili kaks gruppi: **linux** ja **windows**.  
2. Lisa sinna vähemalt 2 Linuxi ja 2 Windowsi hosti.  
3. Linuxi puhul seadista ühendus SSH võtmega.  
4. Windowsi puhul luba seadista esimesele serverile ühendus OpenSSH abil ja teisele WinRM-i abil.
5. Lisa vajalikud *inventory* muutujad.  
6. Kontrolli ühendust:  
   ```bash
   ansible -i hosts.ini linux -m ping
   ansible -i hosts.ini windows -m win_ping
   ```

---

## Rohkem infot

- [Connecting to Linux Hosts](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html#connecting-to-hosts){:target="_blank"}   
- [Connecting to Windows Hosts](https://docs.ansible.com/ansible/latest/user_guide/windows.html){:target="_blank"}   
- [WinRM Setup Guide](https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html){:target="_blank"}   
