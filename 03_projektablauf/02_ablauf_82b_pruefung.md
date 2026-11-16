---
id: D08
beschreibung: Detaillierter Durchlauf der 82b-Prüfung in neun Schritten, noch aus 82b-Sicht formuliert.
status: muss verallgemeinert werden
stand: 2026-11-12
verweise: [D07, D11, D15]
annahmen: []
themen: [82b-Prüfung, Ortsbegehung]
---
# 05 – Ablauf 82b-Prüfung

1. **Onboarding** – Stammdaten, Branche (z. B. Autowerkstatt), geführte Fragen (Hebebühne? Lackierhalle? Behinderten-WC?) -> Recommender schlägt Prüfkatalog mit wiederkehrenden Prüfpflichten vor. Datenquelle: Liste prüfpflichtiger Objekte von SiBP (Format offen).
2. **Aktenaushebung** – Fotos aus dem Behördenakt (BH) plus elektronische Unterlagen des Betriebs hochladen.
3. **Digitalisierung und Gliederung** – OCR, Seiten zu Dokumenten clustern, Dokumenttyp erkennen, Duplikate erkennen. Pläne werden erkannt, aber vorerst nicht ausgewertet.
4. **Auflagen extrahieren und abgleichen** – Auflagen gliedern (Prozess wird noch detailliert), mit Nachweisen matchen: erfüllt, offen, hinfällig, geändert/nachgeschärft.
5. **Offene Punkte kommentieren** – Ingenieurbüro oder Firma: "erledigt", "nicht relevant", "Maßnahme nötig" usw.
6. **Ortsbegehung** – Checkliste aus offenen Punkten; vor Ort abhaken, Fotos, Kommentare, neue Mängel erfassen. MVP: Checkliste als PDF/Ausdruck, Fotos danach im Büro zuordnen. Später: mobile App, ggf. offline.
7. **Risikoklassifizierung** – Mängel nach SiBP-Risikomatrix (lebensgefährlich bis nicht relevant).
8. **Maßnahmenplan** – vordefinierte Umsetzungswege, Prioritäten, Fristen.
9. **Prüfbericht** – Snapshot, Freigabe durch Prüfer, danach unveränderlich (neue Version möglich).

## Nach dem Bericht
- Maßnahmen abarbeiten und nachverfolgen
- Laufende wiederkehrende Prüfungen mit Fälligkeiten, Kalender, Erinnerungen
- Änderungen am Objekt (neue Bescheide, Umbauten) fließen ins Projekt ein
- Nach Ablauf des Intervalls (konfigurierbar) startet der Zyklus neu – idealerweise ist der Bericht dann fast ein Nebenprodukt der laufenden Arbeit

**Siehe auch:** [[D07]] (allgemeiner Projektablauf) · [[D11]] (82b als Pfad) · [[D15]] (Technik zu den Schritten 3 und 4)
