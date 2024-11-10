# Ansible ülevaade

## Ansible tutvustus

Ansible on avatud lähtekoodiga ja tasuta IT-süsteemide haldamise tööriist, mis aitab automatiseerida konfiguratsioonihaldust, rakenduste juurutamist ja serverite haldamist. Ansible on "agentless" tööriist, mis tähendab, et see ei nõua sihtsüsteemidel (klientidel) eraldi agendi installimist – kõik toimub üle SSH (Linuxi puhul) või WinRM-i (Windowsi puhul).

Ansible algne arendaja ja haldaja on ettevõte Red Hat, mis omandas Ansible 2015. aastal. Kuigi Red Hat pakub ka Ansible’ile tuge ja täiendavaid tööriistu (näiteks Red Hat Ansible Automation Platform), on Ansible’i põhiversioon ja paljud selle komponendid avatud lähtekoodiga ja kõigile tasuta kättesaadavad GitHubis.

Kuna Ansible on avatud lähtekoodiga projekt, panustavad sellesse lisaks Red Hat’ile ka paljud teised arendajad ja IT-spetsialistid üle maailma. Red Hat koordineerib arendust ja haldamist, kuid kogukondlikud panused ja kasutajate tagasiside mängivad olulist rolli Ansible’i edasises arengus ja uuendustes.

Ansible’i peamised võimalused ja eelised on:

- Võimaldab korduvate ülesannete automatiseerimist, näiteks serverite seadistamist ja tarkvarauuendusi. Selle abil saab vähendada käsitsi tööd ja inimlikke vigu
- Aitab hoida süsteemid järjepidevana, rakendades samu konfiguratsioone erinevates keskkondades (nt arendus, testimine ja tarbimine).
- Selle abil saab käske ja ülesandeid samaaegselt täita sadades või tuhandetes serverites. Näiteks saab paigaldada tarkvara või muuta seadistusi kõigis masinates ühe käsuga.
- On eriti kasulik suurte ja keerukate rakenduste juurutamiseks, kuna see võimaldab määratleda ja hallata kogu infrastruktuuri "playbookide" (käsukogumite) abil.
- Ansible ülesandeid saab korduvalt käivitada ilma süsteemi muutmata, kui see juba soovitud olekus on ehk toimub kontroll, kas tegevus on juba soovitud olekus.
- Ansible playbookide kirjeldamine toimub YAML-formaadis, mis on lihtsalt loetav ja arusaadav ka neile, kellel pole palju kogemusi programmeerimisega.

## Ansible ülesehitus

Ansible’i struktuur koosneb mitmest põhikomponendist, mis töötavad koos, et võimaldada IT-automatiseerimist. Siin on peamised elemendid ja nende rollid, mida on oluline mõista Ansible õppimisel:

- **Ansible control node (CN):** see on arvuti, millelt Ansible’i käske ja playbooke käivitatakse. See on süsteemi haldamise keskus, kust administraatorid Ansible’i abil hallatavaid masinaid kontrollivad ja neile ülesandeid saadavad.
- **Managed Nodes:** need on kõik süsteemid, mida Control Node haldab. Need on masinad, kuhu Ansible saadab käske ja ülesandeid.
- **Inventory:** fail või skript, mis sisaldab kõik hallatavad masinad. Need võivad olla staatilised (lihtsad tekstifailid) või dünaamilised (skriptid, mis hangivad andmeid välistest allikatest).
- **Playbooks:** playbookid on Ansible’i skriptid, mis määravad ülesannete jada ja nende täitmise järjekorra. Need on kirjutatud YAML-formaadis ja määravad, milliseid ülesandeid tuleb teatud hostidel täita. Iga playbook koosneb "playdest", mis kirjeldavad konkreetset ülesannete kogumit, mida käivitatakse sihtmasinates.
- **Modules:** moodulid on Ansible’i põhikomponendid, mis täidavad konkreetseid toiminguid, näiteks tarkvara installimine, teenuse taaskäivitamine või failide kopeerimine. Ansible sisaldab sadu sisseehitatud mooduleid, kuid võimaldab ka luua kohandatud mooduleid vastavalt vajadusele.
- **Roles:** rollid on struktuur, mis võimaldab playbooke ja teisi faile paremini organiseerida ja koodi taaskasutada. Rollid jagavad ülesanded erinevatesse kaustadesse ja failidesse, nagu tasks, vars, handlers, templates, files. Rollide abil saab konfiguratsioone ja ülesandeid jagada ja uuesti kasutada, muutes keerukate automatiseerimisprotsesside loomise lihtsamaks.
- **Plugins:** pluginad laiendavad Ansible’i funktsionaalsust, lisades näiteks logimist, ühenduvust või muid täiendavaid töötlusvõimalusi. Ansible sisaldab sisseehitatud pluginaid, kuid toetab ka kohandatud pluginate loomist.
- **Variables (vars):** muutujad võimaldavad konfiguratsioonidesse paindlikkust lisada. Nendes saab hoida kasutajanimesid, paroole, porte või muid väärtusi, mida saab määratleda playbookis, inventuurifailis või väliste failide kaudu. Ansible võimaldab määrata muutujate väärtusi mitmetel eri tasanditel, näiteks globaalsed, rollipõhised või hostipõhised muutujad.
- **Templates:** mallid võimaldavad dünaamilisi konfiguratsioone. Need on tavaliselt kirjutatud Jinja2-süntaksis ja kohanduvad automaatselt vastavalt hostidele määratud muutujatele. Neid kasutatakse sageli konfiguratsioonifailide loomiseks ja redigeerimiseks, lisades hostidele unikaalseid parameetreid.

## Playbookide loomise head tavad

X