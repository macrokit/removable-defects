# The converse, proved

This document proves the detector-placement converse (FORMALIZATION.md §6,
Conjecture B) in two forms, after an honest repair that the proof itself
forced. The proofs are elementary once the model is right; per repo
convention, we say so — the content is in the definitions and in the
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
- Corollary 3 (growth form): under multiplicative dynamics, survival and
  specialization gain cannot coexist inside the defect.

**What the proof forced us to admit:** the drafted definition of "removable"
was vacuous, and the drafted Conjecture B was false under it (§2). The
repaired notion is *profitable removability*.

**Still open:** the full costed achievability (Conjecture A), the iterated
adversary, and the capacity-defect analogue of the exact value formula.

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

## 7. Corollary 3 — the growth form

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

## 8. Positioning

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

## 9. Open

1. **Conjecture A (costed achievability)** remains open: Theorem 1(3) proves
   the separation witness, but the full statement with detector rent $c_d$,
   escalation-frequency accounting, and general $\mathcal{P}$ is unproved.
2. **Capacity-defect value formula:** the analogue of Theorem 1′ — a
   removability coefficient for function-class defects (presumably a
   disagreement/covering quantity on $H$ restricted to $C \times E$).
3. **Iterated adversary** (QUESTIONS.md #3, second half): the adversary here
   chooses $P$ once; the repeated game with a learning detector is untouched.
4. **Beyond mixture families:** the theorems quantify over
   $\mathcal{F}(\nu_0,\nu_1)$; characterizing collapse for arbitrary
   $\mathcal{P}$ (not built from a confusable ensemble pair) is open, though
   any $\mathcal{P}$ *containing* such a family inherits the collapse.
