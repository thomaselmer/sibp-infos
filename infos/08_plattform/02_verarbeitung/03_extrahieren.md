---
id: D25
beschreibung: Extrahieren – Auflagen aus normalen Behördendokumenten und Bestätigungen aus Prüfdokumenten, je als Key-Info.
status: grob
stand: 2026-11-15
verweise: [D15, D24, D04, D46, D48, D38]
baut_auf: [D46, D48, D04, D24]
annahmen: []
themen: [Auflagen extrahieren, Herkunftsregeln, Bestätigung, Zusatzinformation, Klassifizierung]
nodes:
  - {id: V08, eingang: V07, ausgang: V10}
  - {id: V09, eingang: V07, ausgang: V10}
---
# 03 – Extrahieren (V08–V09)

Teil von [[D15]]. Fachliche Grundlage: [[D04]]. Datenobjekt Key-Info: [[D46]].

## V08 – Auflagen extrahieren
- Nur aus **normalen** Bescheiden und Verhandlungsschriften, nur aus den relevanten Abschnitten ([[V06]]) – spart Kosten, verbessert Qualität.
- **MVP: eine Nummer = eine Auflage** aus den nummerierten Listen.
- Ergebnis: je Auflage eine **Key-Info vom Typ Auflage** = Root-Element für V10/V11 ([[D46]]).
- **Auflagenart:** einmalig oder wiederkehrend (Intervall, z. B. "alle 6 Monate"). Wiederkehrende Auflagen werden nicht agentisch untersucht, sondern zur Prüfpflicht ([[D48]]).
- Herkunftsregeln ([[D04]]):
 - Auflagen aus Bescheid und Verhandlungsschrift extrahieren.
 - Gleiche Auflage in beiden = Duplikat -> Bescheid-Fassung gilt.
 - Fehlt der Bescheid, gelten die Auflagen aus der Verhandlungsschrift.
- Bekannte Lücke: Auflagen/Ergänzungen im Fließtext (Stellungnahmen von Amtssachverständigen u. a.) werden im MVP nicht erfasst.

## V09 – Informationen aus Prüfdokumenten extrahieren
- Prüfdokumente liefern **Informationen (Bestätigungen), keine Auflagen** – je Punkt eine **Key-Info vom Typ Zusatzinformation**.
- Ebenfalls nummerierte Aufzählungen; dazu Geschäftszahl(en) und Datum der geprüften Dokumente.
- **Klassifizierung gleich beim Auslesen:** jeder Punkt bekommt seine Art – erfüllt / hinfällig / ergänzt (Zusatzinformation) – und einen Status. Das LLM klassifiziert hier, damit [[V10]] ohne LLM auskommt.

## Optimierungen für später
- Auflagen und Ergänzungen im Fließtext von Verhandlungsschriften erfassen (V08)
