# ansible_playbook_full

urgent - w repo brakuje:
`~/ansible/playbooks/configfiles/debian-sshd-default.conf`
``` conf
[sshd]
enabled = true
bantime = 3600
```

Wstępinie wymagane:
 - wygenerowanie klucza ssh: `ssh-keygen -t rsa -b 4096`
 - instalacja ansible:

``` shell
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y
```
 - edycja pliku ./inventory/hosts (wpisujemy hosty, na których ma zostać wykonany playbook)
``` yaml
[ubuntu]
192.168.1.100
192.168.1.101
192.168.1.102
```
 - edycja pliku ./playbooks/full.yaml
``` yaml 
  vars:
    username: test
    userpass: zaq1@WSX
```

Playbook wykonujący kolejno:
1. Zakłada usera na podstawie wpisanych wartości w full.yaml oraz dodaje do pliku sudo.
2. Wysyła klucze na docelowe hosty.
3. Apt - playbook - wykonuję masę rzeczy:
- Sprawdzanie miejsca na dysku
- Ilość pozostałego miejsca na dysku
- Sprawdzanie nieużywanych pakietów
- Lista pakietów do usunięcia
- Usuwanie zbędnych pakietów
- Odświeżenie repozytorium
- Sprawdzenie pakietów do aktualizacji
- Lista pakietów do aktualizacji
- Wyrażenie zgody na eksterminację całej ludzkości (wymagane zatwierdzenie ENTER lub przerwanie poprzez kombinację klawiszy CTRL+C)
- Ponowne odświeżenie repozytorium oraz aktualizacja wszystkich pakietów
- Sprawdzenie pozostałego miejsca na dysku
- Pozostałe miejsce na dysku (powtórzenie tych czynności ma na celu umożliwienie weryfikacji czy nie potrzebujemy zwiększyć przestrzeni dyskowej na danej maszynie)
- Sprawdzanie czy był aktualizowany kernel
- Wykonanie restartu jeśli kernel był aktualizowany (ansible nie dokona restartu maszyn hurtowo, czeka aż każda z maszyn się uruchomi i dopiero przejdzie do restartu następnej maszyny)
4. Instaluje fail2ban.
5. Instaluje dockera, docker-compose oraz upoważnia usera do uruchamiania dockera bez sudo.
