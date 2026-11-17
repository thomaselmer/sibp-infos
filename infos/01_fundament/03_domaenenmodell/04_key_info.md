---
id: D46
beschreibung: Domänenmodell-Segment Key-Info – ein Objekt für Auflagen, Bestätigungen, Textpassagen und Kommentare – Typen, Herkunft vs. Verknüpfung, Status, Kommentare, Rekursion.
status: in Arbeit
stand: 2026-11-15
verweise: [D03, D04, D07, D19, D20]
andockpunkt: [D07]
annahmen: []
themen: [Key-Info, Herkunft vs. Verknüpfung, Status, Kommentar, Verfasser/Kontext von Kommentaren, Subauflage, Root-Ebene]
---
# 04 – Key-Info

Teil von [[D03]]. Stand: 15.11.2026 – aus D03 ausgelagert.

## Objekt
- **Ein Objekt, verschiedene Typen** (bestätigt, aus dem Prototyp):
 - Auflage (aus Bescheid/Verhandlungsschrift, V08)
 - Zusatzinformation / Bestätigung (aus Prüfdokument, V09)
 - Textpassage aus einem sonstigen Dokument (Treffer aus V11)
 - Kommentar (menschlich erfasst, z. B. bei späterer Prüfung/Begehung zu noch nicht beantwortbaren Punkten – [[D07]])
- **Einheitlich:** Textpassage aus sonstigem Dokument landet im **gleichen Datenobjekt** wie eine Bestätigung aus einem Prüfdokument; unterscheidet sich nur in Typ und Herkunft.
- **Herkunft und Verknüpfung sind getrennte Beziehungen:**
 - **Herkunft** = woraus die Key-Info stammt: ein **Dokument** oder ein **Nutzer** (Kommentar). Auch Subauflagen stammen aus einem Dokument (z. B. Brandschutzplan), nicht aus der Eltern-Key-Info. Regelwerke/Gesetze oder externe Systeme (QM App) wären als Dokument abbildbar.
 - **Verknüpfung** = worauf sie sich bezieht: andere Key-Info (Eltern/Root) oder Dokument (z. B. Bestätigung -> leeres Behördendokument).
 - Modellierung der Herkunft "Nutzer" offen: Kommentar als eigener Dokumenttyp (aus Akt und Seitenzählung ausgeschlossen) oder Herkunft Dokument-oder-Nutzer – [[D19]]
- **Status:** bestätigende Key-Infos bekommen beim Erzeugen gleich einen Status (erfüllt / hinfällig / ergänzt …). Bestimmte Status sind **abbrechend** (mind. erfüllt, hinfällig) und beenden die Suche für diesen Zweig.
- **Herkunft** wird immer mitgeführt (Behörde vs. sonstiges Dokument vs. Kommentar). Erfüllt bleibt erfüllt (Abbruchkriterium), aber es ist sichtbar, was nicht behördlich bestätigt ist – Grundlage, um der Behörde bereits erfüllte Auflagen mitzuteilen ([[D04]]).
- **Kommentare:**
 - Vermerken **Verfasser** (z. B. Inhaber, Ingenieurbüro) und **Kontext** (z. B. nur Angabe des Inhabers, Begehung).
 - Dürfen grundsätzlich **"erfüllt"** setzen; die Verantwortung trägt der Verfasser (z. B. Inhaber unterschreibt und legt der Behörde vor). Rechtliche Prüfung später – [[D20]] (Haftung).
 - Ob das Gewicht eines Kommentars von Verfasser/Kontext abhängt, offen – [[D19]]
- **Rekursiv verlinkbar:** Key-Infos verweisen aufeinander; dadurch entstehen Subauflagen über mehrere Stufen.
 - **Root-Ebene** = die eigentliche Auflage aus dem Behördendokument (z. B. "Brandschutz laut Brandschutzplan auszuführen").
 - Darunter: aus Nachweisen abgeleitete Subauflagen (z. B. Punkte aus dem Brandschutzplan) – keine konkreten Auflagen, aber wieder zu beantworten.
 - Genaue Art der Verknüpfung (Elternverweis vs. typisierte Verknüpfung) offen – [[D19]]

## Beziehungen
- Key-Info -> Herkunft (Dokument oder Nutzer); Key-Info -> Verknüpfung (Key-Info oder Dokument)
- Bestätigung -> leeres Behördendokument, wenn der geprüfte Bescheid fehlt
- Key-Info -> Key-Info (rekursiv; Root = Auflage)
