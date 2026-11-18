# paragraph82b

Processing of a building's files (Akt) into documents, key information and obligations, and the job pool that does
it. Code and model use the English term; each maps to one term of the concept in `infos/` (German, with its ID).

## Project and people

**Project** (Betriebsobjekt / Projekt, D03):
The premises whose files are processed. All content belongs to exactly one project; it is the unit of separation.

**User** (Nutzer, D43):
A person who works in the application.

**Project access** (Zugriff, D43):
The right of a user to work on a project.

## Akt

**File** (Datei):
One item a user uploaded: an image or a PDF.
_Avoid_: reference file, upload (for the single item)

**Page** (Seite, D44):
An image file, or one page of a PDF.

**Document** (Dokument, D44):
Pages that belong together as one document, e.g. one Bescheid.

**Document section** (Abschnitt, D04, V06):
A part of a document, such as the requirements part of a Bescheid; sections can contain sections.
_Avoid_: section alone where a page range could be meant

**Reference number** (Geschäftszahl, D04, D40):
The number an authority gives a matter; documents carry it.
_Avoid_: Zahl, GZ, Aktenkennzahl

## Knowledge

**Key info** (Key-Info, D46):
A verbatim text passage of a document that matters for the obligations.
_Avoid_: key information (as a code name), extract

**Requirement** (Auflage, D47, D42):
An obligation stated in a key info. A requirement under another one is a sub-requirement (Unterauflage).

**Requirement point** (Auflagenpunkt / Nummer, D04):
The number of a requirement in its document.

**Evidence** (Bestätigung / Nachweis, D47, V11):
A key info that shows how far a requirement is met. Its kinds keep the German values: erfüllt, teilweise erfüllt,
mangelhaft, nicht erfüllt, hinfällig.

**Evidence claim** (Zuordnung):
The link from an evidence to the requirement or reference number it is about.
_Avoid_: match, mapping

**Note** (Hinweis):
A message to the user about something that needs attention, e.g. a suspected duplicate.
_Avoid_: warning, alert

## Processing

**Run** (Verarbeitung):
The processing of a project's files, started by the user with "Start run".

**Event** (Ereignis):
Something that happened in a project: a user action, or a result that means something to the user.
_Avoid_: message, signal

**Apply changes** (Änderungen anwenden):
The user action that hands the collected corrections of a project to processing as one event.

**Job** (Verarbeitungsschritt):
One unit of work on one object, e.g. reading one page.
_Avoid_: task, step

**Job type**:
The kind of a job, e.g. `ocr_page`; it fixes the job's inputs, result and follow-ups.

**Job target**:
An object a job reads or writes.

**Attempt**:
One try at running a job; a job runs again with the next attempt after a crash or a lost lease.

**Engine**:
The processing logic of one project. It consumes the project's events and job results and is the only writer of
the project's processing state.
_Avoid_: project orchestrator

**Orchestrator**:
A process that hosts the engines of many projects and knows no project logic.

**Worker**:
A process that runs jobs of any project. It does not access the database.

**Job pool**:
All jobs of all projects that may run now, shared by all workers.
_Avoid_: pipeline

**Lane**:
The part of the job pool for one model, or `code` for jobs without a model call.
_Avoid_: queue (for the lane as a whole)

**Fast lane**:
The priority of jobs that come from "Apply changes"; they go before waiting jobs of other projects.

**Project cap**:
The largest number of a project's jobs in the job pool at the same time.

**Result stream**:
The channel through which workers return the results of one project's jobs to its engine.

## Quality

**Golden dataset**:
One representative project with correct documents, document types and a sample of key infos, used to measure
processing quality.
