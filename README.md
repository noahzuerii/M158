# M158 – Software-Migration planen und durchführen

## 📚 Modul

**M158 – Software-Migration planen und durchführen**  
Berufsschule | ICT-Berufsbildung Schweiz

---

## 📋 Beschreibung

Dieses Repository enthält die Übungen und Lösungen zum Modul M158. Im Modul geht es darum, Software-Migrationen zu planen und durchzuführen – von der Analyse bestehender Systeme bis zur erfolgreichen Überführung in eine neue Umgebung.

---

## 📁 Inhalt

### 📂 Aufgaben Theorie

| Datei | Beschreibung |
|-------|-------------|
| [`Grundelemente_der_Migration.md`](./Aufgaben%20Theorie/Grundelemente_der_Migration.md) | Übung zu den Grundelementen der Migration (Kapitel 1 & 2): Definition, Gründe, Hard-/Software-Migration, Reengineering |
| [`Migrationsansätze.md`](./Aufgaben%20Theorie/Migrationsansätze.md) | Übung zu Migrationsansätzen (Kapitel 3): Übersicht der Ansätze, gewählter Ansatz, Durchführungsplan |
| [`übung_pfade.md`](./Aufgaben%20Theorie/übung_pfade.md) | Übung zu absoluten und relativen Pfaden (lokal und im Netz) |
| [`loesungen_cms_db.md`](./Aufgaben%20Theorie/loesungen_cms_db.md) | Lösungen zu den Übungen CMS-DB Connection: Prozessablauf, Technologiezuordnung, Implementierung der Datenbank-Konfiguration |

### 📂 Projekt LB2

| Datei | Beschreibung |
|-------|-------------|
| [`Phase1.md`](./Projekt%20LB2/Phase1.md) | Projektplan (Auftrag #1) für die CRM-Migration: Ausgangslage, Migrationsvarianten, Ressourcenplanung, Ausfallzeit-Strategie, Zeitplan (KW 12–17) und Testkatalog |
| [`Phase2.md`](./Projekt%20LB2/Phase2.md) | Aufbau der Zielumgebung (SOLL-System): Architektur (Webserver & DB-Server), Installation & Konfiguration von MariaDB, Apache, UFW, Netplan sowie Dokumentation aller aufgetretenen Probleme und deren Lösungen |

---

## 🎯 Lernziele

- Software-Migration korrekt definieren und einordnen
- Verschiedene Migrationsansätze (Big Bang, Rehost, Replatform, Refactor, u. a.) kennen und vergleichen
- Einen geeigneten Migrationsansatz für ein konkretes Szenario wählen und begründen
- Einen Migrationsplan strukturiert erstellen
- Absolute und relative Pfade verstehen und anwenden
- Eine getrennte Server-Infrastruktur (Webserver + Datenbankserver) aufbauen und konfigurieren
- MariaDB remote-fähig einrichten und absichern (UFW, Benutzerrechte)
- Apache 2.4 mit VirtualHost und PHP konfigurieren
- Netzwerkprobleme in virtuellen Umgebungen diagnostizieren und beheben (Netplan, NAT, Host-only)
- Datenmigration via SCP/SFTP durchführen und Fehler beheben

---

## 🛠 Verwendete Technologien

- Markdown (`.md`) für die Dokumentation
- Git / GitHub für die Versionsverwaltung
- **Apache 2.4** – Webserver auf dem SOLL-System
- **MariaDB** – Datenbanksystem auf dem SOLL-System
- **PHP** – Serverseitige Skriptsprache für Vtiger CRM
- **UFW** – Firewall-Konfiguration unter Ubuntu
- **Netplan** – Netzwerkkonfiguration unter Ubuntu
- **VMware** – Virtualisierungsplattform (Host-only + NAT Netzwerk)
- **SSH / SCP / SFTP** – Fernzugriff und Dateiübertragung
