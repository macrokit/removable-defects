# arXiv submission package

## Contents

- `main.tex` — full manuscript: paper body (PAPER.md, Abstract + §§1–10) followed by
  the technical appendix (PROOFS.md §§1–14 as Appendices A–N, preceded by an
  unnumbered "Overview of the appendix" taken from PROOFS.md's summary block).
- `references.bib` — BibTeX for every work cited in either file; bibliographic data
  from LITERATURE.md's source index, gaps filled from standard data (see TODO list below).
- `main.pdf` — compiled output (29 pages).

## How to compile

```
cd submission
tectonic main.tex
```

Tectonic runs BibTeX and the required rerun passes automatically in one
invocation. Plain `pdflatex` also works: `pdflatex main.tex && bibtex main &&
pdflatex main.tex && pdflatex main.tex`. Only arXiv-safe packages are used
(geometry, amsmath, amssymb, amsthm, natbib, hyperref, booktabs, microtype,
enumitem); the file is pure ASCII, pdflatex-compatible, `\documentclass[11pt]{article}`.

Current build status: zero errors, zero undefined references/citations; a
handful of overfull-hbox warnings (cosmetic).

## What was adapted from the markdown (everything else is verbatim)

1. **Header block replaced.** PAPER.md's "Draft v2" note (repo conventions,
   pointers to FORMALIZATION.md / PROOFS.md / LITERATURE.md) is replaced by a
   `\thanks` footnote on the title: "Extended notes, the research backlog, and
   per-claim literature verdicts are maintained at
   github.com/macrokit/removable-defects." A `\date{July 13, 2026}` was set from
   the draft date.
2. **File references adapted.** All mentions of repo files were rewritten
   minimally: "LITERATURE.md" → "the project repository('s literature notes)",
   "FORMALIZATION.md §n" → "the (project's) formalization notes (§n)",
   "QUESTIONS.md #n" / "Backlog #n" → "research backlog #n". PROOFS.md's opening
   "This document" → "This appendix".
3. **Cross-references converted.** Paper-internal "§3"/"§5.2" → `\S\ref{sec:...}`;
   "appendix, §12"-style references → `Appendix~\ref{app:...}` (PROOFS §1–14 =
   Appendices A–N); "appendix, Theorem 2′" → "Appendix G, Theorem 2′", etc.
4. **Citations converted to natbib** (plainnat, round, author-year). Compound
   author-name mentions render in natbib's form, e.g. "Goldwasser–Kalai–Kalai–
   Montasser (2020)" → "Goldwasser et al. (2020)", "Cortes–DeSalvo–Mohri (2016)"
   → "Cortes et al. (2016)"; sentence text is otherwise unchanged. Author names
   used adjectivally keep their text with `\citeyearpar` (e.g. "the
   Ehrlich–Becker (1972) substitution margin"). Abstract mentions (no years)
   were left as plain text, as is conventional for arXiv abstracts.
5. **Theorem environments.** All results use *unnumbered* amsthm environments
   with explicit hand-set titles, so rendered labels read exactly as
   established: Theorem 1, Theorem 1′, Theorem 2, Theorem 2′, Theorem A, M, R,
   E, E′, Lemmas 1–4, Propositions 1/4/5/6, Corollary 3, Corollary M1/M2,
   Corollary (the iff), Corollary (unification), Corollary A+B. Nothing is
   auto-numbered.
6. **Formatting.** The §2 payoff matrix (and its appendix copy) → booktabs
   tabular; the Agent World JSON snippet → verbatim; markdown bullets/numbered
   lists → itemize/enumerate; unicode (σ₀, ⇒, ×, ′, en/em dashes, typographic
   quotes) → LaTeX equivalents; `\*` escapes fixed (`$\Pi^\*$` → `$\Pi^*$`).

## Non-obvious adaptation decisions (for author review)

- **Neyman–Pearson (1933) citation added** in Related Work: the source says
  "randomization realizing the ROC convex hull is the Neyman–Pearson lemma plus
  Provost & Fawcett (2001)"; the lemma name-drop was given a citation
  (`neymanpearson1933`) to keep the bibliography self-contained. Remove if
  unwanted.
- **PROOFS §13 Littlewood–Rushby bullet**: the citation `\citep{littlewood2012}`
  was attached at the end of the bullet (the source names them only in the
  bullet header).
- **PROOFS §11, Remark 2**: "(FORMALIZATION.md §2, point 2)" → "(formalization
  notes §2, point 2)" — the section number was kept since it points into the
  repo notes, not into this paper.
- **Appendix K's Theorem A vs. main-text §5.4's Theorem A**: both are rendered
  with the same unnumbered label "Theorem A" (matching the markdown, where the
  paper states an abridged version and the appendix the full version). Same for
  Theorem 1, Lemma (coupling)/Lemma 1, and Corollary (the iff)/Corollary A+B.
- **"Proposition 1 / the advantage condition (FORMALIZATION.md §3)"** in
  Appendix K was pointed at the paper's own §3 (`\S\ref{sec:advantage}` "of the
  main text") since Proposition 1 now lives there.
- **Proposition 6 and Theorem E′** end with a literal `$\square$` inside the
  statement (as in the markdown, where argument and statement are one block);
  all separated "*Proof.*" blocks use amsthm `proof` environments with
  automatic QED.
- **Hyperref link color** is plain `blue` (mixed colors would need xcolor,
  which was not on the allowed package list).

## Bib entries marked "% TODO verify"

| key | what needs checking |
|---|---|
| `williamson1985` | LITERATURE.md says only "Williamson"; filled with *The Economic Institutions of Capitalism*, Free Press, 1985 |
| `vanmieghem2003` | volume/number/pages (M&SOM 5(4):269–302) from standard data |
| `eckhardt1985` | volume/number/pages (IEEE TSE SE-11(12):1511–1517) from standard data |
| `littlewood2012` | volume/number/pages (IEEE TSE 38(5):1178–1194) from standard data |
| `spitznagel2021` | publisher (Wiley) from standard data |
| `hendrickx2024` | Machine Learning journal volume/pages (113:3073–3110) from standard data |
| `fillatre2017` | exact title (notes give journal/volume/pages only) |
| `fauss2021` | IEEE TSP volume/pages (69:2252–2283) from standard data |
| `managoli2025` | exact title and author first names (notes give surnames + arXiv:2501.12938) |
| `ehrenfeucht1989` | volume/number/pages (Inf. & Comp. 82(3):247–261) from standard data |
| `rigollet2011` | notes say "COLT 2011"; entry uses the JMLR 12:2831–2855 version |
| `casacuberta2025` | exact title and first names (notes: "Casacuberta–Kanade, NeurIPS 2025") |
| `mozannar2023` | full author list and title (notes: "Mozannar et al. 2023, PMLR v206") |
