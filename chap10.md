# Chapter 10: The Bellman Optimality Operator and the Central Gap

---

## 10.1 Introduction

In Chapter 9, we established that the Bellman **evaluation** operator \( T^\pi \) is a \( \lambda \)-contraction with a unique fixed point \( Q^\pi \). This is a proven result—the complex action-value function for any fixed policy exists and can be computed by value iteration.

Now we turn to the **optimality** operator. In standard RL, the Bellman optimality operator is a contraction, guaranteeing convergence to the optimal value function. In the complex setting, the situation is fundamentally different. The lack of a total order on \( \mathbb{C} \) creates a geometric obstruction that prevents the standard contraction proof from going through.

This chapter identifies the obstruction and its three faces. We present a partial positive result—a scalar modulus contraction—and then introduce the Bergman Conjecture as a proposed resolution. The contraction of the Bellman optimality operator is the central open problem (OP1) of this book.

The chapter is organized as follows:
- **Section 10.2:** Definition of the Bellman optimality operator.
- **Section 10.3:** The geometric obstruction.
- **Section 10.4:** Three faces of the obstruction.
- **Section 10.5:** A partial positive result.
- **Section 10.6:** The Bergman Conjecture.
- **Section 10.7:** Alternative approaches.
- **Section 10.8:** Implications for complex RL.

---

## 10.2 The Bellman Optimality Operator

### 10.2.1 Definition

**Definition 10.1 (Bellman Optimality Operator).** The Bellman optimality operator \( T: \mathcal{Q} \to \mathcal{Q} \) is defined by:
\[
(TQ)(s, a) = \sum_{s' \in \mathcal{S}} p(s' \mid s, a) \left[ z(s, a, s') + \lambda \cdot Q(s', \pi_Q(s')) \right],
\]
where
\[
\pi_Q(s') = \arg\min_{a' \in \mathcal{A}} |Q(s', a')|.
\]

**Interpretation:** The operator evaluates the expected complex utility of taking action \( a \) in state \( s \), then following the modulus-greedy policy thereafter.

### 10.2.2 The Fixed-Point Equation

A fixed point \( Q^* \) of \( T \) satisfies:
\[
Q^*(s, a) = \sum_{s'} p(s' \mid s, a) \left[ z(s, a, s') + \lambda \cdot Q^*(s', \pi_{Q^*}(s')) \right].
\]

This is the **Bellman optimality equation** for cMDPs.

### 10.2.3 The Desired Result

In standard RL, the Bellman optimality operator is a \( \lambda \)-contraction:
\[
\| T Q_1 - T Q_2 \|_\infty \leq \lambda \| Q_1 - Q_2 \|_\infty.
\]

This guarantees:
1. **Existence:** A unique fixed point exists.
2. **Convergence:** Value iteration converges at rate \( \lambda \).

In the complex setting, we would like the same result. However, as we shall see, it does not hold in general.

---

## 10.3 The Geometric Obstruction

### 10.3.1 The Core Issue

To prove that \( T \) is a contraction, we need to show:
\[
|(TQ_1)(s, a) - (TQ_2)(s, a)| \leq \lambda \| Q_1 - Q_2 \|_\infty.
\]

Expanding:
\[
|(TQ_1)(s, a) - (TQ_2)(s, a)| = \lambda \left| \sum_{s'} p(s' \mid s, a) \left[ Q_1(s', a_1(s')) - Q_2(s', a_2(s')) \right] \right|,
\]
where \( a_1(s') = \arg\min_{a'} |Q_1(s', a')| \) and \( a_2(s') = \arg\min_{a'} |Q_2(s', a')| \).

The standard proof requires:
\[
|Q_1(s', a_1(s')) - Q_2(s', a_2(s'))| \leq \| Q_1 - Q_2 \|_\infty.
\]

### 10.3.2 Why This Fails in \( \mathbb{C} \)

In \( \mathbb{R} \), this inequality holds because the real line is **totally ordered**. For any \( a_1, a_2 \):
\[
|Q_1(s, a_1) - Q_2(s, a_2)| \leq \max_a |Q_1(s, a) - Q_2(s, a)| \leq \| Q_1 - Q_2 \|_\infty.
\]

In \( \mathbb{C} \), **no such implication holds**. The identity \( a_1 = \arg\min_{a'} |Q_1(s, a')| \) says nothing about the complex relationship between \( Q_1(s, a_1) \) and \( Q_2(s, a_2) \). The two values can have equal modulus while pointing in opposite directions.

---

## 10.4 Three Faces of the Obstruction

### 10.4.1 Face 1: No Total Order

**Face 1 — No Total Order.** The complex numbers are not totally ordered. The inequality
\[
|Q_1(s, a_1) - Q_2(s, a_2)| \leq \max_a |Q_1(s, a) - Q_2(s, a)|
\]
does not hold in general.

**Example:** Let:
- \( Q_1(s, a_1) = 1 + i \cdot 0 \)
- \( Q_2(s, a_2) = 0 + i \cdot 1 \)

Then \( |Q_1(s, a_1)| = 1 \), \( |Q_2(s, a_2)| = 1 \), but:
\[
|Q_1(s, a_1) - Q_2(s, a_2)| = |1 - i| = \sqrt{2} \approx 1.414.
\]

If \( \| Q_1 - Q_2 \|_\infty = 1 \), the inequality fails.

### 10.4.2 Face 2: Discontinuity of the Selector

**Face 2 — Discontinuity of the Selector.** The map
\[
Q \mapsto \arg\min_{a} |Q(s, a)|
\]
is discontinuous at ties.

**Example:** Let \( \mathcal{A} = \{a_1, a_2\} \). Suppose:
- \( Q(s, a_1) = 1 + i \cdot 0 \)
- \( Q(s, a_2) = 0 + i \cdot 1 \)

Both have modulus 1. A tiny perturbation can switch the selected action:
- If \( Q(s, a_1) = 1 + i \cdot 0 \), \( Q(s, a_2) = 0 + i \cdot (1 - \epsilon) \), then \( a_2 \) is selected.
- If \( Q(s, a_1) = 1 + i \cdot 0 \), \( Q(s, a_2) = 0 + i \cdot (1 + \epsilon) \), then \( a_1 \) is selected.

**Implication:** The selected action can change discontinuously, causing a discrete jump in the selected value.

### 10.4.3 Face 3: Phase Cancellation

**Face 3 — Phase Cancellation.** Two complex numbers can be close in modulus while pointing in nearly opposite directions. The selected values can then be nearly antipodal, making their difference large relative to \( \| Q_1 - Q_2 \|_\infty \).

**Example:** Let:
- \( Q_1(s, a_1) = 1 + i \cdot 0 \)
- \( Q_2(s, a_2) = -1 + i \cdot \epsilon \)

Then:
- \( |Q_1(s, a_1)| \approx 1 \)
- \( |Q_2(s, a_2)| \approx 1 \)
- \( |Q_1(s, a_1) - Q_2(s, a_2)| = |2 - i\epsilon| \approx 2 \)

The modulus difference is \( O(\epsilon) \), but the complex difference is \( O(1) \).

### 10.4.4 The Obstruction Summarized

| Face | Issue | Consequence |
|------|-------|-------------|
| 1 | No total order | Inequality fails for complex values |
| 2 | Discontinuity of selector | Action selection can jump |
| 3 | Phase cancellation | Large difference despite small modulus difference |

**Conjecture:** In the worst case, the contraction modulus can be \( \kappa > 1 \), meaning \( T \) is not a contraction in the supremum norm.

---

## 10.5 A Partial Positive Result

### 10.5.1 The Scalar Modulus Contraction

Although \( T \) is not a contraction, a related operator on the scalar modulus **is**.

**Definition 10.2 (Scalar Modulus Operator).** Define \( \hat{T}: \mathcal{B}(\mathcal{S}, \mathbb{R}_+) \to \mathcal{B}(\mathcal{S}, \mathbb{R}_+) \) by:
\[
(\hat{T} V)(s) = \min_{a \in \mathcal{A}} \sum_{s' \in \mathcal{S}} p(s' \mid s, a) \left[ |z(s, a, s')| + \lambda \cdot V(s') \right].
\]

**Theorem 10.3 (Scalar Modulus Contraction).** \( \hat{T} \) is a \( \lambda \)-contraction on \( (\mathcal{B}(\mathcal{S}, \mathbb{R}_+), \|\cdot\|_\infty) \).

*Proof.* This follows the standard Bellman optimality contraction proof in \( \mathbb{R} \). For any \( V_1, V_2 \in \mathcal{B}(\mathcal{S}, \mathbb{R}_+) \):
\[
|(\hat{T} V_1)(s) - (\hat{T} V_2)(s)| \leq \lambda \| V_1 - V_2 \|_\infty.
\]
\( \square \)

### 10.5.2 Interpretation

The scalar modulus contraction computes the optimal **magnitude** of the value function:
\[
V^*(s) = \min_\pi \mathbb{E}^\pi \left[ \sum_{k=0}^\infty \lambda^k |z(S_{t+k}, A_{t+k}, S_{t+k+1})| \mid S_t = s \right].
\]

**What it discards:** The phase information—the cost-debt decomposition. The scalar modulus operator treats the complex utility as a single scalar \( |z| \), losing the distinction between cost and debt.

**What it preserves:** The optimal expected modulus. This is a lower bound on the true complex objective.

### 10.5.3 The Gap

There is a gap between:
1. The optimal expected modulus: \( V^*(s) = \min_\pi \mathbb{E}[|G_t^\pi| \mid S_t = s] \).
2. The optimal modulus of expectation: \( J^*(s) = \min_\pi |\mathbb{E}[G_t^\pi] \mid S_t = s] \).

The scalar modulus contraction computes (1), not (2). To recover the phase information, we need the full complex framework.

---

## 10.6 The Bergman Conjecture

### 10.6.1 Motivation

The obstruction arises from the lack of structure on \( \mathbb{C} \). If we restrict the space of value functions to a space with more structure, the obstruction may disappear.

The Bergman space \( \mathcal{A}^2(\Omega) \) is a natural candidate. It consists of **holomorphic** functions—complex functions with constrained phase behavior. The argument principle and the open mapping theorem restrict the range of holomorphic functions, potentially eliminating phase cancellation.

### 10.6.2 Background: The Bergman Space

**Definition 10.4 (Bergman Space).** For a bounded domain \( \Omega \subset \mathbb{C} \), the Bergman space is:
\[
\mathcal{A}^2(\Omega) = \left\{ f: \Omega \to \mathbb{C} \text{ holomorphic} \,\middle|\, \int_\Omega |f(z)|^2 \, dA(z) < \infty \right\}.
\]

The Bergman space is a Hilbert space with inner product:
\[
\langle f, g \rangle_{\mathcal{A}^2} = \int_\Omega f(z) \overline{g(z)} \, dA(z).
\]

**Definition 10.5 (Bergman Metric).** The Bergman metric on \( \Omega \) is the Hermitian metric induced by the Bergman kernel \( K(z, w) \).

**Properties:**
1. **Completeness:** The Bergman metric is complete on bounded domains.
2. **Curvature:** The Bergman metric has negative holomorphic sectional curvature on bounded symmetric domains.
3. **Compactness:** The inclusion \( \mathcal{A}^2(\Omega) \hookrightarrow C(\Omega) \) is compact (Montel's theorem).

### 10.6.3 The Conjecture

**Conjecture 10.6 (Bergman Contraction Conjecture).** Suppose the state-action space \( \mathcal{S} \times \mathcal{A} \) is a bounded domain \( \Omega \subset \mathbb{C} \). Restrict the Bellman optimality operator \( T \) to the Bergman space:
\[
T_{\mathcal{A}}: \mathcal{A}^2(\Omega) \to \mathcal{A}^2(\Omega), \quad (T_{\mathcal{A}} Q)(s, a) = (TQ)(s, a).
\]

Then \( T_{\mathcal{A}} \) is a contraction in the Bergman norm:
\[
\| T_{\mathcal{A}} Q_1 - T_{\mathcal{A}} Q_2 \|_{\mathcal{A}} \leq \kappa \| Q_1 - Q_2 \|_{\mathcal{A}},
\]
for some \( \kappa < 1 \).

### 10.6.4 Why the Conjecture Might Hold

Three properties support the conjecture:

1. **Holomorphic Constraint Eliminates Phase Cancellation.** Holomorphic functions on a connected domain cannot have arbitrary phase behavior. The argument principle constrains the range, so two holomorphic functions that are close in norm cannot have nearly antipodal values at the same point.

2. **Compactness Gives Existence.** The inclusion \( \mathcal{A}^2(\Omega) \hookrightarrow C(\Omega) \) is compact, so \( T_{\mathcal{A}} \) maps bounded sets to relatively compact sets. Schauder's fixed-point theorem then gives existence of a fixed point without requiring a contraction.

3. **Negative Curvature.** The Bergman metric has negative holomorphic sectional curvature on bounded symmetric domains. This is the geometric analogue of contractivity: negative curvature means that geodesics converge.

### 10.6.5 What Needs to Be Proved

To prove the conjecture, we need to establish:

1. The modulus-greedy selector \( \pi_Q(s) = \arg\min_a |Q(s, a)| \) maps holomorphic \( Q \) to a selector that is measurable (and ideally continuous) in the Bergman topology.

2. The composition \( Q \mapsto Q \circ \pi_Q \) maps \( \mathcal{A}^2(\Omega) \) into itself (i.e., the composed function is holomorphic).

3. \( \| T_{\mathcal{A}} Q_1 - T_{\mathcal{A}} Q_2 \|_{\mathcal{A}} \leq \kappa \| Q_1 - Q_2 \|_{\mathcal{A}} \) for some \( \kappa < 1 \).

**Remark 10.7.** The Bergman conjecture requires a continuous state-action space. It does not directly resolve OP1 in the finite-state tabular setting. However, a positive resolution for continuous domains would provide strong evidence that the contraction gap is an artifact of the supremum norm on discrete spaces.

---

## 10.7 Alternative Approaches

### 10.7.1 Phase-Weighted Norms

**Definition 10.8 (Phase-Weighted Norm).** For \( \beta > 0 \), define:
\[
\| Q \|_{\infty, \beta} = \max_{s, a} e^{\beta |\arg Q(s, a)|} |Q(s, a)|.
\]

**Idea:** Penalize functions with large phase variation. If \( \beta \) is large enough, the phase-weighted norm may make \( T \) a contraction.

### 10.7.2 Cone Restrictions

**Definition 10.9 (Cone).** A cone in \( \mathbb{C} \) is a set of the form:
\[
\mathcal{C}_\theta = \{ z \in \mathbb{C} \mid |\arg z| \leq \theta \}.
\]

**Idea:** Restrict value functions to a cone \( \mathcal{C}_\theta \). If \( \theta < \pi/4 \), the cross-action term can be controlled.

### 10.7.3 Smoothed Operators

**Definition 10.10 (Smoothed Operator).** For \( \tau > 0 \), define:
\[
(T_\tau Q)(s, a) = \sum_{s'} p(s' \mid s, a) \left[ z(s, a, s') + \lambda \cdot \sum_{a'} \pi_\tau(a' \mid s') Q(s', a') \right],
\]
where
\[
\pi_\tau(a \mid s) = \frac{\exp(-|Q(s, a)| / \tau)}{\sum_{a'} \exp(-|Q(s', a')| / \tau)}.
\]

**Idea:** The smoothed operator is Lipschitz. Its limit as \( \tau \to 0 \) recovers the greedy selector.

---

## 10.8 Implications for Complex RL

### 10.8.1 The Central Open Problem

The contraction of the Bellman optimality operator is **the central open problem (OP1)** of this book. Its resolution would:

1. **Prove convergence** of complex \( Q \)-learning.
2. **Establish existence** of optimal policies.
3. **Provide a theoretical foundation** for complex RL algorithms.

### 10.8.2 What We Know

**Proven:**
- The evaluation operator \( T^\pi \) is a \( \lambda \)-contraction.
- The scalar modulus operator \( \hat{T} \) is a \( \lambda \)-contraction.
- The imaginary \( Q \)-value telescopes to a closed form.

**Conjectured:**
- The Bergman contraction conjecture.
- Phase-weighted norms may yield contractions.
- Cone restrictions may yield contractions.

**Open:**
- Is \( T \) a \( \lambda \)-contraction in the supremum norm?
- What is the effective contraction modulus \( \kappa^* \)?
- Does an alternative norm exist under which \( T \) is a contraction?

### 10.8.3 A Heuristic at Equilibrium

**Remark 10.11 (Phase Vanishes at Equilibrium).** At epistemic equilibrium (Axiom 7.19), \( Q_I^\pi(s, a) \to 0 \), so:
\[
\arg Q^\pi(s, a) \to 0.
\]

All complex values approach the positive real axis. The maximum phase difference approaches zero, and the contraction gap may vanish.

**This is a heuristic observation, not a proof.** Establishing a uniform contraction bound near the fixed point requires a separate argument.

---

## 10.9 Summary

This chapter has addressed the Bellman optimality operator:

1. **Definition:** \( T \) is the Bellman optimality operator for cMDPs.

2. **The Obstruction:** The lack of a total order on \( \mathbb{C} \) prevents the standard contraction proof.

3. **Three Faces:**
   - No total order.
   - Discontinuity of the selector.
   - Phase cancellation.

4. **Partial Positive Result:** The scalar modulus operator \( \hat{T} \) is a \( \lambda \)-contraction.

5. **The Bergman Conjecture:** Restricting \( T \) to the Bergman space may yield a contraction.

6. **Alternative Approaches:** Phase-weighted norms, cone restrictions, smoothed operators.

7. **Implications:** The contraction of \( T \) is the central open problem (OP1).

---

## Exercises

**Exercise 10.1 (Optimality Operator).** Define the Bellman optimality operator \( T \). What is the fixed-point equation?

**Exercise 10.2 (The Obstruction).** Show that \( |Q_1(s, a_1) - Q_2(s, a_2)| \leq \| Q_1 - Q_2 \|_\infty \) does not hold in general. Provide a counterexample.

**Exercise 10.3 (Phase Cancellation).** Give an example of two complex numbers with equal modulus but large difference. What does this imply for the contraction proof?

**Exercise 10.4 (Scalar Modulus).** Prove that the scalar modulus operator \( \hat{T} \) is a \( \lambda \)-contraction. What is the contraction constant?

**Exercise 10.5 (Bergman Space).** Define the Bergman space \( \mathcal{A}^2(\Omega) \). What properties make it a candidate for resolving the contraction gap?

**Exercise 10.6 (Bergman Conjecture).** State the Bergman contraction conjecture. What needs to be proved to establish it?

**Exercise 10.7 (Phase-Weighted Norm).** Define the phase-weighted norm. How might it resolve the contraction gap?

---

## Further Reading

- Krantz, S. G. (2001). *Function Theory of Several Complex Variables*. American Mathematical Society. — Bergman spaces and the Bergman metric.

- Kobayashi, S. (1998). *Hyperbolic Complex Spaces*. Springer-Verlag. — Negative curvature and hyperbolicity.

- Puterman, M. L. (2014). *Markov Decision Processes*. Wiley. — The standard MDP optimality operator.

- Szepesvári, C. (2010). *Algorithms for Reinforcement Learning*. Morgan & Claypool. — Convergence of RL algorithms.

