# CLAUDE.md – Arbeitsanleitung für den Ordner "paragraph82b – Konzept"

Diese Datei regelt nur, **wie** mit dem Ordner gearbeitet wird. Inhalte zum Projekt stehen ausschließlich in den Beschreibungsdateien.

## Schutz dieser Datei
- Beim Neuschreiben der CLAUDE.md darf **keine Regel entfernt oder abgeschwächt** werden, ohne dass Thomas es ausdrücklich freigibt.
- Vorher Thomas zeigen, was sich ändert (entfernt / geändert / neu). Kürzen heißt nur: gleiche Aussage knapper, nie weniger Inhalt.

## Einstieg in jede Session
Auslöser: Thomas sagt sinngemäß "Bitte lies die CLAUDE.md von paragraph82b". Damit beginnt eine frische Session.
1. Diese Datei lesen.
2. `00_index.md` (Wurzel) lesen.
3. `13_handover.md` lesen – das Handover der letzten Session.
4. **Inhaltlichen Einstiegspunkt finden:** die Datei(en) aus "Nächste Session" im Handover vollständig lesen.
5. **Eine Ebene ausholen:** alle `[[...]]`-Verweise in diesen Dateien öffnen (inkl. referenzierter Annahmen in `12_annahmen.md` und `10_offene_punkte.md`). Nicht weiter in die Tiefe. Ziel: sehen, wo wir im Gesamtbild stehen, damit Thomas nichts nachreichen muss.
 Schritte 2 bis 5 passieren **automatisch, ohne bei Thomas nachzufragen.**
6. Während der Session gilt "Kontext nachladen" (unten). In Unterordnern immer zuerst deren `00_index.md`.
7. **Kurzes inhaltliches Briefing nur zum Einstiegspunkt:** in wenigen Sätzen, wo wir im Gesamtbild stehen und was im Fokus-Bereich steht bzw. fehlt – für eine gemeinsame Gesprächsbasis. Kein Durchgehen des ganzen Projekts. Dann den Plan für die Session nennen und mit den Einstiegsfragen aus dem Handover beginnen.

## Kontext nachladen (Pflicht, laufend)
- Sobald im Gespräch ein Thema, Begriff oder Bereich auftaucht, zu dem es eine Datei gibt (Verweis, Themenregister, Index), diese Datei **sofort und ohne Nachfrage** öffnen, bevor geantwortet wird.
- Beispiel: Gespräch dreht sich um Datenstruktur -> Domänenmodell lesen.
- Ziel: bestehendes Wissen nutzen, Widersprüche früh erkennen, Thomas muss nichts wiederholen.
- Bereits in der Session gelesene Dateien nur neu laden, wenn sie inzwischen geändert wurden.

## Phase
Wir sind in der **Planungsphase**. Ziel: erst vollständig verstehen und beschreiben, was gebaut wird, bevor gebaut wird. Keine Code- oder Umsetzungsdiskussion, außer Thomas fragt ausdrücklich danach.

## Gesprächsführung
- **Ein Fokus pro Session:** Am Anfang festhalten, woran gearbeitet wird.
- **Parkplatz für Abzweigungen:** Nebenthema kurz benennen, mit einem Satz in `10_offene_punkte.md` parken, zum Fokus zurückführen. Vertiefen nur, wenn Thomas es will.
- **Nicht zu viel auf einmal:** Fragen und Hinweise einzeln oder in kleinen Portionen.
- Thomas erzählt oft Schritt für Schritt aus dem Prototyp; nicht vorgreifen, sondern nachfragen, ob ein Punkt abgeschlossen ist.

## Zusammenfassen, Sessionwechsel und Handover
- **Hinweis auf guten Zeitpunkt:** Ist ein zusammenhängender Teil durch, darauf hinweisen, dass jetzt ein guter Zeitpunkt zum Zusammenfassen wäre. Thomas entscheidet. Nicht ungefragt schreiben.
- **Schreiben:** nach Zustimmung kompakt in die passenden Dateien eintragen, nicht nach jeder einzelnen Interaktion.
- **Neue Session vorschlagen:** wenn ein Teil geschrieben ist und die Session lang geworden ist. Thomas entscheidet.
- **Handover:** Entscheidet sich Thomas für eine neue Session, schreibt Claude `13_handover.md`. Immer **genau ein** Handover: Datei vollständig ersetzen (neue Version anlegen, alte in den Papierkorb).
 - **Erst ausarbeiten, dann schreiben:** Das Handover wird zuerst **gemeinsam mit Thomas inhaltlich ausgearbeitet** – Claude stellt alle vier Teile vor, Thomas ergänzt oder korrigiert. Geschrieben wird erst, wenn Thomas den Inhalt **ausdrücklich freigibt**. Eine Korrektur oder ein Themenwunsch ist **keine** Freigabe; im Zweifel nachfragen.
 - **Aufbau (kurz halten):**
 1. **Letzte Session:** kurze Zusammenfassung, was gemacht wurde, mit Verweisen auf die Dateien.
 2. **Nächste Session:** was wir am Ende vereinbart haben, mit Kennung des Einstiegspunkts (z. B. `[[V05]]`).
 3. **Einstiegsfragen:** falls vorhanden, ein paar Fragen, mit denen die nächste Session direkt beginnt.
 4. **Nicht in den Dateien:** nur falls etwas besprochen, aber noch nicht eingetragen ist.
 - Das Handover ersetzt keine Fachdateien – es verweist nur darauf.

## Länge der Dateien
- CLAUDE.md, Handover und alle Notizen **kurz und knapp** halten: Stichpunkte statt Fließtext, keine Wiederholungen, lieber verweisen als erklären.
- Wird eine Fachdatei zu lang, nach "Komplexe Themen aufspalten" teilen.
- Beim Ergänzen der CLAUDE.md prüfen, ob sich Bestehendes kürzen oder zusammenlegen lässt (Schutzregel oben beachten).

## Arbeitsmethode: vom Groben ins Feine (wie Stable Diffusion)
- Erst das Ganze grob, dann Bereiche, dann Hotspot für Hotspot verfeinern.
- **Baum und Netz:** Ordner geben jedem Thema einen festen Platz (Baum), Verweise spannen das Netz.
- Jeder Bereich wird so weit ausgearbeitet, dass er **in sich abgeschlossen** ist. Keine halbfertigen Baustellen.
- Jede `00_index.md` enthält **Status** (stabil / in Arbeit / HOTSPOT / Vorschlag / ruht / offen), **Stopp / Weiter** und **Verweise**.
- Aktuelle Hotspots stehen im Wurzel-Index.
- Neue Themen dort einordnen, wo sie andocken; Andockpunkte als Verweis festhalten.
- Verfeinerung darf höheren Ebenen nicht stillschweigend widersprechen; Widersprüche melden und bereinigen.

## Bereichsübergreifende Zusammenhänge
- **Selbstständig erkennen:** Berührt ein neuer Punkt einen anderen Bereich (z. B. Verarbeitung -> Preise), im **Themenregister** des Wurzel-Index nachsehen, die Datei laden (siehe "Kontext nachladen") und den Punkt **in der passenden Fachdatei** knapp vermerken, mit Verweisen in beide Richtungen.
- In `10_offene_punkte.md` nur parken, was wirklich offen ist – Parken ersetzt nicht das Eintragen in der Fachdatei.
- **Themenregister pflegen:** Neues Schlagwort -> Zeile im Wurzel-Index ergänzen (Schlagwort, Kennung). Kurz halten, nur tragende Begriffe.

## Annahmen
- Stehen in `12_annahmen.md`, je mit Kennung (A01 …), Status (gilt / widerlegt), bekannten Abweichungen und betroffenen Dateien. Widerlegte bleiben mit Begründung stehen.
- **Nur echte Prämissen**, mit denen das Konzept gilt oder fällt. Fachliche Regeln (z. B. Herkunftsregeln für Auflagen), Sonderfälle und Fehlerbehandlung gehören in die fachliche Datei.
- **Verweise in beide Richtungen:** Annahme nennt die Dateien, jede Datei verweist mit `[[A01]]` zurück.
- **Aktiv nachfragen:** Neue Annahme erkannt -> Thomas fragen, ob sie ergänzt wird. Nicht ungefragt eintragen.

## Verweise und Kennungen
- **Ziel: Verweise bleiben beständig.** Aufspalten, Umbenennen oder Verschieben darf keine Verweise in anderen Dateien brechen.
- Keine Drive-Links und **keine Pfade** in Verweisen. Verwiesen wird **nur über feste Kennungen**: `[[V04]]`, `[[A01]]`, `[[D07]]`.
- **Jede Datei hat eine feste Datei-Kennung** (Präfix D) im Frontmatter (`id: D07`). Sie bleibt beim Umbenennen, Verschieben oder Aufspalten erhalten; beim Aufspalten behält der neue Ordner-Index die alte Kennung, neue Dateien bekommen neue.
- **Einträge innerhalb von Dateien** über Kennungen mit Präfix je Art (A = Annahme, V = Verarbeitungs-Node; weitere bei Bedarf hier ergänzen). Kennungen sind global eindeutig, daher reicht `[[V04]]` ohne Datei.
- **Kennungen sind Nodes, keine Reihenfolge.** Die tatsächliche Reihenfolge steht separat (z. B. Übersichtstabelle).
 - Kennungen werden beim Neuschreiben unverändert mitkopiert, **nie umnummeriert, nie neu vergeben**. Neue Einträge bekommen die nächste freie Nummer.
- **Auflösung:** Der Wurzel-Index führt eine **Kennungstabelle** (Kennung -> aktueller Ort). Bei Umbenennen, Verschieben oder Aufspalten wird nur diese Tabelle (und das Frontmatter der Datei) angepasst, sonst nichts.
- **Übergang:** Bestehende Pfad-Verweise bleiben gültig, solange die Kennungstabelle sie auflöst; umgestellt wird, wenn eine Datei ohnehin überarbeitet wird.

## Frontmatter, Index und Diagramme
Ziel: langfristig ein Flussdiagramm des gesamten Programms, eigene Diagramme für Teillogiken und Index-Tabellen ohne Extra-Aufwand.
- **YAML-Frontmatter** in jeder Datei:
 - `id` (Datei-Kennung), `beschreibung` (ein Satz), `status` (Werte wie im Index), `stand` (Datum)
 - `verweise` (Kennungen), `annahmen` (Kennungen) und `themen` (Schlagworte für das Themenregister)
 - Dateien mit Nodes zusätzlich `nodes`: je Node `id`, `eingang`, `ausgang`, `verzweigungen` (Bedingung -> Ziel-Kennung).
- **Index baut auf dem Frontmatter auf:** Tabellen in `00_index.md` fassen nur `beschreibung` und `status` der Dateien zusammen, das Themenregister die `themen`, die Kennungstabelle `id` und `nodes` (später automatisch erzeugbar). Abweichungen zwischen Index und Frontmatter: Frontmatter gilt.
- **Mermaid-Diagramme** wo möglich im Text einbauen: Kette in der Übersicht, Detail-Logik direkt im Node. Kästen tragen die Kennungen; Grundlage ist das Frontmatter.
- **Nur nachziehen, wenn eine Datei oder ein Index ohnehin überarbeitet wird** – keine Umschreib-Aktion nur dafür.

## Ablage-Regeln
- Jede Datei hat ein klares Thema; Neues in die **passende bestehende Datei**.
- Kein Inhalt doppelt: ein Thema an genau einer Stelle, andere verweisen.
- Neue Datei nur für eigenständige Themen.
- Namen: `NN_thema.md`, Ordner `NN_thema/` (zweistellig, Kleinbuchstaben, Unterstriche), fortlaufend ab 01.
- Bei neuer, umbenannter oder entfernter Datei: `00_index.md` und Kennungstabelle sofort anpassen.
- Recherche-Quellen in der fachlichen Datei unter "Recherche".
- Entscheidungen aus dem Prototyp mit Begründung; spätere Verbesserungen unter "Optimierungen für später".

## Komplexe Themen aufspalten
1. Ordner mit gleicher Nummer und gleichem Namen anlegen (z. B. `02_verarbeitung/`).
2. Inhalt in thematische Dateien aufteilen; Kennungen bleiben erhalten.
3. Eigene `00_index.md` mit Status, Stopp/Weiter, Verweisen; sie übernimmt die Datei-Kennung der ursprünglichen Datei.
4. Ursprüngliche Datei erst entfernen, wenn alles übernommen ist.
5. Übergeordneten Index und Kennungstabelle anpassen – Verweise in anderen Dateien bleiben unverändert.

## Pflege
- Offene Fragen in `10_offene_punkte.md`, nach Bereich; geklärte Punkte dort entfernen und übernehmen.
- Entscheidungen nicht stillschweigend treffen – Vorschläge kennzeichnen, bis Thomas bestätigt.
- Widersprüche ansprechen und bereinigen.
- Im Index "Stand" bei größeren Änderungen aktualisieren.

## Technischer Hinweis
- Dateien in Google Drive als Markdown, ohne Umwandlung in Google Docs.
- Umbenennen und Verschieben über die Metadaten – die Datei bleibt dieselbe.
- Inhalt ändern: neue Version mit vollständigem Inhalt anlegen, dann alte in den Papierkorb – nie halb ersetzte Inhalte.
