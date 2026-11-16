---
id: D23
beschreibung: Einlesen des Akts – Upload, Speichern, Auslesen/Einstufen, Gruppierung der Seiten zu Dokumenten, Duplikate.
status: in Arbeit
stand: 2026-11-13
verweise: [D15, D27, D02, D20, D12, D19, D25, D39]
baut_auf: [D02, D27]
andockpunkt: [D12, D39]
annahmen: [A01, A02, A03, A04, A05, A06, A07]
themen: [Upload, Formate, Auslesen, PDFs seitenweise, Embeddings, Vektordatenbank, Gruppierung, Page Stream Segmentation, Duplikate]
nodes:
  - {id: V01, eingang: Dateien, ausgang: V02}
  - {id: V02, eingang: V01, ausgang: V03}
  - {id: V03, eingang: V02, ausgang: V04}
  - {id: V04, eingang: V03, ausgang: V05}
  - {id: V05, eingang: V04, ausgang: V06}
---
# 01 – Einlesen (V01–V05)

Teil von [[D15]]. Querschnittsthemen (Fehlerbehandlung, Einstiegspunkte, PDF-Sonderfälle): [[D27]].

## V01 – Upload
- **Unterstützt:** Handyfotos, Scans, PDFs (auch saubere Original-PDFs). Bunt gemischt.
- **Nicht unterstützt (vorerst):** Office-Dateien (Word, Excel …), Plandateien (DWG o. Ä. evtl. später), alle anderen Formate.
- Nicht unterstützte Dateien werden beim Upload abgewiesen, **weder gespeichert noch verarbeitet** (schließt Sicherheitsrisiken aus).
- Kein Bombardement an Einzelfehlern: **ein gesammelter Hinweis pro Upload** ("12 Dateien nicht übernommen – Format nicht unterstützt"), Liste aufklappbar.
- Ein Upload ist grundsätzlich ein kompletter Akt (Einstiegspunkte: [[D27]]).

## V02 – Speichern
- Dateien ablegen und in der Datenbank erfassen.
- Korrigierte Versionen (z. B. gedrehte Seiten aus V03) werden gespeichert und weiterverwendet, auch für die Anzeige im Akt-Editor.

## V03 – Auslesen und Einstufen
**Fotos/Einzelseiten:** ein LLM-Aufruf pro Seite, der drei Dinge auf einmal macht:
1. Seite einstufen (Aktenseite / nicht ausgewertet, aber relevant / irrelevant – siehe Tabelle unten)
2. Text auslesen – bei Plänen, Anlagenfotos und irrelevanten Seiten kein Text
3. Merkmale (Features) für die Gruppierung herausziehen ([[V04]])

**PDFs (bisherige MVP-Entscheidung):** das ganze PDF in einem Aufruf an das LLM, zurück kommt nur der Text, **keine Merkmale** (die wären über mehrere Seiten vermischt; PDF = Dokument nach [[A03]]). Das LLM nimmt die Fallunterscheidung ab: Text-PDF, gescanntes und gemischtes PDF laufen über denselben Weg.

**Vorgemerkte Änderung (Thomas, noch zu bestätigen):** PDFs gleich zu Beginn in Einzelseiten zerlegen und seitenweise auslesen wie Fotos.
- [[A03]] bleibt gültig: inhaltlich ist ein PDF weiterhin ein Dokument, nur das Auslesen läuft je Seite.
- Nutzen: einheitlicher Weg für Fotos und PDFs; je Seite ein Embedding (Vektordatenbank, siehe unten); hilft bei Duplikaten ([[V05]]); einheitliche Seitenzählung als Grundlage für ein Preismodell nach Seiten ([[D20]]).
- Kosten: mehr Aufrufe bei PDFs.
- PDFs sollen später inhaltlich allgemein noch aufgearbeitet werden.

**Embeddings:** werden beim Auslesen je Seite aus dem Text erzeugt und abgelegt – kein eigener Node. Damit entsteht nebenbei eine Vektordatenbank des Akts; genutzt u. a. für die semantische Suche in [[V11]].

**Gedrehte Seiten:** wird eine Drehung erkannt, Seite richtig drehen, gedrehte Version speichern (V02), Aufruf neu starten – verlässlicherer Text, kostet bei diesen Seiten einen zweiten Aufruf.

**Entscheidung fürs MVP (bestätigt):** weniger Entwicklungsaufwand und ein einheitlicher Weg, dafür mehr verbrauchte Tokens – auch für Seiten, die am Ende verworfen werden. Bewusst in Kauf genommen. Optimierung siehe unten.

### Einstufung der Inhalte
| Kategorie | Beispiele | Umgang |
|---|---|---|
| Aktenseite | Bescheide, Gutachten, Protokolle, Schriftverkehr | wird ausgewertet |
| Nicht ausgewertet, aber relevant | Pläne, Fotos der Betriebsanlage (auch innerhalb von PDFs) | aussortiert, aufbewahrt, mit Grund angezeigt; später evtl. genutzt ([[D12]]) |
| Irrelevant | Ordnerrücken, Fehlaufnahmen | verworfen, aber einsehbar und zurückholbar ([[A04]]) |

## V04 – Gruppierung (Seiten -> Dokumente)
Fachbegriff: **Page Stream Segmentation**. Betrifft nur Foto-Seiten – PDFs sind bereits je ein Dokument ([[A03]]). Setzt die Reihenfolge nach [[A02]] voraus.

- **Rollierendes Fenster:** MVP mit **zwei Seiten davor und zwei danach**; Fenstergröße einstellbar, größeres Fenster ist spätere Optimierung. (Erster Versuch nur mit einer Seite war halbwegs brauchbar, dann ±1, dann ±2.)
- **Eingang:** nur die Merkmale aus V03 – nicht Bilder oder Text. Günstig und konstant.
- **Entscheidung:** gewichtetes Regelwerk. Jede Regel trägt mit ihrem Gewicht zu einem Gesamtwert bei; ab einer Schwelle beginnt ein neues Dokument. Aktuell entscheidet das Regelwerk allein.
- **Merkmale sind Hinweise, keine harten Regeln** – erst mehrere zusammen ergeben eine Entscheidung (Beispiel: Unterschriften kommen auch auf Zwischenseiten vor).
 - Bekannte Merkmale: neue Titelseite (Adresskopf, Firmenkennung …), Seitenangabe "Seite x von y" (z. B. Wechsel von "von 5" zu "von 2" = Widerspruch -> neues Dokument).
 - Konkretere Merkmale hat Thomas bereits – werden beim Vergleich mit dem Prototypen ergänzt.
 - Kandidaten (Vorschlag Claude, ungeprüft): Geschäftszahl/Aktenzahl, Datum, Absender/Behörde, Ende-Signale, Dokumenttyp, Layout-Ähnlichkeit zur Vorseite.
- **Als Komponente für maschinelles Lernen beschrieben:** klare Schnittstelle – hinein die Merkmale aus dem Fenster, heraus die Wahrscheinlichkeit, dass hier ein neues Dokument beginnt. Das Innere ist austauschbar:
 - MVP: handgesetzte Gewichte, gefunden über eine **Stabilitätsanalyse** – Extreme sind "jede Seite ein Dokument" und "alles ein Dokument"; ein stabiles Plateau dazwischen gilt als guter Ansatz.
 - Einschränkung: stabil heißt nicht automatisch richtig; Gewichte aus einem einzelnen Akt können überangepasst sein.
 - Später: Gewichte lernen, z. B. aus den Korrekturen im Akt-Editor (keine perfekte Wahrheit, aber Hinweise, wo Nutzer Grenzen anders sehen).
- **Bewertung:** Es gibt keine gelabelten Akte und es wird sie voraussichtlich nicht geben ([[A07]]). Gemessen wird deshalb am Ergebnis: Werden die Auflagen gefunden? Wichtig ist, dass Bescheide mit ihren Auflagen zusammenbleiben; unklare Grenzen im Schriftverkehr sind egal ([[A05]], [[A06]]).
- **Vorschlag (nicht bestätigt):** zwei Schwellen statt einer – dazwischen eine Grauzone, deren Fälle markiert und dem Nutzer gezeigt werden ([[D02]]: unsichere Fälle zum Nutzer).
- **Duplikate auf Seitenebene:** werden im Fenster miterkannt – zwei fast gleiche Seiten direkt hintereinander sind wahrscheinlich dasselbe Foto doppelt; die bessere Version bleibt.

## V05 – Duplikate auf Dokumentebene
Nach der Gruppierung werden ganze Dokumente verglichen:
- Foto-Dokument gegen hochgeladenes PDF (z. B. Bescheid bei der BH abfotografiert und vom Betrieb als PDF hochgeladen)
- dasselbe Dokument mehrfach im Originalakt
Vorgehen noch offen ([[D19]]); Embeddings je Seite aus V03 sind ein möglicher Baustein. Zusammen mit V04 deckt das die Duplikat-Ebenen aus [[D02]] ab. Nicht zu verwechseln mit Duplikaten auf Auflagenebene ([[V08]]).

## Optimierungen für später
- Vorgelagerte, günstige Einstufung vor V03, damit verworfene Seiten keine teuren Aufrufe verbrauchen
- Effizienteres Auslesen von Text-PDFs (Text direkt statt über das LLM)
- Größeres Fenster in V04
- Gewichte in V04 lernen statt von Hand setzen
- Mehrere Grenzen pro Aufruf vorhersagen (Recherche in [[D27]])
