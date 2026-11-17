---
id: D14
beschreibung: Plattform-Bausteine mit Verweisen, gesammelt und nicht ausformuliert.
status: gesammelt, nicht ausformuliert
stand: 2026-11-12
verweise: [D02, D06, D07, D09, D13, D15, D16]
baut_auf: [D02, D06]
andockpunkt: [D13]
annahmen: []
themen: [Modellwahl, Kosten, Stripe, Bausteine]
---
# 01 – Plattform-Bausteine

Technische Grundlage unter allen Bereichen. Noch nicht ausformuliert – hier nur gesammelt, damit nichts verloren geht.

| Baustein | Inhalt (grob) | Verweise |
|---|---|---|
| Mandantenfähigkeit / Mandantenlogik | Mandantentrennung, Feature-Flags pro Mandantentyp und Plan | [[D06]] |
| Authentifizierung | Login, Rollen; später SSO für Konzerne | [[D09]] |
| Abrechnung und Zahlung | Richtung **Stripe** (Thomas); Pläne, evtl. Einmalkauf | [[D16]] |
| Frontend / Backend | noch nicht ausdefiniert | – |
| Speicherung | Akte, Seiten, Dokumente, Fotos | – |
| Logging / Ereignisprotokoll | automatische Entscheidungen, Korrekturen, Zugriffe | [[D02]] |
| Systemmonitoring | Überwachung des Systems selbst (nicht Objekt-Monitoring) | – |
| Sicherheit und Datenschutz | DSGVO, EU-Hosting, Backups, Verschlüsselung pro Mandant, Zugriffsprotokoll für Kunden | [[D06]] |
| Benachrichtigungen | Erinnerungen, Fälligkeiten | [[D07]] |
| Dokumenterzeugung | Berichte, Checklisten, Exporte als PDF, Branding des Büros | – |
| Backoffice | Plattform-Admin: Pläne, Support-Freigaben, Feature-Flags | [[D06]] |
| KI-Anbindung | Modelle für Texterkennung und Extraktion, Kosten, Abrechnung pro Mandant | [[D15]] |
| Schnittstellen / API | Andocken der QM App und anderer Systeme | [[D13]] |
| **Verarbeitung** | Upload -> Ergebnis – **HOTSPOT** | [[D15]] |
