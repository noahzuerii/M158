# Phase 2: Aufbau der Zielumgebung (SOLL-System)

## 10. Ziel der Phase
Ziel dieser Phase war der Aufbau einer modernen, sicheren und skalierbaren Infrastruktur für das CRM-System. Dabei wurde die bestehende monolithische Architektur durch eine getrennte Serverstruktur ersetzt.

---

## 11. Architektur

- Webserver (crm-web): 192.168.42.135
- Datenbankserver (crm-db): 192.168.42.134
- Netzwerk: Host-only (intern) + NAT (Internet)

Begründung:
Die Trennung ermöglicht höhere Sicherheit, bessere Wartbarkeit und zukünftige Skalierbarkeit.

---

## 12. Installation & Vorbereitung

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install curl wget unzip -y
```

---

## 13. Netzwerkprobleme & Lösungen

### Problem 1: Kein Internetzugang
Fehlermeldung:
```
Network is unreachable
```

### Ursache:
- Nur Host-only Netzwerk aktiv → kein Internet

### Lösung:
- Zweiten Adapter hinzufügen (NAT)
- Netplan konfigurieren:

```yaml
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: true
    ens37:
      dhcp4: true
```

```bash
sudo netplan apply
```

---

### Problem 2: Interface DOWN

```bash
ip a
```

ens37 war DOWN.

### Lösung:

```bash
sudo ip link set ens37 up
```

---

### Problem 3: Keine DHCP Adresse

### Lösung:
Netplan korrekt konfigurieren (dhcp4: true)

---

## 14. MariaDB Setup

```bash
sudo apt install mariadb-server -y
sudo mysql_secure_installation
```

### DB erstellen

```sql
CREATE DATABASE vtiger;
CREATE USER 'vtigeruser'@'192.168.42.135' IDENTIFIED BY 'StrongPassword!';
GRANT ALL PRIVILEGES ON vtiger.* TO 'vtigeruser'@'192.168.42.135';
FLUSH PRIVILEGES;
```

---

### Problem: Remote Zugriff nicht möglich

### Ursache:
```
bind-address = 127.0.0.1
```

### Lösung:

```ini
bind-address = 0.0.0.0
```

```bash
sudo systemctl restart mariadb
```

---

## 15. Firewall

```bash
sudo apt install ufw -y
sudo ufw allow OpenSSH
sudo ufw allow from 192.168.42.135 to any port 3306
sudo ufw enable
```

---

## 16. Webserver Setup

> **Hinweis:** Es wird **PHP 5.6** verwendet, da Vtiger CRM keine höheren PHP-Versionen unterstützt. Neuere PHP-Versionen sind mit der eingesetzten Vtiger-Version inkompatibel.

```bash
sudo apt install apache2 -y
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:ondrej/php -y
sudo apt update
sudo apt install php5.6 php5.6-mysql php5.6-cli php5.6-curl php5.6-xml php5.6-mbstring php5.6-gd php5.6-zip libapache2-mod-php5.6 -y
```

```bash
sudo a2enmod rewrite
sudo systemctl restart apache2
```

---

### Problem: Apache Seite nicht erreichbar

### Ursache:
- Firewall oder Dienst nicht gestartet

### Lösung:

```bash
sudo systemctl start apache2
sudo systemctl status apache2
```

---

## 17. VirtualHost

```apache
<VirtualHost *:80>
    ServerName crmserver.ch
    DocumentRoot /var/www/html/vtigercrm

    <Directory /var/www/html/vtigercrm>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

```bash
sudo a2dissite 000-default.conf
sudo a2ensite vtiger.conf
sudo systemctl reload apache2
```

---

## 18. DNS (hosts)

```bash
192.168.42.135 crmserver.ch
```

---

## 19. SFTP / SCP

```bash
scp -r vtigercrm user@server:/tmp/
sftp noah@192.168.42.135
```

---

### Problem: SCP Fehler

Fehlermeldung:
```
no hostkey alg
```

### Ursache:
- Alte SSH Version auf dem alten Server

### Lösung:
- Transfer vom neuen System oder PC starten
- SSH Optionen erweitern

---

## 20. Verbindungstest

```bash
mysql -h 192.168.42.134 -u vtigeruser -p
```

---

## 21. Weitere Probleme & Lösungen

### Problem: Passwort vergessen (VM Login)

Während der Installation wurde das Benutzerpasswort vergessen, wodurch kein Login mehr möglich war.

### Lösung:

Die Wiederherstellung erfolgte über den Recovery Mode:

1. VM starten und GRUB-Menü öffnen
2. „Advanced options for Ubuntu“ auswählen
3. „Recovery Mode“ starten
4. Root-Shell öffnen

Dann:

```bash
mount -o remount,rw /
ls /home
passwd noah
reboot
```

---

### Problem: Copy/Paste in VM schwierig

Die Arbeit über die VM-Konsole erschwerte das Einfügen von Befehlen erheblich.

### Lösung:

Es wurde SSH verwendet, um die Server komfortabel zu administrieren:

```bash
ssh noah@192.168.42.135
ssh noah@192.168.42.134
```

Zusätzlich wurde SSH installiert und aktiviert:

```bash
sudo apt install openssh-server -y
sudo systemctl enable ssh
sudo systemctl start ssh
```

Alternative Lösung:

```bash
sudo apt install open-vm-tools -y
reboot
```

---

### Problem: Unterschiedliche Netzwerke

Der Webserver und der Datenbankserver befanden sich initial in unterschiedlichen Netzwerken:

- Webserver: 172.x.x.x
- Datenbankserver: 192.168.x.x

Dadurch war keine Kommunikation möglich.

### Lösung:

Beide VMs wurden in dasselbe Netzwerk gebracht (VMware):

- Adapter 1: NAT (Internet)
- Adapter 2: Host-only (intern)

---

### Problem: Kein Internetzugang

Nach der Umstellung auf Host-only Netzwerk funktionierte kein Internetzugang mehr.

Fehlermeldung:

```
Network is unreachable
```

### Ursache:

- NAT Interface war nicht aktiv

### Lösung:

Interface aktivieren:

```bash
ip a
sudo ip link set ens37 up
```

Netzwerk konfigurieren:

```bash
sudo nano /etc/netplan/*.yaml
```

```yaml
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: true
    ens37:
      dhcp4: true
```

```bash
sudo netplan apply
```

Test:

```bash
ping 8.8.8.8
ping google.com
```

---

### Problem: SCP Fehler (no hostkey alg)

Beim Kopieren der Daten vom alten System trat folgender Fehler auf:

```
no hostkey alg
```

### Ursache:

- Alte SSH-Version auf dem alten Server

### Lösung:

Der Transfer wurde vom neuen System oder vom Client gestartet:

```bash
scp -O -P 2222 -o HostKeyAlgorithms=+ssh-rsa -o PubkeyAcceptedAlgorithms=+ssh-rsa root@localhost:/var/www/html/vtigercrm noah@192.168.42.135:/tmp/
```

---

### Problem: Datenbank nicht erreichbar

Der Webserver konnte initial keine Verbindung zur Datenbank herstellen.

### Ursache:

- MariaDB nur auf localhost gebunden

### Lösung:

```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

```ini
bind-address = 0.0.0.0
```

```bash
sudo systemctl restart mariadb
```

Zusätzlich wurde der Zugriff eingeschränkt:

```sql
CREATE USER 'vtigeruser'@'192.168.42.135' IDENTIFIED BY 'StrongPassword!';
GRANT ALL PRIVILEGES ON vtiger.* TO 'vtigeruser'@'192.168.42.135';
FLUSH PRIVILEGES;
```

---

## 22. Auftrag #8 - Adminer im Webroot integriert

### Umsetzung

Adminer wurde in den Webroot von Vtiger verschoben:

```bash
sudo mv /var/www/html/adminer /var/www/html/vtigercrm/
```

Anschliessend wurden die Berechtigungen gesetzt:

```bash
sudo chown -R www-data:www-data /var/www/html/vtigercrm/adminer
sudo find /var/www/html/vtigercrm/adminer -type d -exec chmod 755 {} \;
sudo find /var/www/html/vtigercrm/adminer -type f -exec chmod 644 {} \;
```

### Zugriff

http://crm.local/adminer

### Funktionen

- SQL-Queries ausführen
- Daten anzeigen und bearbeiten
- Export/Import von Datenbanken

### Ergebnis

- Erfolgreicher Zugriff auf die Datenbank über das Webinterface
- Migration und Verwaltung vereinfacht

---

## 23. Auftrag #9 - SFTP (Secure File Transfer)

### Ziel

Sicherer Zugriff auf das System zur Datenübertragung.

### Umsetzung

Der Zugriff erfolgt über SFTP, welches standardmässig im SSH-Dienst integriert ist.

Verbindung vom Client:

```bash
sftp noah@192.168.42.135
```

Alternativ mit Hostname:

```bash
sftp noah@crm.local
```

### Eigenschaften

- Verschlüsselte Verbindung (SSH)
- Port: 22
- Authentifizierung via Benutzer/Passwort

### Vorteile

- Keine zusätzliche Installation nötig
- Sicherer Datentransfer
- Einfach integrierbar

### Einsatz im Projekt

- Übertragung von Vtiger-Dateien
- Migration von Daten
- Verwaltung von Webinhalten

### Ergebnis

- Sicherer Zugriff auf das System gewährleistet
- Datenmigration effizient unterstützt