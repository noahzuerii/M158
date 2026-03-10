# Lösungen – Übungen CMS-DB Connection

**Thema:** Analyse des Datenflusses und Konfiguration der Datenbank-Schnittstelle  
**Projekt:** supercms.ch  
**Datum:** 10. März 2026

---

## Übung 1: Prozessablauf beim Aufruf des CMS (9 Schritte)

Der folgende Ablauf beschreibt die technischen Einzelschritte, die beim Aufruf der Domain `https://supercms.ch` im Browser initiiert werden (unter Ausschluss des DNS-Auflösungsprozesses).

1.  **Anfrage-Initiierung:** Die Benutzerin oder der Benutzer gibt die URL in den Browser ein, woraufhin der Client eine Anfrage an den Zielserver sendet.
2.  **Verbindungsaufbau:** Der Browser baut eine verschlüsselte Verbindung via **HTTPS** (Port 443) zum Webserver auf.
3.  **Anfrage-Verarbeitung:** Der Webserver nimmt den Request entgegen und identifiziert das Zielverzeichnis im Dateisystem (`/var/www/html/super-cms`).
4.  **Dateizugriff:** Der Webserver greift auf die CMS-Dateien zu und initiiert die Ausführung der PHP-Skripte durch den PHP-Interpreter.
5.  **Konfigurations-Check:** Das CMS lädt während der Initialisierung die Datei `database.php`, um die notwendigen Zugangsdaten für die Datenbank zu beziehen.
6.  **Treiber-Aufruf:** Über die Datei `mysqli-db-connect.php` wird die PHP-Erweiterung **mysqli** genutzt, um eine Verbindung zur Datenbank-Instanz herzustellen.
7.  **Datenbank-Kommunikation:** Das CMS authentifiziert sich am Datenbank-Server unter der IP `192.168.22.10` und sendet SQL-Abfragen zur Inhaltsabfrage (z. B. für Seiteninhalte oder Menüstrukturen).
8.  **Daten-Rückfluss:** Der Datenbank-Server liefert die angeforderten Rohdaten aus der Datenbank `superCMS` an das PHP-Skript zurück.
9.  **Rendering & Auslieferung:** Das CMS generiert aus den Daten und Templates ein HTML-Dokument, welches der Webserver als Antwort an den Browser zurücksendet, wo die Seite schließlich gerendert wird.

---

## Übung 2: Technologische Zuordnung

In der Infrastruktur kommen verschiedene Protokolle und Technologien an spezifischen Schnittstellen zum Einsatz:

| Technologie | Einsatzbereich | Funktion |
| :--- | :--- | :--- |
| **HTTPS** | Client ↔ Webserver | Gewährleistet die verschlüsselte und authentifizierte Übertragung der Webseiteninhalte. |
| **mysqli** | PHP-Applikation ↔ DB-Server | Dient als Schnittstelle (Interface) für die Kommunikation zwischen dem CMS und der MySQL/MariaDB-Datenbank. |
| **ext4** | Webserver ↔ Speichermedium | Das Standard-Dateisystem unter Linux, welches die physische Organisation der CMS-Dateien auf der Festplatte übernimmt. |

---

## Übung 3: Implementierung der Datenbank-Konfiguration

Nachfolgend findet sich die technische Umsetzung der Verbindungskonfiguration basierend auf den vorgegebenen Parametern der Infrastruktur.

### A. Konfigurationsdatei (`database.php`)

```php
<?php
/**
 * Datenbank-Konfigurationsparameter
 */
return [
    'host'     => '192.168.22.10',
    'port'     => 3306,
    'dbname'   => 'superCMS',
    'user'     => 'sCMS',
    'password' => 'xYzzAB23!'
];
```

### B. Verbindungsaufbau (`mysqli-db-connect.php`)

```php
<?php
// Laden der Konfigurationsdaten
$config = require 'database.php';

// Initialisierung der MySQLi-Verbindung
$conn = new mysqli(
    $config['host'],
    $config['user'],
    $config['password'],
    $config['dbname'],
    $config['port']
);

// Prüfung der Verbindung auf Fehler
if ($conn->connect_error) {
    error_log("Datenbankverbindung fehlgeschlagen: " . $conn->connect_error);
    die("Verbindung zum Datenbank-Server aktuell nicht möglich.");
}

// Optionale Einstellung des Zeichensatzes
$conn->set_charset("utf8mb4");
?>
```
