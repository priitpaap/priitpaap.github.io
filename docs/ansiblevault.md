# Turvalisus ja Ansible Vault

YL1 setup
```bash
#!/bin/bash
# setup.sh - ettevalmistusskript Linuxi käsurea harjutuse jaoks
# Käivita root kasutajana (nt sudo ./setup.sh)

set -e

STUDENT_HOME="/home/student"
MARKER="[SETUP]"

echo ">>> Alustan harjutuse ettevalmistust..."

# --- Kontroll, et kasutaja student eksisteeriks ---
if ! id -u student >/dev/null 2>&1; then
    echo "Kasutajat 'student' ei eksisteeri! Loo see enne harjutust."
    exit 1
fi

# --- 1. Loo vajalik grupp juhusliku nime ja GID-ga (5000–6000) ---
RANDOM_GID=$((5000 + RANDOM % 1001))
GROUP_NAME="salajane_$RANDOM_GID"

if ! getent group "$GROUP_NAME" >/dev/null; then
    groupadd -g "$RANDOM_GID" "$GROUP_NAME"
    echo "Loodi grupp '$GROUP_NAME' GID=$RANDOM_GID"
else
    echo "Grupp '$GROUP_NAME' juba olemas, käivita uuesti."
fi


# --- 2. /var/skriptid: loo 25 skripti ---
mkdir -p /var/skriptid
for i in {1..25}; do
  echo '#!/bin/bash' > /var/skriptid/script$i.sh
  echo "echo \"$MARKER Tere, see on skript $i!\"" >> /var/skriptid/script$i.sh
  chmod +x /var/skriptid/script$i.sh
done

# --- 3. /var/vanalogi.txt ---
echo "$MARKER See on vana logifail" > /var/vanalogi.txt

# --- 4. /srv/ohoo: vähemalt 20 faili ---
mkdir -p /srv/ohoo
for i in {1..20}; do
  echo "$MARKER Ohoo fail $i" > /srv/ohoo/file$i.txt
done

# --- 5. /srv/somefiles/somefolder: vähemalt 20 faili ---
mkdir -p /srv/somefiles/somefolder
for i in {1..20}; do
  echo "$MARKER Somefolderi fail $i" > /srv/somefiles/somefolder/doc$i.txt
done

# --- 6. /srv/filemess: 100 faili (50 .txt + 50 muud laiendid) ---
mkdir -p /srv/filemess

# 50 .txt faili
for i in {1..50}; do
  echo "$MARKER See on kustutatav .txt fail $i" > /srv/filemess/delete$i.txt
done

# 20 .log faili
for i in {1..20}; do
  echo "$MARKER See on logifail $i" > /srv/filemess/log$i.log
done

# 20 .conf faili
for i in {1..20}; do
  echo "$MARKER See on conf fail $i" > /srv/filemess/conf$i.conf
done

# 10 .dat faili
for i in {1..10}; do
  echo "$MARKER See on dat fail $i" > /srv/filemess/data$i.dat
done

# --- 7. Kodukaust: vajalikud failid ---
su - student -c "echo '$MARKER See on data1' > ${STUDENT_HOME}/data1.txt"
su - student -c "echo '$MARKER See on rämps' > ${STUDENT_HOME}/junk"
su - student -c "rm -f ${STUDENT_HOME}/ajalugu.txt"

echo ">>> Ettevalmistus tehtud! Saad nüüd harjutusega alustada."
```

check.sh
```bash
#!/bin/bash
# check.sh - kontrollib õppija tegevusi Linuxi käsurea harjutuses
# Käivita root kasutajana (nt sudo ./check.sh)

STUDENT_HOME="/home/student"
MARKER="[SETUP]"
TOTAL=0
SCORE=0

ok()  { echo "✅ $1"; SCORE=$((SCORE+1)); TOTAL=$((TOTAL+1)); }
fail(){ echo "❌ $1"; TOTAL=$((TOTAL+1)); }

echo ">>> Alustan kontrolli..."

# 1. Kontrolli, kas ajalugu pole puhastatud (ajalugu.txt peab sisaldama käskusid hiljem)
if [ -f "$STUDENT_HOME/ajalugu.txt" ]; then
    if grep -q "load average" "$STUDENT_HOME/ajalugu.txt"; then
        ok "Fail ajalugu.txt sisaldab uptime käske"
    else
        fail "Fail ajalugu.txt puudub või ei sisalda uptime käske"
    fi
else
    fail "Fail ajalugu.txt puudub"
fi

# 2. Kontrolli kausta ajutine olemasolu ja alamkaustad
if [ -d "$STUDENT_HOME/ajutine/failid1" ] && \
   [ -d "$STUDENT_HOME/ajutine/failid2" ] && \
   [ -d "$STUDENT_HOME/ajutine/failid3" ]; then
    ok "Kaust ajutine koos alamkaustadega on olemas"
else
    fail "Kaust ajutine või alamkaustad puuduvad"
fi

# 3. Kontrolli kausta september koos 30 alamkaustaga
if [ -d "$STUDENT_HOME/september" ]; then
    count=$(ls -1 "$STUDENT_HOME/september" | grep -c '^sept[0-9]\{1,2\}$')
    if [ "$count" -eq 30 ]; then
        ok "Kaust september sisaldab 30 alamkausta"
    else
        fail "Kaust september ei sisalda täpselt 30 alamkausta (leiti $count)"
    fi
else
    fail "Kaust september puudub"
fi

# 4. Kontrolli kausta "olulised failid" olemasolu
if [ -d "$STUDENT_HOME/olulised failid" ]; then
    ok "Kaust 'olulised failid' on olemas"
else
    fail "Kaust 'olulised failid' puudub"
fi

# 5. Kontrolli faile andmed1 ja andmed2
if [ -f "$STUDENT_HOME/andmed1" ] && [ -f "$STUDENT_HOME/andmed2" ]; then
    ok "Failid andmed1 ja andmed2 olemas"
else
    fail "Failid andmed1 ja/või andmed2 puuduvad"
fi

# 6. Kontrolli, kas /var/vanalogi.txt kopeeriti ajutine/failid1 alla
if [ -f "$STUDENT_HOME/ajutine/failid1/vanalogi.txt" ]; then
    if grep -q "$MARKER" "$STUDENT_HOME/ajutine/failid1/vanalogi.txt"; then
        ok "Fail vanalogi.txt kopeeriti õigesti"
    else
        fail "Fail vanalogi.txt kopeeriti, aga marker puudub"
    fi
else
    fail "Fail vanalogi.txt puudub ajutine/failid1 kaustas"
fi

# 7. Kontrolli, kas data1.txt on ümber nimetatud ajutised_andmed
if [ -f "$STUDENT_HOME/ajutised_andmed" ]; then
    if head -n1 "$STUDENT_HOME/ajutised_andmed" | grep -qv "$MARKER"; then
        ok "Fail ajutised_andmed olemas ja õppija on midagi kirjutanud"
    else
        fail "Fail ajutised_andmed on alles, aga sisu võib olla vale"
    fi
else
    fail "Fail ajutised_andmed puudub"
fi

# 8. Kontrolli, kas /etc/group kopeeriti ajutine/failid2 alla
if [ -f "$STUDENT_HOME/ajutine/failid2/group" ]; then
    ok "Fail group kopeeriti õigesti"
else
    fail "Fail group puudub ajutine/failid2 kaustas"
fi

# 9. Kontrolli faili salajane.txt olemasolu
if [ -f "$STUDENT_HOME/salajane.txt" ]; then
    ok "Fail salajane.txt olemas"
else
    fail "Fail salajane.txt puudub"
fi

# 10. Kontrolli, kas /var/skriptid kaust kopeeriti ajutine/failid3 alla
if [ -d "$STUDENT_HOME/ajutine/failid3/skriptid" ]; then
    count=$(ls -1 "$STUDENT_HOME/ajutine/failid3/skriptid" | wc -l)
    if [ "$count" -ge 25 ]; then
        ok "Kaust skriptid kopeeriti õigesti ($count faili)"
    else
        fail "Kaust skriptid kopeeriti, aga failide arv vale ($count)"
    fi
else
    fail "Kaust skriptid puudub ajutine/failid3 all"
fi

# 11. Kontrolli, kas kaust ohoo liigutas ajutine4 nime alla
if [ -d "$STUDENT_HOME/ajutine/ajutine4" ]; then
    ok "Kaust ohoo liigutas õigesti ajutine4 nime alla"
else
    fail "Kaust ohoo puudub või ei ole nimega ajutine4"
fi

# 12. Kontrolli, kas kaust /srv/somefiles/somefolder kustutati
if [ ! -d "/srv/somefiles/somefolder" ]; then
    ok "Kaust somefolder on kustutatud"
else
    fail "Kaust somefolder ikka olemas"
fi

# 13. Kontrolli, kas /srv/filemess kaustas on ainult mitte-.txt failid alles
txtcount=$(ls /srv/filemess/*.txt 2>/dev/null | wc -l)
if [ "$txtcount" -eq 0 ]; then
    ok "Kõik .txt failid kustutati kaustast /srv/filemess"
else
    fail "Kaustas /srv/filemess on veel $txtcount .txt faili alles"
fi

# 14. Kontrolli, kas junk fail kustutati
if [ ! -f "$STUDENT_HOME/junk" ]; then
    ok "Fail junk kustutati"
else
    fail "Fail junk ikka alles"
fi

# 15. Kontrolli viimased.txt ja esimesed.txt
if [ -f "$STUDENT_HOME/viimased.txt" ]; then
    if [ "$(wc -l < $STUDENT_HOME/viimased.txt)" -eq 5 ]; then
        ok "Fail viimased.txt olemas ja sisaldab 5 rida"
    else
        fail "Fail viimased.txt olemas, aga ridade arv vale"
    fi
else
    fail "Fail viimased.txt puudub"
fi

if [ -f "$STUDENT_HOME/esimesed.txt" ]; then
    if [ "$(wc -l < $STUDENT_HOME/esimesed.txt)" -eq 5 ]; then
        ok "Fail esimesed.txt olemas ja sisaldab 5 rida"
    else
        fail "Fail esimesed.txt olemas, aga ridade arv vale"
    fi
else
    fail "Fail esimesed.txt puudub"
fi

# --- Kokkuvõte ---
echo ">>> Kontroll valmis."
echo "Tulemused: $SCORE / $TOTAL õiget kontrollpunkti."
```