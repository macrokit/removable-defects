# The converse and the achievability half, proved

This document proves both directions of the detector-placement iff
(FORMALIZATION.md §6, Conjectures B and A), after an honest repair that the
proof itself forced. The proofs are elementary once the model is right; per
repo convention, we say so — the content is in the definitions and in the
quantifier structure, not in analytic difficulty.

**What is proved here:**
- Lemma 1 (coupling): inside an observation defect, the detector's gain-capture
  rate and its fatal-miss rate are the same number.
- Theorem 1 (observation defects): the specialization premium collapses to
  zero, *unconditionally* — transductive access does not help.
- Theorem 1′ (value formula): the exact within-defect value, with the
  removability coefficient σ₀ (residual separability) governing the collapse.
- Theorem 2 (capacity defects): the premium collapses in the inductive regime,
  and the inductive condition is exactly a minimax gap — transduction closes
  it, matching Goldwasser et al. (2020).
- **Theorem 2′ (capacity value formula, §7):** for *any* detector class, the
  premium is the support function of the class's ROC set at an economic price
  vector, with removability coefficient the zero-leak capture capacity
  $\bar\sigma_0(H)$. This subsumes Theorem 1′ — observation and capacity
  defects obey **one value formula** — and relocates the split entirely to
  joint realizability across confusable directions (the transduction gap).
- **The deficit characterized (§8):** the joint-realizability deficit is the
  limiting transduction gap in premium units; it decomposes into exactly two
  obstructions (cross-leak, non-closure); exact formulas for monotone classes
  ("global paranoia") and halfspaces (convex non-separability); and Theorem R
  bounds it by a direction router's confusion mass at the same two prices —
  the router is itself a detector, the question recurses, and the recursion
  is the observation/capacity split in its third formulation.
- Corollary 3 (growth form): under multiplicative dynamics, survival and
  specialization gain cannot coexist inside the defect.
- **Theorem A (achievability, §10):** a detector *outside* the defect earns a
  worst-case premium bounded below by an explicit $\Pi^\* > 0$ under the
  advantage condition; the premium identity has three per-unit-priced terms.
  A meets B exactly at the $L \to \infty$ boundary, and the two give the
  **full iff** (Corollary A+B).

**What the proof forced us to admit:** the drafted definition of "removable"
was vacuous, and the drafted Conjecture B was false under it (§2). The
repaired notion is *profitable removability*.

**Still open (§12):** end-to-end achievability with the detector's rates
*learned* rather than assumed, the iterated adversary, and the exact deficit
for non-union-closed classes on ensemble directions.

---

## 1. Model

Task space $(X, \Sigma)$, one task per period. Competence set $C \in \Sigma$;
fatal exposure $E := F \setminus C \in \Sigma$, disjoint from $C$. Per-task
payoffs for the two actions:

| | $x \in C$ | $x \in E$ |
|---|---|---|
| **act** | $+g$ | $-L$ |
| **escalate** | $-p$ | $-p$ |

with $g, p > 0$ and $L$ the fatal loss. A detector is a measurable
$d : X \to \{a, e\}$; the agent's per-period expected payoff under task
distribution $P$ is $G_P(d) = \mathbb{E}_P[\text{payoff}]$. The detector rent
$c_d$ is a constant shift within any fixed detector class and is dropped. A
third action *decline* (payoff $0$, task not done) changes nothing below
except replacing the forfeit value $-p$ by $0$; we note where.

Throughout, fix $\bar\varepsilon \in (0,\, g/(g+p))$ — the fatal-mass budget —
and define the **critical loss**

$$L^\* \;:=\; \frac{(1-\bar\varepsilon)\,g + p}{\bar\varepsilon}.$$

**Two formalizations of the defect.**

- **Observation defect.** The agent perceives tasks only through a measurable
  map $\varphi : X \to Z$; its detector must be $\sigma(\varphi)$-measurable.
  Write $\mathcal{D}_R$ for this class. (Coarsened observation channel;
  covers pruned/quantized perception.)
- **Capacity defect.** The agent perceives $x$ fully but its detector must
  come from a restricted class $H \subseteq \{a,e\}^X$ containing the two
  constant maps. (Restricted function class.)

**Adversarial families.** For probability measures $\nu_0$ (supported in $C$)
and $\nu_1$ (supported in $E$), define the mixture family

$$\mathcal{F}(\nu_0, \nu_1) \;:=\; \{\, P_\lambda = (1-\lambda)\nu_0 + \lambda \nu_1 \;:\; \lambda \in [0, \bar\varepsilon] \,\}.$$

All results are stated for uncertainty classes $\mathcal{P}$ containing such a
family; protection claims are always relative to the declared class, never to
nature (FORMALIZATION.md §7's honesty rule).

## 2. The definition was vacuous — profitable removability

FORMALIZATION.md defined a defect *removable* if some policy keeps $G$
bounded below uniformly over $\mathcal{P}$. Writing the proof exposed the
flaw: **the constant detector $d \equiv e$ (always escalate) lies in every
detector class** — constants are measurable for any $\sigma(\varphi)$ and were
assumed in $H$ — **and bounds $G \geq -p$ regardless of the defect.** Under
the drafted definition every defect is removable and Conjecture B was
vacuously false, independently of the Goldwasser escape already recorded in
LITERATURE.md. Survival was never the question; *paid-for* survival is.

**Definition (specialization premium).** For a detector class $\mathcal{D}$
and family $\mathcal{P}$,

$$\Pi(\mathcal{D}) \;:=\; \sup_{d \in \mathcal{D}} \inf_{P \in \mathcal{P}} G_P(d) \;-\; \inf_{P \in \mathcal{P}} G_P(d \equiv e),$$

the worst-case value above the always-escalate baseline. (With *decline*
available, the baseline is $0$; all premium statements are unchanged.)

**Definition (profitably removable).** The defect is *profitably removable*
on $\mathcal{P}$ if $\Pi(\mathcal{D}_{\text{defect}}) > 0$: some within-defect
policy retains part of the specialization gain while avoiding ruin.

The theorems below say: with a confounded detector the premium is zero (or
$\to 0$), while an out-of-defect detector on the same family earns premium
$(1-\bar\varepsilon)(g+p)$. That is the honest, non-vacuous form of "the
defect must not include the detector."

## 3. Lemma 1 — the coupling lemma

**Lemma 1.** Let $d \in \mathcal{D}_R$ (observation defect) and let
$\nu_0, \nu_1$ satisfy $\varphi_\* \nu_0 = \varphi_\* \nu_1$ (equal
pushforwards: the coarsened view cannot distinguish the safe ensemble from
the fatal one). Then

$$\underbrace{\nu_0(d = a)}_{\text{gain-capture rate } q_0} \;=\; \underbrace{\nu_1(d = a)}_{\text{fatal-miss rate } q_1}.$$

*Proof.* By Doob–Dynkin, $\sigma(\varphi)$-measurability of $d$ gives a
measurable $d' : Z \to \{a,e\}$ with $d = d' \circ \varphi$. Then
$\nu_i(d = a) = \nu_i(\varphi^{-1}(d'{=}a)) = (\varphi_\*\nu_i)(d'{=}a)$, and
the two pushforwards are equal. $\square$

**Reading.** Inside the defect, **the miss rate *is* the capture rate**: one
scalar $q := q_0 = q_1$ prices both. Every unit of specialization gain the
detector lets through carries a proportional unit of fatal exposure — the
two cannot be decoupled by any cleverness downstream of $\varphi$, because
they are literally the same number. Outside the defect they decouple
completely ($q_0 = 1$, $q_1 = 0$ is available). This is the formal content of
"shared difficulty function ⇒ correlated failure" (Eckhardt–Lee;
Littlewood–Rushby), upgraded from a correlation model to an identity.

**Quantitative version.** Without equal pushforwards, let
$\tau := \mathrm{TV}(\varphi_\*\nu_0, \varphi_\*\nu_1)$. Then for every
$d \in \mathcal{D}_R$: $\;q_0 - q_1 \leq \tau$ (immediate from the definition
of total variation). The retained distinguishability $\tau$ is the *only*
wedge the restricted detector can drive between capture and miss.

## 4. Theorem 1 — observation defects: unconditional collapse

**Theorem 1.** Suppose $\varphi_\*\nu_0 = \varphi_\*\nu_1$ and
$\mathcal{P} \supseteq \mathcal{F}(\nu_0, \nu_1)$. Then for every
$L \geq L^\*$:

1. **(Collapse.)** $\displaystyle \sup_{d \in \mathcal{D}_R} \inf_{\lambda} G_{P_\lambda}(d) = -p$, attained only by policies with $q = 0$ (total forfeit on the family). Hence $\Pi(\mathcal{D}_R) = 0$.
2. **(Transduction is powerless.)** Minimax equality holds:
   $\displaystyle \inf_{\lambda} \sup_{d \in \mathcal{D}_R} G_{P_\lambda}(d) = -p$ as well. Even a detector chosen *after* seeing the deployment distribution earns zero premium.
3. **(Separation.)** The unrestricted detector $d^\*= \mathbf{1}[x \in C]\cdot a$ achieves $\inf_\lambda G_{P_\lambda}(d^\*) = (1-\bar\varepsilon)g - \bar\varepsilon p > 0$, i.e. premium $(1-\bar\varepsilon)(g+p)$. This is exactly optimal: $\sup_{d \text{ } \Sigma\text{-meas.}} \inf_\lambda G_{P_\lambda}(d) = (1-\bar\varepsilon)g - \bar\varepsilon p$.

*Proof.* Let $d \in \mathcal{D}_R$ with common rate $q$ (Lemma 1). Direct
computation:

$$G_{P_\lambda}(d) \;=\; (1-\lambda)\big[qg - (1-q)p\big] + \lambda\big[-qL - (1-q)p\big] \;=\; q\big[(1-\lambda)g - \lambda L\big] - (1-q)\,p.$$

(1) The expression is linear in $q$ and decreasing in $\lambda$ for $q>0$, so
$\inf_\lambda$ sits at $\lambda = \bar\varepsilon$, giving
$q\big[(1-\bar\varepsilon)g - \bar\varepsilon L\big] - (1-q)p$. For
$L \geq L^\*$ the bracket is $\leq -p$, so the whole expression is maximized
at $q = 0$ with value $-p$; any $q > 0$ does strictly worse (and $\to -\infty$
as $L \to \infty$).

(2) For fixed $\lambda$, $\sup_{d}$ over $\mathcal{D}_R$ is the same linear
program in $q \in [0,1]$ (both endpoints are realized by the constant
detectors, which lie in $\mathcal{D}_R$):
$\sup_d G_{P_\lambda} = \max\big(-p,\; (1-\lambda)g - \lambda L\big)$. This is
decreasing in $\lambda$; the adversary plays $\lambda = \bar\varepsilon$ and
for $L \geq L^\*$ the max is $-p$.

(3) For arbitrary $\Sigma$-measurable $d$ let $q_0 := \nu_0(d{=}a)$,
$q_1 := \nu_1(d{=}a)$ — now free to differ. $G_{P_\lambda}(d) = (1-\lambda)[q_0 g - (1-q_0)p] + \lambda[-q_1 L - (1-q_1)p]$
is maximized by $q_1 = 0$, $q_0 = 1$, i.e. by $d^\*$, giving
$(1-\lambda)g - \lambda p$, whose infimum over the family is at
$\lambda = \bar\varepsilon$. $\square$

**Remarks.**
- Point (2) is stronger than the repaired Conjecture B asked for: for
  observation defects the inductive-regime condition is *unnecessary*.
  Transduction tells you *that* fatal mass is present ($\lambda$), never
  *which* task is fatal — the information was destroyed per-task by
  $\varphi$, before any distributional knowledge can act. Knowing $\lambda$,
  the optimal within-defect play is precisely a Chow rule on the coarsened
  posterior — and at the confounded points that posterior equals $\lambda$
  itself, forcing all-or-nothing behavior: acting everywhere or escalating
  everywhere. Chow's rule executes the collapse; it cannot prevent it.
- The gap between inside and outside is $(1-\bar\varepsilon)(g+p)$ — the
  entire specialization premium, independent of $L$ beyond the threshold
  $L^\*$.

## 5. Theorem 1′ — exact value and the removability coefficient

Drop the equal-pushforward assumption. Write $m_i := \varphi_\* \nu_i$ and
define the **residual separability**

$$\sigma_0 \;:=\; \sup\{\, m_0(S) \;:\; S \subseteq Z \text{ measurable},\; m_1(S) = 0 \,\}$$

— the share of the safe ensemble recoverable at *zero* coarsened fatal risk.

**Theorem 1′.** On the family $\mathcal{F}(\nu_0,\nu_1)$, within-defect
detectors correspond exactly to measurable act-regions $S \subseteq Z$, and

$$\sup_{d \in \mathcal{D}_R} \inf_{\lambda} G_{P_\lambda}(d) \;=\; \sup_{S \subseteq Z}\; \Big\{ (1-\bar\varepsilon)\big[m_0(S)\,g - (1{-}m_0(S))\,p\big] \;-\; \bar\varepsilon\big[m_1(S)\,L + (1{-}m_1(S))\,p\big] \Big\},$$

a Neyman–Pearson problem on the coarsened likelihood ratio $dm_1/dm_0$.
Consequently, as $L \to \infty$,

$$\Pi(\mathcal{D}_R) \;\longrightarrow\; \sigma_0 \cdot (1-\bar\varepsilon)(g+p) \;=\; \sigma_0 \cdot \Pi(\Sigma\text{-measurable}).$$

*Proof.* The correspondence $d \leftrightarrow S = \{d' = a\}$ is Lemma 1's
factorization; the displayed value is the direct computation of
$\inf_\lambda G$ (infimum again at $\lambda = \bar\varepsilon$ for any $S$
with $m_1(S) \geq 0$, since the $\lambda$-coefficient is negative whenever
$S$ has any mass). For $L \to \infty$, any $S$ with $m_1(S) > 0$ has value
$\to -\infty$, so the supremum concentrates on $\{S : m_1(S) = 0\}$, over
which the value is increasing in $m_0(S)$ with supremum
$(1-\bar\varepsilon)[\sigma_0 g - (1-\sigma_0)p] - \bar\varepsilon p = -p + \sigma_0(1-\bar\varepsilon)(g+p)$. $\square$

**Reading.** $\sigma_0$ is the fraction of the specialization premium that
survives the coarsening: $\sigma_0 = 1$ recovers the full premium,
$\sigma_0 = 0$ is Theorem 1's collapse. **For observation defects on mixture
families this settles the "iff" direction of the slogan**: the defect is
profitably removable in the large-$L$ limit iff the restriction retains a
region where safe mass lives at zero fatal mass — iff the detector-relevant
distinction survives outside the defect. Removability is not a yes/no after
all (QUESTIONS.md #6): it is the coefficient $\sigma_0$.

## 6. Theorem 2 — capacity defects: collapse in the inductive regime, and the minimax gap

Now the detector sees $x$ fully but is confined to
$H \subseteq \{a,e\}^X$ (constants included). Fix distinct pairs
$(x_0^i, x_1^i)_{i=1}^k$ with $x_0^i \in C$, $x_1^i \in E$, and let
$\mathcal{P} = \bigcup_i \mathcal{F}(\delta_{x_0^i}, \delta_{x_1^i})$.

- **Confounding condition:** every $h \in H$ pools some pair:
  $\forall h\, \exists i : h(x_0^i) = h(x_1^i)$.
- **Resolution condition:** every pair is resolved by some member:
  $\forall i\, \exists h_i \in H : h_i(x_0^i) = a,\; h_i(x_1^i) = e$.

**Theorem 2.** Under the confounding condition, for $L \geq L^\*$:

1. **(Inductive collapse.)** $\displaystyle \sup_{h \in H} \inf_{P \in \mathcal{P}} G_P(h) = -p$; hence $\Pi(H) = 0$ in the inductive regime.
2. **(Transductive escape.)** Under the resolution condition additionally,
   $\displaystyle \inf_{P \in \mathcal{P}} \sup_{h \in H} G_P(h) \geq (1-\bar\varepsilon)g - \bar\varepsilon p$, so the minimax gap is at least $(1-\bar\varepsilon)(g+p)$ — the entire premium.

*Proof.* (1) Fix $h$; pick a pooled pair $i = i(h)$. On
$P^i_{\bar\varepsilon}$: if $h$ acts on both members,
$G = (1-\bar\varepsilon)g - \bar\varepsilon L \leq -p$ for $L \geq L^\*$; if
it escalates both, $G = -p$. Either way
$\inf_P G_P(h) \leq -p$, and the constant $e$ attains $-p$. (2) For any
$P^i_\lambda$, play the resolving $h_i$:
$G = (1-\lambda)g - \lambda p \geq (1-\bar\varepsilon)g - \bar\varepsilon p$. $\square$

**Remarks.**
- **The inductive condition of the repaired Conjecture B is now identified
  formally: it is the order of quantifiers.** Inductive = detector committed
  before the deployment distribution ($\sup_h \inf_P$); transductive =
  detector chosen with knowledge of it ($\inf_P \sup_h$). For capacity
  defects the gap between the two is the whole premium — this is the toy form
  of exactly the mechanism by which Goldwasser–Kalai–Kalai–Montasser (2020)
  escape: unlabeled deployment data moves the system from the first value to
  (toward) the second. For observation defects, Theorem 1(2) says the gap is
  zero. **Backlog #6's "one concept or three" receives its first hard
  evidence: observation and capacity defects are provably different along the
  transduction axis.**
- Realizability of the conditions is generic, not exotic. Example: $H$ =
  threshold detectors on a one-dimensional confidence score $s$, with
  safe/fatal points interleaved in $s$ (scores $1, 2, 3, 4$ for
  $x_0^1, x_1^1, x_0^2, x_1^2$): every threshold pools one of the two pairs,
  yet each pair is resolved by some threshold. Interleaved confidence is the
  norm for restricted scores — this is Ulmer–Cinà's ReLU-confidence failure
  as a two-pair combinatorial fact.

## 7. Theorem 2′ — the capacity value formula, and the unification

Theorem 2 gave the capacity-defect collapse as an inequality under a
combinatorial condition. Here is the exact value, for an arbitrary class — and
it turns out to be the *same formula* as Theorem 1′, which relocates the
observation/capacity split.

**Setup.** Detector class $H \subseteq \{a,e\}^X$ (constants included),
ensemble pair $\nu_0$ (supported in $C$), $\nu_1$ (supported in $E$), family
$\mathcal{F}(\nu_0,\nu_1)$. Each $h \in H$ has an **operating point**
$\big(q_0(h), q_1(h)\big) := \big(\nu_0(h{=}a),\, \nu_1(h{=}a)\big)$ —
capture rate and leak rate; note $q_1$ *is* the miss rate $\mu$ on the fatal
ensemble and $1 - q_0$ the false-alarm rate on the safe one. Call
$\mathrm{ROC}(H) := \{(q_0(h), q_1(h)) : h \in H\} \subseteq [0,1]^2$ the
class's **ROC set** against the pair. Write the **price vector**

$$A := (1-\bar\varepsilon)(g+p) \quad (\text{per unit captured}), \qquad B := \bar\varepsilon\,(L-p) \quad (\text{per unit leaked}).$$

**Lemma 2 (operating-point reduction).** For every $h \in H$, the worst-case
premium over the family depends on $h$ only through its operating point:

$$\Pi(h) \;=\; A\,q_0(h) \;-\; B\,q_1(h).$$

*Proof.* $G_{P_\lambda}(h) = (1-\lambda)[q_0 g - (1-q_0)p] + \lambda[-q_1 L -
(1-q_1)p]$, which is nonincreasing in $\lambda$ (strictly, unless
$q_0 = q_1 = 0$), so the infimum sits at $\lambda = \bar\varepsilon$;
subtracting the baseline $-p$ and collecting terms gives the display. $\square$

**Theorem 2′ (capacity value formula).** For any $H$ containing the
constants,

$$\Pi(H) \;=\; \sup_{(q_0, q_1) \in \mathrm{ROC}(H)} \big[ A\,q_0 - B\,q_1 \big]$$

— the **support function of the class's ROC set at the price vector
$(A, -B)$**. Consequently:

1. The supremum is approached on the Pareto frontier of $\mathrm{ROC}(H)$ —
   the class-restricted ROC curve — and the within-class optimal detector is
   the class-restricted Neyman–Pearson point at slope $B/A$: Chow's rule
   again, executed inside the class.
2. As $L \to \infty$, $\;\Pi(H) \to A \cdot \bar\sigma_0(H)$, where

   $$\bar\sigma_0(H) \;:=\; \lim_{t \downarrow 0}\; \sup\{\, q_0(h) : h \in H,\; q_1(h) \leq t \,\}$$

   is the **zero-leak capture capacity** of the class (the limit exists by
   monotonicity in $t$).

*Proof.* The formula is Lemma 2 plus taking the supremum; $(0,0) \in
\mathrm{ROC}(H)$ via the constant $e$, so $\Pi(H) \geq 0$ automatically. For
(2): upper bound — for any $h$, $\Pi(h) \leq \min\big(A\,q_0(h),\; A -
B\,q_1(h)\big)$, so any $h$ competing with the limit must have $q_1(h) \leq
A/B \to 0$ and hence $q_0(h) \leq \sup\{q_0 : q_1 \leq A/B\} \to
\bar\sigma_0(H)$. Lower bound — for any $t$, pick $h$ with $q_1(h) \leq t$,
$q_0(h)$ within $\eta$ of the sup: $\Pi(h) \geq A(q_0) - Bt$, and let
$t \downarrow 0$ after $L \to \infty$ along the definition of the limit.
$\square$

**Remark (closure).** $\bar\sigma_0(H) \geq \sigma_0(H) := \sup\{q_0 : q_1 =
0\}$, with equality when the ROC set is closed — finite classes, compact
operating sets — and possibly strict otherwise (a class whose every
non-constant member leaks a little, with no zero-leak limit point, has
$\sigma_0 = 0 < \bar\sigma_0$). For σ-algebra classes the two coincide: from
$S_n$ with $m_1(S_n) \to 0$ summably and $m_0(S_n) \geq c$, the set $S =
\limsup S_n$ has $m_1(S) = 0$ and $m_0(S) \geq c$ by reverse Fatou. So
Theorem 1′'s $\sigma_0$ needed no correction.

**Corollary (unification).** Theorem 1′ is the special case $H = $ the
$\sigma(\varphi)$-measurable detectors: by Lemma 1's factorization,
$\mathrm{ROC}(H_\varphi) = \{(m_0(S), m_1(S)) : S \subseteq Z\}$, the support
function reproduces Theorem 1′'s value, and $\bar\sigma_0 = \sigma_0$ by the
closure remark. **Observation and capacity defects obey one value formula:
premium $=$ support function of the detector class's ROC set at the economic
price vector; removability coefficient $=$ zero-leak capture capacity.** What
distinguishes the two defect types is only how the ROC set arises — the image
of a coarsened σ-algebra versus the trace of a function class.

**Where the split actually lives.** On a *single* mixture family the
transduction gap vanishes for **every** class: each $h$'s infimum over
$\lambda$ sits at $\bar\varepsilon$, so
$\inf_\lambda \sup_h G \leq \sup_h G_{P_{\bar\varepsilon}} = \sup_h
\inf_\lambda G$, and the minimax inequality closes the other side. The gap of
Theorem 2 is therefore a **multi-direction phenomenon**: with a union of
confusable families the inductive value is the *joint* max-min
$\sup_h \min_j [A\,q_0^j(h) - B\,q_1^j(h)]$, the transductive value is the
per-direction $\min_j \sup_h$, and the gap measures **joint-realizability
deficit** — whether one member of the class can serve all confusable
directions at once. Theorem 2's confounding/resolution conditions are the
extreme case (each direction resolvable, no member resolves all). This
sharpens the taxonomy result: the observation/capacity split is *not* about
how a detector is priced — the formula above is common — but about whether
deployment knowledge lets the class pick its member per direction. The
limiting coefficient for a union family is accordingly
$\bar\sigma_0(H; \{\nu^j\}) = \lim_{t \downarrow 0} \sup\{\min_j q_0^j(h) :
h \in H,\; \max_j q_1^j(h) \leq t\}$, which recovers Theorem 2(1)'s collapse
as $\bar\sigma_0 = 0$ under confounding.

## 8. The joint-realizability deficit, characterized

Theorem 2′ located the transduction gap in joint realizability. This section
makes that quantitative: what the deficit *is*, which two obstructions
produce it, exact formulas for the natural classes, and a general bound that
turns out to be the theory applied to itself.

**Setup.** Directions $j = 1, \dots, k$: ensemble pairs $(\nu_0^j, \nu_1^j)$,
union family $\mathcal{P} = \bigcup_j \mathcal{F}(\nu_0^j, \nu_1^j)$. Write
$\bar\sigma^j(H)$ for the per-direction zero-leak capture capacity and
$\bar\sigma^{\mathrm{jt}}(H) := \lim_{t \downarrow 0} \sup\{\min_j q_0^j(h) :
h \in H,\ \max_j q_1^j(h) \leq t\}$ for the joint one (§7). Define the
**joint-realizability deficit**

$$\Delta(H) \;:=\; \min_j \bar\sigma^j(H) \;-\; \bar\sigma^{\mathrm{jt}}(H) \;\in\; [0, 1],$$

(nonnegative since any jointly feasible $h$ is feasible per direction). By
Theorem 2′'s limits applied per-direction and jointly, the transduction gap
satisfies $\Gamma_L := \Pi^{\mathrm{tr}}_L - \Pi^{\mathrm{ind}}_L \to
A \cdot \Delta(H)$ as $L \to \infty$: **the deficit is the limiting gap in
premium units.**

**Proposition 4 (the two obstructions).** If for each $j$ and every $t > 0$
there is $h_j \in H$ with $q_0^j(h_j) \geq \bar\sigma^j - \eta$ and
$q_1^i(h_j) \leq t$ for **all** $i$ (*cross-safe* local solutions), and $H$
contains a member whose act region contains the union of the $h_j$'s act
regions (*union closure* over them), then $\Delta(H) = 0$.

*Proof.* The union member captures $q_0^j \geq \bar\sigma^j - \eta$ on each
$j$ and leaks at most $kt$ on each $i$; let $t, \eta \downarrow 0$. $\square$

So every positive deficit is attributable to **cross-leak** (direction $i$'s
solution acts on direction $j$'s fatal mass) or **non-closure** (the class
cannot glue its local solutions), and the two are separated by the examples
below: thresholds fail only by cross-leak, halfspaces only by non-closure.

**Proposition 5 (monotone classes — exact formula, "global paranoia").**
Let $H = \{ \text{act iff } s(x) \geq \theta \}$ for a score $s$ (act regions
nested, hence union-closed). Let $T_j := \inf\{\theta : \nu_1^j(s \geq
\theta) = 0\}$ be direction $j$'s fatal ceiling and $T := \max_j T_j$ the
global one. Then

$$\bar\sigma^j = \nu_0^j(s > T_j), \qquad \bar\sigma^{\mathrm{jt}} = \min_j\, \nu_0^j(s > T), \qquad \Delta = \min_j \nu_0^j(s > T_j) - \min_j \nu_0^j(s > T)$$

(with $>$ relaxed to $\geq$ at ceilings carrying no fatal atom). The deficit
is the safe mass trapped **between each direction's own fatal ceiling and the
global one**: a single threshold must be paranoid enough for the worst
direction and pays that paranoia in every direction. *Point-pair corollary:*
$\Delta \in \{0,1\}$, and $\Delta = 1$ iff the pairs are correctly ordered
but **interleaved** — every $s(x_0^j) > s(x_1^j)$, yet some safe point scores
below another direction's fatal point. (Theorem 2's threshold example, now
exact; Ulmer–Cinà's interleaved-confidence failure is this formula.)

*Proof.* Nested act regions make every supremum one-dimensional in $\theta$;
zero leak on $j$ forces $\theta > T_j$ (all $i$: $\theta > T$), and
$\{s \geq \theta\} \downarrow \{s > T\}$ as $\theta \downarrow T$ gives the
right-limits. $\square$

**Proposition 6 (halfspaces — the deficit is non-separability).** Let $H$ =
halfspace act regions in $\mathbb{R}^n$, directions given by point pairs.
Each pair is *always* individually resolvable (two points), so
$\min_j \bar\sigma^j = 1$; and $\bar\sigma^{\mathrm{jt}} = 1$ iff the safe
points $\{x_0^j\}$ are strictly linearly separable from the fatal points
$\{x_1^j\}$, else $0$. Hence $\Delta \in \{0,1\}$ with

$$\Delta = 1 \iff \mathrm{conv}\{x_0^j\} \cap \mathrm{conv}\{x_1^j\} \neq \emptyset.$$

The minimal witness is the XOR configuration ($k = 2$, $n \geq 2$).
Halfspaces are cross-safe here (a pair's separator can be chosen missing the
other points) but not union-closed — the complementary failure mode to
Proposition 5. $\square$

**Remark (no single capacity number).** If $H$ shatters the $2k$ direction
points, $\Delta = 0$ trivially — so a positive deficit certifies that the
directions witness a non-shattered set. But the converse fails badly:
thresholds (VC 1), intervals (VC 2), and halfspaces (VC $n{+}1$) all admit
$\Delta = 1$ at $k = 2$ under adversarial placement. The deficit is a
function of the **geometry of the directions relative to the class**, not of
class capacity alone; shattering gives sufficiency, never a characterization.

**Theorem R (router bound — the theory applied to itself).** Let
$r : X \to [k]$ be any *direction router* with confusion mass
$\rho := \max_j \max\big(\nu_0^j(r \neq j),\ \nu_1^j(r \neq j)\big)$, and let
the gated composite $h_r(x) := h_{r(x)}(x)$ be admissible (i.e. play in the
augmented class $R \ltimes H^k$). Then at **every** $L$:

$$\Gamma_L(R \ltimes H^k) \;\leq\; \rho \cdot (A + B).$$

*Proof.* Pick per-direction detectors $h_j$ within $\eta$ of the transductive
optimum at the current $L$. On direction $j$ the composite's capture drops by
at most $\nu_0^j(r \neq j) \leq \rho$ and its leak rises by at most
$\nu_1^j(r \neq j) \leq \rho$, so its premium is within
$\rho(A + B) + \eta$ of $\Pi^{\mathrm{tr}}_L$. $\square$

Three consequences:

1. **The deficit is priced by the same two prices** — router confusion on
   safe mass costs $A$ per unit, on fatal mass $B$ per unit. Since
   $B = \bar\varepsilon(L - p)$ grows with $L$, a useful bound at large $L$
   needs router confusion $O(1/L)$ on the fatal ensembles — *exactly*
   Theorem A's law for the primary detector, at the same
   $O(\log L)$ hypothesis-testing rent. The router **is** a detector — of
   directions instead of fatality — and obeys detector economics.
2. **The escape-route question recurses.** "Can the class serve all
   directions at once?" reduces to "can the direction be detected per task?"
   — a second-order instance of the original problem. The recursion
   terminates in one of two ways, and this is the observation/capacity split
   in its third and sharpest formulation: for a **capacity defect** the
   recursion bottoms out in a cheap router (directions distinguishable from
   the task; deficit $\leq \rho(A{+}B)$, bought at log-rent); for an
   **observation defect** the router would need exactly the information the
   coarsening destroyed — its confusion is bounded *below* by the coupling
   lemma — and the recursion halts at level one with nothing to buy.
3. **Fleet corollary** (QUESTIONS.md #5): a menu of $k$ specialists plus a
   router is one generalist detector, and $\rho(A + B)$ is precisely what the
   fleet loses to the ideal; the composition question inherits the whole
   pricing apparatus.

The bound is stated for the augmented class: it characterizes what
*composition closure buys*, while Propositions 5–6 give the exact deficit of
the raw classes. The residual open piece is the exact deficit for
non-union-closed classes on ensemble (non-point) directions, where cross-leak
and non-closure mix.

## 9. Corollary 3 — the growth form

**Corollary 3.** Under multiplicative dynamics (wealth factor $1+r$ per task:
$r = \tilde g$ on safe acts, $r = -\tilde L$ on missed fatals, $r = -\tilde p$
on escalations, i.i.d. tasks), with a confounded detector class: every
within-defect policy with positive specialization premium has, on some
$P \in \mathcal{P}$, expected log-growth $\to -\infty$ as $\tilde L \to 1$;
every within-defect policy with bounded log-growth has premium $0$.

*Proof.* Positive premium requires positive act-rate $q$ on the confounded
family (Theorems 1, 2), and $\mathbb{E}_{P_{\bar\varepsilon}}[\log(1+r)]$
contains the term $\bar\varepsilon\, q \log(1 - \tilde L) \to -\infty$.
Policies avoiding this have $q = 0$, hence premium $0$ by the same theorems. $\square$

**Survival and specialization gain cannot coexist inside the defect.** This
is Proposition 3 (FORMALIZATION.md §5) with its missing half: there, ruin
forces $\mu = 0$; here, inside the defect, $\mu = 0$ forces $q_0 = 0$
(Lemma 1) — no gain. The two halves together are the theorem the founding
statement needed.

## 10. Theorem A — the achievability half (the converse's partner)

The converse (§4–§7) says a confounded detector earns zero premium. The
partner statement: a detector placed *outside* the defect earns a premium
bounded below by an explicit constant, so removal is not merely possible but
*profitable* — closing the "if" direction of the iff. Where §4–§7 fixed the
detector class and varied $P$ adversarially, here we fix a good detector and
lower-bound its worst-case premium over the class.

**Hypotheses.**
- **Structured class with a competence floor.**
  $\mathcal{P} \subseteq \{P : P(E) \leq \bar\varepsilon,\; P(C) \geq \underline{c}\}$
  with $\underline{c} + \bar\varepsilon \leq 1$. The floor $\underline c > 0$
  is necessary: if the world may contain no in-competence mass, no policy can
  guarantee positive premium (there is nothing to specialize *on*).
- **Out-of-defect detector.** There is a detector $d$ achieving, *uniformly
  over $\mathcal{P}$*, miss rate $\sup_P P(d{=}a \mid E) \leq \delta$ and
  false-alarm rate $\sup_P P(d{=}e \mid C) \leq \alpha_0$, at rent $c_d$, and
  escalating the benign region ($P(d{=}a \mid N) = 0$ — the conservative
  choice; acting on benign only helps, by $(p-\ell) \geq 0$ per unit). "Out of
  the defect" is exactly what makes both bounds simultaneously attainable:
  Lemma 1 forbids it *inside* an observation defect (there capture rate $=$
  miss rate), and the repaired Conjecture C (FORMALIZATION.md §7) *constructs*
  such a $d$ for structured $\mathcal{P}$ (finite-VC families,
  $f$-divergence balls). A inherits C's structural restriction — no more, no
  less.

**The premium identity.** Against the always-escalate baseline $B_0 = -p$,
for any detector and any $P$ (writing $\alpha_P, \mu_P$ for the realized
rates, benign escalated):

$$\Pi_P(d) \;=\; P(C)\,(1-\alpha_P)\,(g+p) \;-\; P(E)\,\mu_P\,(L-p) \;-\; c_d.$$

*Derivation.* Add $p = p\,[P(C)+P(E)+P(N)]$ to $G_P(d)$ region by region. On
$C$: $P(C)[(1-\alpha_P)g - \alpha_P p] + pP(C) = P(C)(1-\alpha_P)(g+p)$. On
$E$: $-P(E)p - P(E)\mu_P(L-p) + pP(E) = -P(E)\mu_P(L-p)$ (baseline exactly
cancels the escalate cost on $E$). On $N$ with benign escalated: $-pP(N) +
pP(N) = 0$. Subtract rent $c_d$. $\square$

Three terms, each a per-unit price: **captured competence** earns $(g+p)$ per
unit (gain $g$ where the baseline paid $p$); **leaked fatal** costs $(L-p)$
per unit (loss $L$ where the baseline paid $p$); **rent** is flat.

**Theorem A.** Under the hypotheses,

$$\inf_{P \in \mathcal{P}} \Pi_P(d) \;\geq\; \Pi^\* \;:=\; \underline{c}\,(1-\alpha_0)\,(g+p) \;-\; \bar\varepsilon\,\delta\,(L-p) \;-\; c_d,$$

and the defect is **profitably removable** ($\Pi^\* > 0$) iff the
**advantage condition** holds:

$$\underline{c}\,(1-\alpha_0)\,(g+p) \;>\; \bar\varepsilon\,\delta\,(L-p) + c_d.$$

*Proof.* The identity gives $\Pi_P(d) = P(C)(1-\alpha_P)(g+p) - P(E)\mu_P(L-p)
- c_d$, which is nondecreasing in $P(C)$ and nonincreasing in each of
$\alpha_P, \mu_P, P(E)$. Apply the uniform caps $\alpha_P \leq \alpha_0$,
$\mu_P \leq \delta$ and the class bounds $P(C) \geq \underline c$, $P(E) \leq
\bar\varepsilon$ — all valid for every $P \in \mathcal{P}$ — to get
$\Pi_P(d) \geq \Pi^\*$ pointwise, hence for the infimum. $\square$

**This is Proposition 1 / the advantage condition (FORMALIZATION.md §3),
re-derived as the achievability threshold** with the detector's real
operating characteristics substituted for the abstract "price of
compensation": the escalation cost enters as the $(1-\alpha_0)$ haircut on
captured competence, and the missed-fatal cost as $\bar\varepsilon\delta(L-p)$.

### Remarks — where A meets B

**1. The $L$-dependence, and the exact meeting with the converse.** $\Pi^\*$
carries $L$ only through the leakage $\bar\varepsilon\delta(L-p)$. With
$\delta > 0$ *fixed*, $\Pi^\* \to -\infty$ as $L \to \infty$: **a detector of
any fixed positive miss rate is eventually ruined by a severe enough fatal
event.** This is not a weakness of the proof — it is A shaking hands with
Conjecture B, which drove $G \to -\infty$ precisely when $\mu$ stayed bounded
away from $0$. The two theorems partition the $(\delta, L)$ plane along the
same curve. Profitable removal survives arbitrary $L$ only if the miss rate
shrinks with severity,

$$\delta(L) \;<\; \frac{\underline c(1-\alpha_0)(g+p) - c_d}{\bar\varepsilon(L-p)} \;=\; O\!\big(1/L\big).$$

**2. Two independent routes to a bound uniform in $L$.** Either
(i) **sharpen the detector**, $\delta = O(1/L)$, so leakage stays $\leq
\bar\varepsilon\kappa$ for a constant $\kappa$; or (ii) **cap the blast
radius** — a spend ceiling / bounded compensation making realized loss
$\leq \bar L$ regardless of the true $L$ (FORMALIZATION.md §2, point 2), after
which any fixed $\delta$ satisfying the advantage condition at $\bar L$ works.
The architecture uses both; either alone suffices for a constant lower bound.

**3. Route (i) is cheap — the tie to detector economics.** By the
hypothesis-testing bound (Conjecture D, FORMALIZATION.md §8), miss rate
$\delta$ costs rent $c_d \sim \log(1/\delta)$, so $\delta = O(1/L)$ costs only
$c_d \sim \log L$. Leakage is held constant at the price of *logarithmic*
rent — the detector's cost grows exponentially slower than the severity it
insures against. Profitability then holds up to a finite but **exponentially
large** ceiling $L \leq \exp\big(O(\text{margin})\big)$, where the margin is
the specialization premium net of constant leakage. Cheap detection is what
makes the whole architecture affordable, quantified.

**4. Graceful degradation recovers the converse's witness.** At the ideal
operating point $\alpha_0 = \delta = 0$ (perfect detector) with the fullest
class $\underline c = 1-\bar\varepsilon$, $\Pi^\* = (1-\bar\varepsilon)(g+p)$
— **exactly the separation witness of Theorem 1(3)** and the outside-defect
value against which the converse measured the collapse. A's bound degrades
continuously from that ideal as $(\alpha_0, \delta, c_d)$ rise, so Theorems 1′
and A are two ends of one continuum: $\sigma_0$ measures how much of the
premium a *confined* detector keeps; $(\alpha_0, \delta)$ measure how much a
*free but imperfect* detector keeps.

### Corollary A+B — the full iff

Combining Theorem A with the converse (§4–§7), over structured classes and
with either $L$ capped or $\delta = O(1/L)$:

> A defect is **profitably removable** iff a detector achieving small
> miss and false-alarm rates exists **outside** it — equivalently, iff the
> detector-relevant distinction survives the restriction (zero-leak capture
> capacity $\bar\sigma_0 > 0$, one coefficient for both defect types by
> Theorem 2′) **and** the advantage condition holds.

Necessity is B (inside the defect the distinction is gone, premium $= 0$);
sufficiency is A (outside it, the explicit $\Pi^\* > 0$). Placement supplies
the *possibility*; the advantage condition supplies the *profit*. This is the
theorem the founding statement asked for, in both directions.

## 11. Positioning

- **vs. Fang et al. (2022):** their impossibility is statistical
  (non-learnability of OOD detection in restricted spaces); ours is
  decision-theoretic (value collapse for an actor–detector pair sharing one
  restriction), with the adversarial-distribution move imported as
  technology, as planned.
- **vs. Goldwasser et al. (2020):** not a contradiction but a corner of the
  theory — their escape is Theorem 2's minimax gap, and Theorem 1(2) marks
  its boundary: transduction rescues capacity defects, never observation
  defects.
- **vs. Littlewood–Rushby / common-cause failure:** Lemma 1 is the
  common-cause principle upgraded from a correlation model to an identity
  ($q_0 = q_1$), with the quantitative wedge $\tau$ recovering the graded
  version.
- **vs. Chow:** within the defect, optimal play *is* Chow's rule on the
  coarsened posterior (Theorem 1′ is a Neyman–Pearson problem) — and that is
  precisely why optimal within-defect play cannot help. The failure is not in
  the decision rule but in what reaches it.
- **The slogan, final form:** *a defect is profitably removable exactly to
  the extent that the detector's distinction survives outside it* —
  coefficient $\sigma_0$, not a yes/no.

## 12. Open

1. **Costed achievability with *learned* detector rates.** Theorem A takes the
   uniform caps $(\alpha_0, \delta)$ as given (existence delegated to repaired
   Conjecture C). Folding in finite-sample estimation of those rates — the
   detector trained, not assumed — would make A end-to-end. The concentration
   is standard; the honest work is the union bound over $\mathcal{P}$'s
   structure.
2. **Iterated adversary** (QUESTIONS.md #3, second half): the adversary here
   chooses $P$ once; the repeated game with a learning detector is untouched.
3. **Beyond mixture families:** the theorems quantify over
   $\mathcal{F}(\nu_0,\nu_1)$ and unions thereof; characterizing collapse for
   arbitrary $\mathcal{P}$ (not built from confusable ensemble pairs) is open,
   though any $\mathcal{P}$ *containing* such a family inherits the collapse.
4. **The deficit for mixed obstructions.** §8 characterizes the deficit
   exactly where one obstruction acts alone (monotone classes: pure
   cross-leak; halfspaces on points: pure non-closure) and bounds it via
   Theorem R where composition is admissible. The exact deficit for
   non-union-closed classes on *ensemble* directions — cross-leak and
   non-closure mixed — is open; the conjectured shape is a Neyman–Pearson
   problem over the class's *joint* ROC set in $[0,1]^{2k}$, whose Pareto
   frontier no longer factorizes per direction.
5. **The iterated router.** Theorem R's recursion is one level deep. A
   hierarchy of directions (directions about directions) would iterate it;
   whether the total rent telescopes (each level $O(\log L)$) or compounds
   is open, and bears directly on QUESTIONS.md #5's fleet economics.
