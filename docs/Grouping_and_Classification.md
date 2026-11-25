# From read pages to classified documents (T-18)

Draft, 2026-11-25. Plan for the job types after `ocr_page` up to `classify_document`. Decisions are taken one at a
time with Andreas; proposals not yet decided are marked **Suggestion**.

## Already fixed
- Job types, their inputs, outputs, preconditions and follow-ups: `Data_Model.md`, "Job types".
- `ocr_page` records what is on a page; grouping and classification interpret it and make no calls of their own on
  the page image (data model v39).
- `find_sections` runs before the type is known: a root section over the whole document, then all candidate section
  types beneath it; what was found goes into `DOCUMENT_FEATURES` (Andreas, 2026-11-17).
- The tier limit applies from grouping on: only pages up to the last document border within planned pages + margin
  go further (`Architecture.md`, section 7).
- `decide_border`, `assemble_document`: code, no model. `find_sections`, `classify_document`: text model.
  `embed_page`: embedding model (`Architecture.md`, section 6).
- Quality is measured per stage on the golden dataset, each stage on the golden inputs of the stage before
  (`Architecture.md`, section 11): borders, documents matched exactly, document type.

## Direction: decisions replaceable by a trained model
Andreas, 2026-11-25: the border decision and the document classification may later be replaced by a neural network
or a similar trained model. Both take many input features and produce one output. Consequences for this plan
(Suggestion):
- **One seam per decision:** `decide_border` and the type decision in `classify_document` each call one function
  with a fixed, typed input (the features of the window or of the document) and a fixed output (decision, score,
  explanation). The rule scoring is the first implementation behind it; a trained model is a second one, chosen in
  configuration like the model per job type.
- **Inputs stay stored:** `PAGE_FEATURES` and `DOCUMENT_FEATURES` are kept per page and per document, so every
  decision can be recomputed and the features serve as training data. Signals are kept rather than dropped.
- **Labels:** the golden dataset gives labelled borders and types; user corrections (move page, split, join, change
  type, recorded as `EVENT`) are further labels later.
- **Explanation is optional:** `border_rules` is what the rule scoring reports; a trained model may leave it empty.
  `border_score` stays a 0–1 confidence for both.

## Decided (Andreas, 2026-11-25)

### Models and page features
- **Embedding model:** `text-embedding-3-small`, 1536 dimensions, as in the prototype; named in `job_models` like the
  other job types. Not chosen: `text-embedding-3-large` (3072 dimensions, about 6× the cost, twice the storage). The
  vector size is fixed per column; a switch means re-embedding.
- **`title_*` features:** `ocr_page` reads all 11 on every page in its existing call, with the prototype's field
  descriptions (`DocumentTypeFeatures`); the 6 columns missing in the code are added. Why: every page gets the same
  complete features, which a trained model needs, and no candidate filter can miss a title page; costs more output
  tokens per page. Not chosen: a second vision call for title-page candidates only, as in the prototype
  (`node_reevaluate_titlepage`); dropping the features. Consequence: `ocr_page` runs again on pages already read.
- **Authority or not per page:** derived from `document_type_hint` (verhandlungsschrift, bescheid,
  verhandlungsschrift_mit_bescheid, sonstiges_behoerdendokument = authority); no new column. Replaces the
  prototype's per-page `is_gov_doc` in the border rule "document class changes" and in the authority/other split.
  Not chosen: `title_authority` (only the header, so continuation pages would look like another class); a new flag.

### Pages in grouping
- **`discard` pages** belong to no document; the border is decided across them. A skipped `discard` page between two
  neighbours is a weak input "separator between": it may be an intended separator or just a bad photo, so its
  weight is set by the golden runs and it never makes a border on its own.
- **`keep` pages** (plans, plant photos) form documents of their own and stay in the window as real neighbours (the
  rule "construction plan block ends" needs them). A run of `keep` pages is one document, split further only where
  the border rules find a border; the prototype rule "a plan never starts a document" is dropped.
- **A change between `process` and `keep`** (`discard` pages skipped) is a fixed border, set before any rule
  scoring, so a document stays a contiguous stretch. Known weak cases, left to the golden runs: a blank page inside
  a plan set; a plan in the middle of a Bescheid (splits it; the user joins the parts).
- **Tier limit** counts every page in upload order, whatever its classification, as `ocr_page` already does.

### Attachments
- **One parent link:** `DOCUMENT.attached_to_document_id` (nullable) points to the main document; a main document has
  many attachments, attachments of attachments are allowed. For now; a link table only if a real case needs it.
- **Duplicates:** a document found to be a duplicate stays a document, keeps its own parent link and gets
  `duplicate_of_document_id` (new) pointing to its original. It is not processed further; its content is reached
  through the original, and the link to its main document keeps the information where the copy was filed.
- **Slice 3 links simply:** `assemble_document` attaches each `keep` document to the `process` document right before
  it. This can pick the wrong parent (e.g. a plan behind a Beilage of the main document); the user re-links until
  `link_attachments` exists.
- **`link_attachments` later:** its own slice after `classify_document`; it implements the idea I-1 (attachment hints
  such as "Anlage", "Beilage", an email's attachment list) and also links `process` documents. Input:
  `ideas/I-1-attachment-detection.md`.
- **Prototype rule "attachment: same reference, own first page" (−6) dropped:** it keeps attachments inside their
  main document, against attachments as documents of their own. Open: the whole attachment handling needs further
  clarification with test data, including what it affects downstream (sections, key infos, matching).

### Order and grouping unit
- **Upload redesign, separate task (T-19), before slice 2:** folder upload, and every file keeps its path within the
  upload in `FILE.relative_path` (new, nullable, confidential like `original_name`). Today only files can be
  dropped or picked and only the bare name is stored, so folder uploads lose their order.
- **Grouping unit = batch of one "Verarbeitung starten":** on each press, every ready file without a batch gets the
  next `FILE.batch` (new, integer per project). Grouping runs only within a batch: its first page is a fixed border,
  and its last page closes its last document once all its pages are read. Several uploads before one press form one
  batch; files uploaded during a run wait for the next press. A later batch never joins an older document.
- **Order within a batch:** the api sorts all files of the batch naturally by relative path (folder by folder, then
  file name, numbers compared as numbers); files without a path by name; upload time breaks ties. The result goes
  into `FILE.upload_position`; the browser's order no longer matters. `ocr_page`'s reading order and the tier limit
  follow it.
- **Tier limit:** pages beyond it stay in their batch; after a move to a larger tier, the next press continues the
  batch from the cut.
- **Folder change** between neighbours is a weak input, like the separator. In the golden dataset the subfolder is
  the document label, so every quality run reports border quality with and without this input; the run without it
  is the real grouping quality.
- **Upload into a document:** a user can upload files into an existing document and decides where in the document
  the pages go. These files get no batch and skip grouping; each page runs the steps it needs (`ingest_file`,
  `ocr_page`, `embed_page`, …), and the page and its document are invalidated as far as the page influences them. A
  new row in the user-action table. This is also how a page that was missing or misfiled in an earlier upload gets
  into its older document; an automatic suggestion (a note such as "same reference number, missing page 3") may come
  later.
- **`PAGE.sequence`** (new; revises v17): the page's place in the original Akt (concept A02). Set from the batch
  order, never changed by moving pages; a page uploaded into a document gets the sequence of the place the user chose
  (sparse numbering, so no renumbering). `PAGE.position` stays the place within the document and changes with moves.
  The Akt can always be shown in the order it was photographed, and pages misfiled in the original stay visible.
  Documents are listed in Akt order, by the sequence of their first page.

## Job types in scope
| Job type | Builds on | Existing task |
|---|---|---|
| `embed_page` | `ocr_page` | part of T-15 |
| `decide_border` (with the tier limit) | `ocr_page`, T-19 | T-17 |
| `assemble_document` | `decide_border` | – |
| `find_sections` | `assemble_document` | – |
| `classify_document` | `find_sections` | – |
| `check_reference_numbers` | `classify_document` | – |
| `link_attachments` (later) | `classify_document` | – |

Duplicate checks (`check_file_duplicate`, `check_page_duplicate`, `check_document_duplicate`) need the `NOTE` table;
built in slice 6, the duplicate checks follow in slice 7.

## Order (slices)
Each slice is built test-first, ends with a quality run on the golden dataset and can be reset with the stage reset.

1. **`embed_page` and the `title_*` features:** `PAGE.embedding` (1536), `PAGE.embedding_model`; the 6 missing
   `title_*` columns and their fields in `ocr_page`. Independent of the order.
2. **T-19 upload redesign** (separate task): folder upload, `relative_path`, `batch`, `sequence`, order within a
   batch.
3. **`decide_border` + tier limit:** the prototype's boundary rules (`document_creation/logic/utils.py`; threshold 7)
   as pure functions behind the decision seam, adapted as decided above (fixed borders at batch starts and
   process/keep changes; `discard` skipped; new inputs separator and folder change; attachment rule dropped). Writes
   `border_before`, `border_score`, `border_rules`. Measured: borders right, with and without the folder input.
4. **`assemble_document`:** `DOCUMENT` rows, `PAGE.document_id`, `PAGE.position`, simple attachment links. Measured:
   documents matched exactly, too many joined or split. The "Dokumente" view shows them.
5. **`find_sections`:** `DOCUMENT_SECTION` (root + found sections), `DOCUMENT_FEATURES`. Prototype prompt with
   `[L<n>]` line markers kept as it is.
6. **`classify_document` + `check_reference_numbers`:** `document_type`, `short_title`, `stated_date`,
   `REFERENCE_NUMBER`, `NOTE`, `NOTE_TARGET`. Measured: type right, confusions.
7. **Duplicate checks** (T-14, T-15, `check_document_duplicate`), reusing `NOTE`.
8. **`link_attachments`** (later, own plan).

## Changes to carry into the model documents
Collected here, written as one new version each once this plan is agreed.
- **Data model (v42):** `PAGE_FEATURES` + 6 `title_*` columns and `title_*` filled by `ocr_page`; `PAGE.sequence`;
  `FILE.relative_path`, `FILE.batch`; `DOCUMENT.attached_to_document_id`, `DOCUMENT.duplicate_of_document_id`;
  user action "upload into document"; grouping per batch; `discard`/`keep` handling.
- **Architecture:** section 7 says "the only borders known before are file starts" — replace by batch starts and the
  order within a batch; upload of folders; the decision seams for a trained model (section 6).

### Remaining questions (Andreas, 2026-11-25)
- **`border_score`:** distance of the rule points from the threshold, mapped linearly: 0.5 at the threshold, 1.0 at
  a distance of 10 points or more either way. Threshold and cap in `config/<env>.yaml`. The same 0–1 meaning for a
  trained model.
- **Long documents in `find_sections`:** one call per document with the whole line-numbered text (`keep` documents
  excluded) and the prototype prompt; split only when the golden run shows missed or misplaced sections.
- **How `classify_document` decides,** behind the decision seam: `keep` documents take their type from their pages'
  `document_type_hint` (plan, plant photo), no model call; authority documents by a rule combining the sections
  found (`DOCUMENT_FEATURES`, prototype `evaluate_gov_doc_type`) and the first page's `title_*` features (prototype
  `classify_document_type`): Verhandlungsschrift, Bescheid, both, or other authority document; other documents by
  the text model choosing a kind from `document_type_hint`, with `short_title` and `stated_date` in the same call.
  Reference numbers from the pages' `reference_number_raw`, normalized like the prototype (letters and digits, upper
  case).
- **`NOTE`:** built in slice 6 with `check_reference_numbers`, its first user. The duplicate checks (T-14, T-15,
  `check_document_duplicate`) follow as their own slice after it and reuse `NOTE`, `NOTE_TARGET` and
  `duplicate_of_document_id`.
- **Frontend:** a "Dokumente" view grows with the slices: from slice 4 documents in Akt order (`sequence`) with page
  previews (T-13) and their `keep` attachments nested, low-`border_score` borders marked; slice 6 adds type, short
  title and date. "Upload into document" starts from a document there.

- **Done and tasks:** T-18 (planning) is done when this plan has no open questions, data model v42 and the
  architecture update are written and approved, and every slice is an open task (slice 1 T-20, slice 2 T-19,
  slice 3 T-21, slice 4 T-22, slice 5 T-23, slice 6 T-24, slice 7 T-25; T-14, T-15, T-17 absorbed). A slice task is
  done when its slice runs on the golden dataset and the quality run reports its measures; targets are set after the
  first run.

No open questions remain.
