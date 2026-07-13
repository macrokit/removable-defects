# Removable Defects: The Economics and Limits of Deliberate Deficiency

**Cheng Qian**

**Draft v1 — 2026-07-13.** This draft assembles the finished results in `FORMALIZATION.md` and
`PROOFS.md`; those files are the technical appendix and carry the full proofs.
Status labels follow repo convention: *proposition* (provable with standard
tools, cited), *theorem* (proved here / in the appendix), *conjecture*
(open). Nothing is claimed novel that the literature check (`LITERATURE.md`)
found prior.

---

## Abstract

A specialist tolerates blind spots that a generalist does not. Usually this is
treated as a cost to be minimized. We treat it as a design variable: a
deficiency can be *kept* because it pays — it concentrates capability and
raises output where the specialist is strong — and *removed on demand* in the
rare situation where it would be fatal, by routing that situation to a
compensation channel instead of acting. We give three results. First, an
*advantage condition* under which keeping the deficiency is a computable
economic position rather than a character trait; structurally it is the
Ehrlich–Becker market-vs-self-insurance margin applied to a competence gap,
with the detector as a Townsend costly-state-verification technology. Second,
a two-sided characterization of when removal is possible. The obstruction is
captured by a *coupling lemma*: when the deficiency is a coarsening of
perception, the rate
at which the specialist's feature does good equals the rate at which it does
harm — one number prices both — so no switch, however perfect, can separate
benefit from harm. This yields a converse (a confounded detector earns zero
premium and, under multiplicative dynamics, drives long-run growth to
negative infinity) and a matching achievability result (a detector placed
*outside* the deficiency earns an explicit positive premium under the
advantage condition). Together: **a defect is profitably removable iff the
detector-relevant distinction survives the restriction and the advantage
condition holds** — and a single value formula covers every detector class
(the premium is the support function of the class's ROC set at an economic
price vector, with removability a coefficient, not a yes/no). Third, a
distinction between *observation* and *capacity* defects that is invisible to
the average-case literature but sharp here — they differ exactly on whether
access to the deployment distribution rescues them, and the gap between the
two decomposes exactly: it is cross-leak plus a closure deficit, and per-task
randomization buys back the latter, never the former. The characterization is
constructive end to end — the detector can be *learned* from declared fatal
categories at a one-time training bill linear in the loss severity, with the
advantage condition surviving learning. The results synthesize Chow's reject
option, Kelly growth under ruin, and selective prediction; the residual
contributions are the converse as a worst-case impossibility, the detector's
per-event miss rate (not the event frequency) as the binding constraint, and
a frequency-free form of the advantage condition.

## 1. Introduction

Consider a feature — a capability, a disposition, a trained specialization —
whose value depends on context: harmful in situation $A$, beneficial in
situation $B$. Three policies are available. Enable it everywhere, and you
collect $B$'s benefit and $A$'s harm. Disable it everywhere — the generalist
who never commits — and you forfeit $B$'s benefit to avoid $A$'s harm. Or
*control* it: enable in $B$, disable in $A$, capturing the better outcome in
each context. If control were free, it would simply dominate.

Control is not free, and the reason is the subject of this paper. It requires
two things of very unequal difficulty. A *switch* — the ability to enable or
disable the feature — is usually cheap and usually available. A *detector* —
knowing which context you are in, so you know which way to throw the switch —
is the binding constraint. A flawless switch you cannot time is worthless.

The sharp and economically interesting case is asymmetric: $B$ common and
mildly good, $A$ rare but *fatal*. Here the feature is kept enabled by default
— that default is where the specialist's edge lives — and switched off only on
detection, by routing the rare fatal case to a compensation channel (buy the
missing work, defer to a principal, rent judgment). We call such a
deliberately-kept, on-demand-correctable deficiency a **removable defect**.
The word "defect" names the feature in this asymmetric regime; the general
object is a conditionally-valuable feature and the detectability of its
condition.

Two questions organize the paper. *When is keeping the defect worth it?* — an
economic question, answered by the advantage condition (§3). *When can it
actually be removed?* — a structural question, and the one with teeth, because
"if we can control it" hides the whole difficulty. Our central finding (§5) is
that removal can be *impossible even with a perfect switch*, and the
obstruction is precisely where the detector sits relative to the defect.

### Contributions

1. A growth model (§2) in which the decision to be deficient is a computable
   position, and an **advantage condition** (§3) that is the Ehrlich–Becker
   (1972) insurance margin restated for a competence gap, with the detector a
   Townsend (1979) verification cost.
2. The **coupling lemma** (§5): inside an observation defect, gain-capture
   rate $=$ fatal-miss rate. Benefit and harm are one scalar and cannot be
   decoupled downstream.
3. The **converse** (Theorem 1): a confounded detector earns zero
   specialization premium, and under multiplicative dynamics drives growth to
   $-\infty$. To our knowledge the first worst-case, decision-theoretic
   formalization of detector placement — a principle safety engineering has
   held only as practice.
4. **Achievability** (Theorem A): a detector outside the defect earns an
   explicit positive premium under the advantage condition, giving the
   two-sided **iff**.
5. The **observation/capacity distinction** (§6): the two defect types differ
   exactly on whether transductive access to the deployment distribution
   rescues them — a split the average-case literature does not see.
6. Two robustness results: a **frequency-free** (minimax) advantage condition,
   and the identification of the detector's **per-event miss rate**, not the
   event frequency, as what the ruin condition is written on.
7. **An exact theory of the gap** (§6; appendix §§7–9): one value formula for
   every detector class — premium $=$ support function of the class's ROC set
   at the economic price vector — with the transduction gap decomposed into
   two obstructions (cross-leak, non-closure), exact formulas for thresholds
   and halfspaces, a router bound that makes the theory recurse on itself,
   and a least-favorable-mixture duality showing **a coin buys back the
   gluing failure, never the cross-leak**.
8. **End-to-end learnability** (§4; appendix §12): the detector selected and
   certified from stratified samples of declared fatal categories, at a
   one-time training bill *linear* in severity (rare-event certification)
   plus $O(\log L)$ per-period rent — the advantage condition survives
   learning, and default-deny gating makes the certificate fail-safe against
   cover incompleteness.

## 2. The model

Tasks arrive one per period from a distribution $P$ on a space $X$. The
specialist has a **competence set** $C \subseteq X$ it handles well; the
complement is the defect. A **fatal set** $F$ marks tasks where acting without
competence is catastrophic; the danger zone is the **fatal exposure**
$E := F \setminus C$, with mass $\varepsilon := P(E)$, assumed small. Per task
the agent either *acts* or *escalates* to the compensation channel:

| | $x \in C$ | $x \in E$ |
|---|---|---|
| **act** | $+g$ | $-L$ |
| **escalate** | $-p$ | $-p$ |

with $g$ the specialization gain, $L \gg g$ the fatal loss, $p$ the price of
compensation. A **detector** $d : X \to \{\text{act}, \text{escalate}\}$ gates
the choice, at running cost $c_d$, with two error rates: the **miss rate**
$\mu = P(d{=}\text{act} \mid E)$ (acting on a fatal task — the dangerous
error) and the **false-alarm rate** $\alpha = P(d{=}\text{escalate} \mid C)$
(escalating work the specialist could have done — the self-inflicted tax). The
detector never *solves* the escalated task; it answers one binary question,
"is this mine?", and routes.

Expected per-period growth is

$$G = (1-\alpha)\,g\,P(C) - c_d - p\,\big[\alpha P(C) + (\text{escalations outside } C)\big] - \varepsilon\mu L - (\text{benign terms}).$$

Two structural facts. The fatal term $\varepsilon\mu L$ is a product of three
factors; the architecture attacks $\mu$ (the detector) and caps $L$ (bounded
blast radius — a spend ceiling), and deliberately does *not* lean on
estimating $\varepsilon$ well, because tail frequencies are exactly what
history gets wrong. And the generalist alternative is not free: it pays a
carrying cost for rarely-used breadth and typically earns less than $g$ on
$C$, since breadth dilutes depth.

## 3. The advantage condition

**Proposition 1.** The specialist-with-detector beats the generalist iff the
specialization gain exceeds the cost of insuring the tail:

$$\big[(1-\alpha)g - g_{\text{gen}}\big] P(C) + c_{\text{gen}} \;>\; c_d + p\,\mathbb{E}[\text{escalations}] + \varepsilon\mu L.$$

Informally: *specialization gain per period > fatal-event frequency × price of
compensation*, once the detector's rent and false alarms are charged. When
fatal events are rare, the deliberate deficiency wins, and every term is
measurable in an instrumented environment — which is what makes "should I be
deficient?" a computable question rather than a matter of temperament.

This is not new economics, and we do not claim it is. Structurally it is the
Ehrlich–Becker (1972) substitution margin between *market insurance* (the
per-event compensation channel) and *self-insurance* (the generalist's carried
breadth), with the detector playing the role of Townsend's (1979) costly state
verification — pay to verify, and only in the bad state. The carrying-cost-
versus-per-event framing is the standard make-or-buy / capacity-versus-spot
tradeoff (Williamson; Van Mieghem 2003). Our contribution here is narrow and
we state it as such: the *insured object* is a deliberately maintained
competence gap rather than a portfolio position, and — see §5 — the binding
constraint turns out to be the detector's reliability, not the frequency the
insurance is priced against.

## 4. The detector: what it costs and where it must sit

**The optimal rule is Chow's.** Because $G$ is linear in $(\mu, \alpha)$, the
loss-minimizing escalate-versus-act rule is a likelihood-ratio test whose
threshold is the ratio of what a false alarm wastes to what a miss costs. This
is exactly Chow's reject option (1957, 1970); we cite it and keep only one
design consequence: **the threshold is an economic quantity, not an
engineering constant.** As stakes $L$ rise, the same detector must slide along
its ROC curve toward paranoia; a system whose escalation threshold cannot be
re-priced is mis-built. One caveat we will need (Cortes, DeSalvo & Mohri
2016): with a *restricted* actor, the optimal rejection region is provably not
a threshold on the actor's own confidence — the detector must be a separate
function. This foreshadows §5.

**Detection is cheap — and that is load-bearing.** Detecting whether a task is
out of scope is a binary hypothesis test; by the Chernoff–Stein bound its cost
scales as $\log(1/\mu)$ in the miss rate. Producing the *solution* to an
out-of-scope task is far more expensive. We will see in §5 that survival
against fatal events of severity $L$ forces the miss rate down to $O(1/L)$,
which by the hypothesis-testing bound costs only $O(\log L)$ in detector rent.
The architecture insures against loss $L$ at logarithmic price — this is why
it is affordable at all. The shape survives when the detector must be
*learned* rather than assumed: certifying a near-zero miss rate from data is
a one-sided rare-event problem, costing a **one-time training bill linear in
$L$** in labeled fatal examples (appendix, §12) — amortized over deployment,
both bills grow slower than the per-event loss they insure against, so the
advantage condition survives learning. A side consequence worth naming: the
scarce input is labeled catastrophes, which a well-run system stops producing
— it escalates them — so detector training data must come from history,
simulation, or other agents' failures. (Putting detection and production in a single cost
model — rather than the mismatched units of samples and compute — is possible
via doubly-efficient interactive proofs and property testing; the appendix
treats this as a strengthening, not a load-bearing step.)

## 5. Removability: the central result

### 5.1 Profitable removability

We first correct a definition. It is tempting to call a defect *removable* if
some policy keeps growth bounded below. This is vacuous: the constant
always-escalate detector lies in every detector class and bounds $G \geq -p$
regardless of the defect. Survival is trivially available; it is not the
question. The question is *paid-for* survival:

**Definition.** The **specialization premium** of a detector is its worst-case
growth (over an uncertainty class $\mathcal{P}$ of task distributions) *above
the always-escalate baseline*. A defect is **profitably removable** on
$\mathcal{P}$ if some policy earns strictly positive premium — it retains part
of the specialization gain while avoiding ruin.

### 5.2 The coupling lemma

Formalize the defect as a restriction on what the agent can perceive: an
**observation defect** forces the detector to be a function of a coarsened
view $\varphi(x)$ rather than of $x$ itself. (This covers pruned, quantized,
or attention-limited perception.)

**Lemma (coupling).** *Let the detector factor through $\varphi$. For any pair
of task ensembles — one safe, one fatal — that $\varphi$ cannot tell apart,
the detector's gain-capture rate on the safe ensemble equals its fatal-miss
rate on the fatal ensemble.*

The proof is two lines (Doob–Dynkin factorization plus equality of
pushforwards; appendix, Lemma 1). The content is in what it says: **inside an
observation defect the miss rate *is* the capture rate.** Every unit of
specialization gain the detector lets through carries an equal unit of fatal
exposure — the two rates are literally the same number. This is the
common-cause-failure principle of reliability engineering (Eckhardt–Lee 1985;
Littlewood–Rushby 2012) upgraded from a correlation model to an identity. A
switch that disables the harm disables the benefit in the same motion.

### 5.3 The converse

**Theorem 1 (converse).** *Against an adversarial choice of task distribution
within the fatal-mass budget, a detector confined inside an observation defect
earns zero specialization premium once $L$ exceeds an explicit threshold — and
this holds
even if the detector is chosen after seeing the deployment distribution.
Under multiplicative (Kelly) dynamics with true ruin, any positive premium
forces expected log-growth to $-\infty$.*

The mechanism is the coupling lemma: positive premium requires acting on the
confounded family at some positive rate $q > 0$, which by the lemma is also
the fatal-miss rate, and the missed-fatal term $-\varepsilon q L$ overwhelms
the gain as $L$ grows. An exact value formula (appendix, Theorem 1′) refines
"zero" to a coefficient: the surviving premium fraction is the **residual
separability** $\sigma_0$ — the share of safe mass the coarsening still places
at zero fatal risk. So removability is not a yes/no but a number: *a defect is
profitably removable exactly to the extent that the detector's distinction
survives the restriction.*

That transductive access does not help is worth pausing on. Seeing the
deployment distribution tells you *that* fatal mass is present; it never tells
you *which task* is fatal, because $\varphi$ destroyed that distinction
per-task, before any distributional knowledge can act. Optimal play here is a
Chow rule on the coarsened posterior — and at the confounded points that
posterior is uninformative, forcing all-or-nothing behavior. Chow's rule
executes the collapse; it cannot prevent it. The failure is not in the
decision rule but in what reaches it.

### 5.4 Achievability, and the iff

**Theorem A (achievability).** *A detector placed outside the defect —
achieving miss rate $\leq \delta$ and false-alarm rate $\leq \alpha_0$
uniformly over a structured class with competence floor $\underline c$ — earns
worst-case premium bounded below by the explicit constant*

$$\Pi^\* = \underline c\,(1-\alpha_0)(g+p) - \bar\varepsilon\,\delta\,(L-p) - c_d,$$

*and the defect is profitably removable ($\Pi^\* > 0$) iff the advantage
condition $\underline c(1-\alpha_0)(g+p) > \bar\varepsilon\delta(L-p) + c_d$
holds.*

The premium reduces to three per-unit prices — captured competence at $g+p$,
leaked fatal at $L-p$, flat rent — and Theorem A is just Proposition 1
re-derived with the detector's real operating characteristics in place of the
abstract "price of compensation." Existence of such an out-of-defect detector
is exactly a class-uniform coverage guarantee; it is achievable for structured
uncertainty classes (finite-VC families, divergence balls) and, by the
coupling lemma, *impossible* inside an observation defect. Achievability
inherits that structural restriction — no more, no less — and the existence
assumption is ultimately discharged constructively: appendix §12 selects the
detector from data within that structure, with certified rates.

The two theorems meet at the large-loss boundary. With $\delta$ fixed,
$\Pi^\* \to -\infty$ as $L \to \infty$ — a detector of any fixed miss rate is
eventually ruined, matching the converse. Survival against arbitrary $L$
requires either sharpening the detector to $\delta = O(1/L)$ (rent only
$O(\log L)$, per §4) or capping the blast radius. At the ideal operating point
the bound recovers the converse's separation witness exactly, so the two
results are two ends of one continuum.

**Corollary (the iff).** *A defect is profitably removable iff a detector with
small miss and false-alarm rates exists outside it — iff the detector-relevant
distinction survives the restriction — and the advantage condition holds.*
Necessity is the converse (inside the defect the distinction is gone);
sufficiency is achievability (outside it, an explicit positive premium).
**Placement supplies the possibility; the advantage condition supplies the
profit.**

## 6. Two kinds of defect

The restriction in §5 coarsened *perception*. A different restriction confines
the detector to a limited *function class* $H$ while perception stays full — a
**capacity defect**. The two behave differently, and the difference is the
paper's cleanest structural claim.

For a capacity defect the premium collapses in the **inductive** regime — when
the detector is committed before the deployment distribution is known
($\sup_d \inf_P$) — but is fully recovered in the **transductive** regime, when
the detector may be chosen knowing the distribution ($\inf_P \sup_d$)
(appendix, Theorem 2). The inductive-versus-transductive gap is exactly the
mechanism by which Goldwasser–Kalai–Kalai–Montasser (2020) escape
impossibility using unlabeled deployment data. For an observation defect, by
contrast, the converse holds *even transductively* (§5.3): the gap is zero.

So the two defect types are provably distinct along the transduction axis —
an observation defect destroys the distinction per task, and no amount of
distributional knowledge rebuilds it, while a capacity defect still contains
the right detector; it merely cannot be identified until deployment is seen.
This is the switch/detector
distinction of §1 made precise: a capacity defect is "the switch is confined
but the context is in principle visible"; an observation defect is "the
context itself is unresolvable to you, and switch quality is irrelevant." The
average-case selective-prediction literature, which fixes a distribution, does
not see this split; the worst-case framing makes it unavoidable.

The split is sharper than it first appears, because it is *not* a split in
how removability is priced. A single value formula covers both defect types
(appendix, Theorem 2′): for *any* detector class, the worst-case premium is
the support function of the class's ROC set — its achievable (capture, leak)
pairs against the confusable ensembles — evaluated at the economic price
vector (captured competence at $g+p$, leaked fatal at $L-p$), and the
removability coefficient is the class's **zero-leak capture capacity**, of
which §5.3's $\sigma_0$ is the σ-algebra special case. What genuinely
separates the two types is **joint realizability**: on any single confusable
direction the inductive and transductive values coincide for every class, so
the transduction gap exists only when the adversary has several directions,
each resolvable by some member of the class but no member resolving all. An
observation defect fails per direction; a capacity defect fails only jointly.
One formula for the price; two escape routes.

The joint-realizability deficit is itself characterizable (appendix, §8). It
decomposes into exactly two obstructions — *cross-leak* (one direction's
solution acts on another's fatal mass) and *non-closure* (the class cannot
glue its local solutions) — with exact formulas where each acts alone: for
confidence-threshold detectors the deficit is a "global paranoia" tax (the
safe mass trapped between each direction's fatal ceiling and the worst
direction's), and for halfspaces it is precisely non-separability of the safe
and fatal point clouds. In general the deficit is bounded by the confusion
mass of a *direction router*, priced at the same two prices — and since a
router is itself a detector (of directions rather than of fatality), the
question recurses. The recursion is the observation/capacity split in its
sharpest form: for a capacity defect it terminates in a cheap router,
obeying the same $O(\log L)$ detector economics; for an observation defect
the router would need exactly the information the coarsening destroyed, and
the recursion halts at the coupling lemma with nothing to buy. When the two
obstructions mix, a duality resolves them (appendix, §9): allowing the agent
per-task randomization convexifies its options, and the many-direction game
collapses to a single-direction problem at a *least-favorable mixture* of
directions — whereupon the transduction gap splits additively into
irreducible cross-leak plus the price of determinism. **A coin buys back the
gluing failure, never the cross-leak.**

## 7. A worked example: Agent World

The theory grew out of an engineering problem, and the engineering carries an
instantiation of it. *Agent World* is a protocol for autonomous agents that
act on a principal's behalf: each agent is a keypair, a signed manifest, and an
inbox. The instantiation is at the design and protocol level — a manifest field,
its schema, and a conformance rule, merged to the project's mainline with its
chain test passing — not a deployment with measured data. We describe it as a
demonstration that the paper's objects have a natural protocol footprint, not
as empirical confirmation.

**The defect is a closed list.** A manifest declares the agent's capabilities
as a *positive, closed* list of typed task classes. Everything not listed is a
deliberate deficiency, and silence is the honest way to express it — the
competence set $C$ of §2, written down. A narrow agent with a high capability
score on one class is the specialist the advantage condition rewards; a diluted
generalist is what it warns against.

**The compensation route is declared, the detector is not.** A manifest may
carry one optional field:

```
"onOutOfScope": "escalate:market"    // or "escalate:owner" | "decline"
```

This names the route the agent takes when it meets work outside its competence:
post the missing task to the market, defer to its principal, or refuse.
Crucially, the field declares *that* a route exists — never the detector that
fires it. What recognizes "this task is out of my competence and high-stakes"
is internals, out of the protocol's scope by construction. This is exactly the
paper's division of labor: the switch and its route are public and cheap to
state; the detector is the agent's own always-on burden.

**Placement is enforced, weakly but measurably.** No hub can verify that a
detector actually sits outside the defect. The protocol does two enforceable
things instead. It rejects an *internally inconsistent* declaration — a
manifest claiming `escalate:market` while its mandate lacks the right to post
tasks is read as `decline`, since it has declared a route it cannot take
(a conformance requirement, checked by a chain test that also rejects unknown
routes). And it makes the rest a *measurable divergence*: declaring a route,
like declaring a goal frame, turns silent failure into an observable gap
between declared route and behavior. The advantage condition's terms are, by
design, quantities the hub can read — capability score against market prices on
one side, observed escalation spend on the other — so "should this agent be
deficient?" is meant to be computed, not judged.

**The two-sided theorem, from both sides at once.** Agent World names the
paper's central rule as its *one load-bearing constraint*: "the deficiency
must never include the detector — an agent may be deficient in what it can do,
never in noticing what it cannot." We do not claim independent discovery — the
design and this paper share an origin. What we can honestly claim is the
order: the constraint was articulated as an engineering necessity *before* any
theorem existed, and the theorem then showed the necessity was mathematical,
not stylistic. And the two failure accounts line up as one fact seen twice.
The paper's converse is *ex ante*: a confounded detector cannot earn premium,
full stop. Agent World's ledger supplies the *ex post* image: an agent that
declares a route but whose detector is in fact blind will act out of
competence, overclaim, burn its stake, and go broke — dissipation is the
punishment the market administers to the unremovable defect. Theorem 1 says it
was never removable; the ledger says why that costs.

**Removal in the strongest sense.** One of the declared routes is to *buy a
capability module* — after which the missing competence is no longer missing.
This is the permanent end of the removability spectrum: the defect does not
merely get compensated for one event, it *becomes a capability*, and the word
"removable" is then literal. The sealed-frame agent is the opposite extreme —
a permanent deficiency that is never lifted, where removal means the guardian
machinery compensating the seal's one fatal case. That a single manifest field
spans permanent, per-event, and interpretive removal is the concrete form of an
open question we flag in §9: whether "removable" is one concept or three.

## 8. Related work

Each ingredient of this paper has a home in an existing literature, and we
mean the synthesis, not the parts, to be the contribution.

- **Reject option / selective prediction.** The optimal escalate-versus-act
  rule (§4) is Chow's (1957, 1970); the separate-rejector architecture is the
  established
  "separated rejector" of the taxonomy (Hendrickx et al. 2024), with
  Cortes–DeSalvo–Mohri (2016) showing why a restricted actor forces it. Our
  achievability bound is a growth statement, which the risk-coverage framing
  (El-Yaniv–Wiener 2010; Geifman–El-Yaniv 2017) does not provide.
- **Impossibility.** The converse's proof technology — an adversarial
  distribution defeating every detector in a restricted class — is standard
  (Fang et al. 2022 for OOD detection; Ulmer–Cinà 2021 for same-representation
  detectors). What is not stated anywhere we could find: the *coupling* (one
  restriction defines both the actor's competence and the detector's
  blindness) with an *unbounded-loss* consequence, and the positive converse
  that an out-of-restriction detector restores boundedness. The AI-control
  literature treats "monitor outside the untrusted system" as a design axiom
  with no formal backing; this is the backing.
- **Growth and insurance.** The ruin-exclusion lemma is Kelly (1956) /
  Breiman (1961); "insurance licenses concentration" appears in Peters–Adamou
  (2015) and, informally, Spitznagel (2021). The advantage condition is
  Ehrlich–Becker (1972) plus Townsend (1979). Our residual: the ruin condition
  is written on the detector's *miss rate*, not the event frequency.
- **Common-cause failure.** Eckhardt–Lee (1985) and Littlewood–Rushby (2012)
  formalize "a monitor sharing the system's failure mode gives no protection"
  as an average-case correlation model; the coupling lemma is its worst-case,
  identity-strength form.
- **Minimax testing.** The least-favorable-mixture duality behind the
  obstruction split (§6) is classical: Wald (1945, 1950) for minimax with
  randomized rules against a least favorable prior; Huber (1965) and
  Huber–Strassen (1973) for the exact collapse of composite minimax testing
  to one Neyman–Pearson test against a least favorable pair; Fillatre (2017)
  for the finite-LP form. That randomization realizes the ROC convex hull is
  likewise classical (Neyman–Pearson lemma; Provost & Fawcett 2001; see also
  Fauß–Zoubir–Poor 2021, §V-D). We claim the application — the
  restricted-class premium game, the closure-deficit identification, and the
  coin-versus-cross-leak reading — and distinguish Managoli, Sahasranand &
  Prabhakaran (2025), who pair robust testing with abstention at asymptotic
  exponents over unrestricted detectors; notably, the classical
  robust-detection canon itself contains no abstention action at all.
- **Learning the detector.** Theorem E is standard uniform convergence
  (Vapnik–Chervonenkis 1971; the version-space bound is Blumer–Ehrenfeucht–
  Haussler–Warmuth 1989, Thm 2.1). Theorem E′'s problem statement is
  Kalai–Kanade–Mansour's positive-reliable learning, and its setup is
  Neyman–Pearson classification at $\alpha = 0$ (Cannon et al. 2002;
  Scott & Nowak 2005; Rigollet & Tong 2011) — we cite both frames and claim
  only the analysis they skipped: the zero-empirical-miss certification at a
  one-sided fast rate, per declared cell under reweighting, and the
  linear-versus-quadratic-in-severity training bill, itself an elementary
  corollary of the tight realizable/agnostic $1/\varepsilon$ vs
  $1/\varepsilon^2$ dichotomy. Casacuberta & Kanade (2025) own group-wise
  reliable abstention at agnostic rates and are the closest neighbor; they
  contain neither the zero-miss certificate nor the severity asymmetry.

## 9. Limitations and open questions

We are honest about the boundary of what is proved.

- **The learned detector's certificate is only as good as its cover.** The
  end-to-end result (appendix, §12) trains and certifies the detector from
  stratified samples of *declared* fatal categories, and default-deny gating
  makes it fail-safe against categories left out of the cover — an unknown
  fatal category is escalated, not missed. What remains exposed is
  **mis-declaration**: fatal mass inside a declared-safe cell, where the gate
  admits and the samples mislead. That is model misspecification, mitigated
  structurally by the blast-radius cap, not statistically.
- **The advantage condition is frequency-free but budget-dependent.** It uses
  only an upper bound $\bar\varepsilon$ on fatal mass, never a point estimate —
  the minimax robustness we wanted. But it still trusts that budget, and it
  assumes i.i.d. periods; clustered fatal events (regimes, cascades), where
  frequency estimates fail worst, are unmodeled.
- **The adversary moves once.** The converse is a one-shot game. The iterated
  game against a learning detector — and whether detector *diversity* helps the
  way portfolio diversity does — is open.
- **The price of determinism is exact but not computable-by-us.** The
  deterministic many-direction value is a max-min over a non-convex joint ROC
  set; we resolve it for randomized play (the duality of §6) but have neither
  a simplification nor a hardness proof for the deterministic case — the
  nearby classifier-rejector hardness results suggest NP-hardness, and we
  have not shown it.
- **"Removable" may be one concept or three.** Permanent removal (buy the
  capability — the defect *becomes* a capability), per-event removal (escalate
  this once), and interpretive removal (the sealed-frame case, where the seal
  is never lifted and removal means compensating its one fatal reading) share a
  single manifest field in §7 but may deserve separate treatment. The
  observation/capacity split (§6) is first evidence that the taxonomy is real;
  the removability spectrum is a second, orthogonal axis, and we have unified
  neither.
- **The human case is deliberately excluded.** The model prices artifacts.
  Applying "a person's deficiency is an advantage" to people imports ethical
  questions — who owns the compensation channel, autonomy versus dependency —
  that no theorem here resolves, and we do not pretend otherwise.

## 10. Conclusion

A deliberately narrow system with a tripwire and an escalation path is a
common engineering pattern, usually justified by intuition. This paper gives
it an economics and a limit. The economics: keeping a deficiency is a priced
position, advantageous when specialization gain outruns the insured tail. The
limit: removal is possible exactly when the detector sits outside the defect,
and impossible — regardless of switch quality — when the defect and its harm
share a substrate, because then the rate the feature helps and the rate it
harms are the same number. The single design rule that follows is sharper than
"add a monitor": *narrow the action all you like, but never let the narrowing
reach the detector.* Perception must stay open even where action is closed.
