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

```bash
sudo apt install apache2 -y
sudo apt install php php-mysql php-cli php-curl php-xml php-mbstring php-gd php-zip libapache2-mod-php -y
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

### Lösung:
Recovery Mode → passwd Benutzer

---

### Problem: Copy/Paste in VM schwierig

### Lösung:
SSH verwenden oder VMware Tools installieren

---

### Problem: Unterschiedliche Netzwerke

### Ursache:
- Web und DB in verschiedenen Subnetzen

### Lösung:
- Beide VMs ins gleiche Netzwerk setzen