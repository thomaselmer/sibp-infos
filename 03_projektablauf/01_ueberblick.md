---
id: D07
beschreibung: Projektablauf – allgemeiner Lebenszyklus eines Betriebsobjekts in sieben Ästen mit Andockpunkten zu den anderen Bereichen.
status: grob
stand: 2026-11-15
verweise: [D02, D46, D48, D06, D08, D09, D11, D12, D13, D15, D19, D39]
baut_auf: [D46, D48]
andockpunkt: [D06, D09, D11, D12, D13, D15, D39]
annahmen: []
themen: [Lebenszyklus, Äste, Onboarding, Begehung]
nodes:
  - {id: P01, name: "Objekt anlegen und Onboarding", eingang: Start, ausgang: P02, andockpunkt: [D06]}
  - {id: P02, name: "Akt aufbauen", eingang: P01, ausgang: P03, andockpunkt: [D15, D12]}
  - {id: P03, name: "Pflichten ableiten", eingang: P02, ausgang: P04, andockpunkt: [D15, D13, D11]}
  - {id: P04, name: "Prüfen", eingang: P03, ausgang: P05}
  - {id: P05, name: "Maßnahmen", eingang: P04, ausgang: P06}
  - {id: P06, name: "Bericht als Snapshot", eingang: P05, ausgang: P07}
  - {id: P07, name: "Laufender Betrieb", eingang: P06, ausgang: "neuer Zyklus", andockpunkt: [D09, D13]}
---
# 01 – Projektablauf: Überblick

"Projektablauf" ist ein Arbeitstitel; der endgültige Name ist offen ([[D19]]).

Der Projektablauf ist der **allgemeine Lebenszyklus eines Betriebsobjekts**, unabhängig vom Regelwerk. Er ist der Stamm des Netzes. Die § 82b-Prüfung ist nur ein Pfad durch diesen Ablauf, wie später Hygiene oder die Planungsgrundlage für Architekten – siehe [[D11]].

## Äste
1. **Objekt anlegen und Onboarding** – Stammdaten, Branche, geführte Fragen, Vorschlag eines Prüfkatalogs.
   Andockpunkte: SiBP-Liste prüfpflichtiger Objekte; Zugriffe [[D06]].
2. **Akt aufbauen** – Hochladen aus verschiedenen Quellen (Fotos aus dem Behördenakt, Unterlagen des Betriebs), Digitalisieren und Gliedern, Duplikate, Akt-Editor. Nicht ausgewertete Inhalte (z. B. Pläne) werden erkannt und angezeigt.
   Andockpunkte: Technik [[D15]]; Pläne [[D12]].
3. **Pflichten ableiten** – Auflagen mit Quellverweis extrahieren, mit Nachweisen abgleichen (erfüllt, offen, hinfällig, geändert), Prüfpflichten bilden (aus wiederkehrenden Auflagen, [[D48]]). Nachweise auch aus sonstigen Dokumenten, ggf. mit Subauflagen; alles als Key-Info ([[D46]], [[V11]]).
   Andockpunkte: Technik [[D15]]; QM App [[D13]]; Regelwerk [[D11]].
4. **Prüfen** – offene Punkte kommentieren (Kommentar = Key-Info vom Typ Kommentar, [[D46]]), Begehung, Mängel mit Risikoeinstufung (SiBP-Matrix).
5. **Maßnahmen** – Maßnahmenplan erstellen, später nachverfolgen.
6. **Bericht als Snapshot** – Freigabe durch Prüfer, danach unveränderlich, neue Versionen möglich, Branding des Büros.
7. **Laufender Betrieb** – wiederkehrende Prüfungen über Fragebögen, Fälligkeiten, Änderungen am Objekt, neuer Zyklus.
   Andockpunkte: Monitoring [[D09]]; QM App [[D13]].

## Nicht linear
Die Äste sind keine starre Linie. Man kann jederzeit zurück, z. B. neue Seiten in den Akt nachladen; nur Neues wird verarbeitet – siehe [[D02]].

## Detaillierter Durchlauf
[[D08]] – derzeit noch aus Sicht der 82b-Prüfung formuliert. Wird beim Verfeinern auf den allgemeinen Ablauf umgestellt; 82b-Spezifisches wandert dann in [[D11]].
