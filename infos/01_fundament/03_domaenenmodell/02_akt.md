---
id: D44
beschreibung: Domänenmodell-Segment Akt – Dokument, leeres Behördendokument und Seite.
status: in Arbeit
stand: 2026-11-15
verweise: [D03, D04]
annahmen: []
themen: [Akt, Dokument, Leeres Behördendokument, Seite]
---
# 02 – Akt

Teil von [[D03]]. Stand: 15.11.2026 – aus D03 ausgelagert.

## Instanzen
- **Dokument** – Teil des Akts (Bescheid, Gutachten, Plan, Foto …), mit Typ, ggf. Unterkategorie (bei sonstigen Dokumenten) und Herkunft. Arten und Rangfolge: [[D04]]
 - **Leeres Behördendokument:** bekannt (Geschäftszahl, Datum, Typ) aus einem Prüfdokument, aber nicht im Akt. Kein eigenes Feld: Behördendokument ohne Text = fehlt. Aus ihm entstehen keine Key-Infos, Key-Infos können aber darauf verweisen – [[D04]]
- **Seite** – einzelnes Foto/Scan, gehört zu einem Dokument, verschiebbar
