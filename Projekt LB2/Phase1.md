# Phase 1: Projektplan (Auftrag #1) - CRM Migration

**Modul:** Kompetenznachweis M158 LB2  
**Schule:** Technische Berufsschule Zürich  
**Projekt:** CRM Migration

## 1. Ausgangslage und Projektziele
Das bestehende CRM-System (`crmserver.sample.ch`) wird aktuell als lokale virtuelle Maschine (on-premise) betrieben. Im Rahmen dieses Projekts erfolgt eine umfassende Modernisierung. Der Auftrag umfasst die vollständige Migration auf ein neues Betriebssystem (OS) mit einem neuen Web- und Datenbank-Server. Dabei muss zwingend sichergestellt werden, dass die Datenmigration komplett und verlustfrei abläuft und das Sicherheitsniveau der gesamten Infrastruktur spürbar erhöht wird.

Der erste Schritt besteht in der Projektvorbereitung: Das bestehende System wird anhand eines Exports in einer isolierten Testumgebung bereitgestellt. Anschliessend wird eine detaillierte IST-Analyse durchgeführt, bei der System und Datenbank analysiert und in einem Architekturdiagramm grafisch dokumentiert werden.

---

## 2. Evaluation der Migrationsvarianten

Der Kunde erwartet konkrete Vorschläge zur Umsetzung, inklusive einer Einschätzung des Zeitaufwands und der Kosten. Dafür wurden die zwei folgenden Varianten geprüft:

### Variante A: Migration auf die neuste Version Vtiger
Bei diesem Ansatz wird die bestehende Vtiger-Software aktualisiert und auf die neue Server-Infrastruktur umgezogen.
* **Vorteile:** Da die grundlegende Datenbankstruktur erhalten bleibt, ist das Risiko eines Datenverlusts geringer. Die Mitarbeitenden kennen die Benutzeroberfläche bereits, wodurch teure und zeitaufwändige Schulungen entfallen. Die Migration lässt sich zudem besser planen und testen.
* **Nachteile:** Je nachdem, wie alt die bestehende Version ist, müssen unter Umständen mehrere Zwischen-Updates ("Hops") durchgeführt werden, was bei alten Plugins zu Kompatibilitätsproblemen führen kann.

### Variante B: Migration auf ein anderes Opensource ERP (z.B. OpenERP), Daten migrieren
Dieser Ansatz sieht einen kompletten Systemwechsel vor, beispielsweise auf Odoo (ehemals OpenERP).
* **Vorteile:** Das Unternehmen profitiert von einem hochmodernen System mit einer sehr aktiven Entwickler-Community. Veraltete Prozesse können im Zuge der Einführung direkt entsorgt und optimiert werden.
* **Nachteile:** Der Aufwand für das "Data Mapping" (das Übersetzen der alten Datenbankstruktur in das neue System) ist enorm hoch. Zudem müssen alle Mitarbeitenden komplett neu in die Software eingearbeitet werden, was zu Produktivitätsverlusten in der Anfangsphase führt.

**Empfehlung an den Kunden:**
Da das System an 5 bis 6 Tagen pro Woche (Mo-Sa) aktiv verwendet wird und ein Ausfall so kurz wie möglich sein soll, wird **Variante A** dringend empfohlen. Ein kompletter Systemwechsel gemäss Variante B würde nicht nur das Budget sprengen, sondern auch eine längere Ausfallzeit und Einarbeitungsphase nach sich ziehen, was den Betriebsablauf stören würde.

---

## 3. Ressourcenplanung: Zeitaufwand und Kosten

Die folgende Kostenschätzung basiert auf der empfohlenen Variante A. Sie umfasst alle Schritte von der Planung bis zum finalen Monitoring.

| Projektphase / Aufgabe | Geschätzter Aufwand | Kosten (Basis: CHF 120/h) |
| :--- | :--- | :--- |
| **Planung & IST-Analyse (inkl. Testumgebung)** | 4 Stunden | CHF 480.- |
| **Umgebung aufbauen (VMs, OS, Web, DB, DNS)** | 8 Stunden | CHF 960.- |
| **Zielsystem konfigurieren & Sicherheit erhöhen** | 6 Stunden | CHF 720.- |
| **Test-Migration (Dry-Run) & Fehlerbehebung** | 10 Stunden | CHF 1'200.- |
| **Echt-Migration (Cut-Over am Wochenende)** | 6 Stunden | CHF 720.- |
| **Testing, Monitoring & Projektabschluss** | 4 Stunden | CHF 480.- |
| **Total** | **38 Stunden** | **CHF 4'560.-** |

*Hinweis: Laufende Kosten für das neue Server-Hosting sind hier nicht inkludiert.*

---

## 4. Strategie zur Minimierung der Ausfallzeit

Die wichtigste Rahmenbedingung dieses Projekts lautet: Das System wird während 5-6 Tagen pro Woche Mo-Sa aktiv verwendet, ein Ausfall soll so kurz wie möglich sein. 

Um dies zu garantieren, wird ein "Side-by-Side"-Migrationsansatz gewählt:
1. **Parallelaufbau:** Die neue Umgebung wird komplett unabhängig vom Live-Betrieb unter der Woche aufgebaut und getestet. 
2. **Testlauf:** Es wird eine Testmigration mit einem älteren Datenbank-Dump durchgeführt, um allfällige Fehler bereits im Vorfeld zu bereinigen.
3. **Umschaltung (Cut-Over):** Der finale Wechsel findet am Samstagabend nach Geschäftsschluss statt. Das alte System wird auf "Read-Only" gesetzt. Die finalen Daten werden auf den neuen Server transferiert. Durch die DNS-Umstellung am Sonntag ist das neue System rechtzeitig zum Arbeitsbeginn am Montagmorgen weltweit erreichbar.

---

## 5. Detaillierter Zeit- und Projektplan (KW 12 - KW 17)

Um einen reibungslosen Ablauf zu gewährleisten und die Anforderungen des Moduls M158 zu erfüllen, erstreckt sich das Projekt über einen realistischen Zeitraum von sechs Wochen. Jeder Schritt wird im Arbeitsjournal wochenweise strukturiert festgehalten.

### KW 12: Phase 1 - Planung & IST-Analyse
* **Aufgaben:** Das bestehende System wird in der Testumgebung bereitgestellt. Anschliessend wird die IST-Analyse durchgeführt (System und Datenbank analysieren und grafisch aufzeichnen). Erstellung von Projektplan und Testkatalog.
* **Begründung:** Wer blind migriert, scheitert. Die gründliche Analyse des IST-Zustands ist zwingend erforderlich, um Software-Abhängigkeiten (z.B. zwingend benötigte PHP-Versionen oder Datenbank-Kollationen) zu verstehen. Nur wenn die Ausgangslage klar ist, kann ein valides Architekturdiagramm für den SOLL-Zustand erstellt werden.

### KW 13: Phase 2 - Umgebung aufbauen
* **Aufgaben:** Aufbau der virtuellen Maschinen und Netzwerkkomponenten (Router). Einrichtung von DNS, Webserver, PHP und dem MySQL/MariaDB-Datenbankserver.
* **Begründung:** Bevor die Applikation migriert werden kann, muss das Fundament stehen. Die strikte Trennung dieses Aufbaus in eine eigene Kalenderwoche stellt sicher, dass Netzwerk- oder Installationsprobleme (z.B. bei der DNS-Auflösung) gelöst sind, bevor sensible Kundendaten das System berühren.

### KW 14: Phase 3 - Zielsystem & Sicherheit
* **Aufgaben:** Das Zielsystem wird konfiguriert und die Sicherheit wird aktiv erhöht. Installation von Verwaltungstools wie PhpMyAdmin/Adminer sowie Einrichtung eines sicheren SFTP oder FTPS-Zugangs. Erstellung erster Snapshots.
* **Begründung:** Ein ungesicherter Server im Internet ist ein massives Risiko. Die Härtung des Systems und die Einrichtung verschlüsselter Verbindungen (SFTP/FTPS) müssen abgeschlossen sein, bevor im nächsten Schritt Backups mit echten Daten transferiert werden. Die Snapshots dienen als "Rettungsanker", falls bei der späteren Konfiguration etwas schiefgeht.

### KW 15: Phase 4 - Migration (Testlauf / Dry-Run)
* **Aufgaben:** Durchführung eines Backup Transfers und Backup Imports. Anpassung wichtiger Systemdateien (z.B. wp-config.php bzw. der vTiger-Äquivalente). 
* **Begründung:** Ein kompletter Trockenlauf ist der einzige Weg, um zu beweisen, dass die Theorie in der Praxis funktioniert. Durch diesen Testlauf in KW 15 können wir fehlerhafte Pfade, Berechtigungsprobleme oder fehlende PHP-Erweiterungen ohne Zeitdruck identifizieren und beheben.

### KW 16: Phase 4 & 5 - Echt-Migration, Tests & Deployment
* **Aufgaben:** Das finale Deployment und die Echt-Migration finden zwingend am Wochenende (Sa/So) statt. Danach folgt das umfassende Testing anhand des Testkatalogs und die Einrichtung des Monitorings.
* **Begründung:** Die Echt-Migration am Wochenende ist die direkte technische Antwort auf die Kundenanforderung, dass der Mo-Sa-Betrieb nicht gestört werden darf. Das anschliessende Monitoring stellt sicher, dass die Ressourcen des neuen Servers unter echter Last (ab Montag) ausreichend dimensioniert sind.

### KW 17: Projektabschluss & Dokumentation
* **Aufgaben:** Zeitpuffer für Fehler und deren Bereinigung. Kontrolle und Ablage sämtlicher Unterlagen, Daten und Code in git.
* **Begründung:** IT-Projekte beinhalten immer unvorhersehbare Faktoren. Eine Woche Puffer stellt sicher, dass der Abgabetermin eingehalten wird. Zudem erfordert die Vorgabe, dass wirklich alle Schritte nachvollziehbar dokumentiert werden (inklusive Begründungen, Grafiken, Eingaben/Ausgaben und Fehlerbereinigungen), ausreichend Zeit für den finalen "Feinschliff" des Arbeitsjournals.

---

## 6. Testkatalog (Planungsentwurf)

Folgende Tests werden, dann in der Phase 5 durchgeführt, um den Projekterfolg zu verifizieren:

| Test-ID | Bereich | Prüfschritt | Erwartetes Resultat |
| :--- | :--- | :--- | :--- |
| **T01** | Erreichbarkeit | Webserver über Domain aufrufen. | Seite lädt, HTTP 200 OK. |
| **T02** | DNS | `ping crmserver.ch` ausführen. | Löst auf die neue Server-IP auf. |
| **T03** | Datenbank | Login in PhpMyAdmin/Adminer. | Tabellen des CRM sind vorhanden. |
| **T04** | Applikation | Login ins CRM mit Admin-Credentials. | Dashboard wird fehlerfrei geladen. |
| **T05** | Daten-Integrität | Stichprobe: Letzter erstellter Kunde vor Migration suchen. | Datensatz ist vollständig vorhanden. |
| **T06** | Schreibrechte | Einen neuen Test-Kontakt im CRM anlegen. | Eintrag wird in der DB gespeichert. |