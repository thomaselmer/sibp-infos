---
id: D24
beschreibung: Klassifizieren der Dokumente – typische Abschnitte erkennen und Dokumenttyp bestimmen.
status: grob
stand: 2026-11-13
verweise: [D15, D04, D25, D38, D39]
baut_auf: [D04]
andockpunkt: [D25, D39]
annahmen: [A06]
themen: [Abschnitte, Dokumenttyp, Prüfdokument, sonstige Dokumente, Unterkategorie]
nodes:
  - {id: V06, eingang: V05, ausgang: V07}
  - {id: V07, eingang: V06, ausgang: [V08, V09], verzweigungen: {normales Behördendokument: V08, Prüfdokument: V09, sonstiges Dokument: "vorerst keine Extraktion"}}
---
# 02 – Klassifizieren (V06–V07)

Teil von [[D15]]. Fachliche Grundlage: [[D04]].

## V06 – Abschnitte erkennen
- Behördendokumente haben typische Textabschnitte (Verhandlungsschrift und Bescheid je eigene) – [[D04]].
- Die erkannten Abschnitte dienen **doppelt:** Eingang für V07 (Dokumenttyp) und Fundstelle für V08/V09 (wo Auflagen bzw. Bestätigungen stehen) – vgl. [[A06]].
- Genaue Abschnittsliste im Prototyp abgedeckt -> beim Prototyp-Vergleich ergänzen.

## V07 – Dokumenttyp bestimmen
- Zuerst: Behördendokument oder **sonstiges Dokument**.
- Behörde: Bescheid / Verhandlungsschrift / Verhandlungsschrift mit Bescheid (Abschnitte beider Arten) / sonstiges Behördendokument – aus den Abschnitten von V06.
- Folge: Ein Behördendokument entsteht nur aus erkannten Textabschnitten und hat daher immer Text (Grundlage für das leere Behördendokument, [[V10]]).
- Zusätzlich: **normales Dokument oder Prüfdokument** – vorerst über die Formulierung "Überprüfung eines Bescheides".
- Sonstige Dokumente: alle gleich behandelt, dazu grobe **Unterkategorie**, wo leicht erkennbar (E-Mail, Prüfblatt/Datenblatt, Bestätigung …). Vorerst **keine Extraktion** – [[D04]].
- **Andockpunkt:** Nach V07 läuft die Extraktion **parallel für alle Dokumente**, je Dokumenttyp eine eigene Verarbeitung. Heute nur für Behördendokumente ([[V08]], [[V09]]); später je Unterkategorie der sonstigen Dokumente eigene Extraktion bzw. Weiterverarbeitung (z. B. Datum, Beteiligte/Gewerk als Vorfilter für [[V11]]).

## Optimierungen für später
- Eigene Extraktion je Unterkategorie der sonstigen Dokumente (Andockpunkt nach V07)
