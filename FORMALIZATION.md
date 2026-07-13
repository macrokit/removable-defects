# Formalization

The mathematical model behind the README's claims. Status labels are strict, per
repo convention:

- **Proposition** — provable with standard tools; proof sketched or cited.
- **Conjecture** — theorem-shaped, believed, not proved. Each carries its proof
  obligations.
- **Gap** — a place where the model is known to be crude and the crudeness might
  matter.

Nothing here is claimed as novel until the literature check (§9) is done.

---

## 1. Primitives

- **Task space** $X$, one task drawn per period from distribution $P$.
- **Competence set** $C \subseteq X$: tasks the specialist handles well. The
  **defect** is the complement — everything the specialist cannot do.
- **Fatal set** $F \subseteq X$: tasks where acting *without competence* is
  catastrophic. Tasks in $F \cap C$ are handled competently and are not
  dangerous; the danger zone is the **fatal exposure** $F \setminus C$, with
  probability $\varepsilon := P(F \setminus C)$, assumed small.
- **Payoffs per task:**
  - $x \in C$, act: gain $g > 0$ (mean gain over $C$).
  - $x \notin C \cup F$, act: harmless waste, loss $\ell \geq 0$ (small).
  - $x \in F \setminus C$, act: loss $L \gg g$ (possibly ruinous).
  - escalate (any $x$): pay compensation price $p$; the task is handled by the
    channel (market, principal, rented judgment), so no loss $L$ is realized.
- **Detector** $d: X \to \{\text{act}, \text{escalate}\}$ with running cost
  $c_d$ per period and two error rates:
  - **miss rate** $\mu := P(d = \text{act} \mid x \in F \setminus C)$ — the
    fatal error;
  - **false-alarm rate** $\alpha := P(d = \text{escalate} \mid x \in C)$ — the
    self-inflicted tax, escalating work the specialist could have done.

The detector never solves the out-of-scope task. It answers one binary
question — "is this mine?" — and routes.

## 2. The growth equation

Expected per-period growth of the architecture:

$$G = \underbrace{(1-\alpha)\, g\, P(C)}_{\text{specialization gain}}
    \;-\; c_d
    \;-\; \underbrace{p \,\big[\alpha P(C) + P(\text{escalate},\, x \notin C)\big]}_{\text{compensation bill}}
    \;-\; \underbrace{\varepsilon\, \mu\, L}_{\text{missed-fatal loss}}
    \;-\; (\text{benign waste terms in } \ell).$$

Everything downstream is bookkeeping on this equation. Three structural facts
worth noticing before any theorem:

1. $G$ is **linear in $\mu$ and $\alpha$** — so the optimal detector operating
   point is a standard decision-theory problem (§4).
2. The fatal term is a **product of three factors** ($\varepsilon$, $\mu$, $L$).
   The architecture attacks $\mu$ (detector) and caps $L$ (bounded blast
   radius); it deliberately does *not* rely on estimating $\varepsilon$ well —
   see §7.
3. The generalist's alternative is not free: it pays a **carrying cost**
   $c_{\text{gen}}$ every period for capability it rarely uses, and typically
   $g_{\text{gen}} < g$ on $C$ (breadth dilutes depth — this is the value
   theory's diversity-vs-redundancy result applied to one mind).

## 3. Proposition 1 — the advantage condition

**Claim.** The specialist-with-detector beats the generalist iff

$$\big[(1-\alpha)\,g - g_{\text{gen}}\big] P(C) + c_{\text{gen}}
   \;>\; c_d + p\,\mathbb{E}[\text{escalations}] + \varepsilon\,\mu\,L .$$

Informally: *specialization gain per period > fatal-event frequency × price of
compensation*, once the detector's rent and false alarms are counted.

**Status: Proposition, with pedigree.** This is algebra on the growth
equation; structurally it is the Ehrlich–Becker (1972) market-insurance vs
self-insurance substitution margin (generalist's carried breadth =
self-insurance; per-event channel = market insurance), with the detector rent
being Townsend (1979) costly state verification. Cite both; the residual here
is the growth dynamics and the measurability. The interesting content is
not the proof but the observation that **every term is measurable** in an
instrumented environment (Agent World: capability score × market prices vs.
observed escalation spend), which turns "should I be deficient?" into a
computable position.

## 4. Proposition 2 — the optimal detector is a likelihood-ratio test

Since $G$ is linear in $(\mu, \alpha)$, maximizing $G$ over detectors with a
given ROC curve is a Neyman–Pearson / Bayes-risk problem. The optimal $d$ is a
likelihood-ratio test between the in-scope and out-of-scope task distributions,
with threshold set by the cost ratio

$$\tau \;=\; \frac{g\,P(C) + p\,(\text{net escalation cost on } C)}{\varepsilon\,(L - p)} \quad\text{(up to bookkeeping)},$$

i.e. the detector should get exactly as paranoid as the ratio of "what a false
alarm wastes" to "what a miss costs" dictates — no more.

**Status: Proposition — and it is Chow's rule (1957, 1970), not ours.** The
threshold-as-cost-ratio form, the error–reject tradeoff, and monotonicity are
all proved there; present as an application with citation. **Mandatory scope
caveat (Cortes–DeSalvo–Mohri 2016):** Chow-optimality holds only under exact
posteriors — with a restricted or learned actor, the optimal rejection region
is provably *not* a threshold on the actor's own confidence, and the rejector
must come from a separate hypothesis family. This shrinks the proposition and
strengthens the architecture. What we keep is the design consequence: **the
detector's threshold is an economic quantity**, not a fixed engineering
constant. When $L$ grows (higher-stakes deployment), the same
detector should slide along its ROC curve toward paranoia. A system whose
escalation threshold cannot be re-priced is mis-built by this theory's lights.

## 5. Multiplicative dynamics — the Kelly connection

The additive model above understates the tail. Under multiplicative wealth
dynamics $W_{t+1} = W_t\,(1 + r_t)$, take $G = \mathbb{E}[\log(1+r)]$ (Kelly;
the value theory's growth criterion).

**Proposition 3 (ruin makes the converse trivial).** If a missed fatal event
means true ruin ($r = -1$), then any $\mu > 0$ with $\varepsilon > 0$ gives
$G = -\infty$: long-run growth is destroyed by *any* nonzero miss rate,
regardless of how large the specialization gain is. Concentration is only
Kelly-optimal when ruin is excluded from the support.

**Status: Proposition, canonical engine** ($\log 0 = -\infty$). The
ruin-exclusion lemma is Kelly (1956) / Breiman (1961) folklore ("the E-log
bettor never risks ruin"), drawdown-constrained form in Grossman & Zhou
(1993); the licensing half exists in Peters & Adamou (2015) and Spitznagel
(2021). What is ours is only the argument of the condition: **the detector's
per-event miss rate on ruin states, not the event frequency, is what drives
growth to $-\infty$** — detector reliability, not frequency knowledge, is the
binding constraint. Two consequences:

- In the true-ruin regime, the architecture is not an optimization but a
  **precondition**: the detector-plus-channel is what re-admits concentrated
  strategies into the feasible set at all. Tail insurance does not compete with
  the edge; it *licenses* the edge.
- The economically interesting regime is $L$ **large but finite** (severe
  drawdown, not death). There the advantage condition (§3) has real trade-offs,
  and the theorems below are non-trivial.

**Gap.** The model treats periods as i.i.d. Fatal events cluster (regimes,
cascades); the clustering regime is where $\varepsilon$-estimates break worst.
Flagged, not solved — see §7.

## 6. The detector-placement theorems

Model the defect as a **restriction operator** $D$ on the agent: it maps the
unrestricted agent to one with competence set $C$ (a restricted function class,
a coarsened observation channel, a pruned model — the formalism should cover
all three). Say the **detector lies outside the defect** if $d$'s error pair
$(\mu, \alpha)$ is unchanged by $D$.

**Definition (removable).** A defect $D$ is *removable* if there exists a
routing-plus-compensation policy whose growth $G$ is bounded below,
uniformly over an uncertainty class $\mathcal{P}$ of task distributions
(equivalently: no ruin, for all $P \in \mathcal{P}$).

**Conjecture A (achievability).** If the detector lies outside the defect with
$\mu \leq \delta$, and the compensation channel is affordable
($p < \infty$, and $c_d + p\,\mathbb{E}[\text{escalations}]$ below the
specialization margin), then $D$ is removable, with $G$ bounded below by an
explicit function of $(\delta, p, c_d, L)$.

**Conjecture B (converse).** *In the inductive regime — the agent has no
access to unlabeled deployment-time data* — if $D$ degrades the detector —
$\mu$ bounded away from $0$ for some $P \in \mathcal{P}$ — then for $L$ large
enough no policy makes $D$ removable: $\inf_{P \in \mathcal{P}} G \to -\infty$
as $L \to \infty$, regardless of specialization gain and channel price.

The regime condition is not cosmetic: with transductive access,
Goldwasser–Kalai–Kalai–Montasser (2020) build selective classifiers from
essentially the restricted class itself that bound loss under *arbitrary*
adversarial shift — **B is provably false without the condition.** Usefully,
this pins down the one resource that lets a within-class detector escape the
impossibility: seeing where the questions come from before answering.

Together these give the honest form of the README's slogan. Note it is **not**
a clean iff on placement alone:

> a defect is removable **iff** the detector lies outside it **and** the
> compensation channel is priced below the advantage margin.

Placement is the necessary condition (Conjecture B needs no channel assumption
at all — a blind detector cannot be bought back). Affordability is what turns
necessity into sufficiency.

**Proof obligations.**
- A: mostly assembly — concentration of measure on escalation counts, plus the
  linearity of §2. The work is making the bound uniform over $\mathcal{P}$.
- B: the real theorem. Needs a construction: for any detector *inside* the
  defect, exhibit a $P \in \mathcal{P}$ concentrating fatal mass exactly where
  the restricted detector is blind. This is where the adversarial question
  (backlog #3) enters: Conjecture B *is* the adversarial case with the
  adversary choosing $P$ once. The iterated-adversary version is open.
- Both: pin down the restriction formalism so that "detector unchanged by $D$"
  is well-defined for function-class, observation-channel, and pruning defects
  simultaneously — or split into three theorems (backlog #6 suggests the
  split may be forced).

## 7. Robustness — the heavy-tail honest version (backlog #1)

$\varepsilon$ is estimated from history, and tail events are precisely what
history underestimates. The model's answer is structural, and it can be said
precisely:

The fatal term is $\varepsilon\,\mu\,L$. The architecture's defense does not
sharpen $\hat{\varepsilon}$; it makes the *other two factors* small by
construction — $\mu$ via a detector that conditions on the **current task**,
not on historical frequency, and $L$ via spend ceilings (bounded blast radius).

**Conjecture C (frequency-free removability), repaired form.** The originally
drafted per-task version — $\mu \leq \delta$ conditioned on the current input,
uniformly over a rich distribution class — is **impossible**: distribution-free
object-conditional coverage forces trivial detectors (Vovk 2012; Barber,
Candès, Ramdas & Tibshirani 2021, whose Theorem 2 shows even the approximate
relaxation collapses to adjusted marginal coverage). The achievable statement:

Fix a *structured* uncertainty class $\mathcal{P}$ — a finite-VC family of
subpopulations/shifts (Barber et al. Thm 3; Gibbs–Cherian–Candès 2023) or an
$f$-divergence ball around the training distribution (Cauchois et al. 2024).
If the detector achieves $\mu \leq \delta$ **uniformly over $\mathcal{P}$**
and $L$ is capped at $\bar{L}$, the worst-case growth penalty is at most
$\delta\,\bar{L}$ per fatal event — **independent of the unknown fatal-event
frequency $\varepsilon$**, since none of these guarantees reference it. The
minimax advantage condition then needs only an upper bound on how often
escalation is *triggered* (observable), never a correct estimate of how often
it is *needed*. What died in the repair is "per-task"; what survives —
frequency-freeness — is the part the architecture actually needs.

**Proof obligation:** assemble the growth bound from the cited class-uniform
coverage lemmas; state honestly that protection is relative to the declared
class $\mathcal{P}$, not to nature. Practicality caveat from MoE theory:
router-collapse dynamics show a dispatcher trained *jointly* with its experts
has persistent self-reinforcing failure modes — a training-dynamics argument
for detector independence that complements Conjecture B.

## 8. Detector economics — why the architecture is affordable (backlog #2)

The claim to make precise: *detection is orders of magnitude cheaper than
competence.*

- **Detection** is a binary hypothesis test between in-scope and out-of-scope
  task distributions. Testing error decays exponentially with effort, at rate
  given by the Chernoff information $C(P_{\text{in}}, P_{\text{out}})$; the
  cost of miss rate $\mu$ scales like $\log(1/\mu) / C(P_{\text{in}}, P_{\text{out}})$.
  The detector extracts at most one bit per task, $I(x;\, \mathbb{1}[x \in F \setminus C]) \leq H(\varepsilon)$, which for small
  $\varepsilon$ is itself small.
- **Competence** must *produce the solution*: its cost is bounded below by
  something like the conditional description complexity of the answer,
  $H(y \mid x)$, or the compute to search for it.

**Conjecture D, restated (the units gap is closable).** The original draft
compared detector cost $O(\log 1/\mu)$ (samples, via Chernoff–Stein) against a
competence bound $\Omega(H(y \mid x))$ (bits) — different units, and the
entropy lower bound has no literature support (conditional entropy bounds
description length, not computation). Both halves get repaired:

- **Detection half, with regime condition.** Chernoff–Stein gives
  $n \approx \log(1/\mu)/C(P_{\text{in}}, P_{\text{out}})$ asymptotically, but
  Pensia–Jog–Loh (COLT 2024) show that under skewed priors — and fatal events
  *are* the skewed-prior case, $\pi = \varepsilon$ small — there is a
  moderate-$\mu$ regime where sample complexity is $\mu$-independent, governed
  by $\pi \log(1/\pi)/I(\Theta; X)$. The $\log(1/\mu)$ law holds for
  $\mu \lesssim \pi^2$. State D only in that regime.
- **One cost model.** Put both detection and production in query/time
  complexity: doubly-efficient interactive proofs (Goldwasser–Kalai–Rothblum,
  JACM 2015: verifier nearly-linear, prover polynomial), doubly-efficient
  debate (Brown-Cohen–Irving–Georgiev 2023), and property testing / IPs of
  proximity (GGR 1998; RVW 2013: sublinear detection of "$\epsilon$-far from
  in-scope" vs. linear-plus production, with the promise gap playing $\mu$'s
  role). In this model the detection/competence separation is a **theorem, not
  a conjecture** — at the price that the verifier interacts with a prover
  (possibly the competent system itself). The standalone-detector version
  keeps only the hypothesis-testing bound, which is weaker but true.

The NP-flavored slogan stays out of the paper; the interactive-proof
restatement replaces it.

## 9. Literature check — DONE (2026-07-13); see LITERATURE.md

The check ran as four parallel research passes; full per-paper verdicts,
citations, and the source index live in `LITERATURE.md`. Net effect on this
document (repairs already applied above):

- **Prop 1** — structure is Ehrlich–Becker (1972) + Townsend (1979); cited.
- **Prop 2** — is Chow's rule (1957, 1970); relabeled as an application, with
  the Cortes–DeSalvo–Mohri (2016) scope caveat.
- **Prop 3** — engine is Kelly–Breiman canonical; residual claim narrowed to
  miss-rate-as-binding-constraint.
- **Conjecture B** — nobody states it (the AI-control literature holds it as
  an axiom with no formal backing), but it was false as drafted; now
  restricted to the inductive regime per Goldwasser et al. (2020).
- **Conjecture C** — per-task form killed by conditional-coverage
  impossibility (Vovk 2012; Barber et al. 2021); repaired to the
  frequency-free, class-uniform form.
- **Conjecture D** — entropy bound dropped; restated in a single
  query/time-complexity model via doubly-efficient IPs / debate / property
  testing, where the separation is a theorem.

**The defensible residual** (LITERATURE.md §6): the repaired converse; miss
rate as the argument of the ruin condition; frequency-free class-uniform
removability; the synthesis growth equation; and removability-as-framing (a
detector that survives removal of the competence because it never depended on
the actor's representations). One tension the paper must engage directly:
SelectiveNet-style *shared* rejectors empirically beat independent post-hoc
ones on average-case selective risk — the reconciliation is that sharing
optimizes the average case while independence is demanded by the worst-case
fatal guarantee; both can be true.

## 10. Mapping to the backlog

| QUESTIONS.md | Here |
|---|---|
| #1 heavy tails | §7, Conjecture C |
| #2 detector economics | §8, Conjecture D + the units gap |
| #3 adversarial removal | §6, Conjecture B's proof obligation (one-shot case) |
| #5 fleet composition | §2's carrying-cost term, summed over N detectors — not yet modeled |
| #6 one concept or three | §6's restriction-formalism obligation may force the split |

\#4 (the human case) is deliberately absent: this document prices artifacts.
Applying the growth equation to persons imports every ethical issue flagged in
QUESTIONS.md #4, and no theorem here resolves any of them.
