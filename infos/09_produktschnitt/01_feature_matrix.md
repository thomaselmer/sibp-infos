---
id: D16
beschreibung: Feature-Matrix – Features nach Mandantentyp, Plan, Rolle und Phase.
status: Vorschlag
stand: 2026-11-12
verweise: [D05, D06, D17, D18, D20]
annahmen: []
themen: [Pläne, Tarife, Features]
---
# 07 – Feature-Matrix (grob, vorläufig)

Legende Mandantentyp: U = Unternehmen, IB = Ingenieurbüro, A = Architekt, B = Behörde
Rollen: Admin, Bearbeiter, Leser, Prüfer (Freigabe)

| Bereich | Feature | Mandantentyp | Plan (Annahme) | Rollen | Phase |
|---|---|---|---|---|---|
| Kern | Betriebsobjekt anlegen, Stammdaten | U, IB | alle | Admin, Bearbeiter | MVP |
| Kern | Onboarding + Prüfkatalog-Vorschlag | U, IB | alle | Admin, Bearbeiter | MVP |
| Kern | Akt hochladen, gliedern, Duplikate | U, IB | alle | Bearbeiter | MVP |
| Kern | Auflagen extrahieren und abgleichen | U, IB | alle | Bearbeiter; Leser lesen | MVP |
| Kern | Akt-Editor, Korrekturen, Ereignisprotokoll | U, IB | alle | Bearbeiter | MVP |
| Prüfung | Kommentieren offener Punkte | U, IB | alle | Bearbeiter | MVP |
| Prüfung | Begehungs-Checkliste (PDF) | IB (U später) | alle | Bearbeiter | MVP |
| Prüfung | Risikoklassifizierung (SiBP-Matrix) | IB | alle | Prüfer | MVP |
| Prüfung | Maßnahmenplan | IB, U | alle | Prüfer, Bearbeiter | MVP |
| Prüfung | Prüfbericht + Freigabe/Versionierung | IB | alle | Prüfer | MVP |
| Büro | Eigenes Branding für Ausgaben | IB | Pro+ | Admin | MVP/früh |
| Büro | Mehrere Kundenobjekte verwalten | IB | nach Objektzahl | Admin | MVP |
| Büro | Akt übergeben/übernehmen | U, IB | alle | Admin | Später |
| Betrieb | Wiederkehrende Prüfungen, Fälligkeiten, Erinnerungen | U, IB | Pro+ | Bearbeiter | Später |
| Betrieb | Maßnahmen-Nachverfolgung | U, IB | Pro+ | Bearbeiter | Später |
| Betrieb | Mobile Erfassung / offline | U, IB | Pro+ | Bearbeiter | Später |
| Konzern | Monitoring mit Karte, Hierarchien | U (IB für eigene Objekte) | Enterprise | Admin, Leser | Später |
| Konzern | SSO | U | Enterprise | – | Später |
| Integration | QM-App-Schnittstelle (bidirektional) | U, IB | Add-on | Admin | Später |
| Pläne | Pläne verorten, Planvergleich, vorausschauende Prüfung | IB, A | Pro+ | Bearbeiter | Vision |
| Weitere | Architekten-Zugang (Planungsgrundlage) | A | offen | – | Vision |
| Weitere | Behörden-Zugang | B | offen | – | Vision |
| Weitere | Hygiene/HACCP, internationale Regelwerke | U | offen | – | Vision |

Hinweis: Plan-Zuordnung ist eine erste Annahme; Plannamen (Basis/Pro/Enterprise) und ein evtl. Einmalkauf für Kleinbetriebe sind offen.

**Siehe auch:** [[D17]] (MVP) · [[D18]] (Roadmap) · [[D05]] (Mandantentypen) · [[D06]] (Rollen und Berechtigungen) · [[D20]] (Preise und Pläne)
