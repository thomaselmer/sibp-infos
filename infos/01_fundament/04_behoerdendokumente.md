---
id: D04
beschreibung: Behördendokumente – Dokumentarten, Rangfolge, Herkunftsregeln, Prüfdokumente, Nummerierung, Abschnitte, fehlender Bescheid.
status: in Arbeit
stand: 2026-11-13
verweise: [D44, D45, D46, D47, D49, D15, D19, D21, D24, D25, D38, D39]
baut_auf: [D44, D45, D46, D47, D49]
andockpunkt: [D24, D25, D38, D39]
annahmen: [A06]
themen: [Leeres Behördendokument, fehlender Bescheid, Bescheid, Verhandlungsschrift, Prüfdokument, sonstige Dokumente, Herkunftsregeln]
---
# 04 – Behördendokumente

Status: **in Arbeit**
Stand: 13.11.2026 – Behördenseite grob beschrieben; sonstige Dokumente grob eingeordnet; Arten der Antworten in Prüfdokumenten ergänzt; Erfüllung aus sonstigen Dokumenten mit Herkunftsvermerk; fehlende geprüfte Bescheide (leeres Behördendokument).

Andockpunkte: [[D44]] (Dokument), [[D47]] (Auflage, Bestätigung), [[D45]] (Verfahren), [[D46]] (Key-Info), [[V06]] bis [[V11]]

## Oberste Trennung
- **Behördendokumente** – Quelle von Auflagen und Bestätigungen
- **Sonstige Dokumente** (alle Nicht-Behördendokumente) – siehe unten

## Sonstige Dokumente
- Vorerst **alle gleich behandelt**: als "sonstiges Dokument" klassifiziert, **nichts ausgelesen**.
- Zusätzlich grobe **Unterkategorie**, wo leicht erkennbar (z. B. E-Mail, Prüfblatt/Datenblatt, Bestätigung wie Elektroattest). Noch ohne Folgen für die Verarbeitung – Sortierung liegt bereit, wenn wir mehr wissen.
- Bestätigungen (z. B. Elektroinstallation ordnungsgemäß) inhaltlich kaum standardisierbar.
- Spätere eigene Verarbeitung je Unterkategorie dockt am parallelen Verarbeitungsschritt an ([[V07]]).

## Arten von Behördendokumenten
| Art | Inhalt | Rolle |
|---|---|---|
| Verhandlungsschrift | Protokoll einer Verhandlung | Auflagen möglich |
| Bescheid | Entscheidung der Behörde nach der Verhandlung | Hauptquelle der Auflagen |
| Verhandlungsschrift mit Bescheid | beides in einem Dokument (einfachere Verfahren) | Auflagen möglich |
| Sonstige Behördendokumente | alles andere von Behörden | vorerst keine Auflagen |

## Ablauf im Verfahren
- Erst Verhandlung -> Verhandlungsschrift.
- Danach Bescheid: bei einfachen Verfahren direkt mit der Verhandlungsschrift, bei komplexen extra zugestellt, evtl. deutlich später.
- **Rangfolge:** Bescheid steht über der Verhandlungsschrift.

## Herkunftsregeln für Auflagen (Verarbeitungsregeln, keine Annahmen)
- Auflagen kommen aktuell **nur aus Bescheiden und Verhandlungsschriften**. Später sind weitere Quellen für Vorschriften wahrscheinlich.
- Auflagen werden aus **beiden** extrahiert.
- Dieselbe Auflage in Bescheid und Verhandlungsschrift = Duplikat -> **Bescheid-Fassung gilt**.
- Fehlt der Bescheid, gelten die Auflagen aus der Verhandlungsschrift.
- Umsetzung: [[V08]]

## Normale Dokumente und Prüfdokumente
- Bescheide und Verhandlungsschriften können ein anderes Behördendokument **überprüfen** (typisch: "Überprüfung eines Bescheides"; durchwegs in Verhandlungsschriften, vermutlich auch in Bescheiden).
- **Normales Dokument** -> liefert **Auflagen**.
- **Prüfdokument** -> liefert **Informationen** (Art Bestätigung), keine Auflagen.
- Erkennung vorerst über die Formulierung. Prüfdokumente nennen Geschäftszahl(en) und Datum der geprüften Dokumente.

## Geprüfter Bescheid fehlt im Akt
- Ein Prüfdokument kann einen Bescheid prüfen, der selbst nicht im Akt liegt. Aus dem Prüfdokument sind trotzdem **Geschäftszahl, Datum und Anzahl der Auflagen** (über die Nummern) bekannt -> der Akt ist nachweislich unvollständig.
- Es wird ein **leeres Behördendokument** (Typ Bescheid) mit Geschäftszahl und Datum angelegt. Die Bestätigungen aus dem Prüfdokument verweisen darauf; ihre Herkunft bleibt das Prüfdokument.
- **Erkennbar ohne eigenes Feld:** Behördendokument ohne Text = fehlendes Dokument. Eindeutig, weil ein echtes Behördendokument per Definition aus erkannten Textabschnitten entsteht (V06/V07) und daher immer Text hat.
- **Hinweis an den Nutzer:** den Bescheid mit Geschäftszahl und Datum konkret bei der Behörde anfordern.
- Wird er nachgeladen: Einstiegspunkt "Erweiterung" ([[D15]]); das leere Dokument wird befüllt.
- Platzhalter-Auflagen aus dem Prüfdokument (und Suche danach) vorerst nicht – geparkt in [[D19]].
- Umsetzung: [[V10]]

## Arten der Antworten in Prüfdokumenten
Jeder nummerierte Punkt bezieht sich auf eine Auflage und hat eine Art:
- **erfüllt** (bestätigt) -> Auflage erledigt
- **hinfällig** (entfällt) -> Auflage erledigt
- **ergänzt / erweitert** -> Zusatzinformation zur Auflage, hängt an ihr
- Nicht erfüllte Auflage = **Mangel** ([[D49]]).
- Domänensicht: Bestätigung gehört zur Auflage. Verarbeitungssicht: getrennt in [[V09]] und [[V10]].
- **Erfüllung aus sonstigen Dokumenten:** "erfüllt" kann auch aus einem Nachweis in einem sonstigen Dokument kommen ([[V11]]). Gleiche Arten, aber Herkunft (nicht behördlich bestätigt) wird vermerkt – Grundlage, um der Behörde bereits erfüllte Auflagen mitzuteilen. Form und Ort offen ([[D19]]).

## Nummerierung
- Auflagen stehen grundsätzlich in einer nummerierten Aufzählung: **eine Nummer = eine Auflage**.
- Abweichung: In Verhandlungsschriften können Auflagen oder Ergänzungen auch im Fließtext stehen, v. a. in Stellungnahmen von Amtssachverständigen. **MVP: nur nummerierte Listen.**
- Prüfdokumente haben ebenfalls Aufzählungen. Ihre Nummern **entsprechen den Auflagennummern des geprüften Dokuments** -> Zuordnung über Geschäftszahl + Nummer ([[V10]]).
- Prüft ein Dokument mehrere Bescheide, ist die Trennung meist textuell erkennbar – Details später.

## Beziehungen zu Verfahren
- Ein Bescheid kann **mehrere Verfahren** abdecken (ggf. für mehrere Akte gelten).
- Ein Prüfdokument kann **mehrere Bescheide** prüfen.
- -> Viele-zu-viele zwischen Dokument und Verfahren; Auflagen hängen am Verfahren, nicht nur am Dokument ([[D45]]). **Hinweis:** "Verfahren" als eigene Instanz ist ein unbestätigter Vorschlag; automatische Erkennung offen ([[D19]]).

## Struktur und Abschnitte
- Verhandlungsschrift und Bescheid haben je **typische Textabschnitte** (beim Bescheid z. B. Spruch, Begründung, Rechtsmittelbelehrung).
- Abschnitte beider Arten in einem Dokument -> Verhandlungsschrift mit Bescheid.
- Die Abschnitte dienen doppelt: **Dokumenttyp bestimmen** und **Fundstelle** der Auflagen/Bestätigungen eingrenzen ([[A06]]).
- Genaue Abschnittsliste ist im Prototyp abgedeckt -> beim Prototyp-Vergleich ergänzen. Wichtig fürs Datenmodell.
