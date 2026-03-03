# Übung -- Migrationsansätze -- Vorname Nachname

## 📚 Modul M158

------------------------------------------------------------------------

# 1️⃣ Übersicht der Migrationsansätze

Basierend auf Skript Kapitel 3 sowie der *Cloud Migration Checklist*
(Seite 7) fileciteturn1file0

## Cold Turkey / Big Bang

Komplet Umstellung auf das neue System zu einem festen Zeitpunkt.
Altsystem wird vollständig abgeschaltet.

**Vorteil:** Schnell\
**Nachteil:** Hohes Risiko

------------------------------------------------------------------------

## Rehost (Lift & Shift)

Migration der Anwendung unverändert in die Cloud.

**Vorteil:** Schnell, geringe Anpassung\
**Nachteil:** Keine Architekturverbesserung

------------------------------------------------------------------------

## Replatform

Leichte Optimierung der Anwendung für Cloud-Umgebung.

------------------------------------------------------------------------

## Retire

Altes System wird ersetzt oder abgeschaltet.

------------------------------------------------------------------------

## Refactor

Code wird angepasst, um Cloud-Funktionen besser zu nutzen.

------------------------------------------------------------------------

## Rebuild / Reengineering

Komplette Neuentwicklung der Anwendung.

------------------------------------------------------------------------

## Chicken-Little-Ansatz

Schrittweise Migration einzelner Komponenten.

------------------------------------------------------------------------

## Butterfly-Ansatz

Paralleler Betrieb von Alt- und Neusystem mit schrittweiser
Datenübernahme.

------------------------------------------------------------------------

## COREM-Ansatz

Strukturierte, modellbasierte Analyse und Migration.

------------------------------------------------------------------------

## Repurchase

Wechsel zu neuer Standardsoftware (z.B. SaaS).

------------------------------------------------------------------------

## Retain

System bleibt vorerst unverändert bestehen.

------------------------------------------------------------------------

# 2️⃣ Gewählter Migrationsansatz

## ✅ Rehost (Lift & Shift)

Gemäss *Cloud Migration Checklist* (Seite 7) fileciteturn1file0
eignet sich Rehosting besonders, wenn:

-   Zeitdruck besteht\
-   Budget begrenzt ist\
-   Minimales Risiko gewünscht wird\
-   Bestehende Software stabil läuft

Für meine imaginäre Migration (On-Prem → Cloud) ist Rehosting am
sinnvollsten, da primär die Infrastruktur modernisiert werden soll.

------------------------------------------------------------------------

# 3️⃣ Grober Durchführungsplan (10--20 Schritte)

1.  Projektstart\
2.  Zieldefinition\
3.  Stakeholder-Analyse\
4.  Rollen & Verantwortlichkeiten definieren\
5.  Workload-Analyse\
6.  Cloud-Anbieter evaluieren\
7.  Zielarchitektur definieren\
8.  Sicherheits- und Compliance-Anforderungen prüfen\
9.  Backup aller Systeme erstellen\
10. Testumgebung in Cloud aufbauen\
11. Applikation migrieren\
12. Datenbank migrieren\
13. Netzwerk konfigurieren\
14. Systemtests durchführen\
15. Performance-Tests durchführen\
16. Benutzerakzeptanztest\
17. Produktivsetzung\
18. Monitoring & Logging aktivieren\
19. Altsystem abschalten\
20. Post-Migration Review

------------------------------------------------------------------------

# 4️⃣ Wichtige Aspekte gemäss Cloud Migration Checklist

Laut PDF sind folgende Punkte besonders wichtig fileciteturn1file0:

-   Klare Zieldefinition\
-   Rollen & Verantwortlichkeiten\
-   Analyse der Workloads\
-   Sicherheits- & Compliance-Prüfung\
-   Roadmap & Zeitplanung\
-   Notfallplan\
-   Performance-Messung (KPIs)\
-   Post-Mortem & ROI-Bewertung