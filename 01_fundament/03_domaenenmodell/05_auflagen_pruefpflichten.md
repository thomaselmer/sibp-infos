---
id: D47
beschreibung: Domänenmodell-Segment Auflagen und Prüfpflichten – Auflage, Bestätigung, Prüfpflicht, Prüfung/Nachweis, einmalige Auflage.
status: in Arbeit
stand: 2026-11-15
verweise: [D03, D04, D19, D38, D39]
annahmen: []
themen: [Auflage, Bestätigung, Prüfpflicht, Prüfung, Nachweis, Auflagenart, Einmalige Auflage]
---
# 05 – Auflagen und Prüfpflichten

Teil von [[D03]]. Stand: 15.11.2026 – aus D03 ausgelagert.

## Instanzen
- **Auflage** – Key-Info vom Typ Auflage: aus Dokument/Seite extrahiert, mit Nummer, Quellverweis und Status (offen, erfüllt, hinfällig, geändert) inkl. Begründung; später auch Risikoklasse (SiBP-Matrix) – Vorschlag, siehe [[D19]]
- **Bestätigung (Information)** – Key-Info vom Typ Zusatzinformation: aus einem Prüfdokument extrahiert, verweist über Geschäftszahl + Nummer auf eine Auflage; Art: erfüllt / hinfällig / ergänzt (Zusatzinformation) – [[D04]]
- **Prüfpflicht** – wiederkehrende Pflicht mit Intervall (täglich bis mehrjährig), Verantwortlichem. Entsteht aus einer wiederkehrenden Auflage (siehe Auflagenart) oder aus dem Prüfkatalog des Onboardings
- **Prüfung / Nachweis** – durchgeführte Prüfung mit Foto, Kommentar, Protokoll (später auch aus QM App)

## Auflagenart
- **Einmalige Auflage:** wird beantwortet, bis sie erfüllt oder hinfällig ist ([[V10]], [[V11]]).

## Beziehungen
- Auflage -> Seite/Dokument (Herkunft, nötig für Teil-Neuberechnung)
- Bestätigung -> Auflage (über Geschäftszahl + Nummer); erfüllt/hinfällig erledigt die Auflage, ergänzt hängt Zusatzinformation an
- Auflage aus Bescheid hat Vorrang vor derselben Auflage aus der Verhandlungsschrift
- Auflage <-> Prüfpflicht <-> Nachweis (Matching); wiederkehrende Auflage -> Prüfpflicht mit Intervall (siehe Auflagenart)
- Prüfpflicht = generisch: Feuerlöscher (halbjährlich), Kühlschranktemperatur (täglich), 82b (mehrjährig) sind nur andere Intervallwerte.
