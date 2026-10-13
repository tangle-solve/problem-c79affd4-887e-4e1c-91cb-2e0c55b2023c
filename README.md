<!-- solve-problem:c79affd4-887e-4e1c-91cb-2e0c55b2023c -->
# Sorting X + Y in quadratic time

A [Tangle Solve problem](https://solve.tangle.tools/problems/c79affd4-887e-4e1c-91cb-2e0c55b2023c).

## What is the problem?

Given two sorted lists of real numbers \(X = (x_1 \le \dots \le x_n)\) and \(Y = (y_1 \le \dots \le y_n)\), output all \(n^2\) sums \(\{x_i + y_j : 1 \le i,j \le n\}\) in sorted order, in \(O(n^2)\\) total time on a real RAM (comparisons and arithmetic on reals cost \(O(1)\); a comparison sort of \(n^2\) arbitrary reals costs \(\Theta(n^2 \log n)\)).

The informal statement hides a precise dichotomy. A *comparison bound* is a bound on the number of comparisons between sums \(x_i+y_j\) vs. \(x_{i'}+y_{j'}\) needed in the worst case to determine the sorted order; by Fredman's theory this is \(O(n^2)\), since the partial order given by the \(2n-1\) "obvious" chains \(x_i + y_1 \le \dots \le x_i + y_n\) and \(x_1 + y_j \le \dots \le x_n + y_j\) has few linear extensions. An *algorithm* must additionally *decide which comparisons to make adaptively* and perform \(O(1)\) overhead work per sum. The open problem: exhibit a uniform algorithm achieving both. Note some formulations ask only for a non-uniform/decision-tree result — that is already known; the census record targets the uniform algorithm.

## Why does it matter?

It is a canonical test of whether information-theoretic comparison bounds can always be algorithmically realized — a rare place where "how many comparisons suffice" and "how fast can we compute" provably diverge (as far as we know). It appears as Problem 41 in the Open Problems Project (TOPP, computational geometry), the \(X+Y\) matrix underlies algorithms for \(k\)-sets, minimum-area triangles, and distance problems. A quadratic algorithm (or a proof of impossibility) would settle a 40+ year-old question and clarify the power of adaptive versus non-adaptive sorting strategies.

## What's already known?

- **Fredman (1976):** In general, sorting under a partial order with \(e\) linear extensions needs \(\log_2 e + O(n)\) comparisons. Applied here, the sumset partial order yields an \(O(n^2)\) comparison bound — but Fredman's method is non-constructive in the required sense: it doesn't specify how to find the right comparisons quickly (his "merge insertion"-style strategy for this poset has superquadratic overhead). Best-known consequence: sorting \(X+Y\) in \(O(n^2 \log(\log n / \log\log n))\)-ish time via refined Fredman-type arguments (verify exact bound; improvements by e.g. Kane & Fredman for related posets).
- **Lambert (1992):** Gave a uniform algorithm sorting \(X+Y\) using \(O(n^2)\) comparisons but with more than quadratic *total* time, because bookkeeping per comparison is superconstant. This is the crucial distinction in the problem.
- Naive baselines: \(O(n^2 \log n)\) by full sorting; \(O(n^2 \log n)\)-ish via merging \(n\) sorted rows with heaps (actually \(O(n^2 \log n)\)); best published uniform algorithms sit slightly superquadratic.
- Additive combinatorics tells us the *structure* of \(X+Y\) (few distinct sums imply additive structure), but hasn't translated into a sorting algorithm.

## What would count as solving it?

1. **Positive:** an explicit, uniform real-RAM algorithm that, given sorted \(X, Y\), outputs the sorted multiset \(X+Y\) in \(O(n^2)\) worst-case time (total operations, not just comparisons). A referee accepts a proof of the running time; "expected" time vs. adversarial inputs must be clarified (worst case preferred).
2. **Negative:** a proof that any uniform algorithm needs \(\omega(n^2)\) time even though \(O(n^2)\) comparisons suffice — i.e., a genuine separation between comparison complexity and time complexity. This would be a landmark result.
3. **Partial:** an \(O(n^2 \log\log n)\) or \(O(n^2 \,\alpha(n))\) uniform algorithm would be significant progress over the current best.

## How to start

1. Read Fredman (1976) §on poset sorting, then Lambert (1992) — understand exactly where Lambert's bookkeeping loses the quadratic bound; the loss is in a priority queue / merging structure, which suggests the attack.
2. Model the problem as repeatedly extracting minima from a grid where the poset of "known" comparisons forms a staircase frontier; ask whether the frontier can be maintained in amortized \(O(1)\) per element (this is where heap-type lower bounds of \(\Omega(\log n)\) for merging may or may not apply).
3. Try to exploit the geometry: \(X+Y\) sums correspond to a rank-one structure; consider whether persistence/fractional cascading on the \(n \times n\) matrix can beat merging overheads.
4. Main obstacle: sorting under a partial order with \(O(n^2)\) linear-extension entropy still requires *locating* each element among comparable-but-unresolved ones, and the standard merge machinery costs \(\log\) factors. Any impossibility intuition from merging \(n\) lists should be examined — it is *not* a proof of hardness here.

## Key references

- Michael L. Fredman, "How Good Is the Information Theory Bound in Sorting?", *Theoretical Computer Science* 1(4):355–361, 1976. Proves that sorting under a known partial order with \(e\) linear extensions needs at most \(\log_2 e + O(n)\) comparisons, implying \(O(n^2)\) comparisons suffice for \(X+Y\).
- Jean-Luc Lambert, "Sorting the sums \((x_i + y_j)\) in \(O(n^2)\) comparisons", *Theoretical Computer Science* 103(1):137–141, 1992. (Verify exact title/pages.) Gives a uniform \(O(n^2)\)-comparison algorithm for \(X+Y\), but with superquadratic total running time.
- Demaine, Demaine, Hoffmann, "The Open Problems Project," Problem 41 ("Sorting \(X+Y\)"), https://topps.openproblem.net/. States the uniform quadratic-time version and surveys status.

## Warnings

- The most common error: conflating "O(n²) comparisons" (known) with "O(n²) time" (open). Lambert's result is often miscited as solving the problem — it does not.
- Fredman's bound is often misread as algorithmic; it is a decision-tree/existence bound.
- The problem is sometimes stated for distinct reals; duplicates in \(X\) or \(Y\) change linear-extension counts but not the asymptotic question.
- Beware claimed \(O(n^2)\) algorithms in obscure venues; none is accepted by the community (verify any claimed breakthrough independently).

---
*`ALGO-000155` · unverified*

## Acceptance guidance

1. **Positive:** an explicit, uniform real-RAM algorithm that, given sorted \(X, Y\), outputs the sorted multiset \(X+Y\) in \(O(n^2)\) worst-case time (total operations, not just comparisons). A referee accepts a proof of the running time; "expected" time vs. adversarial inputs must be clarified (worst case preferred).
2. **Negative:** a proof that any uniform algorithm needs \(\omega(n^2)\) time even though \(O(n^2)\) comparisons suffice — i.e., a genuine separation between comparison complexity and time complexity. This would be a landmark result.
3. **Partial:** an \(O(n^2 \log\log n)\) or \(O(n^2 \,\alpha(n))\) uniform algorithm would be significant progress over the current best.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/c79affd4-887e-4e1c-91cb-2e0c55b2023c?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000155.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000155)
