---
id: D38
art: inhalt
beschreibung: Matching der Bestätigungen aus Prüfdokumenten mit den Auflagen über Geschäftszahl + Nummer (V10), inkl. fehlender geprüfter Bescheid.
status: grob
stand: 2026-11-15
verweise: [D46, D49, D04, D19, D24, D25, D26, D27]
baut_auf: [D46, D49, D04, D24, D25]
annahmen: []
themen: [Matching, Geschäftszahl + Nummer, fehlender Bescheid, leeres Behördendokument, Mangel]
nodes:
  - {id: V10, eingang: [V08, V09], ausgang: V11, verzweigungen: {"erfüllt/hinfällig": "Auflage erledigt", "ergänzt": "Zusatzpunkt anhängen, offen", "geprüfter Bescheid fehlt": "leeres Behördendokument + Hinweis", "offen": V11}}
blatt: ausformuliert
---
# 01 – Matching (V10)

Teil von [[D26]]. Fachliche Grundlage: [[D04]]. Datenobjekt Key-Info: [[D46]].

## V10 – Bestätigungen den Auflagen zuordnen
- Nummern im Prüfdokument entsprechen den Auflagennummern des geprüften Dokuments -> Zuordnung über **Geschäftszahl + Nummer**.
- **Matching mit normalem Code** (deterministisch, nachvollziehbar, günstig), möglich durch die Klassifizierung in [[V09]].
- **Abbruchkriterium je Auflage:** erfüllt oder hinfällig gefunden -> Auflage erledigt. Ergänzung -> Zusatzpunkt hängt an der Auflage, Auflage bleibt offen.
- Nicht erfüllte Auflage = Mangel ([[D49]]).
- Ein Prüfdokument kann mehrere Bescheide prüfen; Trennung meist textuell erkennbar – Details später.
- **Geprüfter Bescheid fehlt im Akt:**
 - Prüfdokument nennt Geschäftszahl und Datum eines Bescheids, der nicht im Akt liegt -> **leeres Behördendokument** (Typ Bescheid, Geschäftszahl, Datum) anlegen.
 - Erkennbar ohne eigenes Feld: Behördendokument ohne Text = fehlt (echte Behördendokumente haben immer Text, [[V07]]).
 - Bestätigungen verweisen auf das leere Dokument; ihre Herkunft bleibt das Prüfdokument.
 - **Hinweis an den Nutzer:** Bescheid mit Geschäftszahl und Datum konkret bei der Behörde anfordern.
 - Nachgeladen -> Einstiegspunkt "Erweiterung" ([[D27]]); leeres Dokument wird befüllt.
 - Platzhalter-Auflagen und Suche danach vorerst nicht ([[D19]]).
- **Ausgang:** Liste der offenen Auflagen -> Eingang für V11.
