---
icon: material/debian
---

# OpenLDAP-i ja LDAP Account Manageri seadistamine

Juhendis kasutatav operatsioonisüsteem: **Debian 13**

Selles juhendis:

- Mis on LDAP ja kuidas keskne autentimine töötab
- Kuidas paigaldada OpenLDAP ja LDAP Account Manager (LAM)
- Kuidas luua LAM-is Linuxi kasutajaid ja gruppe
- Kuidas lubada LDAP-kasutajaga klientarvutisse sisselogimine

---

## Mis on LDAP?

**LDAP** (Lightweight Directory Access Protocol) on protokoll kataloogiteenuse kasutamiseks. Kataloogis saab hoida näiteks kasutajate ja gruppide andmeid. **OpenLDAP** on selle protokolli serveritarkvara ning **LAM** on veebiliides kontode haldamiseks.

Keskse autentimise korral saab klient kasutaja andmed LDAP-serverist ja kontrollib seal tema parooli. Sama kontot saab kasutada mitmes vastavalt seadistatud Linuxi arvutis.

### Olulised mõisted

| Mõiste | Selgitus |
|---|---|
| **Base DN** | Kataloogi juur ehk otsingubaas, näiteks `dc=firma,dc=lan`. |
| **DN** | Kirje täielik nimi kataloogis, näiteks `uid=liisa,ou=kasutajad,dc=firma,dc=lan`. |
| **OU** | Kataloogi allüksus ehk kirjete rühmitamise koht. OU ise ei ole Linuxi kasutajagrupp. |
| **LDIF** | Tekstivorming LDAP-kirjete ja seadistusmuudatuste kirjeldamiseks. |
| **SSSD** | Klienditeenus, mis vahendab Linuxile LDAP-i kasutajaandmeid ja autentimist. |

### Kasutatavad pordid

| Teenus | TCP-port | Kasutus |
|---|---:|---|
| LDAP / StartTLS | 389 | LDAP-päringud; StartTLS muudab ühenduse krüpteerituks. |
| LDAPS | 636 | LDAP kohe ühenduse algusest TLS-i sees. Selles juhendis ei seadistata. |
| LAM-i veebiliides | 80 | Veebiliidese avamine selle labori HTTP-seadistusega. |

Klient algatab ühendused serverisse. Kui kasutad tulemüüri, luba vajalikud ühendused oma laborivõrgust.

---

## Näidisvõrk

| Parameeter | Näidisväärtus |
|---|---|
| Võrk | `10.100.0.0/24` |
| LDAP-server | `10.100.0.4` |
| Serveri DNS-nimi | `ldap.firma.lan` |
| LDAP-i domeen / Base DN | `firma.lan` / `dc=firma,dc=lan` |
| Kasutajate asukoht | `ou=kasutajad,dc=firma,dc=lan` |
| Gruppide asukoht | `ou=grupid,dc=firma,dc=lan` |

**Asenda näidisväärtused ülesandes nõutud väärtustega.** Näidiskasutaja on `liisa` ja grupp `praktika`.

Serveri nimi peab nii serveris kui ka kliendis lahenema õigeks IP-aadressiks. Loo oma DNS-serveris sobiv A-kirje. Kui DNS-kirjet veel pole, lisa **mõlema masina** faili `/etc/hosts`:

```text
10.100.0.4 ldap.firma.lan ldap
```

Kontrolli nime lahendamist:

```bash
getent hosts ldap.firma.lan
```

**Seadistamise käsud käivita administraatori käsureal (`su -` või `sudo -i`), kui pole öeldud teisiti.** Serveri ja kliendi kellaaeg peab olema õige, et sertifikaatide kontroll toimiks.

---

## 1. OpenLDAP-i paigaldus ja algseadistus

**Järgmised tegevused tee LDAP-serveris.**

```bash
apt update
apt install slapd ldap-utils openssl openssh-server
dpkg-reconfigure slapd
```

Paigaldamisel küsitakse LDAP-i administraatori parooli. Ümberseadistamisel saad määrata ka domeeni:

| Küsimus | Valik selles näites |
|---|---|
| Omit OpenLDAP server configuration? | **No** |
| DNS domain name | `firma.lan` |
| Organization name | `Firma` |
| Administrator password | Vali LDAP-i administraatori parool. |
| Remove the database when slapd is purged? | **No** |
| Move old database? | **Yes**, kui küsitakse. |

Tee see algseadistus **enne kasutajate loomist**: ümberseadistamine võib senise kataloogi asendada.

```bash
systemctl enable --now slapd
systemctl status slapd
```

Kontrolli kataloogi juurt:

```bash
ldapsearch -x -LLL -H ldap://localhost -b dc=firma,dc=lan -s base dn
```

Vastuses peab olema `dn: dc=firma,dc=lan`. LDAP-i administraatori DN on selles näites **`cn=admin,dc=firma,dc=lan`**; see on LDAP-i konto, mitte Linuxi `root`.

---

## 2. Kasutajate ja gruppide allüksused

Loo fail:

```bash
nano /root/struktuur.ldif
```

Lisa järgmine sisu. **Kirjete vahele jäta tühi rida.**

```ldif
dn: ou=kasutajad,dc=firma,dc=lan
objectClass: organizationalUnit
ou: kasutajad

dn: ou=grupid,dc=firma,dc=lan
objectClass: organizationalUnit
ou: grupid
```

Lisa kirjed kataloogi:

```bash
ldapadd -x -H ldap://localhost -D cn=admin,dc=firma,dc=lan -W -f /root/struktuur.ldif
```

`-D` määrab sisselogimiseks kasutatava DN-i, `-W` küsib tema parooli ja `-f` määrab sisendfaili. Kasuta eelmises sammus valitud **LDAP-i administraatori parooli**.

---

## 3. Krüpteeritud LDAP-ühendus – StartTLS

SSSD nõuab parooliga autentimiseks krüpteeritud ühendust. Loome labori jaoks isesigneeritud sertifikaadi, mida klient hiljem usaldama seadistatakse.

```bash
mkdir -p /etc/ldap/tls
chmod 755 /etc/ldap/tls

openssl req -x509 -newkey rsa:3072 -noenc -days 365 \
  -keyout /etc/ldap/tls/ldap.key \
  -out /etc/ldap/tls/ldap.crt \
  -subj "/CN=ldap.firma.lan" \
  -addext "subjectAltName=DNS:ldap.firma.lan,IP:10.100.0.4"

chown root:openldap /etc/ldap/tls/ldap.key
chmod 640 /etc/ldap/tls/ldap.key
chmod 644 /etc/ldap/tls/ldap.crt
```

Sertifikaadi nimi ja IP peavad vastama sinu serverile. Privaatvõti peab olema loetav teenuse kasutajale `openldap`.

Loo seadistusmuudatuse fail:

```bash
nano /root/tls.ldif
```

```ldif
dn: cn=config
changetype: modify
replace: olcTLSCertificateFile
olcTLSCertificateFile: /etc/ldap/tls/ldap.crt
-
replace: olcTLSCertificateKeyFile
olcTLSCertificateKeyFile: /etc/ldap/tls/ldap.key
```

Rakenda muudatus kohaliku administraatoriühenduse kaudu:

```bash
ldapmodify -Q -Y EXTERNAL -H ldapi:/// -f /root/tls.ldif
systemctl restart slapd
```

`ldapi:///` kasutab selle masina kohalikku soklit. `-Y EXTERNAL` võimaldab administraatoril ennast kohaliku Linuxi konto kaudu tuvastada.

Kontrolli StartTLS-i, määrates testis usaldatavaks loodud sertifikaadi:

```bash
LDAPTLS_CACERT=/etc/ldap/tls/ldap.crt ldapwhoami -x -ZZ -H ldap://ldap.firma.lan
```

Oodatud vastus on **`anonymous`**. See tähendab, et krüpteeritud ühendus õnnestus; kasutajaga sisselogimist selles käsus veel ei tehtud. `-ZZ` nõuab StartTLS-i õnnestumist.

---

## 4. LDAP Account Manageri paigaldus

Paigalda LAM koos Apache'i ja PHP-ga:

```bash
apt install apache2 libapache2-mod-php ldap-account-manager
```

Kui paigaldus küsib veebiserverit, vali **apache2**; veebiaadressi aliaseks jäta **lam**.

```bash
a2enconf ldap-account-manager
apache2ctl configtest
systemctl enable --now apache2
systemctl restart apache2
```

Ava klientarvuti veebilehitsejas:

```text
http://10.100.0.4/lam/
```

!!! info "Veebiliides laboris"

    Selles juhendis kasutame LAM-i HTTP kaudu isoleeritud laborivõrgus. Päriskeskkonnas kasuta HTTPS-i. Kliendi LDAP-autentimine toimub siin juba StartTLS-iga krüpteeritult.

### LAM-i serveriprofiil

1. Ava **LAM configuration → Edit server profiles**.
2. Vali profiil **lam**. Selle algne seadistusparool on **`lam`**.
3. Määra **General settings** vaates järgmised väärtused:

    | Väli | Väärtus |
    |---|---|
    | Server address | `ldap://127.0.0.1:389` |
    | Activate TLS | Väljas: LAM ja LDAP asuvad samas serveris ning ühendus jääb kohalikuks. |
    | Login method | `Fixed list` |
    | List of valid users | `cn=admin,dc=firma,dc=lan` |

4. Määra **Account types** vaates asukohad ja kontrolli **Modules** vaates mooduleid:

    | Kontotüüp | LDAP suffix | Moodulid |
    |---|---|---|
    | Users | `ou=kasutajad,dc=firma,dc=lan` | Personal (`inetOrgPerson`), Unix (`posixAccount`), Shadow (`shadowAccount`) |
    | Groups | `ou=grupid,dc=firma,dc=lan` | Unix (`posixGroup`) |

5. **Module settings** vaates kasuta UID-de ja GID-de jaoks vahemikku **10000–19999**, et need ei kattuks labori kohalike kontodega.
6. Salvesta profiil. Muuda profiili seadistusparool ning **Edit general settings** kaudu ka üldseadistuse algne parool `lam`.
7. Naase LAM-i sisselogimislehele. Logi sisse kontoga **`cn=admin,dc=firma,dc=lan`** ja kasuta **LDAP-i administraatori parooli**.

LAM-i seadistusparool ja LDAP-i administraatori parool on eri asjade jaoks.

---

## 5. Grupi ja kasutaja loomine LAM-is

Loo esmalt **Groups → New group** kaudu Unix-grupp:

| Väli | Näidisväärtus |
|---|---|
| Group name | `praktika` |
| GID number | `10000` |

Seejärel loo **Users → New user** kaudu kasutaja. Täida isikuandmed ja **Unix** vaates järgmised väljad:

| Väli | Näidisväärtus |
|---|---|
| First name / Last name | `Liisa` / `Kask` |
| User name | `liisa` |
| UID number | `10000` |
| Primary group | `praktika` |
| Home directory | `/home/liisa` |
| Login shell | `/bin/bash` |

Määra **Set password** abil kasutajale vähemalt 10 märgi pikkune parool ja salvesta konto. Esimeseks testiks jäta parooli sundvahetus välja. LAM-i kaudu pole vaja kodukausta serverisse luua — selle loob klient esimesel sisselogimisel.

**Ära loo sama kasutajat klientarvutisse käsuga `adduser`.** Kasutajaandmed peab klient saama LDAP-ist.

---

## 6. Kliendi ettevalmistus ja sertifikaat

**Järgmised tegevused tee Debian klientarvutis.**

```bash
apt update
apt install sssd-ldap sssd-tools libnss-sss libpam-sss ldap-utils ca-certificates openssh-client
```

Kontrolli, et `ldap.firma.lan` laheneb serveri aadressiks, nagu juhendi alguses seadistasid.

Kopeeri serveri avalik sertifikaat kliendile. Asenda `student` serveris olemasoleva Linuxi kasutajaga, kellel on SSH-ligipääs:

```bash
scp student@10.100.0.4:/etc/ldap/tls/ldap.crt /usr/local/share/ca-certificates/labor-ldap.crt
update-ca-certificates
```

Kontrolli sertifikaadi usaldamist ja krüpteeritud ühendust:

```bash
ldapwhoami -x -ZZ -H ldap://ldap.firma.lan
```

Oodatud vastus on **`anonymous`**. Enne SSSD seadistamist peab see kontroll õnnestuma.

---

## 7. SSSD seadistamine

Loo fail:

```bash
nano /etc/sssd/sssd.conf
```

Lisa sisu:

```ini
[sssd]
config_file_version = 2
domains = firma.lan

[domain/firma.lan]
id_provider = ldap
auth_provider = ldap
ldap_uri = ldap://ldap.firma.lan
ldap_search_base = dc=firma,dc=lan
ldap_schema = rfc2307
ldap_id_use_start_tls = true
ldap_tls_cacert = /etc/ssl/certs/ca-certificates.crt
ldap_tls_reqcert = demand
```

| Parameeter | Selgitus |
|---|---|
| `domains` ja `[domain/...]` | SSSD seadistatava domeeni nimi; need peavad omavahel kattuma. |
| `id_provider` / `auth_provider` | Kasutajaandmed ja parooli kontroll saadakse LDAP-ist. |
| `ldap_uri` | LDAP-serveri aadress. |
| `ldap_search_base` | Kust kataloogis kasutajaid ja gruppe otsida. |
| `ldap_schema` | Unix-kontode ja gruppide andmevorming; sobib eespool valitud LAM-i moodulitega. |
| `ldap_id_use_start_tls` | Krüpteerib ka kasutajaandmete otsingud. |
| `ldap_tls_cacert` | Usaldatud sertifikaatide fail. |
| `ldap_tls_reqcert = demand` | Nõuab serveri sertifikaadi kontrolli õnnestumist. |

Määra faili omanik ja õigused ning kontrolli seadistust:

```bash
chown root:root /etc/sssd/sssd.conf
chmod 600 /etc/sssd/sssd.conf
sssctl config-check
```

Kontrolli failis `/etc/nsswitch.conf`, et **`passwd` ja `group` ridadel oleks `sss`**. Säilita nende ridade muud olemasolevad allikad. Näiteks:

```text
passwd: files systemd sss
group:  files systemd sss
```

Luba PAM-is SSSD ja kodukausta automaatne loomine:

```bash
pam-auth-update --enable sss mkhomedir
systemctl enable --now sssd
systemctl restart sssd
systemctl status sssd
```

`libnss-sss` teeb LDAP-kasutaja Linuxile nähtavaks. `libpam-sss` vahendab sisselogimise paroolikontrolli. `mkhomedir` loob kasutaja kodukausta **selles klientarvutis** esimesel sisselogimisel.

---

## 8. Kasutaja ja sisselogimise kontroll

Kontrolli klientarvutis:

```bash
getent passwd liisa
id liisa
```

Näha peavad olema LDAP-is määratud UID, grupp, kodukaust ja käsukest. Pelgalt `id` õnnestumine ei tõesta veel parooliga autentimist.

Kontrolli kasutaja LDAP-parooli:

```bash
ldapwhoami -x -ZZ -H ldap://ldap.firma.lan -D uid=liisa,ou=kasutajad,dc=firma,dc=lan -W
```

Sisesta **Liisa parool**. Vastuses peab olema tema DN.

Nüüd ava **klientarvuti tavalise kohaliku kasutaja** terminal ning proovi:

```bash
su - liisa
```

Sisesta LDAP-is määratud parool. Ära tee seda kontrolli `root`-kasutajana, sest tema saab kasutajat vahetada parooli küsimata.

Pärast sisselogimist kontrolli:

```bash
whoami
id
pwd
ls -ld /home/liisa
touch /home/liisa/kontroll.txt
```

Oodatud kasutaja on `liisa`, kodukaust `/home/liisa` ning testfaili loomine peab õnnestuma. Proovi sisse logida ka klientarvuti sisselogimisaknast või tekstikonsoolist. Kui kasutajat loendis ei näidata, sisesta tema nimi käsitsi.

!!! info "Kodukaust ei ole võrguketas"

    Keskne konto ei jaga automaatselt kasutaja faile arvutite vahel. Siin loodud `/home/liisa` asub klientarvuti kohalikul kettal.

---

## Kui midagi ei tööta

| Sümptom | Mida kontrollida? |
|---|---|
| LAM ei avane | Apache'i teenus, aadress `/lam/`, TCP-port 80. |
| LAM-i sisselogimine ebaõnnestub | Kas kasutad LDAP-i administraatori DN-i ja parooli, mitte LAM-i seadistusparooli? |
| StartTLS ebaõnnestub | Serveri nimi, kellaaeg, sertifikaadi usaldamine ning serveri võtme failiõigused. |
| `id liisa` ei leia kasutajat | SSSD teenus, Base DN, `nsswitch.conf` ning kasutaja Unix-mooduli UID ja GID. |
| Kasutaja leitakse, aga sisse ei saa | Kasutaja parool, konto lukustus/aegumine ja PAM-i SSSD tugi. |
| Kodukausta ei teki | Kas `mkhomedir` on lubatud ja kodukausta asukoht LAM-is õige? |

Logid vastavas masinas:

```bash
# LDAP-serveris
journalctl -u slapd -n 30 --no-pager

# Klientarvutis
journalctl -u sssd -n 30 --no-pager
```

---

## Rohkem infot

- [OpenLDAP-i dokumentatsioon](https://www.openldap.org/doc/admin26/)
- [LAM-i serveriprofiilid](https://www.ldap-account-manager.org/static/doc/manual/ch03s02.html)
- [Debian 13: SSSD LDAP-seadistus](https://manpages.debian.org/trixie/sssd-ldap/sssd-ldap.5.en.html)
- [Debian 13: LAM-i pakett](https://packages.debian.org/trixie/ldap-account-manager)

---

*Õppematerjali koostaja: Priit Paap, 2026*
