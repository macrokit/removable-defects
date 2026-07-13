# Removable Defects

A deficiency kept deliberately because it pays, and removable on demand because a
compensation channel takes it out of the loop in the rare situation where it would
be fatal.

## The claim

The genus is broader than defects: a **feature whose value depends on context** —
harmful in one situation, beneficial in another. The best you can do with such a
feature is *control* it — enable it where it helps, disable it where it hurts —
and if that were free, it would simply dominate. It is not free, and this project
is the study of why. Control needs two things: a switch (cheap, usually
available) and a *detector* that tells you which situation you are in (the binding
constraint). A flawless switch you cannot time is worthless.

A **removable defect** is the sharp, asymmetric corner of that genus: the feature
is beneficial in the common case and fatal in a rare one, so it is kept enabled by
default and switched off — routed to compensation — only on detection. Concretely,
it is a deficiency that is:

1. **Beneficial by default.** In most situations it is non-fatal, and it is what
   the edge is made of: it reduces noise, concentrates capability, and raises
   output in the chosen area. It is kept on purpose.
2. **Removable on demand.** In the rare fatal situation, an owned, bounded
   compensation channel — buy the missing work, defer to a principal, rent
   judgment — takes the defect out of the loop. Removal is a mechanism, not luck.

Two things distinguish this from the familiar praise of specialization:

- **The economic framing.** The defect is kept because it pays, and removal is
  priced per event. The specialist pays for its tail per event; the generalist
  pays always, carrying rarely-used capability. The advantage condition:

  > specialization gain per period > fatal-event frequency × price of compensation

  When fatal events are rare — the premise — the deliberate deficiency is a
  computable economic position, not a character trait.

- **The detector-placement constraint.** The defect must never include the
  detector. A flaw that prevents *recognizing* the fatal situation is not
  removable — the compensation channel is unreachable exactly when it is needed.
  So the asymmetry: action-competence may be arbitrarily narrow; risk perception
  must stay general, cheap, and always-on. The detector never has to solve the
  out-of-frame situation, only notice it and route — and detection is orders of
  magnitude cheaper than competence, which is what makes the architecture
  affordable. The theorem-shaped version, **now proved** (see `PROOFS.md`):
  **a defect is profitably removable exactly to the extent that the detector's
  distinction survives outside it** — and in the worst corner, where the feature
  and its harm share a substrate, the two rates coincide and no switch, however
  perfect, can separate benefit from harm.

The pattern is not specific to any one kind of entity. It reads on humans
(specialization and the attention it buys), organisms (immune tolerance, costly
signals), organizations (focus strategies with insurance and outsourcing as the
removal channel), and ML systems (mixture-of-experts is narrow experts plus a
router; selective prediction and learning-to-defer are the detector constraint
under another name).

## Shape of the project

The primary deliverable is a **short paper**: the general principle, the
advantage condition formalized, the detector constraint as the central claim,
grounded in the value theory (below), with Agent World as the worked engineering
example.

Two companions hang off it:

- **`FORMALIZATION.md`** — the mathematical model: the growth equation, the
  advantage condition and optimal-detector results (propositions), and the
  detector-placement theorems.
- **`PROOFS.md`** — both directions of the iff, proved. Converse: inside an
  observation defect the gain-capture rate and the fatal-miss rate are the same
  number (coupling lemma), so the specialization premium collapses —
  unconditionally for observation defects, in the inductive regime for capacity
  defects. Achievability: a detector *outside* the defect earns an explicit
  positive premium under the advantage condition, and the two halves meet
  exactly at the large-loss boundary. Removability is a coefficient (residual
  separability $\sigma_0$), not a yes/no.
- **`LITERATURE.md`** — the literature check: per-claim verdicts, what was
  killed or repaired, and the defensible residual.
- **`QUESTIONS.md`** — the live research backlog (formalizing the advantage
  condition under heavy tails, detector economics, adversarial removal, the human
  case, fleet composition, whether "removable" is one concept or three). Questions
  graduate from the backlog into the paper's arguments or into their own
  investigations.
- **A design-pattern document** (planned, after the paper stabilizes) —
  runtime-agnostic engineering guidance: when to ship a deliberately narrow
  system plus tripwire plus escalation path. A translation of the paper's
  conclusions, citable from engineering projects.

## Grounding and related projects

- **A Mathematical Theory of Value** (Cheng Qian, public preprint) supplies the
  foundations: diversity is worth paying for while redundancy adds zero (why the
  defect pays); overconfident error produces negative realized growth —
  dissipation is the ex-post punishment of the *unremovable* defect, and this
  architecture is the ex-ante fix; the is/ought asymmetry (beliefs must keep
  updating even when the goal frame does not) is the ancestor of the detector
  constraint.
- **Agent World** carries the first engineering instantiation: a design-rationale
  section and a normative spec field (`onOutOfScope`) by which an agent declares
  its escalation posture. That work lives there; this project is the concept in
  its own right, and agent-world is one application of it.

Related literature to position against (to be verified before citing): division
of labor (Smith), bounded rationality (Simon), hedgehog and fox (Berlin), optimal
foraging, ensemble diversity, mixture-of-experts routing, selective prediction
and learning to defer, defense-in-depth, graceful degradation. The ground to
defend is the combination: kept because it pays, removal priced per event, and
the detector placed outside the defect.

## Honest caveats

Carried into everything downstream:

- Fatal-event frequency is estimated from history, and tail events are precisely
  what history underestimates. The mitigation is structural — the detector
  constraint plus bounded blast radius — not statistical.
- The framing is about *competence*, not values. An entity whose narrow persona
  serves a different goal frame than its declared one is not exhibiting this
  pattern; that is undeclared misalignment.

## Repo conventions

- Declare what is proven vs. conjectured; publish what flunks.
- Docs before code; plain prose over hype.
- `HANDOFF.md` is the founding document from the originating session and stays
  authoritative on the concept's history and naming.
