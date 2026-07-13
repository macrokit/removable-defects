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

**Status: Proposition.** This is algebra on the growth equation; the only work
is defining the generalist's payoff symmetrically. The interesting content is
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

**Status: Proposition** (classical detection theory). Its role here is a design
consequence: **the detector's threshold is an economic quantity**, not a fixed
engineering constant. When $L$ grows (higher-stakes deployment), the same
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

**Status: Proposition** ($\log 0 = -\infty$; the content is the framing). Two
consequences:

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

**Conjecture B (converse).** If $D$ degrades the detector — $\mu$ bounded away
from $0$ for some $P \in \mathcal{P}$ — then for $L$ large enough no policy
makes $D$ removable: $\inf_{P \in \mathcal{P}} G \to -\infty$ as
$L \to \infty$, regardless of specialization gain and channel price.

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

**Conjecture C (frequency-free removability).** Fix the uncertainty class
$\mathcal{P} = \{P : P(F \setminus C) \leq \bar\varepsilon\}$ with
$\bar\varepsilon$ *unknown or adversarial*. If $\mu \leq \delta$ uniformly over
$\mathcal{P}$ (a per-task guarantee, not a frequency guarantee) and $L$ is
capped at $\bar{L}$, then the worst-case growth penalty is at most
$\delta\,\bar{L}$ per fatal event — independent of $\varepsilon$. The minimax
advantage condition then needs only an upper bound on how often escalation is
*triggered* (which is observable), never a correct estimate of how often it is
*needed*.

**Proof obligation:** make "per-task $\mu \leq \delta$ uniformly over
$\mathcal{P}$" achievable by some detector class — this is exactly a selective
prediction / conformal-style coverage guarantee, and the literature (§9) may
already contain the needed lemma.

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

**Conjecture D (one-bit lemma).** Under a cost model where both detection and
competence are priced in the same units, detector cost is
$O\!\big(\log(1/\mu)\big)$ while competence cost over $X \setminus C$ is
$\Omega\!\big(\sup_x H(y\mid x)\big)$, so the ratio diverges as the defect
widens — *the wider the defect, the better the bargain.*

**Gap (serious).** The two costs above are currently in **different units**
(samples-to-decide vs. bits-to-produce). Conjecture D is not well-posed until a
single cost model covers both — compute, or money via the market. This is the
hardest open modeling choice in the document, and the NP-flavored slogan
("verify is easier than solve") stays out of the paper until it is closed or
replaced with the honest hypothesis-testing statement, which is weaker but
true.

## 9. Literature check — do this before claiming anything

Each item names what would kill or shrink a novelty claim. From memory —
**verify all of it**:

- **Chow (1957, 1970)** — the reject option; optimal rejection threshold as a
  cost ratio. Proposition 2 is almost certainly a reskin of Chow's rule. Cite,
  don't claim.
- **Learning to defer / selective prediction** (Cortes et al.; Geifman &
  El-Yaniv; Madras et al.; Mozannar & Sontag) — cost-based deferral to an
  expert, coverage/risk curves. Conjecture A's machinery likely exists here.
  The check that matters: does anyone state **Conjecture B** — an impossibility
  result for detectors *sharing the restriction* that created the blind spot?
  OOD-detection-fails-under-shift results are nearby; find the closest and
  position against it.
- **Kelly (1956), ruin theory, drawdown constraints** — §5 is standard; the
  contribution is only the framing (insurance *licenses* concentration).
- **Insurance economics / catastrophe pricing** — the advantage condition is
  structurally a premium-vs-self-insurance decision; find the canonical form.
- **Mixture-of-experts routing; conformal prediction** — for §7's per-task
  coverage guarantee.
- The defensible residual, if the checks go as expected: the **converse
  (Conjecture B)**, the **per-event pricing of removal** as a declared economic
  position, and the **synthesis** — one growth equation in which Chow, Kelly,
  and the deferral literature are all visible as terms.

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
