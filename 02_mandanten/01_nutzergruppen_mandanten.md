---
id: D05
beschreibung: Mandantentypen, Kleinbetrieb gegen Konzern, Hierarchien und Büros.
status: stabil
stand: 2026-11-12
verweise: [D09, D10]
annahmen: []
themen: [Mandantentypen, Kleinbetrieb, Konzern, Ingenieurbüro]
---
# 01 – Nutzergruppen und Mandantentypen

## Mandantentypen
| Mandantentyp | Beschreibung | Phase |
|---|---|---|
| Unternehmen | Betrieb mit einem oder mehreren Betriebsobjekten/Standorten | MVP (eingeschränkt) / später voll |
| Ingenieurbüro | Betreut Objekte verschiedener Kunden (z. B. SiBP) | MVP |
| Architekt / Planer | Nutzt Akt als Planungsgrundlage | Später |
| Behörde | BH, Gemeinden | Langfristig, Rolle offen |

## Spannweite der Kunden
- **Kleinbetrieb (1–2 Personen):** will nur seine Prüfung, möglichst einfach und günstig, evtl. Einmalkauf statt Abo. Kann später ins laufende Monitoring hineinwachsen.
- **Konzern (z. B. XXXLutz):** viele Standorte, Hierarchien, Monitoring mit Karte, viele Nutzer, evtl. SSO.
- Beide laufen auf demselben Kern; der Kleinbetrieb darf nicht von Komplexität erschlagen werden.

## Hierarchie bei Unternehmen
- Konzernebene: Übersicht/Karte mit Status aller Objekte – siehe [[D09]]
- Standortleiter: betreut sein eigenes Objekt
- Ausführende: z. B. Hausmeister für laufende Kontrollgänge
- Externe Fachfirmen (z. B. Aufzugswartung): vorerst **außerhalb** des Systems – siehe [[D10]]

## Ingenieurbüros
- Zuordnung **pro Objekt**, nicht pro Unternehmen (XXXLutz Salzburg -> SiBP, Wien -> anderes Büro).
- Jedes Büro sieht nur seine betreuten Objekte; Übersicht darüber siehe [[D09]].
- Eigenes Branding für Ausgaben/Berichte; Unternehmen nutzen das Standard-Layout.

## Übergabe von Akten
Ausgelagert nach [[D10]].
