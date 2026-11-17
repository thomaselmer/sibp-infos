---
id: D13
beschreibung: Bidirektionale QM-App-Schnittstelle und allgemeine API.
status: ruht
stand: 2026-11-15
verweise: [D07, D14, D17, D20]
baut_auf: [D14]
andockpunkt: [D07]
annahmen: []
themen: [QM App, API, Fragebogen]
---
# 02 – QM App und Schnittstellen

## QM App
Andockpunkte: Ast 3 "Pflichten ableiten" und Ast 7 "Laufender Betrieb" in [[D07]].
- Eigene Prüf-/Fragebogen-Funktion bleibt im Produkt (zentrale Plattform, Wunsch von SiBP und Büros).
- Zusätzlich bidirektionale Schnittstelle: Prüfpflichten als Fragebögen exportieren, Protokolle als Nachweise zurückholen.
- Der QM App fehlt die automatische Erfassung aus dem gesamten Akt (bisher nur einzelne Bescheide via Claude) -> paragraph82b als "Gehirn" für deren Checklisten.
- Abgleich: Welche Auflagen decken bestehende QM-App-Fragebögen schon ab, wo sind Lücken?
- Mehraufwand bewusst in Kauf genommen.
- Vertriebskanal und Konditionen: siehe [[D20]].

## Allgemeine API
- Schnittstelle, über die später die QM App oder andere Systeme andocken (Vorschlag).
- Technische Grundlage: [[D14]].

## Phase
Nach dem MVP; Architektur im MVP bereits vorbereiten – siehe [[D17]].
