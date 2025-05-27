# VPS_Cloudlinux_Jutrzenka

# VPS DevOps Lab

Ten projekt dokumentuje moje działania administracyjne, naukę oraz eksperymenty na prywatnym VPS opartym o CloudLinux. Celem jest stworzenie stabilnego środowiska do nauki i wdrażania rozwiązań DevOps.

---

## 📦 Zakres projektu

- ✅ Instalacja i konfiguracja VPS (CloudLinux)
- ✅ Zabezpieczenie serwera (CSF, UFW)
- ✅ Hostowanie aplikacji w Dockerze
- ✅ Backupy i monitoring (Prometheus + Grafana, opcjonalnie NetworkChuck stack)

---

## 📅 25.05.2025 – Aktywacja systemu i pierwsza aktualizacja

```bash
/usr/sbin/rhnreg_ks --activationkey=TWÓJ_KLUCZ_AKTYWACYJNY
/usr/bin/cldetect --update-license
dnf update
```

#Refleksja:
Cena najniższej licencji CloudLinux to obecnie 25 zł miesięcznie. Zobaczymy, jak to się sprawdzi w praktyce i czy koszt jest uzasadniony w kontekście dostępnych funkcji.

# 📅 26.05.2025 – Fail2Ban: niekompatybilność z CloudLinux
Próby instalacji fail2ban zakończyły się niepowodzeniem.
CloudLinux nie wspiera tej technologii natywnie i nie dostarcza oficjalnej dokumentacji.

Decyzja:
Jako alternatywę wybrałem CSF (ConfigServer Security & Firewall) – rozwiązanie zalecane przez CloudLinux.

# 27.05.2025 – Konfiguracja dostępu SSH (Windows)

Generowanie kluczy SSH
Metoda 1: PowerShell (Windows 10+)

ssh-keygen -t rsa -b 4096

Zapisz w domyślnym katalogu:

C:\Users\TwojaNazwa\.ssh\id_rsa

Metoda 2: PuTTYgen

    Wygeneruj klucz RSA

    Zapisz:

        Klucz prywatny .ppk (do PuTTY/WinSCP)

        Klucz publiczny (w formacie OpenSSH)

Kopiowanie klucza publicznego na VPS

Z Windowsa:
scp C:\Users\User\.ssh\id_rsa.pub cloudlinux@146.59.14.176:/home/cloudlinux/.ssh

Na VPS:
mkdir -p ~/.ssh
cat /home/cloudlinux/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
rm /home/cloudlinux/.ssh/id_rsa.pub

Ustawienie podwójnej autoryzacji SSH

Edytuj plik konfiguracyjny:
sudo nano /etc/ssh/sshd_config

Zmiany:
PasswordAuthentication yes
PubkeyAuthentication yes
AuthenticationMethods publickey,password

ℹ️ Info:
AuthenticationMethods publickey, password oznacza, że użytkownik musi uwierzytelnić się zarówno kluczem, jak i hasłem.


# CSF – Alternatywa dla Fail2Ban | A nawet wskazane rozwiązanie!

Wymagania:
sudo yum install perl-Math-BigInt -y
perl -MMath::BigInt -e1 #Testujemy działanie

Instalacja CSF (ConfigServer Security & Firewall)

cd /usr/src
wget https://download.configserver.com/csf.tgz
tar -xzf csf.tgz
cd csf
sudo sh install.sh

Test poprawności instalacji:
perl /usr/local/csf/bin/csftest.pl

Restartowanie CSF:
csf -r


# TODO (następne kroki)

Konfiguracja Dockera i kontenerów

Setup CI/CD (GitHub Actions / GitLab CI)

Monitoring: Prometheus + Grafana lub Inne

Automatyzacja backupów

Testowanie firewalli i alertów