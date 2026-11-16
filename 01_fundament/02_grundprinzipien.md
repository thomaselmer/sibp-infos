---
id: D02
beschreibung: Grundprinzipien – Fehlertoleranz, automatisch aber transparent, iterativ, Teilkorrekturen, Duplikate, Quellverweise.
status: stabil
stand: 2026-11-13
verweise: [D03, D15, D21, D23]
annahmen: [A02, A03, A05]
themen: [Fehlertoleranz, Transparenz, menschliche Korrekturen, Duplikate, Akt-Editor]
---
# 02 – Grundprinzipien

Annahmen: [[A02]], [[A03]], [[A05]]

## Verarbeitung ist fehlerbehaftet
Extraktion und Clustering sind nicht deterministisch und werden nie 100 % richtig sein. Dazu kommen menschliche Fehler (vergessene Seiten/Dokumente, falsch abgelegte Seiten im Originalakt – siehe [[A02]]). Einzelne gescheiterte Seiten sind tolerierbar und führen nie zum Abbruch ([[A05]]).

## Automatisch, aber transparent
- Das System entscheidet automatisch (z. B. Duplikat entfernen, bessere Fotoversion behalten), statt den Nutzer ständig zu fragen.
- Jede automatische Entscheidung wird nachvollziehbar protokolliert, ist einsehbar und rückgängig zu machen.
- Nur wirklich unsichere Fälle landen aktiv beim Nutzer.
- Nicht ausgewertete Inhalte (Fotos ohne Text, Pläne, unlesbare Seiten) werden mit Grund angezeigt.

## Iterativ statt linear
- Viele Einstiegspunkte: Seiten oder Dokumente können jederzeit nachgeladen werden.
- Neues fügt sich in den bestehenden Akt ein; nur Neues wird verarbeitet.
- Menschliche Korrekturen und Freigaben werden nie überschrieben.

## Nicht blockierend, Teilkorrekturen
- Verarbeitung läuft durch, Nutzer korrigiert parallel.
- Eine Korrektur stößt nur die betroffenen Folgeschritte neu an.
- Voraussetzung: lückenlose Herkunftskette (Auflage -> Seite -> Dokument).

## Korrigierbarer Akt
- Editor: Seiten verschieben, Dokumente teilen/zusammenführen, Typ und Reihenfolge ändern.
- Sammel-PDFs mit mehreren Dokumenten werden nur hier von Hand getrennt – siehe [[A03]].
- Korrekturen langfristig nutzbar zur Verbesserung der Erkennung.

## Duplikate auf mehreren Ebenen
- Seite doppelt fotografiert (bessere Version gewinnt) – [[V04]]
- Dokument als Foto und als PDF – [[V05]]
- Dokument bereits im Originalakt mehrfach – [[V05]]

## Quellverweise
Jede extrahierte Auflage verweist auf die Originalseite; idealerweise mit Sicherheitsangabe.

## Alles als Instanz
Jedes Konzept als eigenständige, verknüpfbare Instanz modellieren (siehe [[D03]]).

## Technische Umsetzung
Siehe [[D15]].
