# Turvalisus ja Ansible Vault

## Eesmärk

Selles peatükis õpid:

- Mis on **Ansible Vault** ja milleks seda kasutatakse  
- Kuidas **krüpteerida** tundlikke faile ja muutujaid  
- Kuidas **avatud**, **redigeerida** ja **dekrüpteerida** Vault-faile  
- Kuidas kasutada **Vault-parooli** automaatselt  
- Kuidas **integreerida Vault** playbookidesse ja varafailidesse

---

## Mis on Ansible Vault?

**Ansible Vault** võimaldab kaitsta tundlikku teavet – näiteks paroole, API võtmeid, sertifikaate ja muid konfidentsiaalseid andmeid – krüpteerides need.  

Vaulti kasutamine on oluline, sest:
- Playbookid ja muutujad jagatakse tihti meeskonnas või versioonihalduses (nt GitHub).  
- Ilma krüpteerimiseta võivad paroolid ja võtmed sattuda avalikult kättesaadavaks.  

Vault kasutab **symmeetrilist AES-krüpteerimist** ja on täielikult integreeritud Ansible töövoogu.

---

## Vaulti loomine

Vaultiga kaitstud faili saab luua käsuga:

```bash
ansible-vault create secrets.yml
```

See avab vaikimisi redaktori (nt `vim`), kuhu saad sisestada oma tundlikud andmed:

```yaml
db_user: admin
db_password: salajaneParool123
api_token: 4bfa02d7...
```

Fail salvestatakse krüpteerituna:

```yaml
$ANSIBLE_VAULT;1.1;AES256
643939313566366534653433396338663266633764336135393265...
```

💡 **Näpunäide:**  
Kui soovid krüpteerida olemasoleva faili, kasuta:
```bash
ansible-vault encrypt secrets.yml
```

---

## Vault-faili avamine ja muutmine

Krüpteeritud faili saab avada või redigeerida ainult Vault-parooliga.

- **Avamine (ilma muutmata):**
  ```bash
  ansible-vault view secrets.yml
  ```

- **Muutmine:**
  ```bash
  ansible-vault edit secrets.yml
  ```

- **Dekrüpteerimine (tavalisse teksti):**
  ```bash
  ansible-vault decrypt secrets.yml
  ```

⚠️ *Dekrüpteerimine eemaldab kaitse – kasuta seda ainult vajadusel!*

---

## Vault-faili kasutamine playbookis

Vault-faile kasutatakse tavaliselt muutujaid sisaldavate failidena, mida Ansible loeb `vars_files:` kaudu.

### Näide

```yaml
---
- name: Kasuta Vaultis hoitavaid muutujaid
  hosts: dbservers
  become: yes
  vars_files:
    - secrets.yml

  tasks:
    - name: Näita andmebaasi kasutajat (testiks)
      debug:
        msg: "DB kasutaja on {{ db_user }}"
```

Playbooki käivitamisel küsitakse Vault-parooli:

```bash
ansible-playbook db.yml --ask-vault-pass
```

---

## Vault-parooli automaatne kasutamine

Et vältida parooli käsitsi sisestamist iga kord, võib kasutada eraldi **Vault paroolifaili**.

Näiteks loo fail `vault_pass.txt` (hoia see turvaliselt!):

```
MinuParool123
```

Seejärel lisa käsureale:
```bash
ansible-playbook db.yml --vault-password-file vault_pass.txt
```

💡 **Hea tava:**  
Ära salvesta `vault_pass.txt` versioonihaldusesse (nt `.gitignore` alla).

---

## Vault-muutujate loomine otse käsurealt

Vaulti saab kasutada ka **üksiku muutuja** krüpteerimiseks otse käsurealt:

```bash
ansible-vault encrypt_string 'SalajaneParool123' --name 'db_password'
```

Väljund:
```yaml
db_password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          323935393462633634633464643438326565333162353534373731383932...
```

Seda saab otse lisada playbooki või `group_vars` failidesse.

---

## Mitme Vaulti kasutamine

Suuremates projektides võib kasutada mitut Vaulti (nt eraldi dev, test, prod keskkondade jaoks).  
Sellisel juhul saab kasutada **Vault ID-sid**:

```bash
ansible-vault encrypt --vault-id dev@prompt secrets-dev.yml
ansible-vault encrypt --vault-id prod@prompt secrets-prod.yml
```

Käivitamisel:
```bash
ansible-playbook site.yml --vault-id dev@prompt --vault-id prod@prompt
```

See võimaldab hallata eri parooliga krüpteeritud failide komplekte.

---


## Rohkem infot

- [Ansible Vault dokumentatsioon](https://docs.ansible.com/ansible/latest/vault_guide/index.html){:target="_blank"}  
- [Krüpteeritud stringide kasutamine](https://docs.ansible.com/ansible/latest/user_guide/vault.html#encrypting-individual-variables-with-ansible-vault){:target="_blank"}  
- [Vault ID süsteem](https://docs.ansible.com/ansible/latest/vault_guide/vault_id.html){:target="_blank"}  
