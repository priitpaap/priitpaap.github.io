# Ansible ülevaade

Ansible on avatud lähtekoodiga ja tasuta IT-süsteemide haldamise tööriist, mis aitab automatiseerida konfiguratsiooni haldust, rakenduste juurutamist ja serverite haldamist. See on "agentless" tööriist, mis tähendab, et see ei nõua sihtsüsteemidel eraldi agendi installimist – kõik toimub üle SSH (Linuxi puhul) või WinRM-i (Windowsi puhul).

Ansible algne arendaja ja haldaja on ettevõte Red Hat, mis omandas Ansible 2015. aastal. Kuigi Red Hat pakub ka Ansible’ile tuge ja täiendavaid tööriistu (näiteks Red Hat Ansible Automation Platform), on Ansible’i põhiversioon ja paljud selle komponendid avatud lähtekoodiga ja kõigile tasuta kättesaadavad GitHubis.

Kuna Ansible on avatud lähtekoodiga projekt, panustavad sellesse lisaks Red Hat’ile ka paljud teised arendajad ja IT-spetsialistid üle maailma. Red Hat koordineerib arendust ja haldamist, kuid kogukondlikud panused ja kasutajate tagasiside mängivad olulist rolli Ansible’i edasises arengus ja uuendustes.

Ansible’i peamised võimalused ja eelised on:

- Võimaldab korduvate ülesannete automatiseerimist, näiteks serverite seadistamist ja tarkvarauuendusi. Selle abil saab vähendada käsitsi tööd ja inimlikke vigu
- Aitab hoida süsteemid järjepidevana, rakendades samu konfiguratsioone erinevates keskkondades (nt arendus, testimine ja tarbimine).
- Selle abil saab käske ja ülesandeid samaaegselt täita sadades või tuhandetes serverites. Näiteks saab paigaldada tarkvara või muuta seadistusi kõigis masinates ühe käsuga.
- On eriti kasulik suurte ja keerukate rakenduste juurutamiseks, kuna see võimaldab määratleda ja hallata kogu infrastruktuuri "playbookide" (käsukogumite) abil.
- Ansible ülesandeid saab korduvalt käivitada ilma süsteemi muutmata, kui see juba soovitud olekus on ehk toimub kontroll, kas tegevus on juba soovitud olekus.
- Ansible playbookide kirjeldamine toimub YAML-formaadis, mis on lihtsalt loetav ja arusaadav ka neile, kellel pole palju kogemusi programmeerimisega.

