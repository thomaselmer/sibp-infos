---
id: D39
art: inhalt
beschreibung: Agentischer Abgleich offener Auflagen mit sonstigen Dokumenten (V11) – Überblick, Ergebnis, offene Fragen und Teile.
status: in Arbeit
stand: 2026-11-15
verweise: [D46, D48, D49, D04, D07, D19, D23, D26, D40, D41, D42]
baut_auf: [D46, D49, D04, D23]
andockpunkt: [D07]
annahmen: []
themen: [Agent, Nachweise, Mangel, Erheblichkeit, Kommentar]
nodes:
  - {id: V11, eingang: V10, ausgang: Ergebnis, verzweigungen: {"abbrechender Status": "Zweig beendet", "kein Nachweis": "Mangel", "Subauflagen": "erneut V11 je Subauflage"}}
optimierung: {schnittstelle: "Eingang V10 -> Ergebnis", heute: "Ein Agent je offener Auflage mit Werkzeugen (u. a. Suche, Dokument lesen); MVP: vorerst einfacher Fall – Auflage, Bestätigung gefunden, prüfen ob ausreichend; Prototyp auf LangChain", spaeter: "Mehrstufige Subauflagen voll ausbauen; Unterkategorien der sonstigen Dokumente bzw. deren eigene Extraktion (Andockpunkt V07) können die Suche verbessern; Stellschrauben im Prototyp testen (Schichten und Grenzen, Gewichtung der Indizien, Schlagworte, Datumsfilter hart vs. weich)", hebel: gross}
---
# 02 – Agentischer Abgleich (V11)

Teil von [[D26]]. Fachliche Grundlage: [[D04]]. Datenobjekt Key-Info: [[D46]].

## V11 – Offene Auflagen mit sonstigen Dokumenten abgleichen
- **Eingang:** offene Auflagen aus V10 (inkl. angehängter Ergänzungen); Text und Embeddings der sonstigen Dokumente aus [[V03]]. Nur einmalige Auflagen – wiederkehrende werden nicht agentisch untersucht, sondern über Fragebögen abgearbeitet ([[D48]]).
- **Ziel:** Zusatzinformationen zu einer Auflage finden – es können **mehrere** Treffer relevant sein.
- **Richtung:** von der Auflage zum Nachweis – je offener Auflage in den sonstigen Dokumenten suchen (bestätigt; einfacher als umgekehrt, überschaubare Liste).
- **Agentisch (bestätigt):** ein Agent je offener Auflage mit Werkzeugen (u. a. Suche, Dokument lesen); sucht, bis ein Nachweis gefunden ist oder die Suche erschöpft ist. Im Gegensatz zu V10 (Code) hier LLM, da es um Textinhalte geht. Prototyp dazu auf LangChain begonnen ([[D19]]).

### Ergebnis
- **Kein Nachweis gefunden = Mangel** (kein eigener Zwischenstatus).
- **Erheblichkeit des Mangels** ergibt sich daraus, wie viel der Root-Auflage samt Subauflagen offen bleibt ([[D49]]).
- Noch nicht beantwortbare Punkte können bei späterer Prüfung/Begehung **kommentiert** werden (Key-Info vom Typ Kommentar) – [[D07]].

- Offen: Kriterien der Zuordnungsprüfung, Rekursionstiefe/Abbruch, abbrechende Status-Kategorien, Messung der Erheblichkeit; wann Beteiligte/Gewerke erfasst werden – [[D19]]

## Teile
| Kennung | Datei | Inhalt | Status |
|---|---|---|---|
| [[D40]] | 01_suche | Hybride Suche, Indizien, Schichtenmodell, Stellschrauben | in Arbeit |
| [[D41]] | 02_treffer_pruefen | Ablauf je Treffer: Zuordnungsprüfung, Textpassage, Klassifizieren, abbrechender Status | in Arbeit |
| [[D42]] | 03_subauflagen | Mehrstufige Subauflagen (Endpunkt) | in Arbeit |
