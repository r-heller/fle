# FLE — roadmap to completion

**Audited 2026-09-09** against `curriculum` v1.1.0 and the `pagegen` template.
The July `HANDOVER.md` remains as the historical record; **two of its five
headline findings have since been resolved** and are retired below.

**Status: content-complete for its declared span, structurally non-conformant.**
FLE has more audio than any other course in the suite and a full set of exam
pages. What it carries is a naming and front-matter convention that no other
course uses.

---

## 1. Measured state

| | |
|---|---|
| Unit bundles | **156** |
| Exam bundles | **156** (`page_type: exam`) |
| `page_type` discriminator | on all 312 pages |
| `curriculum:` front-matter block | on all 312 pages |
| Naming convention | `unitNN_slug` — **non-conformant** (template mandates `unitNN-slug`) |
| Forbidden `slug:` in front matter | **all 312 pages** |
| Raw HTML section markup in content | none |
| Quarto remnants (`.qmd`) | none |
| Materials | 643 PDF |
| Audio | **1340 files** — the largest audio set in the suite |
| Exam PDFs | 0 (exams are HTML only) |
| `conformance.yml` | **missing** |
| `[taxonomies]` in `hugo.toml` | **missing** |

### Retired findings from the July handover

- ~~"156 exams publish ZERO pages."~~ **Resolved.** FLE publishes 156 exam
  bundles carrying `page_type: exam`.
- ~~"Content is single files, not page bundles."~~ **Resolved.** All 312 pages are
  leaf bundles.
- ~~"Zero curriculum-ID conformance."~~ **Partly resolved.** Every page carries a
  `curriculum:` block; what is still missing is the manifest that makes those
  IDs auditable (Phase 2 below).
- ~~"Raw HTML in content instead of shortcodes."~~ **Resolved.** No content file
  embeds `card-grid` or `hero-kicker` markup.

The migration off Quarto is complete. What remains is smaller than the handover
implies, but it touches every page.

## 2. What "finished" means here

`declared_conformance: core` (A1–B1) proven by `conformance_audit.py resolve`,
with the same bundle naming and front-matter schema as `efl`, so that a reader
of one repo can read the other.

## 3. Roadmap

### Phase 1 — converge on the template naming (M, scriptable, touches URLs)

- [ ] Rename all 312 bundle directories `unitNN_slug` → `unitNN-slug`.
- [ ] Remove the `slug:` key from all 312 front matters. The template forbids it;
      Hugo derives the slug from the bundle name once the rename lands.
- [ ] **Publish redirects for every changed URL.** This is the one step that can
      break live links, and 312 pages are indexed. Generate the alias list from
      the rename map rather than by hand.
- [ ] Verify with a link check (`link-check.yml`) before and after, and compare
      the two reports rather than trusting a green run.

**Sequencing note:** do the rename *before* the conformance manifest. The
manifest references page paths; writing it first means writing it twice.

### Phase 2 — declare conformance (S, after Phase 1)

- [ ] Copy the `conformance.yml` shape from `efl` once it exists (see the suite
      roadmap — EFL is the pilot for this file).
- [ ] Populate `realizations` from the `curriculum:` blocks already on all 312
      pages. Extraction, not authoring.
- [ ] Gate on `conformance_audit.py resolve` in CI.

### Phase 3 — close the template gaps (S)

- [ ] Declare `[taxonomies]` in `hugo.toml`.
- [ ] Rename `navbarTitle` → `navTitle`.
- [ ] Reconcile the licence statement: `README.md` says CC BY 4.0, the framework
      and sister repos use CC BY-SA 4.0. One of the two is wrong.

### Phase 4 — exam materials (M, authoring)

- [ ] FLE has 156 exam *pages* and zero exam *PDFs*; `daf` has the inverse. Decide
      whether printable exam PDFs are in scope for FLE, and if so generate them
      with `sheetgen` rather than by hand.

## 4. Gaps declared, not hidden

- The Bildungsplan-BW tracks (Kl. 6–13) do not map cleanly onto CEFR levels. The
  conformance manifest will make the actual level coverage visible for the first
  time; expect it to show gaps that the track structure currently hides.
- B2/C1 coverage is unquantified until Phase 2 produces a coverage report.

## 5. Dependencies

- `efl` Phase 1 — FLE copies the manifest shape rather than inventing it.
- `curriculum` ≥ 1.1.0, `kit` v1.21.0, `boulingua/.github` for CI.
