---
id: D19
beschreibung: Offene Fragen und Parkplatz, nach Bereich sortiert, plus allgemeiner Fahrplan der Planungsphase.
status: laufend
stand: 2026-11-15
verweise: [D45, D46, D48, D49, D04, D07, D11, D15, D16, D20, D26, D27, D06, D10, D13, D14, D17, D23, D38, D39]
annahmen: []
themen: [offene Punkte, Parkplatz, Fahrplan, LangChain, Prototyp]
---
# 10 – Offene Punkte

Ungeklärte Fragen und noch nicht getroffene Entscheidungen, sortiert nach Bereich. Geklärte Punkte hier entfernen und in die passende Datei übernehmen. Dient auch als **Parkplatz** für Abzweigungen, die im Gespräch auftauchen.

## Fahrplan (allgemein offen, Reihenfolge vom Groben ins Feine)
Vorschlag, wo es inhaltlich weitergeht; jeder Schritt erst, wenn der vorige in sich rund ist.
1. **Verarbeitung abschließen** ([[D15]]):
 - Einstiegspunkte und Ungültigkeit – was wird beim Nachladen ungültig, wie bleiben Korrekturen und Kommentare erhalten? ([[D27]])
 - Kriterien der Zuordnungsprüfung in V11 – zuordenbar vs. Mismatch ([[V11]])
 - Diagramm V10 -> V11 aus dem Bestand ableiten ([[D26]])
2. **Projektablauf, Äste 4–7** ([[D07]]): Prüfen (Begehung, Kommentare, Mängel/Risikoeinstufung), Maßnahmen, Bericht als Snapshot, laufender Betrieb – was passiert mit dem Ergebnis der Verarbeitung?
3. **Regelwerke und Pfade** ([[D11]]): was genau gehört zu einem Pfad wie § 82b – läuft quer durch die Äste.
4. **Mandanten und Berechtigungen** ([[D06]]): wer darf in welchem Ast was?
5. **Produktschnitt** ([[D17]], [[D16]]): MVP gegen das fertige Gesamtbild prüfen.
6. **Prototyp-Vergleich** inkl. Frage LangChain (siehe 08_plattform unten).

## 01_fundament
- Sonstige Dokumente: Unterkategorien verfeinern, wenn mehr bekannt (E-Mail, Prüfblatt, Bestätigung …) – [[D04]]
- Kommen Überprüfungen auch in Bescheiden vor (nicht nur in Verhandlungsschriften)? – [[D04]]
- Typische Abschnitte von Bescheid und Verhandlungsschrift: Liste aus dem Prototyp übernehmen – [[D04]]
- Weitere Quellen für Vorschriften/Auflagen neben Bescheid und Verhandlungsschrift (später)
- Fehlender geprüfter Bescheid: aus dem Prüfdokument Platzhalter-Auflagen anlegen und schon nach Nachweisen suchen? Vorerst nur Hinweis an den Nutzer (Wortlaut der Auflagen nur bruchstückhaft bekannt) – [[D04]], [[V10]]
- Verfahren: als eigene Instanz überhaupt nötig (Vorschlag Claude, nicht bestätigt)? Wenn ja, wie automatisch erkennbar (Geschäftszahlen oft fehlerhaft ausgelesen)? – [[D45]]
- Bei Begehung neu gefundener Mangel: mit zugehöriger Auflage oder ohne? – [[D49]]
- Risikoklasse (SiBP-Matrix) auch für Auflagen und erfasste Punkte, nicht nur für Mängel – wo andocken? (Vorschlag) – [[D49]]
- Key-Info: Art der Verknüpfung – Elternverweis oder typisierte Verknüpfung (z. B. bestätigt, ergänzt, leitet ab)? – [[D46]]
- Key-Info: Herkunft "Nutzer" modellieren – Kommentar als eigener Dokumenttyp (aus Akt und Seitenzählung ausgeschlossen) oder Herkunft Dokument-oder-Nutzer? Bewusst offen – [[D46]]
- Key-Info: welche Status-Kategorien sind abbrechend (mind. erfüllt, hinfällig – weitere?) – [[D46]]
- Kommentare: hängt ihr Gewicht (z. B. darf "erfüllt" abbrechen) von Verfasser (Inhaber, Ingenieurbüro) und Kontext (Angabe, Begehung) ab? – [[D46]]
- Erheblichkeit des Mangels: wie gemessen (Anteil offener Subauflagen, Skala)? Verhältnis zur Risikoklasse (SiBP-Matrix)? – [[D49]]
- Mitteilung bereits erfüllter Auflagen (Nachweis aus sonstigem Dokument) an die Behörde: Form und Ort – [[D04]]
- Wiederkehrende Auflagen: auch durch V10 (Bestätigungen aus Prüfdokumenten) beantworten oder nur über Fragebögen? Status je Zyklus ("erfüllt" gilt nur für den laufenden Zyklus) – [[D48]]

## 02_mandanten
- Admin-Ebenen: Wer darf was und wann (z. B. nach Berichtsfreigabe)? – [[D06]]

## 03_projektablauf
- Endgültiger Name für den "Projektablauf" (Arbeitstitel) – [[D07]]
- Format der SiBP-Liste prüfpflichtiger Objekte (Onboarding)
- Kommentare (Key-Info) bei späterer Prüfung/Begehung zu noch nicht beantwortbaren Punkten: in welchem Ast andocken? – [[D07]], [[D46]]

## 05_zusammenarbeit
- Akt-Übergabe: Wer entscheidet bei einem Büro-Wechsel, ob der Akt mitgeht? – [[D10]]
- Externe Fachfirmen (z. B. Aufzug): eigener Zugang oder nur Berichte hochladen? (vorerst außerhalb)

## 06_regelwerke_pfade
- Was gehört genau zu einem Pfad? – [[D11]]
- Internationalisierung: Zeitpunkt, Regelwerk-Module

## 07_erweiterungen_integrationen
- Rolle und Integration der Behörden
- QM App: Umfang der Schnittstelle – [[D13]]

## 08_plattform
- **Verarbeitung (Hotspot):**
 - V06–V10 verfeinern – [[D15]]
 - V11: mehrstufige Subauflagen (Rekursion) – Abbruch bei Tiefe/Kosten, wann ist die Suche erschöpft? MVP vorerst eine Stufe – [[V11]]
 - V11: Zuordnungsprüfung (zuordenbar vs. Mismatch) – Kriterien – [[V11]]
 - V11: Beteiligte/Gewerke der sonstigen Dokumente – vorab erfassen (V03 bzw. Extraktion nach V07) oder erst durch den Agenten? – [[V11]]
 - V11: Stellschrauben im Prototyp testen (Schichten, Datum als Reihenfolge, Gewichtung der Indizien, Schlagworte) – [[V11]]
 - V03: PDFs in Einzelseiten zerlegen und seitenweise auslesen (vorgemerkt, ändert MVP-Entscheidung) – endgültig bestätigen – [[V03]]
 - Prüfdokument mit mehreren Bescheiden: Nummern den richtigen Bescheiden zuordnen – [[V10]]
 - Konkrete Merkmale für die Gruppierung: Thomas' Liste mit dem Prototypen abgleichen, Lücken suchen – [[V04]]
 - Vorschlag zwei Schwellen / Grauzone in der Gruppierung bestätigen oder verwerfen – [[V04]]
 - Duplikate auf Dokumentebene: Vorgehen (Embeddings pro Seite als möglicher Baustein) – [[V05]]
 - Einstiegspunkte und Ungültigkeit: welche Einstiegspunkte, was wird jeweils ungültig, wird der alte Status versioniert? Wie bleiben menschliche Korrekturen (auch Kommentare) erhalten, wenn ab Root neu beantwortet wird? – [[D27]]
 - Fehlerbehandlung: Vorgehen bei PDFs, die zu groß für einen Aufruf sind – [[D27]]
 - Modellwahl, Kosten pro Akt, Qualität messen – [[D14]]
- **Prototyp-Technik:** läuft auf LangChain – nach dem Durchsprechen prüfen, ob das die richtige Grundlage ist (Claude soll darauf hinweisen)
- **Vergleich mit dem Prototypen:** nach dem Durchsprechen das Gesamtbild mit dem Prototypen abgleichen – Übereinstimmungen, Differenzen, Lücken
- Vertraulichkeit: konkrete technische Umsetzung (Support-Freigabe, Verschlüsselung pro Mandant)
- Mobile App und Offline-Fähigkeit: Zeitpunkt, Sync-Konzept

## 09_produktschnitt
- Pläne/Tarife: Namen, Umfang, Einmalkauf für Kleinbetriebe – [[D16]]

## 11_geschaeftliches
- Partnerschaftskonditionen mit SiBP (Thomas bringt zusätzlich Vertriebskanäle ein)
- QM App: Partnerschafts- und Provisionskonditionen
- Haftung und Formulierung im Marketing/AGB (nicht "rechtssicher" versprechen); dazu: Haftung bei Kommentaren, die "erfüllt" setzen – [[D20]]
- Pilotkunde: XXXLutz-Kontakt aktiv halten
- Preismodell: Projekte nach maximaler Seitenanzahl bepreisen (Grundlage: einheitliche Seitenzählung, auch PDFs seitenweise – [[V03]])
