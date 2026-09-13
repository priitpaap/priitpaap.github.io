### `grep` ja toru

Linuxi käsureal saab ühe käsu väljundi suunata teise käsu sisendiks. Selleks kasutatakse **toru** ehk märki `|`.

```text
käsk 1 → | → käsk 2
```

Näiteks kui kataloogis on palju faile ja soovid `ls -l` väljundist leida faili, mille nimes esineb `raport`:

```bash
ls -l | grep "raport"
```

Esimene käsk:

```bash
ls -l
```

kuvab kataloogi sisu. Toru `|` suunab selle väljundi `grep`-ile, mis jätab alles ainult read, kus esineb sõna `raport`.

Näiteks võib tulemus olla:

```text
-rw-r--r-- 1 student student 2450 Sep 13 10:15 raport.txt
-rw-r--r-- 1 student student 1830 Sep 12 14:20 raport_vana.txt
```

Samal viisil saab filtreerida ka teiste käskude väljundit. Näiteks töötavate protsesside hulgast SSH-ga seotud ridade otsimiseks:

```bash
ps aux | grep "ssh"
```

Siin:

```text
ps aux → kuvab töötavad protsessid
|      → suunab väljundi järgmisele käsule
grep   → jätab alles otsingumustrile vastavad read
```

!!! tip "Toru muudab käsud kombineeritavaks"
    `grep` ei pea otsima ainult faili sisust. Seda kasutatakse väga sageli mõne teise käsu väljundi filtreerimiseks. See on Linuxi käsureal üks olulisemaid töövõtteid.

!!! note "`grep` filtreerib teksti"
    Käsus `ls -l | grep "raport"` ei otsi `grep` otse failisüsteemist. `ls -l` loob tekstilise väljundi ja `grep` otsib sellest tekstist sobivaid ridu. Failide omaduste järgi otsimiseks sobib paremini `find`.
