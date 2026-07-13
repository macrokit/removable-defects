# Submission runbook — arXiv first, then TMLR

Status: LaTeX package in `submission/` (built from PAPER.md v2 + PROOFS.md as
appendix). This file is the operator's checklist; steps marked **[you]**
need the author's own accounts and cannot be delegated.

## Phase 1 — arXiv

1. **[you] Account & endorsement.** Register at arxiv.org with the email you
   want permanently attached to the paper. First-time submitters to `cs.LG`
   from a non-institutional email may need an *endorsement* — arXiv shows an
   endorsement code after you start a submission; any established cs.LG
   author can endorse with it. Budget a day or two for this if needed.
2. **Compile locally and keep the `.bbl`.** arXiv does **not** run BibTeX.
   The upload must contain `main.tex` + `main.bbl` (the compiled
   bibliography), not just `references.bib`. Locally:
   `cd submission && tectonic --keep-intermediates main.tex` — then confirm
   `main.bbl` exists. Upload set: `main.tex`, `main.bbl` (and `references.bib`
   optionally, harmless).
3. **Metadata.**
   - Title: *Removable Defects: The Economics and Limits of Deliberate
     Deficiency*.
   - Abstract: plain-text version of the paper abstract (strip `**bold**`
     and markdown; arXiv renders `$...$` math).
   - Primary category: **cs.LG**. Cross-lists: **stat.ML**, **cs.AI**
     (the insurance-economics content would also justify `econ.TH`, but two
     cross-lists is the tasteful maximum).
   - License: the default **arXiv non-exclusive license** — safest for a
     later journal; TMLR (open access) is compatible with it. Choose CC-BY
     only if you affirmatively want it.
4. **[you] Submit and wait for the announcement cycle** (next business-day
   cutoff, 14:00 US Eastern). Note the assigned identifier
   `arXiv:2607.NNNNN`.
5. **After the ID exists:** add it to README.md and cite it as the public
   preprint. (Optional: `gh repo edit --description` to include the arXiv
   link.)

## Phase 2 — TMLR (after OpenReview activation)

1. **[you] Activate OpenReview now, in parallel** — don't wait for arXiv.
   Accounts registered with non-institutional emails go through a
   **moderation queue that can take one to two weeks**; institutional emails
   are near-instant. Start this today at openreview.net.
2. **Style port.** TMLR requires its own style: `tmlr.sty` from
   `github.com/JmlrOrg/tmlr-style-file`. The port from `submission/main.tex`
   is mechanical: swap the documentclass/preamble per their template, keep
   body and appendix verbatim.
3. **Anonymize.** TMLR review is double-blind:
   - remove the author line;
   - the `\thanks` footnote pointing at `github.com/macrokit/removable-defects`
     must go (it deanonymizes) — replace with "code and extended notes will
     be linked upon acceptance";
   - the §7 worked example (Agent World) can stay but must not link the repo;
     describe it as "a protocol project" without the org name if strict.
   - The arXiv preprint is **explicitly allowed** under TMLR's preprint
     policy; you do not need to hide it, but do not cite it in the
     submission in a way that names you as its author in first person.
4. **[you] Submit at openreview.net → TMLR.** Choose reviewers-suggestion
   fields honestly; expected review cycle ≈ 2 months. TMLR's acceptance
   criteria are exactly the two questions this paper was built to answer
   well: are the claims supported (PROOFS.md, both literature rounds), and
   is the audience plausibly interested.

## Notes

- The repo remains the canonical living version; `submission/` is a build
  artifact of PAPER.md v2 + PROOFS.md. If the paper changes, regenerate
  rather than patch the LaTeX.
- Both bib data and per-claim novelty scoping trace to LITERATURE.md; any
  referee challenge on a citation should be answerable from there.
