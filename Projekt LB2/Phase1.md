# Phase 1: Projektplan (Auftrag #1) - CRM Migration

[cite_start]**Modul:** Kompetenznachweis M158 LB2 [cite: 2, 30]  
[cite_start]**Schule:** Technische Berufsschule Zürich [cite: 1, 29]  
[cite_start]**Projekt:** CRM Migration [cite: 51]

## 1. Ausgangslage und Projektziele
[cite_start]Das bestehende CRM-System (`crmserver.sample.ch`) wird aktuell als lokale virtuelle Maschine (on-premise) betrieben[cite: 52]. Im Rahmen dieses Projekts erfolgt eine umfassende Modernisierung. [cite_start]Der Auftrag umfasst die vollständige Migration auf ein neues Betriebssystem (OS) mit einem neuen Web- und Datenbank-Server[cite: 54]. [cite_start]Dabei muss zwingend sichergestellt werden, dass die Datenmigration komplett und verlustfrei abläuft [cite: 55] [cite_start]und das Sicherheitsniveau der gesamten Infrastruktur spürbar erhöht wird[cite: 56].

[cite_start]Der erste Schritt besteht in der Projektvorbereitung: Das bestehende System wird anhand eines Exports in einer isolierten Testumgebung bereitgestellt[cite: 52, 62]. [cite_start]Anschliessend wird eine detaillierte IST-Analyse durchgeführt, bei der System und Datenbank analysiert und in einem Architekturdiagramm grafisch dokumentiert werden[cite: 62].

---

## 2. Evaluation der Migrationsvarianten

[cite_start]Der Kunde erwartet konkrete Vorschläge zur Umsetzung, inklusive einer Einschätzung des Zeitaufwands und der Kosten[cite: 60]. Dafür wurden die zwei folgenden Varianten geprüft:

### [cite_start]Variante A: Migration auf die neuste Version Vtiger [cite: 58]
Bei diesem Ansatz wird die bestehende Vtiger-Software aktualisiert und auf die neue Server-Infrastruktur umgezogen.
* **Vorteile:** Da die grundlegende Datenbankstruktur erhalten bleibt, ist das Risiko eines Datenverlusts geringer. Die Mitarbeitenden kennen die Benutzeroberfläche bereits, wodurch teure und zeitaufwändige Schulungen entfallen. Die Migration lässt sich zudem besser planen und testen.
* **Nachteile:** Je nachdem, wie alt die bestehende Version ist, müssen unter Umständen mehrere Zwischen-Updates ("Hops") durchgeführt werden, was bei alten Plugins zu Kompatibilitätsproblemen führen kann.

### [cite_start]Variante B: Migration auf ein anderes Opensource ERP (z.B. OpenERP), Daten migrieren [cite: 59]
Dieser Ansatz sieht einen kompletten Systemwechsel vor, beispielsweise auf Odoo (ehemals OpenERP).
* **Vorteile:** Das Unternehmen profitiert von einem hochmodernen System mit einer sehr aktiven Entwickler-Community. Veraltete Prozesse können im Zuge der Einführung direkt entsorgt und optimiert werden.
* **Nachteile:** Der Aufwand für das "Data Mapping" (das Übersetzen der alten Datenbankstruktur in das neue System) ist enorm hoch. Zudem müssen alle Mitarbeitenden komplett neu in die Software eingearbeitet werden, was zu Produktivitätsverlusten in der Anfangsphase führt.

**Empfehlung an den Kunden:**
[cite_start]Da das System an 5 bis 6 Tagen pro Woche (Mo-Sa) aktiv verwendet wird und ein Ausfall so kurz wie möglich sein soll[cite: 61], wird **Variante A** dringend empfohlen. Ein kompletter Systemwechsel gemäss Variante B würde nicht nur das Budget sprengen, sondern auch eine längere Ausfallzeit und Einarbeitungsphase nach sich ziehen, was den Betriebsablauf stören würde.

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

[cite_start]Die wichtigste Rahmenbedingung dieses Projekts lautet: Das System wird während 5-6 Tagen pro Woche Mo-Sa aktiv verwendet, ein Ausfall soll so kurz wie möglich sein[cite: 61]. 

Um dies zu garantieren, wird ein "Side-by-Side"-Migrationsansatz gewählt:
1. **Parallelaufbau:** Die neue Umgebung wird komplett unabhängig vom Live-Betrieb unter der Woche aufgebaut und getestet. 
2. **Testlauf:** Es wird eine Testmigration mit einem älteren Datenbank-Dump durchgeführt, um allfällige Fehler bereits im Vorfeld zu bereinigen.
3. **Umschaltung (Cut-Over):** Der finale Wechsel findet am Samstagabend nach Geschäftsschluss statt. Das alte System wird auf "Read-Only" gesetzt. Die finalen Daten werden auf den neuen Server transferiert. Durch die DNS-Umstellung am Sonntag ist das neue System rechtzeitig zum Arbeitsbeginn am Montagmorgen weltweit erreichbar.

---

## 5. Detaillierter Zeit- und Projektplan (KW 12 - KW 17)

Um einen reibungslosen Ablauf zu gewährleisten und die Anforderungen des Moduls M158 zu erfüllen, erstreckt sich das Projekt über einen realistischen Zeitraum von sechs Wochen. [cite_start]Jeder Schritt wird im Arbeitsjournal wochenweise strukturiert festgehalten[cite: 50].

### [cite_start]KW 12: Phase 1 - Planung & IST-Analyse [cite: 40]
* [cite_start]**Aufgaben:** Das bestehende System wird in der Testumgebung bereitgestellt[cite: 62]. [cite_start]Anschliessend wird die IST-Analyse durchgeführt (System und Datenbank analysieren und grafisch aufzeichnen)[cite: 62]. Erstellung von Projektplan und Testkatalog.
* **Begründung:** Wer blind migriert, scheitert. Die gründliche Analyse des IST-Zustands ist zwingend erforderlich, um Software-Abhängigkeiten (z.B. zwingend benötigte PHP-Versionen oder Datenbank-Kollationen) zu verstehen. Nur wenn die Ausgangslage klar ist, kann ein valides Architekturdiagramm für den SOLL-Zustand erstellt werden.

### [cite_start]KW 13: Phase 2 - Umgebung aufbauen [cite: 41]
* [cite_start]**Aufgaben:** Aufbau der virtuellen Maschinen und Netzwerkkomponenten (Router)[cite: 46]. [cite_start]Einrichtung von DNS [cite: 12][cite_start], Webserver [cite: 13][cite_start], PHP [cite: 14] [cite_start]und dem MySQL/MariaDB-Datenbankserver[cite: 15].
* **Begründung:** Bevor die Applikation migriert werden kann, muss das Fundament stehen. Die strikte Trennung dieses Aufbaus in eine eigene Kalenderwoche stellt sicher, dass Netzwerk- oder Installationsprobleme (z.B. bei der DNS-Auflösung) gelöst sind, bevor sensible Kundendaten das System berühren.

### [cite_start]KW 14: Phase 3 - Zielsystem & Sicherheit [cite: 42]
* [cite_start]**Aufgaben:** Das Zielsystem wird konfiguriert [cite: 47] [cite_start]und die Sicherheit wird aktiv erhöht[cite: 56]. [cite_start]Installation von Verwaltungstools wie PhpMyAdmin/Adminer [cite: 16] [cite_start]sowie Einrichtung eines sicheren SFTP oder FTPS-Zugangs[cite: 17]. [cite_start]Erstellung erster Snapshots[cite: 47].
* **Begründung:** Ein ungesicherter Server im Internet ist ein massives Risiko. Die Härtung des Systems und die Einrichtung verschlüsselter Verbindungen (SFTP/FTPS) müssen abgeschlossen sein, bevor im nächsten Schritt Backups mit echten Daten transferiert werden. Die Snapshots dienen als "Rettungsanker", falls bei der späteren Konfiguration etwas schiefgeht.

### [cite_start]KW 15: Phase 4 - Migration (Testlauf / Dry-Run) [cite: 43]
* [cite_start]**Aufgaben:** Durchführung eines Backup Transfers und Backup Imports[cite: 48]. [cite_start]Anpassung wichtiger Systemdateien (z.B. wp-config.php bzw. der vTiger-Äquivalente)[cite: 48]. 
* **Begründung:** Ein kompletter Trockenlauf ist der einzige Weg, um zu beweisen, dass die Theorie in der Praxis funktioniert. Durch diesen Testlauf in KW 15 können wir fehlerhafte Pfade, Berechtigungsprobleme oder fehlende PHP-Erweiterungen ohne Zeitdruck identifizieren und beheben.

### [cite_start]KW 16: Phase 4 & 5 - Echt-Migration, Tests & Deployment [cite: 43, 44]
* [cite_start]**Aufgaben:** Das finale Deployment [cite: 22] und die Echt-Migration finden zwingend am Wochenende (Sa/So) statt. [cite_start]Danach folgt das umfassende Testing [cite: 20] [cite_start]anhand des Testkatalogs und die Einrichtung des Monitorings[cite: 21].
* [cite_start]**Begründung:** Die Echt-Migration am Wochenende ist die direkte technische Antwort auf die Kundenanforderung, dass der Mo-Sa-Betrieb nicht gestört werden darf[cite: 61]. Das anschliessende Monitoring stellt sicher, dass die Ressourcen des neuen Servers unter echter Last (ab Montag) ausreichend dimensioniert sind.

### KW 17: Projektabschluss & Dokumentation
* [cite_start]**Aufgaben:** Zeitpuffer für Fehler und deren Bereinigung[cite: 63]. [cite_start]Kontrolle und Ablage sämtlicher Unterlagen, Daten und Code in git[cite: 67].
* **Begründung:** IT-Projekte beinhalten immer unvorhersehbare Faktoren. Eine Woche Puffer stellt sicher, dass der Abgabetermin eingehalten wird. [cite_start]Zudem erfordert die Vorgabe, dass wirklich alle Schritte nachvollziehbar dokumentiert werden (inklusive Begründungen, Grafiken, Eingaben/Ausgaben und Fehlerbereinigungen)[cite: 63], ausreichend Zeit für den finalen "Feinschliff" des Arbeitsjournals.

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