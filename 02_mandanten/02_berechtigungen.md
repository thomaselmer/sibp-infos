---
id: D06
beschreibung: Berechtigungen – vier Dimensionen, Rollen, Admin-Ebenen, Vertraulichkeit.
status: in Arbeit
stand: 2026-11-12
verweise: [D43, D05, D10, D14]
baut_auf: [D43, D05]
annahmen: []
themen: [Rollen, Berechtigungen, Vertraulichkeit]
---
# 04 – Berechtigungen

## Vier Dimensionen
Ob eine Person etwas darf, ergibt sich aus allen vier zusammen:
1. **Mandantentyp** – welche Features es überhaupt gibt (z. B. Branding nur für Ingenieurbüros)
2. **Plan** – Umfang innerhalb des Mandantentyps (z. B. Anzahl Objekte, Filial-Monitoring)
3. **Rolle im Mandanten** – was die Person tun darf (sehen, bearbeiten, freigeben)
4. **Projektzugriff** – auf welche Betriebsobjekte die Person Zugriff hat

Umsetzung: Feature-Flags / Berechtigungssystem pro Mandantentyp und Plan statt fest verdrahteter Logik.

## Rollen (Start)
- Admin, Bearbeiter, Leser
- Freigabe/Unterschrift des Prüfberichts nur durch Prüfer

## Admin-Ebenen (Details offen)
- **Plattform-Admin** (Alpsoft)
- **Mandanten-Admin** (Nutzer, Plan, Abrechnung des eigenen Mandanten)
- **Projekt-Admin** (Zugriffe für ein einzelnes Objekt)
- Offen: Was darf welcher Admin, und **wann** (z. B. nach Freigabe eines Berichts keine Änderung mehr, nur neue Version).

## Vertraulichkeit (Grundprinzip, Details später)
Inhalte sind sensibel: Betriebsgeheimnisse, offene Mängel – ein Leak kann gefährlich sein. Vertrauen ist zentral.
- Plattform-Admins sehen standardmäßig **keine Kundeninhalte**, nur Verwaltungsdaten
- Support-Zugriff nur mit ausdrücklicher, zeitlich begrenzter Freigabe des Kunden
- Jeder Zugriff wird protokolliert und ist für den Kunden einsehbar
- Langfristig: Verschlüsselung pro Mandant
- Innerhalb der Mandanten: nach Zugriffsentzug (z. B. Büro-Wechsel) wirklich kein Zugriff mehr

**Siehe auch:** [[D43]] (Mandant, Nutzer, Zugriff) · [[D05]] (Mandantentypen) · [[D14]] (technische Umsetzung der Vertraulichkeit) · [[D10]] (Zugriffsentzug bei Akt-Übergabe)
