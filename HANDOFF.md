# Removable Defects — founding handoff

This file is the complete handoff from the session that developed the concept inside
Agent World (2026-07-13, branch `claude/controllable-dual-personality-11933e`). It is
self-contained: everything needed to develop *Removable Defects* as an independent
project is here or pointed to from here.

---

## 1. The founding statement (owner's words, verbatim)

> There exists a certain (and can be artificially compensated for) deficiency.
> Because in most cases this deficiency is unlikely to be fatal, it is beneficial to
> the individual, as it reduces noise and helps to realize individual potential and
> increase output in specific areas. If, in a fatal situation, the deficiency can be
> artificially compensated for, then this is an overall advantage. This
> intermittently flawed nature is called controllable dual personality.

Naming history: the working name was **controllable dual personality**; the owner
renamed it **removable defects** (2026-07-13). The rename is deliberate — it drops
the clinical DID connotation and is literally true in the strongest case (see §4,
the market channel, where a defect *becomes* a capability). Keep "dual personality"
out of public-facing text; the two-persona mechanism below is internal vocabulary.

## 2. The concept, refined

A **removable defect** is a deficiency that is:

1. **Beneficial by default** — non-fatal in most situations; it reduces noise,
   concentrates capability, and raises output in the chosen area. It is kept
   deliberately, because it is what the individual's edge is made of.
2. **Removable on demand** — in the rare *fatal* situation, an owned, bounded
   compensation channel takes the defect out of the loop. Removal is a mechanism,
   not luck.

An entity built this way is *intermittently flawed by design*, running two personas:

- **Persona A — the specialist.** Deliberately deficient: ignores most of the world,
  holds a narrow, deep competence, produces at a rate a generalist cannot match.
- **Persona B — the guardian.** Does not do the work. Its single competence is
  *knowing the boundary of Persona A*: detecting that the current situation is
  (a) outside A's competence and (b) high-stakes, and routing to a compensation
  channel instead of letting A act.

## 3. The load-bearing constraint

**The defect must never include the detector.** A flaw that prevents *recognizing*
the fatal situation is not removable — the compensation channel is unreachable
exactly when it is needed. So the asymmetry:

- Action-competence may be arbitrarily narrow (Persona A).
- Risk perception must stay general, cheap, always-on (Persona B). It never has to
  *solve* the out-of-frame situation, only notice it and route — detection is orders
  of magnitude cheaper than competence, which is what makes the architecture
  affordable.

This mirrors the value theory's is/ought asymmetry (beliefs must keep updating;
the goal frame need not): *perception stays open even when action is narrow.* An
individual may be deficient in what it can do, never in noticing what it cannot.

## 4. The economics

The specialist pays for its tail *per event*; the generalist pays *always* (carrying
rarely-used capability). **Advantage condition:**

> specialization gain per period > fatal-event frequency × price of compensation

When fatal events are rare — the premise — the specialist wins. In Agent World both
sides are measurable (capability score Î × market prices vs. observed escalation
spend), making the *choice* to be deficient a computable economic position.

Compensation channels, in rising order of cost (agent-world instantiation):
- **Market** — buy the missing work; or buy a capability module and convert the
  defect into a capability permanently — removal in the strongest sense.
- **Owner/principal** — defer (the mandate's reserved list = "acts Persona A must
  never take").
- **Rented judgment** — staked third-party review.
- **Guardian** — for the orphaned (sealed-frame) agent; there, removal never means
  unsealing (the seal is forever), it means compensating the seal's fatal case.

Honest caveats (carry these into everything): (a) fatal-event frequency is estimated
from history, and tail events are precisely what history underestimates — the
mitigation is the detector constraint plus bounded blast radius (spend ceilings),
not the statistics; (b) the framing is about *competence*, not values — two personas
serving different goal frames is not this pattern, it is undeclared misalignment.

## 5. What already exists elsewhere (do not duplicate)

**Agent World** (`/Users/jameswalstonn/Documents/agent-world`) instantiated the
concept for its own purposes. As of this handoff the changes are **uncommitted** on
worktree branch `claude/controllable-dual-personality-11933e`
(`.claude/worktrees/controllable-dual-personality-11933e/`):

- `DESIGN.md` §7 "Removable defects — deficiency by design" — the rationale section
  (the canonical prose as of today; a good starting text to generalize *from*).
- `core/spec/01-agent.md` §3.1 + §4.4 — the normative footprint: optional manifest
  field `onOutOfScope ∈ {escalate:market, escalate:owner, decline}`; the
  inconsistency rule (`escalate:market` without `task.post` in the mandate reads as
  `decline`); declared-but-behaviorally-measurable posture.
- `core/packages/protocol` — typed schema + chain test for the field.

Agent World keeps its §7 and its spec field regardless of what this project becomes.
This project is the concept *in its own right*; agent-world is one application.

**The theory repo** — *A Mathematical Theory of Value* (byline Cheng Qian),
`/Users/jameswalstonn/Documents/macrokit/value`. The hooks used so far:
- docs 04/06: diversity is worth paying for; redundancy adds exactly zero (why the
  defect pays).
- docs 06/09 R2: overconfident error → negative realized growth; dissipation is the
  *ex post* punishment of the **unremovable** defect. Removable-defect architecture
  is the *ex ante* fix — Persona B fires before the overclaiming act.
- doc 09: capacity theorem `ΔG ≤ I`; Î tracks realized capability (ρ = 0.977).
- is/ought asymmetry → the detector constraint (§3 above).

## 6. Scope of THIS project (open — decide in the first session)

The founding statement is not agent-specific: it reads on humans (specialization,
attention, the savant tradeoff), organisms (immune tolerance, costly signals),
organizations (focus strategies with insurance/outsourcing as removal), and ML
systems (mixture-of-experts is literally Persona A × N + a router as Persona B;
fallback chains; abstention/selective prediction). Candidate shapes, not mutually
exclusive:

1. **A standalone essay/paper** — the general principle, the advantage condition
   formalized, the detector constraint as the central theorem-shaped claim
   ("a defect is removable iff the detector lies outside it"), grounded in the value
   theory, with Agent World as the worked engineering example. Could sit beside the
   value repo's doc series or as its own short paper.
2. **A design-pattern document** — runtime-agnostic engineering guidance (when to
   ship a deliberately narrow system + tripwire + escalation path), citable from
   agent-world and macrokit both.
3. **A research program** — the open questions below, each a small investigation.

Related literature to position against (from memory, verify before citing): division
of labor (Smith), bounded rationality (Simon), hedgehog–fox (Berlin), optimal
foraging, ensemble diversity, mixture-of-experts routing, selective
prediction/abstention (learning to defer), defense-in-depth in safety engineering,
graceful degradation. The differentiator to defend: the *economic* framing (kept
because it pays, removal priced per event) + the detector-placement constraint.

## 7. Open questions

1. Formalize the advantage condition: what distribution over "fatal events" makes it
   well-posed, given tails are precisely what history underestimates? Is there a
   robust (minimax / heavy-tail-safe) version?
2. Detector economics: when is detection *provably* cheaper than competence? (Verify
   is easier than solve — an NP-flavored intuition; make it precise or drop it.)
3. Adversarial removal: an opponent who crafts situations the detector misses.
   What does the detector constraint demand under adversarial distribution shift?
4. The human case: the founding statement is about individuals realizing potential.
   Treat carefully — the ethics of calling a person's deficiency an advantage, and
   who owns the compensation channel (autonomy vs. dependency on the compensator).
5. Composition: a fleet of removable-defect specialists vs. one generalist — the
   value theory's diversity results suggest the fleet wins; where does the summed
   cost of N detectors flip the answer?
6. Is "removable" a spectrum? Permanent removal (buy the capability), momentary
   removal (escalate this once), interpretive removal (the sealed-frame/guardian
   case). One concept or three?

## 8. Practical notes

- This repo is freshly initialized, no commits yet. `HANDOFF.md` is its first file;
  authoring `README.md` and choosing the project's shape (§6) is the first session's
  job.
- Owner's project conventions (from the sibling repos): honesty culture — declare
  what is proven vs. conjecture, publish what flunks; docs before code; plain
  prose over hype. The value theory is cited as the public preprint, byline
  **Cheng Qian**.
- Private vertical/domain content from other projects never appears here.
