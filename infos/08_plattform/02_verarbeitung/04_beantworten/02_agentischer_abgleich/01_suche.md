---
id: D40
art: inhalt
beschreibung: Suche des Agenten (V11) – hybride Suche, Indizien, Schichtenmodell, Stellschrauben.
status: in Arbeit
stand: 2026-11-15
verweise: [D04, D23, D24, D39]
baut_auf: [D04, D23]
andockpunkt: [D24]
annahmen: []
themen: [hybride Suche, Schichtenmodell, Indizien, Geschäftszahl, Stellschrauben]
blatt: ausformuliert
---
# 01 – Suche (Teil von V11)

Teil von [[D39]]. Fachliche Grundlage: [[D04]]. Embeddings: [[V03]].

- **Hybride Suche:**
 - Regex/Grep für Exaktes (v. a. Geschäftszahl); Suchmuster gibt das LLM aus der Auflage vor.
 - Semantische Suche über die Embeddings aus V03 für inhaltliche Nähe zur Auflage.
- **Indizien (grobe Vorgaben):**
 - **Geschäftszahl** – starkes Indiz. In sonstigen Dokumenten meist gut ausformuliert; im Behördendokument selbst (meist links oben) aber oft falsch ausgelesen und bei alten Dokumenten nicht standardisiert -> **potenziell fehlerbehaftet**, unscharf vergleichen. Selten mehrere Geschäftszahlen auf einem Dokument (Behörde fasst Verfahren zusammen) -> muss unterstützt werden.
 - **Inhalt der Auflage** (Thema, Prüfgegenstand).
 - **Datum** – eine Auflage wird grundsätzlich erst nach dem Bescheid bestätigt.
 - **Beteiligte/Gewerk** (Elektriker, Baumeister, Tischler …), falls vorab bekannt.
- **Schichtenmodell (Vorschlag Thomas):** von innen nach außen suchen – zuerst die wahrscheinlichsten Dokumente (z. B. nach dem Bescheiddatum, mit Geschäftszahl), dann schrittweise ausweiten. Dokumente ohne Datum grundsätzlich im Set. **Ältere Dokumente werden nicht ausgeschlossen**, sondern in äußeren Schichten durchsucht (z. B. altes Datenblatt als Nachweis). Datum bestimmt die Reihenfolge, nicht den Ausschluss.

- **Stellschrauben (im Prototyp testen):** Schichten und Grenzen, Gewichtung der Indizien, Schlagworte, Datumsfilter hart vs. weich.

- Später: Unterkategorien der sonstigen Dokumente bzw. deren eigene Extraktion (Andockpunkt [[V07]]) können die Suche verbessern.
