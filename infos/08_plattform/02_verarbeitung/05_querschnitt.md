---
id: D27
beschreibung: Querschnittsthemen der Verarbeitung – Umfang, Einstiegspunkte und Ungültigkeit, Fehlerbehandlung, PDF-Sonderfälle, Verhalten, Recherche.
status: in Arbeit
stand: 2026-11-13
verweise: [D15, D23, D25, D02, D19, D38]
baut_auf: [D02]
annahmen: [A05, A06]
themen: [Umfang, Neu-Beantwortung, Einstiegspunkte, Ungültigkeit, Fehlerbehandlung, PDF-Sonderfälle, Fehlertoleranz]
---
# 05 – Querschnitt der Verarbeitung

Teil von [[D15]].

## Umfang
- Von einem einzelnen Bescheid (~15 Seiten) bis über 5.000 Seiten; langfristig vermutlich mehr.
- Folgen: Verarbeitung läuft im Hintergrund, Fortschritt sichtbar, Teilergebnisse früh nutzbar; Kosten pro Seite werden bei großen Akten zum echten Thema.

## Einstiegspunkte und Ungültigkeit
Beim Nachladen startet nicht die ganze Verarbeitung neu. Der Ablauf hat mehrere Einstiegspunkte; je nach Einstiegspunkt werden bestimmte Daten ungültig.
- **Ergänzung:** z. B. vergessene Seite zu einem bestehenden Dokument, oder ein Dokument wird im Editor geteilt – nur Betroffenes neu berechnen und ungültig setzen.
- **Erweiterung:** z. B. Hotelerweiterung mit neuen Unterlagen – der alte Stand bleibt gültig, Neues setzt darauf auf. Beispiele: später zugestellter Bescheid ersetzt Auflagen aus der Verhandlungsschrift ([[V08]]); nachgeladener fehlender Bescheid befüllt ein leeres Behördendokument ([[V10]]).
- **Neu-Beantwortung (bestätigt):** wird das Beantworten erneut angestoßen, beginnt es **immer nur mit den Root-Elementen** (Auflagen); Subauflagen ergeben sich daraus neu.
- Offen: ob der alte Status abgelegt (versioniert) wird oder verloren gehen darf ([[D19]]).
- Spannung: Menschliche Korrekturen und Kommentare dürfen beim Ungültigsetzen nicht verloren gehen ([[D02]]).

## Fehlerbehandlung
- **Einzelne Seiten dürfen scheitern** (unlesbar, schlechte Qualität, falsch zugeordnet): nie Abbruch der Verarbeitung, schon gar kein Systemabsturz. Seite markieren und mit Grund als "nicht ausgewertet" anzeigen ([[A05]]).
- Unlesbare Seiten in einem Bescheid eher melden als im Schriftverkehr ([[A06]]).
- **PDF zu groß für einen Aufruf:** bisher in Tests nie vorgekommen (auch bei sehr großen Akten), muss aber abgefangen werden – wie, ist offen (entfällt weitgehend, falls PDFs seitenweise ausgelesen werden, [[V03]]).

## Sonderfälle bei PDFs (für die Verarbeitungslogik, keine Annahmen)
- gemischte PDFs (teils Text, teils Scan) – durch den LLM-Ansatz in V03 abgedeckt
- geschützte PDFs (Passwort, Sperre) – evtl. nicht lesbar
- eingebettete Inhalte (Anhänge, Formulare, Skripte) – ignorieren, nie ausführen
- Pläne als großformatige PDF-Seiten
- gedrehte oder schiefe Seiten
- Fotos der Betriebsanlage innerhalb von PDFs
- dasselbe Dokument als PDF und als Foto ([[V05]])

## Wie sich die Verarbeitung verhalten muss
Siehe [[D02]]: fehlertolerant, automatisch aber transparent, iterativ, nicht blockierend mit Teilkorrekturen, lückenlose Herkunftskette, menschliche Korrekturen nie überschreiben.

## Recherche
- Page Stream Segmentation mit LLMs, Versicherungsbranche: https://aclanthology.org/2025.coling-industry.26/ – Fehler auf Seitenebene summieren sich, Mensch im Ablauf bleibt nötig
- Kostengünstiger: mehrere Grenzen pro Aufruf (MSBD): https://arxiv.org/abs/2609.22620
- DocSplit-Benchmark (Seiten außer der Reihe, verschachtelte Dokumente): https://arxiv.org/html/2602.15958v1
