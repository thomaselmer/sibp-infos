---
id: D49
beschreibung: Domänenmodell-Segment Mangel und Erheblichkeit – Mangel, Maßnahme, Prüfbericht; Erheblichkeit offen, bewusst offener Endpunkt.
status: grob
stand: 2026-11-15
verweise: [D03, D19, D39]
annahmen: []
themen: [Mangel, Erheblichkeit, Maßnahme, Prüfbericht, Risikoklasse]
blatt: endpunkt
endpunkt: {art: vertiefung, fehlt: "Messung bzw. Skala der Erheblichkeit, Verhältnis zur Risikoklasse (SiBP-Matrix), Umgang mit bei Begehung neu gefundenen Mängeln (mit oder ohne Auflage)."}
---
# 07 – Mangel und Erheblichkeit

Teil von [[D03]]. Stand: 15.11.2026 – aus D03 ausgelagert.

## Instanzen
- **Mangel** – **nicht erfüllte Auflage**; auch: kein Nachweis gefunden = Mangel ([[V11]]). Mit Risikoklasse (SiBP-Matrix) und **Erheblichkeit** (siehe unten). Bei Begehung neu gefundene Mängel: ob mit oder ohne Auflage, offen ([[D19]])
- **Maßnahme** – Teil des Maßnahmenplans, verknüpft mit Mangel/Auflage
- **Prüfbericht** – Snapshot zu einem Stichtag (82b-Intervall als konfigurierbare Variable), nach Freigabe unveränderlich, nur neue Version

## Erheblichkeit des Mangels
- Ergibt sich daraus, **wie viel** einer Root-Auflage samt Subauflagen am Ende offen bleibt.
- Messung bzw. Skala offen – [[D19]]

## Beziehungen
- Auflage nicht erfüllt bzw. ohne Nachweis -> Mangel
- Mangel -> Maßnahme -> Prüfbericht
