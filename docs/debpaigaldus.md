icon:material/debian

# Debian Linuxi paigaldamine

##  Riistvara nõuded

Debian Linuxi puhul saab paigaldada ühest iso failist operatsioonisüsteemi nii graafilise töölauaga (GUI) kui ilma (CLI). Järgnevalt on toodud soovitatavad parameetrid virutaalmasinal (VM) ülesannete sooritamiseks.

Graafilise keskkonnaga VM-i jaoks on vajalikud kõrgemad ressursid, kuna kasutajaliides ja GUI-rakendused nõuavad rohkem mälu ja protsessorijõudlust.

Soovitavad parameetrid GUI puhul:
  
- Protsessor (CPU): Vähemalt 2 virtuaalset protsessorit
- Mälu (RAM): Vähemalt 4 GB
- Kõvaketas: Vähemalt 16 GB
- Graafikamälu Vähemalt 16MB suurte monitoride jaoks
- Soovitav kasutada Xfce töölauda, sest see on vähem ressursinõudlikum ja tööta seetõttu VM-il sujuvamalt.

Ilma graafilise töölauata VM-i jaoks on vaja vähem mälu ja töötlemisvõimsust, sest töö toimib ainult CLI-režiimis.

Soovitavad parameetrid CLI puhul:

- Protsessor (CPU): Vähemalt 1 virtuaalset protsessor
- Mälu (RAM): Vähemalt 2 GB
- Kõvaketas: Vähemalt 16 GB

=== "GUI puhul"

    Content for Tab 1

=== "CLI puhul"

    Content for Tab 2





## Alamteema

!!! warning

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

Siin on `/var/lob/somefolder` kood

Nüüd tuleb pikem koodirida:

```
sudo -i
apt update
// See on kommentaar
# some other comment
```

Code for specific language:

``` ps1
Get-Childitem
echo kala

```


### Kolmanda taseme teema