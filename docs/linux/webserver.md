---
icon: material/debian
---

# Veebiserver ja WordPress LAMP-platvormil

Juhendis kasutatav operatsioonisüsteem: **Debian 13**  
Veebilehe aadress: **`http://www.firma.lan`**

Selles juhendis:

- Mis on LAMP ja kuidas veebirakendus töötab
- Kuidas paigaldada Apache, MariaDB ja PHP
- Kuidas luua WordPressile andmebaas ja kasutaja
- Kuidas seadistada veebileht ning paigaldada WordPress
- Kuidas kontrollida lahenduse toimimist

!!! info "Labori ulatus"

    Seadistame ühe veebilehe kooli laborivõrgus, kasutades HTTP-d.
    HTTP-liiklus on krüptimata. HTTPS-i ja keerukamaid veebiserveri seadistusi
    käsitleme veebiserverite moodulis.

---

## Mis on LAMP?

**LAMP** on veebirakenduse käitamiseks kasutatavate tarkvarakomponentide kooslus.

| Osa | Selles laboris | Ülesanne |
|---|---|---|
| **L** – Linux | Debian 13 | Operatsioonisüsteem. |
| **A** – Apache | Apache HTTP Server | Võtab vastu brauseri HTTP-päringuid ja saadab vastused. |
| **M** – MySQL või MariaDB | MariaDB | Hoiab rakenduse andmeid andmebaasis. |
| **P** – PHP | PHP | Käivitab serveris rakenduse programmikoodi. |

**WordPress** on PHP-s kirjutatud sisuhaldussüsteem. Brauseri päring jõuab Apache'ile, PHP käivitab WordPressi koodi ning WordPress loeb vajaduse korral andmeid MariaDB-st. Brauser saab vastuseks valmis veebilehe, mitte PHP lähtekoodi.

### Kasutatavad pordid

| Teenus | Port | Kasutus |
|---|---|---|
| Apache HTTP | TCP 80 | Brauseri ühendus veebiserveriga. Tulemüür peab seda klientidele lubama. |
| MariaDB | TCP 3306 | Andmebaasi võrguühendused. Selles laboris kasutab WordPress samas masinas olevat andmebaasi kohaliku Unixi sokli kaudu. |

MariaDB porti pole klientidele vaja avada. Brauser suhtleb Apache'iga, WordPress suhtleb andmebaasiga serveri sees.

---

## Eeldused ja näidisvõrk

- Käivita serveri halduskäsud **root-kasutajana** või lisa nende ette `sudo`.
- Veebiserveril peab olema püsiv IP-aadress ning ligipääs internetile pakettide ja WordPressi allalaadimiseks.
- Kliendi brauser peab pääsema veebiserveri TCP-pordile 80.
- Nimega avamiseks peab klient kasutama sinu sisevõrgu DNS-serverit.

| Parameeter | Näidisväärtus |
|---|---|
| Veebiserveri aadress | `10.100.0.2` |
| Veebilehe nimi | `www.firma.lan` |
| WordPressi failid | `/var/www/wordpress` |
| Andmebaasi nimi | `wordpress` |
| Andmebaasi kasutaja | `wpuser` |

Apache, MariaDB ja PHP paigaldatakse samasse masinasse. See võib olla ka varasemates laborites kasutatud DNS- ja DHCP-server. Muuda näidisväärtused vastavalt oma võrgule.

---

## 1. LAMP-komponentide paigaldamine

```bash
apt update
apt install apache2 mariadb-server libapache2-mod-php php-cli php-mysql \
    php-curl php-gd php-mbstring php-xml php-zip php-intl curl ca-certificates
```

| Pakett | Kasutus |
|---|---|
| `apache2` | Veebiserver. |
| `mariadb-server` | Andmebaasiserver. |
| `libapache2-mod-php` | PHP-koodi käivitamine Apache'i moodulina. |
| `php-cli` | PHP käsureatööriist. |
| `php-mysql` | PHP ühendus MariaDB või MySQL-i andmebaasiga. |
| Ülejäänud `php-*` paketid | WordPressi pildi-, teksti-, arhiivi- ja muude toimingute laiendused. |
| `curl`, `ca-certificates` | WordPressi allalaadimine ametlikult veebilehelt. |

Käivita teenused ja luba automaatkäivitus:

```bash
systemctl enable --now apache2 mariadb
systemctl restart apache2
systemctl status apache2 mariadb --no-pager
```

Mõlemad teenused peavad olema olekus **active (running)**. Ava kliendi brauseris **`http://10.100.0.2`**. Näha peab olema Apache'i vaikimisi avaleht.

---

## 2. PHP töö kontrollimine

Loo ajutine testfail:

```bash
nano /var/www/html/test.php
```

Lisa sisu:

```php
<?php
echo 'PHP töötab';
```

Ava brauseris **`http://10.100.0.2/test.php`**. Vastuseks peab tulema **PHP töötab**.

Kui brauser kuvab PHP lähtekoodi või pakub faili allalaadimist, kontrolli PHP mooduli paigaldamist ja Apache'i taaskäivitamist. Jätka pärast PHP töölesaamist.

Eemalda testfail:

```bash
rm /var/www/html/test.php
```

---

## 3. WordPressi andmebaasi loomine

Ava MariaDB käsurida Linuxi root-kasutajana:

```bash
mariadb
```

Kui töötad tavakasutajana, kasuta `sudo mariadb`. Debiani värskes paigalduses saab Linuxi root-kasutaja MariaDB-sse tavaliselt kohaliku sokli kaudu eraldi andmebaasiparoolita.

Käivita MariaDB käsureal järgmised SQL-käsud. **Asenda `ASENDA_OMA_PAROOLIGA` enda valitud parooliga ja jäta see meelde.**

```sql
CREATE DATABASE wordpress CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'ASENDA_OMA_PAROOLIGA';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
EXIT;
```

`wpuser` saab õigused ainult `wordpress` andmebaasile. `localhost` tähendab siin, et kasutaja ühendub samast masinast. WordPress ei kasuta andmebaasiga suhtlemiseks root-kontot.

---

## 4. WordPressi failide paigaldamine

Laadi WordPress alla ametlikult veebilehelt:

```bash
curl -fL https://wordpress.org/latest.tar.gz -o /tmp/wordpress.tar.gz
```

Eduka allalaadimise järel paki arhiiv veebifailide kataloogi:

```bash
tar -xzf /tmp/wordpress.tar.gz -C /var/www/
```

Tulemuseks tekib `/var/www/wordpress`. Kontrolli:

```bash
ls /var/www/wordpress
```

Näha peavad olema näiteks `index.php`, `wp-admin`, `wp-content` ja `wp-includes`.

Määra selle labori WordPressi failide omanikuks Apache'i kasutaja:

```bash
chown -R www-data:www-data /var/www/wordpress
```

Nii saab WordPress selles ühe rakendusega laboris luua oma seadistusfaili, salvestada pilte ja paigaldada uuendusi. Failiõiguste täpsemat piiramist käsitleme veebiserverite moodulis.

---

## 5. Veebilehe nime lisamine DNS-i

Tee see samm **DNS-serveris**. Lisa `firma.lan` päriteisenduse tsoonifaili A-kirje:

```text
www    IN    A    10.100.0.2
```

Suurenda tsooni serial-numbrit. Kui kasutasid eelneva DNS-juhendi failiteed, kontrolli tsooni:

```bash
named-checkzone firma.lan /var/lib/bind/db.forward.firma.lan
```

Eduka kontrolli järel laadi DNS-seadistus uuesti:

```bash
systemctl reload named
```

Kontrolli **kliendist**, et tema tavapärane DNS-seadistus nime lahendab:

```bash
dig www.firma.lan +short
```

Vastuseks peab olema `10.100.0.2`. Veendu, et kliendile antud DNS-server on sinu sisevõrgu DNS, mitte avalik `1.1.1.1`.

---

## 6. Apache'i veebilehe seadistamine

Jätka **veebiserveris**. Loo veebilehe seadistusfail:

```bash
nano /etc/apache2/sites-available/wordpress.conf
```

Lisa järgmine sisu:

```apache
<VirtualHost *:80>
    ServerName www.firma.lan
    DocumentRoot /var/www/wordpress

    <Directory /var/www/wordpress>
        Options FollowSymLinks
        AllowOverride FileInfo
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/wordpress-error.log
    CustomLog ${APACHE_LOG_DIR}/wordpress-access.log combined
</VirtualHost>
```

| Parameeter | Selgitus |
|---|---|
| `<VirtualHost *:80>` | Selle veebilehe seadistus HTTP-pordil 80. |
| `ServerName` | Nimi, millega veebilehte avatakse. See rida ise DNS-kirjet ei loo. |
| `DocumentRoot` | Veebilehe failide asukoht. |
| `Options FollowSymLinks` | Lubab sümbollinkide järgimise, mida vajab siin ka Apache'i URL-ide ümberkirjutamine. |
| `AllowOverride FileInfo` | Lubab `.htaccess` failis URL-ide ümberkirjutamise reegleid, mida WordPress kasutab püsiviidete jaoks. |
| `Require all granted` | Lubab klientidel selle kataloogi veebisisule ligi pääseda. |
| `ErrorLog`, `CustomLog` | Selle veebilehe vea- ja juurdepääsulogi. |

Luba URL-ide ümberkirjutamise moodul ja uus veebileht ning keela Apache'i vaikimisi leht:

```bash
a2enmod rewrite
a2ensite wordpress.conf
a2dissite 000-default.conf
apache2ctl configtest
```

Kui kontrolli lõpus on **`Syntax OK`**, taaskäivita Apache:

```bash
systemctl restart apache2
```

---

## 7. WordPressi paigaldamine brauseris

Ava kliendi brauseris **`http://www.firma.lan`** ja vali paigalduse keel.

!!! info "Kasuta kohe õiget veebiaadressi"

    Ava WordPressi paigaldus nimega `www.firma.lan`. WordPress salvestab kasutatava
    veebiaadressi oma seadetesse. IP-aadressiga paigaldamine ja hiljem nimele üleminek
    vajab aadressi muutmist WordPressi seadetes.

Sisesta andmebaasi ühenduse andmed:

| Väli | Väärtus |
|---|---|
| Andmebaasi nimi | `wordpress` |
| Kasutajanimi | `wpuser` |
| Parool | Eelmises etapis loodud **andmebaasikasutaja parool**. |
| Andmebaasi server | `localhost` |
| Tabelite eesliide | Jäta vaikimisi `wp_`. |

Jätka paigaldusega. Määra veebilehe pealkiri ning loo **WordPressi administraatori kasutaja**, parool ja e-posti aadress. See on veebilehe halduskonto, mis erineb MariaDB kasutajast `wpuser`.

Pärast paigaldamist logi haldusliidesesse:

**`http://www.firma.lan/wp-admin/`**

---

## 8. Tulemuse kontrollimine

1. Loo WordPressis uus leht pealkirjaga **Minu laboriveeb** ja avalda see.
2. Lisa lehele pilt meediateegist või laadi üks pilt üles.
3. Vali seadetes **Püsiviited / Permalinks → Postituse nimi / Post name** ja salvesta.
4. Ava avaldatud leht kliendi brauseris. Kontrolli, et sisu ja pilt kuvatakse ning püsiviide töötab.
5. Logi haldusliidesest välja ja kontrolli lehe avalikku vaadet.

### Kui midagi ei tööta

| Probleem | Mida kontrollida? |
|---|---|
| Nimi `www.firma.lan` ei lahene | Kliendi DNS-server ja `www` A-kirje. |
| Kuvatakse Apache'i vaikimisi leht | `a2ensite`, `a2dissite`, õige veebiserveri aadress ja Apache'i taaskäivitus. |
| Andmebaasiühenduse viga | MariaDB töö, andmebaasi nimi, `wpuser` parool ja õigused. |
| Püsiviide annab 404-vea | `rewrite` moodul, `AllowOverride FileInfo` ning püsiviidete uuesti salvestamine. |
| Pilti ei saa üles laadida | Kataloogi `/var/www/wordpress/wp-content` omaniku- ja kirjutamisõigused. |

Apache'i teenuse logi:

```bash
journalctl -u apache2 -b -n 30 --no-pager
```

WordPressi veebilehe vealogi:

```bash
tail -n 30 /var/log/apache2/wordpress-error.log
```

---

## Rohkem infot

- [Apache'i virtuaalhostid](https://httpd.apache.org/docs/2.4/vhosts/name-based.html)
- [WordPressi paigaldusjuhend](https://developer.wordpress.org/advanced-administration/before-install/howto-install/)
- [WordPressi serverinõuded](https://wordpress.org/about/requirements/)
- [WordPressi PHP-laiendused](https://make.wordpress.org/hosting/handbook/server-environment/)
- [MariaDB kasutaja loomine](https://mariadb.com/docs/server/reference/sql-statements/account-management-sql-statements/create-user)

---

*Õppematerjali koostaja: Priit Paap, 2026*
