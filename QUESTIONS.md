# Open questions

The live research backlog. Each entry graduates into the paper's arguments or
into its own investigation; nothing is deleted, only marked resolved with a
pointer to where the answer lives.

## 1. Formalizing the advantage condition under heavy tails

The condition — specialization gain per period > fatal-event frequency × price of
compensation — needs a distribution over "fatal events" to be well-posed, and
tail events are precisely what history underestimates. Is there a robust
(minimax / heavy-tail-safe) version? What does the condition look like when the
frequency estimate itself carries fat uncertainty?

## 2. Detector economics

When is detection *provably* cheaper than competence? The intuition is
NP-flavored — verifying is easier than solving — but intuition is not a bound.
Make it precise or drop it. A useful target: conditions under which a detector
with cost o(competence cost) achieves a given miss rate.

## 3. Adversarial removal

An opponent who crafts situations the detector misses defeats the whole
architecture at its single point of failure. What does the detector constraint
demand under adversarial distribution shift? Does the detector need diversity
(multiple cheap detectors) the way the value theory says portfolios do?

**Status (2026-07-13): one-shot case resolved** — the adversary choosing the
distribution once is exactly PROOFS.md Theorems 1–2 (value collapse inside a
confounded defect). Open remainder: the iterated game with a learning
detector, and detector diversity as a defense.

## 4. The human case

The founding statement is about individuals realizing potential. Treat with
care: the ethics of calling a person's deficiency an advantage, and who owns the
compensation channel — autonomy vs. dependency on the compensator. The economic
framing that is clean for artifacts is loaded for people.

## 5. Composition

A fleet of removable-defect specialists vs. one generalist. The value theory's
diversity results suggest the fleet wins, but every specialist carries its own
detector — where does the summed cost of N detectors flip the answer? Related:
can detectors be shared (one router, many experts), and what does sharing do to
the single-point-of-failure problem in #3?

## 6. Is "removable" one concept or three?

Permanent removal (buy the capability — the defect becomes a capability),
momentary removal (escalate this once), interpretive removal (the sealed-frame
case, where the seal is never lifted and removal means compensating its fatal
case). One spectrum, or three mechanisms that deserve separate treatment?

**Status (2026-07-13): first hard evidence the split is real, on a different
axis than expected** — observation defects and capacity defects provably
differ (PROOFS.md): transductive access rescues capacity defects and never
observation defects, so a unified removability theorem was the wrong target.
Also: removability is a coefficient ($\sigma_0$, residual separability), not
a yes/no.
