# Page previews (T-13), paging and grouping (T-26)

Plan agreed with Andreas on 2026-11-25.

## Problem
- The page grid on the processing page shows the full page image (PNG at 150 DPI, about 1–3 MB; plans and photos up
  to 50 megapixels) in a 130px tile.
- `/pages/{id}/image` signs a new URL on every request, so the browser caches nothing; every refresh of the list
  loads every image again.
- Every page and every file of a project is listed at once.

## Decisions
- **Previews:** a ~320px wide WebP per page, written by ingest_file next to the page image; ocr_page writes one for
  a turned image. Key derived from `image_path`, no schema change. Width in `config/<env>.yaml`. JPEG when WebP
  saving is not available (check in the built image).
- **Older pages without a preview:** a one-off command creates the missing previews per environment; the frontend
  never falls back to the full image (it can be an A0 plan). Ingest has no stage reset.
- **Caching:** keys are immutable (run id in the name); the 307 redirect and the signed URL carry
  `Cache-Control: private, max-age=N` with N below the signed URL's lifetime.
- **Full image:** clicking a preview still opens the full PNG (ocr_page needs it).
- **Storage limit:** unchanged; `storage_used` counts only uploaded files, previews are an accepted small overshoot.
- **Cleanup:** previews follow the turned images: they stay until a later task removes unreferenced files in the
  object store (old turned images, images of repeated ingest attempts, their previews).
- **Upload page (file table):** grouped by file status, attention first: upload cancelled, read error, uploading,
  ready, in processing, processed; empty groups hidden. Each group collapsible and paged, with one "per page"
  selection (25/50/100) for all groups, remembered in the browser. Open/closed remembered per project in the browser;
  "processed" starts closed. Newest upload first within a group. Counts update live; an open group refreshes its
  page at most every few seconds, so nothing jumps while reading. The sum row (files, pages, "process" button)
  moves above the groups. A search field by file name across all groups. The api computes the status of all files
  (upload state + latest ingest job, as today) and returns the counts and the requested slice.
- **Processing page (pages):** grouped by the classification ocr_page sets: not read yet / process / keep / discard;
  plans labelled inside "keep" (`is_construction_plan`). Each group collapsible and paged like the file table;
  previews load only for open groups.
- **Api:** paging (`limit`/`offset`) and the group filter on both lists, so the browser never loads a whole project.

## Split (Andreas, 2026-11-25)
- T-13 builds the previews, the cache headers and the content types now.
- T-26 builds the paging and the groups after T-19 (batches, relative paths, `PAGE.sequence` change what the upload
  page groups and orders by) and T-18 Q11 (which view shows documents after grouping).

## Previews: details decided while building (Andreas, 2026-11-25)
- **Image version in the links:** the page list returns `version` (the run part of the image key); the frontend
  puts it into the preview and image links (`?v=`), so a page turned by ocr_page gets new links at once and the
  browser can keep them for `storage.image_cache_seconds` (600 s locally, below the 900 s of a signed URL; the settings
  refuse a longer one).
- **No JPEG fallback:** the built backend image saves WebP (checked 2026-11-25); a missing WebP would fail the
  preview tests.
- **One-off command** `python -m app.pipeline.previews`: also makes the preview of a turned page's rendered image,
  which a stage reset brings back.
- Content type of stored files set by their extension (png, webp, jpg, pdf), from one list in the file store.
- The preview of a rendered page is made from the image already decoded for its PNG (A0: about a quarter less time).
- Not done, by choice: the api does not check that a preview exists before redirecting (a request per tile for a
  case the command prevents); the command works one image at a time (one-off).

## Built (submodule `572030b`, review fixes `2855bcf`; Architecture v40)
- Locally: 36 pages, full images 534.7 MB, previews 0.79 MB; the command took 19 s for them and makes nothing on a
  second run.

## Prerequisite
- T-16: frontend tests (vitest + Testing Library, Playwright against the local Compose stack); the new views are
  built test-first once it is done.

## Out of scope
- Duplicate detection: T-14 (file level, `check_file_duplicate` on `content_hash`, with the NOTE table) and T-15
  (page level, `check_page_duplicate` after `embed_page`), as in `docs/Data_Model.md`.
- Removing unreferenced files from the object store: own task, not yet created.
