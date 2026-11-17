---
id: D03
art: inhalt
beschreibung: Domänenmodell – Grundprinzip, Betriebsobjekt als Zentrum, Übersicht der Objekte und Beziehungen; Details in den Segmenten D43–D49.
status: in Arbeit
stand: 2026-11-15
verweise: [D43, D44, D45, D46, D47, D48, D49]
annahmen: []
themen: [Betriebsobjekt, Grundprinzip, Instanzen, Beziehungen, Ereignisprotokoll]
---
# 03 – Domänenmodell

Stand: 15.11.2026 – in Segmente aufgeteilt (D43–D49); frühere Stände in der Git-Historie.

## Grundprinzip
Alles wird in **eigenständige Instanzen** verpackt, die miteinander verknüpft werden. Das **Betriebsobjekt (Projekt)** ist die kleinste zentrale Einheit, an die alle Nutzer, Mandanten und Inhalte andocken.

## Zentrum
- **Betriebsobjekt / Projekt** – Standort mit Stammdaten, Branche, Onboarding-Antworten

## Objekte (Übersicht)
- Mandant, Plan, Nutzer, Zugriff – [[D43]]
- Dokument, Seite, leeres Behördendokument – [[D44]]
- Verfahren (Vorschlag) – [[D45]]
- **Key-Info** – gemeinsames Objekt für alle extrahierten bzw. erfassten Informationen (Begriff aus dem Prototyp). Siehe [[D46]].
- Auflage, Bestätigung, Prüfpflicht, Prüfung / Nachweis, Auflagenart – [[D47]]
- Wiederkehrende Auflage (Dauerauflage) – [[D48]]
- Mangel, Maßnahme, Prüfbericht, Erheblichkeit – [[D49]]
- **Ereignisprotokoll** – automatische Entscheidungen, Korrekturen, Zugriffe

## Beziehungen im Überblick
- Mandant / Nutzer -> Zugriff -> Betriebsobjekt ([[D43]])
- Betriebsobjekt -> Dokument -> Seite; Dokument <-> Verfahren (Vorschlag) ([[D44]], [[D45]])
- Dokument oder Nutzer -> Key-Info (Herkunft); Key-Info -> Key-Info (rekursiv) ([[D46]])
- Auflage <- Bestätigung / Nachweis; Auflage -> Prüfpflicht ([[D47]], [[D48]])
- Auflage nicht erfüllt -> Mangel -> Maßnahme -> Prüfbericht ([[D49]])
