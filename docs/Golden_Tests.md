# Golden tests with labels and approved results (T-34)

Spec, 2026-11-26, from a grilling session with Andreas. Status: `[ready-for-agent]` once T-33 has recorded.
Background: `Architecture.md` section 11 (golden dataset, measured stage by stage, error to test to fix).

## Problem Statement

The golden dataset is meant to show whether the implementation does what is wanted, but today the golden test only
checks that every file is uploaded and no job fails. It does not compare any result with the expected one, it runs
only locally, and the reference beyond the folder structure (page classification, page order, attachments) does not
exist and cannot be written by hand for about 550 pages. Results of a run are lost with the run's database, so
nothing can be compared or gone back to.

## Solution

- **Ground truth lives only on the share,** never in code: the folder structure of the golden dataset says which
  document a file belongs to and its type; further labels are tables in `ds_labels`, proposed by a run and approved
  by Andreas. The code only reads and compares.
- **A golden run on the dev core host** replays the recorded model answers (no tokens) on a fresh database, measures
  every stage against the labels, writes a report, proposals for everything not yet approved and a database
  snapshot, and fails when a number gets worse or a result differs from an approved label.
- **Recording** happens only in a real run in the dev application (T-33), whose results Andreas inspects in the app.
- **Certainty first:** only what is known counts; unknown or unsorted parts are reported, not counted.

## Data on the share (golden dataset)

- The type folders `1_<type>/` and their document subfolders are the truth for document membership and type.
  Folders without the `1_` prefix (unsorted, earlier prototype output, temporary folders) are not part of the
  dataset.
- Only folders starting with `ds_` are ever written by code; everything else and the whole input dataset stay
  read-only.

| Folder | Content | Written by |
|---|---|---|
| `ds_recordings` | recorded model answers (today `recordings/`, renamed) | the recording run |
| `ds_labels` | `types.csv`; per document one CSV; proposals (`*.proposed.csv`) and approved files | golden runs (proposals only), Andreas (approval) |
| `ds_reports` | one folder per run (`<date>_<time>`): numbers per stage, differences, pages to review | golden runs |
| `ds_snapshots` | a database snapshot at the end of every golden run, plus manual ones | golden runs, Andreas |
| `ds_findings` | `findings.csv`: findings of Andreas and of agents | Andreas, agents |

## User Stories

1. As Andreas, I want the golden run to tell me per stage how many results are right, so that I know whether the
   implementation does what we want.
2. As Andreas, I want the run to fail only when a number gets worse or a result differs from an approved label, so
   that I am not stopped by guessed targets.
3. As Andreas, I want a re-run to cost no tokens as long as inputs and prompts are unchanged, so that I can run it
   after every change.
4. As Andreas, I want to start the golden run by hand on the dev core host, so that the compute load stays there.
5. As Andreas, I want nothing of a golden run uploaded to GitHub, so that real data stays on our machines.
6. As Andreas, I want proposals for page classification, page order within a document and attachments written by
   every run, so that I only review and never write 550 pages of labels by hand.
7. As Andreas, I want only the doubtful pages marked for review, with the reason, so that the review is fast.
8. As Andreas, I want a page counted as correctly ordered only when its page number chain and every content signal
   agree, so that content mistakes are not hidden behind matching page numbers.
9. As Andreas, I want attachments proposed as subfolders of their main document in `ds_labels`, so that I can confirm
   or move them.
10. As Andreas, I want to approve a proposal by renaming it (or with a command), so that approval stays in my hands.
11. As Andreas, I want approved labels never overwritten by a run, so that my review is not lost.
12. As Andreas, I want the approval of a document to lapse when its golden folder changes, so that labels follow the
    dataset; unaffected documents keep their approval.
13. As Andreas, I want the numbers always computed against the current folder structure, so that changes in the
    dataset take effect on the next run.
14. As Andreas, I want files that are missing on either side, or renamed, listed in the report but not counted, so
    that gaps in the dataset do not look like processing errors.
15. As Andreas, I want loose files in a type folder counted for type and classification but not for grouping
    (unless `types.csv` says otherwise for that folder), so that only known data is measured.
16. As Andreas, I want a database snapshot of every golden run on the share, so that I can go back and compare.
17. As Andreas, I want to record findings in one table on the share, and agents to add what they notice there as
    "unsure", so that nothing I miss or doubt gets lost.
18. As Andreas, I want confirmed findings to become tests first, so that every error found stays fixed.
19. As an agent, I want to read reports, labels and findings from the share mounted locally, and restore snapshots
    into a local database, so that I can analyse runs without touching the dev core host.
20. As a developer, I want one module that knows the share's layout and the label format, so that the rest of the
    code only asks it.
21. As a developer, I want later stages measured on golden documents (built from the labels) as well as on our own
    grouping, so that an error shows where it arises.

## Implementation Decisions

- **Matching:** a page is identified by the relative path of its raw file in the input dataset plus its page number
  (0 for an image). Golden files are matched to raw files by content hash (509 of 515 golden images are byte copies
  of raw files); renamed files are reported (a rename may carry information, e.g. a reference number found; to be
  checked later).
- **`types.csv`** (in `ds_labels`, maintained by Andreas): per type folder its classification (`process`, `keep`,
  `discard`), document type, and whether loose files in it count as documents of their own (default: not grouped).
  Starting values: `1_Unrelevant` → `discard`; `1_Pläne` and `1_Sonstige Unterlagen aber nicht ganz unrelevant` →
  `keep`; all other `1_` folders → `process`.
- **Label files:** one CSV per document in `ds_labels`, mirroring the document folders; one row per page, the label
  values as columns (classification, `position`, `check`; later more, e.g. title features). Attachments are
  subfolders of their main document in `ds_labels`, also when the golden dataset has their pages in the main folder.
  Pages without a document go into one table at the root of `ds_labels`.
- **Proposals:** every golden run writes `*.proposed.csv` for everything not approved, overwriting only earlier
  proposals; approval turns a proposal into the plain file.
- **Order within a document:** by printed page number, else file name. Certain only when, compared with the page
  before, the page number chain fits (no gap, no duplicate, "of x" matches) and all content signals agree: same
  reference number and same date where read on both, same kind of document, no sign of a new document on an inner
  page (indicates new document, letterhead appearing, subject or title features), and embedding similarity not below
  a threshold set in the configuration after the first run. Otherwise `check` names the reason. A text model check
  of marked pages comes later, only if needed.
- **Measures (now):** page classification right; grouping per golden document as exact, split or joined (as sets of
  pages; page order within a document is not judged, upload order does not hold); with T-24, document type right.
- **Failing:** a number worse than at the last approved state, or a difference from an approved label. Targets come
  after the first run.
- **Dataset changes:** the folder structure wins; when a golden document changes, its approved labels become
  proposals again and the report lists it as "dataset changed".
- **Golden run:** a workflow on the dev core host's runner, started by hand, replay only (no record mode), with its
  own fresh database and stack beside the dev deployment; the project is set to a tier large enough for the dataset
  (setting of the golden environment). No artifacts are uploaded; the log shows pass or fail and the report folder.
- **Snapshots:** at the end of every golden run a database dump is written to `ds_snapshots`; a snapshot command
  (create, list, restore, compare) serves manual work; it stores only on the share and is refused in prod.
- **Findings:** `ds_findings/findings.csv` with key (path and page, or document), stage, what is wrong, seen by,
  status (`open`, `unsure`, `confirmed`, `not an error`), link to a test. The error catalogue in the repository
  (`Error_Catalogue.md`) keeps only error class, stage, cause and the finding's number; no paths, texts or names.
- **One module** reads the share (folders, `ds_` rules, `types.csv`, label files, matching, measures); the browser
  test and backend tests only call it. It holds no expected values.
- **Access:** the share is mounted on the dev core host (`/mnt/nas/paragraph82b/…`) and on Andreas's machine; agents
  read it locally and analyse snapshots in a local database; agents never query the dev core host.

## Testing Decisions

- The reading and measuring module is tested with a small made-up dataset in a temporary folder (type folders,
  document subfolders, `types.csv`, label files, proposals): matching by hash, renames, gaps, loose files, approval
  lapsing on a change, proposals never overwriting approved files, measures exact/split/joined.
- The order checks are unit-tested on made-up page features (each signal alone flags the page).
- The golden run itself runs only on the dev core host; locally it is skipped with the existing "not everything was
  tested" notice when the dataset or recordings are missing.

## Out of Scope

- Key info labels and measures (the YAML files in the dataset; with key info extraction).
- The second pass on golden documents (acceptance criterion of T-22 and T-24).
- A text model check of page order (only if the rules miss too much).
- Fixed quality targets (after the first run).
- Order across documents (Akt order, `PAGE.sequence`) as a label.

## Further Notes

- Order to start: T-33 records → first golden run writes proposals → Andreas reviews and approves → later runs
  measure.
- Re-runs cost no tokens; new raw files or changed prompts need a recording run again.
