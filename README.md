<!-- solve-problem:4e08821a-10bd-4a82-bd48-58e8cdee07a2 -->
# Arithmetic formulas versus branching programs

A [Tangle Solve problem](https://solve.tangle.tools/problems/4e08821a-10bd-4a82-bd48-58e8cdee07a2).

## What is the problem?

In algebraic complexity theory, polynomials over a field F are computed by several models:

- An **arithmetic formula** is a circuit whose underlying graph is a tree: each gate has fan-out 1, so no intermediate result is reused. Size = number of operations.
- An **algebraic branching program (ABP)** is a layered DAG with a source s and sink t; each edge is labeled by a field element or a variable; the ABP computes the sum over all s–t paths of the product of edge labels along the path. Size = number of edges.

Known facts: formulas of size s can be simulated by ABPs of size poly(s), and ABPs of size s by circuits of size poly(s); circuits of depth d can be balanced into formulas with quasi-polynomial blowup. Hence:

**Formula ⊆ ABP ⊆ Circuit** (up to polynomial blowup).

The open question: **Is every polynomial family with polynomial-size ABPs computable by polynomial-size formulas?** Equivalently, does VF (polynomial-size formula families) equal VFABP (polynomial-size ABP families)? The conjectured answer is **no**: Valiant's classes satisfy VF ⊆ VP ⊆ VNP, and VBP (ABPs) sits between VF and VP, conjectured strictly above VF. The canonical candidate witness is the determinant: Det_n has polynomial-size ABPs (via dynamic programming over clow sequences / Ryser-style expansions) and polynomial-size circuits (Gaussian elimination), but no polynomial-size formula is known. A superpolynomial formula lower bound for Det_n, or for iterated matrix multiplication, would separate VBP (hence VP) from VF.

The precise interpretation: prove VF ⊊ VBP (or, less likely, VF = VBP), over "nice" fields (characteristic 0 or sufficiently large; some results are characteristic-sensitive). Constant-free variants (VP⁰ etc.) are studied but the core problem is over an arbitrary/standard field with variables commuting.

## Why does it matter?

This is the arithmetic analogue of the NC¹ vs. NL/space-bounded question, and it is plausibly the most tractable first step toward VP vs. VNP, since ABPs are a structured, syntactically restricted subclass of circuits. A formula lower bound for the determinant or iterated matrix multiplication would be the first superpolynomial lower bound for any natural algebraic model beyond depth-3/4 circuits, breaking a 40+ year barrier. It also quantifies exactly how much arithmetic computation gains from sharing subcomputations (DAG structure) versus pure tree-structured evaluation — relevant to symbolic computation (expression swell in straight-line vs. tree representations) and to automatic differentiation, where derivative accumulations are naturally DAG-shared.

## What's already known?

- **Valiant et al. (1983)** introduced the classes and showed Det has small ABPs; the VF vs. VBP question dates to this paper.
- **Exponential lower bounds for determinant-sized formulas**: any formula computing the n×n permanent requires size ~n³ (refuting a naive Valiant-type hypothesis for formulas); analogously, determinant requires Ω(n³) formula size via matrix rank/matrix-rigidity-type methods (Snir; Raz). But these are only polynomial — nowhere near separating polynomial-size ABPs from polynomial-size formulas.
- **Structural results**: Brent's balancing gives depth-O(log s) formulas with polynomial blowup for any size-s formula; ABPs are closed under taking partial derivatives and homogeneous components efficiently. VBP has natural characterizations via iterated matrix multiplication (IMM) and noncommutative determinants (Nisan).
- **Nisan (1991)** proved an *exact* exponential separation of formulas vs. ABPs in the **noncommutative** setting: the noncommutative permanent (and determinant) has 2^Ω(n) formula complexity but poly(n) ABP size. This is the strongest known evidence the commutative classes differ — but noncommutative techniques (word-based counting) do not transfer, because commutativity collapses the "path-counting by words" argument.
- **Lower-bound program**: depth-3/depth-4 circuit lower bounds (Nisan–Wigderson, Kayal–Saha–Saptharishi, Gupta et al., Chillara et al.) and the "chasm at depth 4" (Agrawal–Vinay, Koiran) show it suffices to bound depth-4 circuits; the shifting/partial-derivative dimension method powers these, but all known methods degenerate for general formulas (they give only trivial bounds once depth is unbounded).
- **Geometric complexity theory** attacks VNP vs. VNP-representatives; it has yielded obstructions but no separations.

## What would count as solving it?

1. **Separation (expected outcome):** A proof that Det_n (or IMM_{n,2n}, the (2n)-fold product of n×n symbolic matrices, which is VBP-complete) requires superpolynomial formula size. This proves VF ⊊ VBP and hence VF ⊊ VP. Referee criterion: an explicit family {f_n}, explicit polynomial-size ABPs for it, and a proof that any formula for f_n has size n^{ω(1)}.
2. **Collapse (unexpected):** A construction giving polynomial-size formulas for a VBP-complete family (e.g., Det or IMM), proving VF = VBP. Since ABPs capture polynomial-size "algebraic NL", this would be shocking but must be taken seriously as a logical possibility.
3. **Weaker but valuable milestones:** (a) any superpolynomial lower bound for formulas of unbounded degree-fan-in structure beyond known models; (b) a genuinely n^{ω(1)} bound for monotone arithmetic formulas for a Det-like family (Nutanong/other monotone results exist — verify current status); (c) conditional implications, e.g., separating under natural restrictions (read-once ABPs, noncommutative-but-commutative-observable hybrids).

## How to start

1. **Read Shpilka–Yehudayoff (2010), "Arithmetic Circuits: A Survey of Recent Results and Open Questions"** — Sections on branching programs, formula lower bounds, and the chasm at depth 4. This orients the landscape and lists the standard open problems.
2. **Read Nisan (1991), "Lower Bounds for Non-Commutative Computation"** (STOC'91). Understand exactly why rank-of-communication-matrix works noncommutatively and where commutativity kills it — this is the crux you must circumvent.
3. **Study the polynomial lower bounds for Det/Perm formulas** (Snir 1979; Raz 2010 on multilinear formula lower bounds for Det and Perm — note Raz proves n^{Ω(log n)} for multilinear formulas, n^{3/2−o(1)}-type bounds in general; verify exact statements). Ask: does any technique there scale to superpolynomial?
4. **Natural approach:** seek a "path-independence" invariant — a complexity measure on polynomials that is (i) subadditive/submultiplicative under formula composition and (ii) large for some explicit ABP-computable polynomial. Candidate measures: shifted partial-derivative dimension (fails for unbounded depth), rank of higher-order Hessian-like flattenings, recent "rank-codification"/slope measures (Kumar–Saptharishi line of work — verify). Main obstacle: all known dimension-based measures are essentially solved by formulas of depth O(log n) with polynomial blowup, and Brent balancing shows every formula can be so balanced — so any depth-robust measure must be "balancing-sensitive."

## Key references

- L. Valiant, S. Skyum, S. Berkowitz, C. Rackoff, "Fast Parallel Computation of Polynomials Using Few Processors," SIAM J. Comput. 12(4), 1983. Introduces the parallel/arithmetic class framework, shows Det has polynomial-size circuits and ABPs, and implicitly poses the formula-vs-ABP question. https://doi.org/10.1137/0212040
- A. Shpilka, A. Yehudayoff, "Arithmetic Circuits: A Survey of Recent Results and Open Questions," Foundations and Trends in Theoretical Computer Science 5(3–4), 2010. The standard survey: formal definitions of VF/VBP/VP/VNP, lower-bound methods, and the open-problem list including this one. https://doi.org/10.1561/0400000039
- N. Nisan, "Lower Bounds for Non-Commutative Computation," STOC 1991. Proves the exponential formula-vs-ABP separation for the noncommutative permanent/determinant via rank of a communication matrix. https://doi.org/10.1145/103418.103472
- R. Raz, "Separation of Multilinear Formulas and Monotone Arithmetic Formulas" and related 2004–2010 works, e.g., "Multi-linear formulas for permanent and determinant are of super-polynomial size" (STOC 2004). Proves n^{Ω(log n)} lower bounds for multilinear formulas computing Det/Perm. (verify exact statements/bounds)
- M. Agrawal, V. Arvind, S. Saptharishi et al. on the chasm at depth 4 (Agrawal–Vinay 2008, Koiran 2012): show depth-4 lower bounds would lift to general circuits/ABPs — relevant strategy framing. (verify)

## Warnings

- **Do not conflate models.** ABPs ⊆ circuits, formulas ⊆ ABPs; the question is the *middle* inclusion being strict, not circuit-vs-formula (that's the harder VP vs. VF, also open, and a formula lower bound for a VBP-complete family solves both at once).
- **Noncommutative results do not transfer.** Nisan's separation is often misread as resolving this; commutativity destroys the word-counting argument entirely.
- **Balancing subtlety.** Brent's theorem means any argument about "deep formulas" must survive balancing to depth O(log n); measures based solely on depth or fan-in patterns are doomed.
- **Degree issues.** Det_n has degree n; lower-bound methods for high-degree polynomials (e.g., for the permanent) are qualitatively harder — degree-reduction tricks that work for circuits (homogenization) blow up formula size, which is actually a *feature* you may exploit, but handle carefully.
- **Multilinearity is a restriction.** Raz's superpolynomial bounds are for *multilinear* formulas; general formulas can use non-multilinear intermediate gates, and no superpolynomial general-formula bound for Det/Perm is known.
- The census record's "related fields" (expression swell, AD) are motivational context, not established connections — don't cite them as results.

---
*`ALGO-000042` · unverified*

## Acceptance guidance

1. **Separation (expected outcome):** A proof that Det_n (or IMM_{n,2n}, the (2n)-fold product of n×n symbolic matrices, which is VBP-complete) requires superpolynomial formula size. This proves VF ⊊ VBP and hence VF ⊊ VP. Referee criterion: an explicit family {f_n}, explicit polynomial-size ABPs for it, and a proof that any formula for f_n has size n^{ω(1)}.
2. **Collapse (unexpected):** A construction giving polynomial-size formulas for a VBP-complete family (e.g., Det or IMM), proving VF = VBP. Since ABPs capture polynomial-size "algebraic NL", this would be shocking but must be taken seriously as a logical possibility.
3. **Weaker but valuable milestones:** (a) any superpolynomial lower bound for formulas of unbounded degree-fan-in structure beyond known models; (b) a genuinely n^{ω(1)} bound for monotone arithmetic formulas for a Det-like family (Nutanong/other monotone results exist — verify current status); (c) conditional implications, e.g., separating under natural restrictions (read-once ABPs, noncommutative-but-commutative-observable hybrids).

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/4e08821a-10bd-4a82-bd48-58e8cdee07a2?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000042.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000042)
