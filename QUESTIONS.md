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

**Status (2026-07-13): minimax form now in hand** — Theorem A (PROOFS.md §9)
proves the worst-case version $\underline c(1-\alpha_0)(g+p) > \bar\varepsilon
\delta(L-p) + c_d$, which uses only an *upper bound* $\bar\varepsilon$ on fatal
mass and an upper bound $\delta$ on miss rate — never a point estimate of the
frequency. Open remainder: making $\bar\varepsilon$ itself adversarial/unknown
(vs. a declared budget), and the clustered-events non-i.i.d. regime flagged in
FORMALIZATION.md §5.

## 2. Detector economics

When is detection *provably* cheaper than competence? The intuition is
NP-flavored — verifying is easier than solving — but intuition is not a bound.
Make it precise or drop it. A useful target: conditions under which a detector
with cost o(competence cost) achieves a given miss rate.

**Status (2026-07-13): now load-bearing, not just a curiosity** — Theorem A
(PROOFS.md §9, Remark 3) shows the architecture survives arbitrary loss
severity $L$ only if miss rate shrinks as $\delta = O(1/L)$, which by the
hypothesis-testing bound costs rent $c_d \sim \log L$. Cheap detection is
exactly what keeps leakage bounded at logarithmic price — so this question's
answer sets the sustainable ceiling on $L$ (exponential in the premium
margin). Resolving the verification-vs-generation unit model (FORMALIZATION.md
§8, via doubly-efficient proofs) would upgrade this from asymptotic to exact.

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

**Refined (later same day) by Theorem 2′ (PROOFS.md §7):** the value formula
*does* unify — for any detector class, premium = support function of the
class's ROC set at the economic price vector, coefficient = zero-leak capture
capacity $\bar\sigma_0$; Theorem 1′ is the σ-algebra special case. So
removability is priced identically for both defect types; the genuine split
is **joint realizability** — whether one member of the class serves all
confusable directions at once. One concept for the price, (at least) two for
the escape routes.
