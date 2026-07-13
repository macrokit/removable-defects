# Literature check

Executed 2026-07-13 against FORMALIZATION.md §9's kill-list, via four parallel
research passes (selective prediction; deferral & impossibility; economics;
detection cost & conformal). Verdict vocabulary: **KILLED** (claim is prior
work, cite it), **SHRUNK** (partially prior, residual stated), **REPAIRED**
(false or ill-posed as drafted, fixed form stated), **ALIVE** (no counterpart
found).

## Verdict summary

| FORMALIZATION.md claim | Verdict | Disposition |
|---|---|---|
| Prop 1 — advantage condition | SHRUNK | Structure is Ehrlich–Becker (1972); detector rent is Townsend (1979). Residual: growth dynamics + measurability. |
| Prop 2 — likelihood-ratio detector | KILLED | It is Chow's rule (1957, 1970). Cite; keep only the re-pricing design consequence. |
| Prop 3 — ruin licenses concentration | SHRUNK | Kelly–Breiman canonical; licensing half in Peters–Adamou & Spitznagel. Residual: miss rate as the argument of the −∞ condition. |
| Conj A — achievability | SHRUNK | Ingredients standard (Geifman–El-Yaniv 2017; El-Yaniv–Wiener 2010). Residual: coupling to a costed channel with a growth lower bound. |
| Conj B — converse (detector placement) | ALIVE, REPAIRED | No one states it. But false as drafted — must be restricted to the inductive regime. |
| Conj C — frequency-free removability | REPAIRED | Per-task form impossible (Vovk; Barber et al.). Class-uniform form achievable and still frequency-free. |
| Conj D — one-bit lemma | REPAIRED | Entropy lower bound unsupported; units gap closes via doubly-efficient proofs/debate/property testing in one compute model. |
| "Separate rejector" architecture | KILLED as bare claim | Named taxonomy (separated/dependent/integrated rejectors). Residual: removability framing. |

## 1. Proposition 2 is Chow's rule — relabel

- **Chow (1957)** IRE Trans. EC-6: reject option as Bayes decision with
  rejection cost — the first formal treatment.
- **Chow (1970)** IEEE Trans. IT-16: accept iff max posterior ≥
  (C_e − C_r)/(C_e − C_c) — **the threshold is exactly a cost ratio** — plus
  the error–reject tradeoff E(t) = −∫₀ᵗ s dR(s) and monotonicity. In the
  binary case the lineage is older still (Neyman–Pearson 1933; Wald).
- **Mandatory caveat — Cortes, DeSalvo & Mohri (2016), Learning with
  Rejection** (ALT): with a restricted/learned actor, the optimal rejection
  region is provably **not** expressible as a threshold on the actor's own
  confidence; the rejector must be a separate function from a separate
  hypothesis family. This *weakens* Prop 2's scope (exact posteriors only) and
  *strengthens* the architecture: even classically, the detector should not be
  built from the actor's confidence.
- Counter-datapoint to engage: **SelectiveNet (Geifman & El-Yaniv 2019)** —
  a rejector *sharing* representations with the classifier, jointly trained,
  empirically beats post-hoc rejection at low coverage. Reconciliation the
  paper must make explicit: sharing wins **average-case** selective risk;
  independence is demanded by the **worst-case fatal** guarantee (Conj B).
  These are different objectives and both results can be true.

## 2. Conjecture B — alive, nobody states it, but repair required

**The finding that matters most: no paper formalizes "the detector must lie
outside the capability restriction" as a theorem.** The claim exists as three
disconnected fragments:

- **Fang, Li, Lu, Dong, Han & Liu (2022), "Is OOD Detection Learnable?"**
  (NeurIPS Outstanding Paper; JMLR 2024 extension): impossibility theorems for
  OOD detection in restricted hypothesis spaces against adversarially chosen
  out-distributions. The proof *technology* Conj B needs — but no link to an
  actor's restriction, no loss dynamics.
- **Ulmer & Cinà (2021)** (UAI): uncertainty scores computed from a ReLU
  classifier's own representation provably fail at OOD detection — the closest
  concrete "detector inside the defect fails," but architecture-specific, no
  adversarial quantifier.
- **Reliability theory — Eckhardt & Lee (1985); Littlewood & Miller (1989);
  Littlewood & Rushby (2012)**: shared "difficulty function" ⇒ correlated
  failures ⇒ no reliability multiplication from redundancy; a checker channel
  buys a multiplicative safety bound only if its failures are independent of
  the primary's. The conceptual ancestor, in an average-case Bayesian model —
  no adversarial converse, no restriction structure.
- The **AI-control literature** (untrusted monitoring, 2023–) treats
  "monitor outside the untrusted system" as a design axiom with **no formal
  backing** — precisely the gap this paper fills.

**The repair (B is false as drafted):** **Goldwasser, Kalai, Kalai &
Montasser (2020)** construct transductive selective classifiers from
essentially the restricted class itself that achieve bounded loss under
**arbitrary** adversarial test distributions — given access to the unlabeled
test set. **Conjecture B must be stated for the inductive regime (no access to
deployment-time unlabeled data), or it is provably false.** Usefully, this
identifies the exact resource that lets a within-class detector escape:
seeing where the questions come from before answering. (Kalai & Kanade 2021
give the abstention analogue.)

Also noted: **Mozannar et al. (2023)** prove *computational* hardness of
learning classifier–rejector pairs in restricted classes (halfspaces,
NP=RP reduction) — orthogonal to B, which is information-theoretic
non-existence, but worth citing to separate the two axes.

**Novelty residual for B:** the coupling (one restriction defines both the
actor's competence and the detector's blindness) + the consequence (unbounded
loss / −∞ growth, not mere non-learnability) + the positive converse
(out-of-restriction detector restores boundedness). Position as the first
decision-theoretic, worst-case formalization of what safety engineering holds
as practice.

## 3. Conjecture C — per-task form impossible; class-uniform form survives

- **Vovk (2012); Barber, Candès, Ramdas & Tibshirani (2021), "The limits of
  distribution-free conditional predictive inference"**: object-conditional
  (per-input) coverage is impossible distribution-free — any method achieving
  it returns trivial/infinite sets at a.e. nonatomic point; even the
  approximate (1−α, δ) relaxation collapses to adjusted marginal coverage when
  required over all distributions. **This kills "per-task μ ≤ δ uniformly"
  over any rich class, as drafted.**
- What is achievable, and suffices: **group-/class-conditional** coverage over
  a pre-specified finite-VC family (Barber et al. Thm 3; Mondrian conformal),
  **shift-class-uniform** coverage over finite-dimensional covariate-shift
  classes (**Gibbs, Cherian & Candès 2023/2025**, JRSS-B), and
  **f-divergence-ball robust** sets (**Cauchois, Gupta, Ali & Duchi**, JASA
  2024).
- **The frequency-free part survives in every repaired form**: none of the
  achievable guarantees reference the historical frequency ε of fatal events.
  What dies is "per-task"; what lives is "frequency-free, class-uniform" —
  which is what the advantage-condition argument actually needs.
- Practicality caveat from MoE theory: router-collapse results (load-imbalance
  bifurcation; Shazeer 2017 aux-loss lineage) show a cheap dispatcher trained
  **jointly** with its experts has persistent self-reinforcing failure modes —
  a training-dynamics argument for detector independence, complementing B.

## 4. Conjecture D — restate in one compute model; drop the entropy bound

- Detection half confirmed asymptotically: **Chernoff–Stein** — error μ costs
  n ≈ log(1/μ)/C(P₀,P₁) samples. Rare-event caveat from **Pensia, Jog & Loh
  (COLT 2024)**: with skewed priors (π = ε small) there is a moderate-μ regime
  where sample complexity is δ-independent, governed by π·log(1/π)/I(Θ;X);
  the log(1/μ) law holds only for μ ≲ π². State D with the regime condition.
- Competence half as drafted — Ω(H(solution|task)) — has **no literature
  support**: conditional entropy bounds description length, not computation.
  Drop it.
- **The units gap closes** by putting both costs in query/time complexity:
  - **Goldwasser, Kalai & Rothblum (JACM 2015)** doubly-efficient interactive
    proofs: verifier nearly-linear, prover polynomial — verification provably
    cheaper than generation, same units.
  - **Brown-Cohen, Irving & Georgiev (2023)** doubly-efficient debate: the
    ML-safety instantiation (+ Lean formalization).
  - **Goldreich, Goldwasser & Ron (1998)** property testing / **RVW (2013)**
    IPs of proximity: sublinear detection of "ε-far from in-scope" vs linear+
    production — the promise gap mirrors μ.
- Restated D becomes a **corollary**, not a conjecture — at the price that the
  verifier interacts with a prover (possibly the competent system itself)
  rather than testing in isolation. The standalone-detector version keeps only
  the hypothesis-testing bound.

## 5. Economics — cite, don't derive

- **Advantage condition = Ehrlich & Becker (1972)** (market insurance vs
  self-insurance substitution margin: generalist's carried breadth =
  self-insurance; per-event channel = market insurance). **Detector rent =
  Townsend (1979)** costly state verification (verify only in the bad state =
  escalate only on detection). Carrying-cost-vs-per-event = **Williamson**
  (TCE frequency dimension) / **Van Mieghem (2003)** (capacity vs spot).
- **Prop 3's engine is canonical**: Kelly (1956), Breiman (1961),
  MacLean–Thorp–Ziemba ("the E-log bettor never risks ruin");
  drawdown-constrained form Grossman & Zhou (1993). The licensing half exists:
  **Peters & Adamou (2015)** (insurance raises time-average growth for both
  parties), **Spitznagel (2021)** (tail hedges enable larger risky
  allocation), **Gollier & Pratt (1996)** (background-risk mirror in EU).
- **Frequency-free spirit exists, the result doesn't**: Taleb & Douady (2013)
  detect fragility via payoff convexity, "probability-free"; precautionary
  ruin logic (Taleb et al. 2014). No contract-level result makes the buy-vs-
  carry decision invariant to unknown ε via per-event-conditioned guarantees.

## 6. The defensible residual (what the paper may claim)

1. **The converse (repaired Conj B)** — first worst-case, decision-theoretic
   formalization of detector placement: restriction couples actor and
   detector ⇒ unbounded loss; inductive regime; positive converse included.
2. **Miss rate as the binding constraint** — the −∞ condition argued on the
   detector's per-event false-negative rate, not on event frequency (no source
   found makes detector reliability the argument of the ruin condition).
3. **Frequency-free, class-uniform removability (repaired Conj C)** — the
   buy-vs-carry decision needs escalation-trigger observability, never a
   correct ε estimate.
4. **The synthesis** — one growth equation in which Chow (detector threshold),
   Ehrlich–Becker/Townsend (channel pricing), Kelly (dynamics), and
   selective-prediction bounds are all visible as terms, applied to a
   deliberately maintained competence gap rather than a portfolio position.
5. **Removability as a framing** — the detector survives removal of the
   competence because it never depended on the actor's representations; not
   present in the rejector-taxonomy literature.

Claims 1–3 are theorem-shaped and unproved; 4–5 are framing. Everything else
in FORMALIZATION.md now carries citations instead of claims.

## Second-round check (2026-07-13, later): Theorems M and E/E′

Run when the proofs outgrew the first check; two research passes, primary
PDFs read where verdicts depended on precise statements (two confabulated
auto-summaries were caught and corrected against the actual papers).

### Theorem M (least-favorable mixture duality) — technology classical, application survives

- **Duality technology — cite, don't claim.** Wald (1945, *Ann. Math.*
  46:265–280; 1950 book): minimax with randomized rules = Bayes against a
  least favorable prior. Huber (1965, AMS 36:1753–1758): least favorable
  pair + clipped-LR test, minimax at every finite $n$. **Huber & Strassen
  (1973, *Ann. Statist.* 1:251–263):** composite minimax testing under
  2-alternating capacities collapses to an ordinary NP test against a least
  favorable pair — exactly M's collapse move, fifty years early. Fillatre
  (2017, *Signal Processing* 141:322–330): the finite-LP / dual-LP
  least-favorable-prior computation, arbitrary losses.
- **The coin mechanism is in print.** Randomized rules realize the ROC
  convex hull: NP lemma with randomization (Lehmann–Romano TSH Thm 3.2.1),
  Provost & Fawcett (2001, *Mach. Learn.* 42:203–231) for the ML ROCCH/hybrid
  form; Fauß–Zoubir–Poor (arXiv:2105.09836) §V-D states point-mass LR ⇒
  linear ROC segments ⇒ randomization required — our exact mechanism. Cite
  all three; claim none.
- **Mandatory distinction:** Managoli, Sahasranand & Prabhakaran
  (arXiv:2501.12938) already pair robust hypothesis testing with abstention
  — asymptotic error exponents, sample corruption, *unrestricted* detectors.
  "Abstention under distributional uncertainty" is not novel as of 2025. M's
  claim must sit at: the single-shot premium game over a *restricted* class,
  the closure deficit = ROC-set non-convexity identification, and the
  coin-vs-cross-leak split. Affirmative evidence the classical canon skipped
  the reject option: the Fauß–Zoubir–Poor survey contains no abstention as a
  decision action anywhere.

### Theorems E/E′ (learned detector) — E pure technology; E′ a gap-fill between two literatures

- **Canonical bounds confirmed:** Vapnik–Chervonenkis (1971, *TPA*
  16:264–280) two-sided; **BEHW (1989, JACM 36:929–965, Thm 2.1(ii)(a))**
  is the version-space bound E′ uses, with the EHKV Ω(d/ε) lower bound
  making the realizable 1/ε vs agnostic 1/ε² dichotomy tight. Textbooks:
  Anthony–Bartlett 1999; Shalev-Shwartz–Ben-David 2014 (Thms 6.7–6.8).
- **E′'s problem is KKM's.** Kalai–Kanade–Mansour (JCSS 78:1481–1495)
  positive-reliable learning: one error type forced to ~0 against a
  realizable-on-that-side benchmark — E′'s problem statement, verbatim. But
  their reduction pays *agnostic* rates at ε² (the contribution was
  computational); no version-space selection, no one-sided fast rate.
- **E′'s setup is NP classification at α = 0.** Cannon–Howse–Hush–Scovel
  (2002), Scott–Nowak (2005, IEEE-IT 51:3806–3819, Thm 3), Rigollet–Tong
  (2011, COLT), Tong–Feng–Li (2018, *Sci. Adv.*): all α > 0, all two-sided
  √ rates. None does zero-empirical-miss selection; none states the
  constrained-side-linear / objective-side-√ asymmetry in a severity
  parameter.
- **Must engage by name:** Casacuberta & Kanade (NeurIPS 2025), multigroup
  selective classification extending KKM — group-wise abstention
  competitiveness at agnostic rates. No zero-miss certification, no
  cell-reweighting robustness, no severity asymmetry — but it owns
  "group-wise reliable abstention." Also cite Mondrian conformal (Vovk;
  Angelopoulos et al. arXiv:2411.11824) for per-cell calibration and
  multi-distribution learning (arXiv:2307.12135) for
  worst-case-over-k-distributions at agnostic rates.
- **"Training bill linear in L" survives** — no cost-sensitive or NP paper
  states it — but it is an elementary corollary of the 1/ε-vs-1/ε²
  dichotomy and the paper should frame it as exactly that.

### Net effect on claims

M and E′ both keep their applications with narrowed framing: M is not
"abstention under uncertainty" (taken) but the restricted-class premium
game + obstruction split; E′ is not a new framework (KKM + Scott–Nowak own
the frame) but the sample-complexity analysis both literatures skipped,
assembled per-cell under reweighting. All "await the literature check"
flags in PROOFS.md and PAPER.md are now discharged with real citations.

## Source index

Chow 1957 (ieeexplore.ieee.org/document/5222035) · Chow 1970
(dl.acm.org/doi/10.1109/TIT.1970.1054406) · Franc et al. JMLR 2023
(arXiv:2101.12523) · Cortes–DeSalvo–Mohri (cs.nyu.edu/~mohri/pub/rej.pdf) ·
El-Yaniv–Wiener JMLR 2010 · Geifman–El-Yaniv 2017 (arXiv:1705.08500), 2019
SelectiveNet (arXiv:1901.09192) · Hendrickx et al. survey (arXiv:2107.11277) ·
Madras et al. (arXiv:1711.06664) · Mozannar–Sontag 2020 · Mozannar et al. 2023
(proceedings.mlr.press/v206/mozannar23a) · Fang et al. (arXiv:2210.14707;
JMLR 25:23-1257) · Zhang–Goldstein–Ranganath (arXiv:2107.06908) · Ulmer–Cinà
(arXiv:2012.05329) · Jones et al. (arXiv:2010.14134) · Goldwasser et al. 2020
(arXiv:2007.05145) · Kalai–Kanade (arXiv:2105.14119) · Eckhardt–Lee 1985 /
Littlewood–Miller 1989 (ieeexplore.ieee.org/document/58771) ·
Littlewood–Rushby 2012 · Vovk 2012 · Barber et al. 2021 (arXiv:1903.04684) ·
Gibbs–Cherian–Candès (arXiv:2305.12616) · Cauchois et al. (arXiv:2008.04267) ·
Pensia–Jog–Loh (arXiv:2403.16981) · GKR 2015 (dl.acm.org/doi/10.1145/2699436) ·
Brown-Cohen et al. (arXiv:2311.14125) · GGR 1998 · RVW 2013 · Kelly 1956 ·
Breiman 1961 · MacLean–Thorp–Ziemba (stat.berkeley.edu/~aldous/157/Papers/
Good_Bad_Kelly.pdf) · Grossman–Zhou 1993 · Peters–Adamou (arXiv:1507.04655) ·
Spitznagel 2021 · Gollier–Pratt 1996 · Ehrlich–Becker 1972 · Townsend 1979 ·
Taleb–Douady (arXiv:1208.1189) · Taleb et al. 2014 (arXiv:1410.5787) ·
Williamson · Van Mieghem 2003 · Chen et al. MoE (arXiv:2208.02813).

Second round: Wald 1945 (*Ann. Math.* 46:265) & 1950 · Huber 1965 (AMS
36:1753) · Huber–Strassen 1973 (*Ann. Statist.* 1:251) · Fillatre 2017
(*Signal Proc.* 141:322) · Fauß–Zoubir–Poor survey (arXiv:2105.09836) ·
Managoli–Sahasranand–Prabhakaran (arXiv:2501.12938) · Provost–Fawcett 2001
(arXiv:cs/0009007) · Lehmann–Romano TSH 3e Thm 3.2.1 · VC 1971 (*TPA*
16:264) · BEHW 1989 (JACM 36:929, Thm 2.1) · EHKV 1989 ·
Anthony–Bartlett 1999 · Shalev-Shwartz–Ben-David 2014 (Thms 6.7–6.8) ·
Cannon et al. 2002 (LA-UR-02-2951) · Scott–Nowak 2005 (IEEE-IT 51:3806) ·
Rigollet–Tong 2011 (COLT) · Tong–Feng–Li 2018 (*Sci. Adv.* 4:eaao1659) ·
Kalai–Kanade–Mansour (JCSS 78:1481) · Kanade–Thaler 2014 (arXiv:1402.5164) ·
Rothblum–Yona 2021 (arXiv:2105.09989) · Casacuberta–Kanade NeurIPS 2025 ·
Angelopoulos et al. (arXiv:2411.11824) · multi-distribution learning
(arXiv:2307.12135) · Al Makdah et al. (arXiv:2104.02334) · Soen et al.
(arXiv:2405.18686) · Grigoryan et al. (arXiv:1102.3520).
