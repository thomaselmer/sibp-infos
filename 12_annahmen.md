---
id: D21
beschreibung: Bereichsübergreifende Annahmen A01–A07 mit bekannten Abweichungen.
status: laufend
stand: 2026-11-13
verweise: [D01, D02, D15, D19, D23]
annahmen: [A01, A02, A03, A04, A05, A06, A07]
themen: [Annahmen]
---
# 12 – Annahmen

Bereichsübergreifende Annahmen: Was als gesetzt gilt, bis es sich als falsch herausstellt. Gegenstück zu [[D19]].

Jede Annahme hat eine feste Kennung (A01, A02 …), die nie neu vergeben wird. Betroffene Dateien verweisen mit `[[12_annahmen#A01]]` zurück.

Status: **gilt** / **widerlegt** (widerlegte Annahmen bleiben mit Begründung stehen).

---

## A01 – Ein Foto enthält eine Seite
- **Status:** gilt
- **Annahme:** Ein Bild zeigt grundsätzlich genau eine Seite des Akts.
- **Bekannte Abweichungen:** Randbereiche oder Teile einer Nachbarseite können mit drauf sein.
- **Betrifft:** [[D15]]

## A02 – Bilder kommen in der Reihenfolge des Akts
- **Status:** gilt
- **Annahme:** Ein Akt wird fortlaufend fotografiert; die Bilder sind in Aktreihenfolge.
- **Bekannte Abweichungen:**
  - doppelte Fotos derselben Seite
  - Fotos von Dingen, die nicht zum Akt gehören
  - Seiten, die schon im Originalakt falsch abgelegt sind – richtig fotografiert, aber in sich durcheinander
- **Konsequenz:** Die Verarbeitung muss mit diesen Abweichungen umgehen; Fehler werden passieren.
- **Betrifft:** [[V04]], [[D02]]

## A03 – Ein PDF ist ein Dokument
- **Status:** gilt
- **Annahme:** Ein hochgeladenes PDF wird als genau ein Dokument behandelt.
- **Bekannte Abweichungen:**
  - PDF mit nur einer Seite
  - PDF, das nur aus Bildern besteht (braucht Texterkennung)
  - mehrere Dokumente zusammen in ein PDF gescannt
- **Bewusste Entscheidung:** Sammel-PDFs bleiben automatisch ein Dokument – das ist automatisch kaum prüfbar. Trennen nur von Hand im Akt-Editor.
- **Betrifft:** [[V03]], [[V04]], [[D02]] (Akt-Editor)

## A04 – Irrelevante Fotos dürfen verworfen werden
- **Status:** gilt
- **Annahme:** Fotos wie Ordnerrücken oder Fehlaufnahmen enthalten nichts, was gebraucht wird, und dürfen aussortiert werden.
- **Bekannte Abweichungen:** Fehleinstufung möglich – deshalb bleiben sie einsehbar und können vom Nutzer zurückgeholt werden.
- **Betrifft:** [[V03]]

## A05 – Einzelne Seiten dürfen scheitern
- **Status:** gilt
- **Annahme:** Ist eine einzelne Seite falsch zugeordnet, unlesbar oder sonst fehlerhaft, ist das tolerierbar. Ziel ist nicht der perfekte Akt, sondern dass der Großteil der Auflagen gefunden und beantwortet wird.
- **Begründung:** statistisch unwahrscheinlich, dass genau die verlorene Seite die wichtigen Informationen enthält – ein großer Teil des Akts ist Schriftverkehr, Firmenbestätigungen usw. (siehe [[A06]]).
- **Konsequenz:** Die Verarbeitung bricht nie wegen einer einzelnen Seite ab; die Seite wird markiert und mit Grund als "nicht ausgewertet" angezeigt.
- **Bekannte Abweichungen:** Die gescheiterte Seite enthält doch eine Auflage.
- **Betrifft:** [[D15]] (Fehlerbehandlung), [[V04]], [[D02]], [[D01]] (Positionierung)

## A06 – Auflagen sind auf wenige Seiten konzentriert
- **Status:** gilt
- **Annahme:** Selbst in einem Bescheid stehen die Auflagen nur auf wenigen Seiten. Über den ganzen Akt gesehen ist nur ein kleiner Teil der Seiten für das Ergebnis relevant.
- **Konsequenz (Kehrseite):** Genau diese wenigen Seiten verdienen besondere Sorgfalt – eine unlesbare Seite in einem Bescheid eher melden als eine im Schriftverkehr.
- **Betrifft:** [[D15]] (Fehlerbehandlung), [[V04]] (Bewertung)

## A07 – Die Gliederung hat keine eindeutige Wahrheit
- **Status:** gilt
- **Annahme:** Auch ein Mensch kann manche Dokumentgrenzen nicht sicher ziehen. Gelabelte Akte gibt es nicht und wird es voraussichtlich nicht geben.
- **Konsequenz:** Die Gruppierung wird nicht an perfekten Grenzen gemessen, sondern am Ergebnis – ob die Auflagen gefunden werden. Korrekturen im Editor sind Hinweise, keine perfekte Wahrheit.
- **Betrifft:** [[V04]]
