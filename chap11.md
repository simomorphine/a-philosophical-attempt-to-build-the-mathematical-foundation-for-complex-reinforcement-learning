# Chapter 11: Policy Gradients in the Complex Plane

---

## 11.1 Introduction

In Chapters 8-10, we developed value-based methods for complex MDPs. We established the Bellman evaluation equation (proven contraction) and identified the Bellman optimality contraction as the central open problem. While value-based methods are powerful, many problems require parameterized policies—especially in continuous state and action spaces.

This chapter extends the complex RL framework to **policy gradient methods**. We derive the Complex Policy Gradient Theorem, which generalizes the standard REINFORCE identity to complex returns. We then derive the gradient of the squared modulus objective and introduce the complex advantage function. The chapter concludes with the natural gradient in the complex domain, which sets the stage for the CNAC algorithm in Chapter 12.

The chapter is organized as follows:
- **Section 11.2:** The policy gradient setup for cMDPs.
- **Section 11.3:** The Complex Policy Gradient Theorem.
- **Section 11.4:** Gradient of the squared modulus objective.
- **Section 11.5:** The complex advantage and variance reduction.
- **Section 11.6:** Wirtinger calculus and complex gradients.
- **Section 11.7:** The natural gradient in the complex domain.
- **Section 11.8:** Examples and interpretations.

---

## 11.2 Policy Gradient Setup

### 11.2.1 Parameterized Policies

**Definition 11.1 (Parameterized Policy).** A parameterized policy is a family of stochastic policies
\[
\pi_\theta(a \mid s), \quad \theta \in \mathbb{R}^n,
\]
where \( \pi_\theta(a \mid s) \) is differentiable in \( \theta \).

**Regularity Assumption 11.2.** The policy satisfies:
1. \( \theta \mapsto \pi_\theta(a \mid s) \) is differentiable for all \( (s, a) \).
2. The gradient \( \nabla_\theta \log \pi_\theta(a \mid s) \) is bounded uniformly: \( \| \nabla_\theta \log \pi_\theta(a \mid s) \| \leq B < \infty \).

### 11.2.2 The Complex Expected Return

**Definition 11.3 (Complex Expected Return).** Under policy \( \pi_\theta \), the complex expected return from initial state \( s_0 \) is:
\[
\eta(\theta) = J^{\pi_\theta}(s_0) = \mathbb{E}^{\pi_\theta}[G_0 \mid S_0 = s_0] \in \mathbb{C}.
\]

**Decomposition:** \( \eta(\theta) = \eta_R(\theta) + i \cdot \eta_I(\theta) \), where:
- \( \eta_R(\theta) = \mathbb{E}^{\pi_\theta}[\sum_{k=0}^\infty \lambda^k c(S_k, A_k, S_{k+1}) \mid S_0 = s_0] \).
- \( \eta_I(\theta) = \mathbb{E}^{\pi_\theta}[\sum_{k=0}^\infty \lambda^k d(S_k, A_k, S_{k+1}) \mid S_0 = s_0] \).

### 11.2.3 The Performance Objective

**Definition 11.4 (Performance Objective).** The performance of policy \( \pi_\theta \) is:
\[
J(\theta) = |\eta(\theta)|^2 = \eta_R(\theta)^2 + \eta_I(\theta)^2 \in \mathbb{R}_+.
\]

**Why the squared modulus?** Minimizing \( |\eta|^2 \) is equivalent to minimizing \( |\eta| \) (both are zero at the same points and have the same sign of directional derivatives). The squared modulus is smooth and differentiable, avoiding the non-differentiability of \( |\eta| \) at zero.

---

## 11.3 The Complex Policy Gradient Theorem

### 11.3.1 The Key Insight

The log-derivative (REINFORCE) identity is **measure-theoretic**. It requires only that the probability measure \( \mathbb{P}_\theta \) is differentiable in \( \theta \). The return \( G_t \) enters as a complex-valued integrand; the measure remains a real probability measure. This separation is what makes the theorem work.

### 11.3.2 The Log-Derivative Identity

Let \( \tau = (S_0, A_0, S_1, A_1, \ldots) \) be a trajectory under \( \pi_\theta \). The probability of a length-\( T \) prefix is:
\[
\mathbb{P}_\theta(\tau_{0:T}) = \prod_{t=0}^{T-1} p(S_{t+1} \mid S_t, A_t) \cdot \pi_\theta(A_t \mid S_t).
\]

The log-derivative is:
\[
\nabla_\theta \log \mathbb{P}_\theta(\tau_{0:T}) = \sum_{t=0}^{T-1} \nabla_\theta \log \pi_\theta(A_t \mid S_t),
\]
since the transition kernel \( p \) does not depend on \( \theta \).

### 11.3.3 The Complex Policy Gradient Theorem

**Theorem 11.5 (Complex Policy Gradient Theorem).** For a cMDP with complex utility \( z = c + id \), under a stochastic policy \( \pi_\theta \) satisfying the regularity assumptions:
\[
\nabla_\theta \eta(\theta) = \mathbb{E}^{\pi_\theta} \left[ \sum_{t=0}^\infty \lambda^t G_t \cdot \nabla_\theta \log \pi_\theta(A_t \mid S_t) \mid S_0 = s_0 \right] \in \mathbb{C}^n,
\]
where \( G_t = \sum_{k=t}^\infty \lambda^{k-t} z(S_k, A_k, S_{k+1}) \) is the complex return from time \( t \).

*Proof.* Write the complex expected return as an integral over trajectories:
\[
\eta(\theta) = \int \left( \sum_{t=0}^\infty \lambda^t z_t \right) \, d\mathbb{P}_\theta(\tau),
\]
where \( z_t = z(S_t, A_t, S_{t+1}) \) and the sum converges absolutely.

Differentiating under the integral:
\[
\nabla_\theta \eta(\theta) = \int \left( \sum_{t=0}^\infty \lambda^t z_t \right) \nabla_\theta \log d\mathbb{P}_\theta(\tau) \, d\mathbb{P}_\theta(\tau).
\]

Applying the log-derivative identity:
\[
\nabla_\theta \eta(\theta) = \mathbb{E}^{\pi_\theta} \left[ \left( \sum_{t=0}^\infty \lambda^t z_t \right) \left( \sum_{s=0}^\infty \nabla_\theta \log \pi_\theta(A_s \mid S_s) \right) \mid S_0 = s_0 \right].
\]

Rearranging the double sum using the causality of the MDP:
- Terms with \( s > t \) vanish by the Markov property (future score functions are independent of past returns given the state).
- Terms with \( s < t \) vanish because \( \mathbb{E}^{\pi_\theta}[\nabla_\theta \log \pi_\theta(A_s \mid S_s) \mid S_s] = 0 \).

This gives:
\[
\nabla_\theta \eta(\theta) = \mathbb{E}^{\pi_\theta} \left[ \sum_{t=0}^\infty \lambda^t G_t \cdot \nabla_\theta \log \pi_\theta(A_t \mid S_t) \mid S_0 = s_0 \right].
\]
\( \square \)

### 11.3.4 Interpretation

The theorem states that the gradient of the complex expected return is the expectation of the product of:
1. The complex return \( G_t \) (a complex number).
2. The score function \( \nabla_\theta \log \pi_\theta(A_t \mid S_t) \) (a real vector).

**Key Point:** The proof is formally identical to the standard real-valued policy gradient proof. The only change is that \( G_t \in \mathbb{C} \) rather than \( G_t \in \mathbb{R} \). The identity holds because the probability measure is real.

---

## 11.4 Gradient of the Squared Modulus Objective

### 11.4.1 Derivation

**Corollary 11.6 (Gradient of Squared Modulus).** The gradient of the performance objective \( J(\theta) = |\eta(\theta)|^2 \) is:
\[
\nabla_\theta J(\theta) = 2 \operatorname{Re} \left( \overline{\eta(\theta)} \cdot \nabla_\theta \eta(\theta) \right) \in \mathbb{R}^n.
\]

*Proof.* By the product rule for real derivatives:
\[
\nabla_\theta |\eta|^2 = \nabla_\theta (\eta \bar{\eta}) = \bar{\eta} \nabla_\theta \eta + \eta \nabla_\theta \bar{\eta}.
\]

Since \( \nabla_\theta \bar{\eta} = \overline{\nabla_\theta \eta} \):
\[
\nabla_\theta |\eta|^2 = \bar{\eta} \nabla_\theta \eta + \eta \overline{\nabla_\theta \eta} = 2 \operatorname{Re}(\bar{\eta} \nabla_\theta \eta).
\]
\( \square \)

### 11.4.2 Substituting the Policy Gradient

Substituting Theorem 11.5:
\[
\nabla_\theta J(\theta) = 2 \operatorname{Re} \left( \overline{\eta(\theta)} \cdot \mathbb{E}^{\pi_\theta} \left[ \sum_{t=0}^\infty \lambda^t G_t \cdot \nabla_\theta \log \pi_\theta(A_t \mid S_t) \mid S_0 = s_0 \right] \right).
\]

**Interpretation:** The gradient is a real vector in \( \mathbb{R}^n \), as required for a descent step on \( \theta \in \mathbb{R}^n \). The complex structure enters through the inner product \( \bar{\eta} \cdot \nabla_\theta \eta \).

### 11.4.3 Automatic Step-Size Adaptation

**Remark 11.7 (Automatic Adaptation).** When \( \eta(\theta) \) is large (the agent is far from equilibrium), the gradient is amplified by \( |\eta| \). As the agent approaches equilibrium and \( \eta(\theta) \to 0 \), the gradient magnitude decreases naturally. This is an automatic step-size adaptation arising from the geometry of \( \mathbb{C} \).

---

## 11.5 The Complex Advantage and Variance Reduction

### 11.5.1 The Complex Baseline

**Proposition 11.8 (Complex Baseline).** For any \( \mathbb{C} \)-valued function \( b(s) \):
\[
\mathbb{E}^{\pi_\theta}[b(S_t) \cdot \nabla_\theta \log \pi_\theta(A_t \mid S_t)] = 0.
\]

*Proof.* By the score function identity, for any fixed \( s \):
\[
\mathbb{E}_{A \sim \pi_\theta(\cdot \mid s)}[\nabla_\theta \log \pi_\theta(A \mid s)] = \int \nabla_\theta \pi_\theta(a \mid s) \, da = \nabla_\theta \int \pi_\theta(a \mid s) \, da = \nabla_\theta 1 = 0.
\]
Multiplying by \( b(s) \) (which is \( S_t \)-measurable) and taking the full expectation gives the result. \( \square \)

**Implication:** The gradient \( \nabla_\theta \eta(\theta) \) is unchanged if \( G_t \) is replaced by \( G_t - b(S_t) \). We can choose \( b(s) = V^\pi(s) \) (the complex state-value function).

### 11.5.2 The Complex Advantage Function

**Definition 11.9 (Complex Advantage).** The complex advantage function is:
\[
A^\pi(s, a) = Q^\pi(s, a) - V^\pi(s) \in \mathbb{C}.
\]

**Decomposition:**
- \( \operatorname{Re}(A^\pi(s, a)) \): Excess real cost of action \( a \) at state \( s \) relative to the average cost of the current policy.
- \( \operatorname{Im}(A^\pi(s, a)) \): Excess epistemic debt of action \( a \) at state \( s \) relative to the average debt of the current policy.

### 11.5.3 The Gradient with Advantage

**Corollary 11.10 (Gradient with Advantage).** Using the complex advantage:
\[
\nabla_\theta \eta(\theta) = \mathbb{E}^{\pi_\theta} \left[ \sum_{t=0}^\infty \lambda^t A^\pi(S_t, A_t) \cdot \nabla_\theta \log \pi_\theta(A_t \mid S_t) \mid S_0 = s_0 \right].
\]

**Interpretation:** The complex advantage naturally encodes both cost and debt:
- Actions with large negative \( \operatorname{Re}(A^\pi) \) reduce cost below average.
- Actions with large negative \( \operatorname{Im}(A^\pi) \) reduce debt below average.
- The gradient automatically rewards both types of actions.

---

## 11.6 Wirtinger Calculus and Complex Gradients

### 11.6.1 Motivation

The objective \( J(\theta) = |\eta(\theta)|^2 \) is not holomorphic in \( \eta \). To compute gradients with respect to complex parameters (if we complexify the parameter space), we need Wirtinger calculus.

### 11.6.2 Wirtinger Derivatives

**Definition 11.11 (Wirtinger Derivatives).** For a complex function \( f(z, \bar{z}) \):
\[
\frac{\partial f}{\partial z} = \frac{1}{2} \left( \frac{\partial f}{\partial x} - i \frac{\partial f}{\partial y} \right), \quad
\frac{\partial f}{\partial \bar{z}} = \frac{1}{2} \left( \frac{\partial f}{\partial x} + i \frac{\partial f}{\partial y} \right).
\]

**Key Property:** A function is holomorphic if and only if \( \partial f / \partial \bar{z} = 0 \).

### 11.6.3 Gradient of the Squared Modulus (Wirtinger Form)

**Proposition 11.12 (Wirtinger Gradient).** For a holomorphic function \( \eta: \mathbb{C}^m \to \mathbb{C} \):
\[
\nabla_z |\eta(z)|^2 = 2 \overline{\eta(z)} \cdot \frac{\partial \eta}{\partial z}.
\]

*Proof.* Since \( \eta \) is holomorphic, \( \partial \eta / \partial \bar{z} = 0 \). The result follows from the Wirtinger chain rule. \( \square \)

**Interpretation:** The gradient points in the direction that decreases the modulus, i.e., toward the origin in \( \mathbb{C} \).

### 11.6.4 Application to Policy Gradients

In our setting, \( \theta \in \mathbb{R}^n \), so we use the real gradient:
\[
\nabla_\theta |\eta(\theta)|^2 = 2 \operatorname{Re} \left( \overline{\eta(\theta)} \cdot \nabla_\theta \eta(\theta) \right).
\]

This is the form we use in practice.

---

## 11.7 The Natural Gradient in the Complex Domain

### 11.7.1 Motivation

The Euclidean gradient \( \nabla_\theta J(\theta) \) is not invariant to reparameterization of the policy. The natural gradient (Amari, 1998) corrects for this by pre-multiplying by the inverse Fisher information matrix.

### 11.7.2 The Hermitian Fisher Information

**Definition 11.13 (Hermitian Fisher Information).** The Hermitian Fisher information matrix of the policy family \( \{\pi_\theta : \theta \in \mathbb{R}^n\} \) is:
\[
F(\theta) = \mathbb{E}^{\pi_\theta} \left[ \nabla_\theta \log \pi_\theta(A_t \mid S_t) \cdot \overline{\nabla_\theta \log \pi_\theta(A_t \mid S_t)}^\top \right] \in \mathbb{C}^{n \times n}.
\]

**Proposition 11.14 (Properties of \( F(\theta) \)).**
1. \( F(\theta) \) is Hermitian: \( F(\theta) = F(\theta)^\dagger \).
2. \( F(\theta) \) is positive semi-definite.
3. If the policy family is identifiable, \( F(\theta) \) is positive definite.

*Proof.*
1. \( (F)_{jk} = \mathbb{E}[(\nabla_\theta \log \pi_\theta)_j (\overline{\nabla_\theta \log \pi_\theta})_k] \), and \( (F)_{kj} = \overline{(F)_{jk}} \).
2. For any \( v \in \mathbb{C}^n \): \( v^\dagger F v = \mathbb{E}[|v^\dagger \nabla_\theta \log \pi_\theta|^2] \geq 0 \).
3. Strict positivity follows from identifiability. \( \square \)

### 11.7.3 The Complex Natural Gradient

**Definition 11.15 (Complex Natural Gradient).** The natural gradient of \( J(\theta) \) is:
\[
\widetilde{\nabla}_\theta J(\theta) = F(\theta)^{-1} \cdot \nabla_\theta J(\theta).
\]

**Interpretation:** The natural gradient is invariant to smooth reparameterization. It compensates for differences in scale between cost and debt by using the curvature encoded in \( F(\theta) \).

**Example:** If cost and debt have very different scales (e.g., \( c \sim O(1) \) but \( d \sim O(10^3) \) early in learning), the Euclidean gradient would be dominated by the imaginary component. The natural gradient compensates through \( F(\theta)^{-1} \).

### 11.7.4 The Natural Gradient Update

The natural gradient descent update is:
\[
\theta \leftarrow \theta - \alpha \cdot F(\theta)^{-1} \nabla_\theta J(\theta).
\]

In practice, we estimate \( F(\theta) \) online and add a regularizer \( \epsilon I \) to handle near-singularity:
\[
\theta \leftarrow \theta - \alpha \cdot (F(\theta) + \epsilon I)^{-1} \nabla_\theta J(\theta).
\]

---

## 11.8 Examples and Interpretations

### 11.8.1 Example 1: A Simple Bandit

Consider a bandit with two actions. The complex utilities are:
- Action 0: \( z_0 = 0.1 + i \cdot 0 \)
- Action 1: \( z_1 = 0.5 - i \cdot 1 \)

The policy is parameterized by \( \theta \in \mathbb{R} \):
\[
\pi_\theta(0) = \frac{1}{1 + e^{-\theta}}, \quad \pi_\theta(1) = \frac{e^{-\theta}}{1 + e^{-\theta}}.
\]

The complex expected return is:
\[
\eta(\theta) = z_0 \cdot \pi_\theta(0) + z_1 \cdot \pi_\theta(1).
\]

The gradient is:
\[
\nabla_\theta \eta(\theta) = (z_0 - z_1) \cdot \nabla_\theta \pi_\theta(0).
\]

The performance objective:
\[
J(\theta) = |\eta(\theta)|^2.
\]

**Interpretation:** The gradient pushes \( \theta \) toward the action with smaller modulus.

### 11.8.2 Example 2: Automatic Exploration

Consider a cMDP where:
- Action \( a_E \) (explore): \( z_E = 0.1 + i \cdot 1 \)
- Action \( a_X \) (exploit): \( z_X = 1 - i \cdot 1 \)

The moduli:
- \( |z_E| = \sqrt{0.01 + 1} \approx 1.005 \)
- \( |z_X| = \sqrt{1 + 1} \approx 1.414 \)

The policy gradient will favor \( a_E \) because it has smaller modulus. The imaginary component (debt reduction) makes exploration attractive.

**Interpretation:** The gradient naturally drives exploration without an explicit exploration bonus.

### 11.8.3 Example 3: The Phase and the Gradient

The phase \( \theta(s, a) = \arg Q^\pi(s, a) \) determines the direction of the gradient:
- If \( \theta > 0 \) (exploration), the gradient pushes toward actions with larger imaginary component.
- If \( \theta < 0 \) (exploitation), the gradient pushes toward actions with smaller imaginary component.

**Interpretation:** The phase encodes the exploration-exploitation balance, and the gradient automatically adjusts the policy accordingly.

---

## 11.9 Summary

This chapter has extended the complex RL framework to policy gradient methods:

1. **Parameterized Policies:** \( \pi_\theta(a \mid s) \), \( \theta \in \mathbb{R}^n \).

2. **Complex Expected Return:** \( \eta(\theta) = \mathbb{E}^{\pi_\theta}[G_0 \mid S_0 = s_0] \in \mathbb{C} \).

3. **Performance Objective:** \( J(\theta) = |\eta(\theta)|^2 \in \mathbb{R}_+ \).

4. **Complex Policy Gradient Theorem:**
   \[
   \nabla_\theta \eta(\theta) = \mathbb{E}^{\pi_\theta} \left[ \sum_{t=0}^\infty \lambda^t G_t \cdot \nabla_\theta \log \pi_\theta(A_t \mid S_t) \right].
   \]

5. **Gradient of Squared Modulus:**
   \[
   \nabla_\theta J(\theta) = 2 \operatorname{Re} \left( \overline{\eta(\theta)} \cdot \nabla_\theta \eta(\theta) \right).
   \]

6. **Complex Advantage:** \( A^\pi(s, a) = Q^\pi(s, a) - V^\pi(s) \in \mathbb{C} \).

7. **Natural Gradient:**
   \[
   \widetilde{\nabla}_\theta J(\theta) = F(\theta)^{-1} \nabla_\theta J(\theta),
   \]
   where \( F(\theta) \) is the Hermitian Fisher information.

8. **Automatic Exploration:** The imaginary component of the advantage drives exploration without an explicit bonus.

---

## Exercises

**Exercise 11.1 (Policy Gradient Setup).** Define the complex expected return \( \eta(\theta) \). What is the performance objective \( J(\theta) \)?

**Exercise 11.2 (Complex Policy Gradient Theorem).** Prove the Complex Policy Gradient Theorem. What is the role of the log-derivative identity?

**Exercise 11.3 (Gradient of Squared Modulus).** Derive \( \nabla_\theta J(\theta) = 2 \operatorname{Re}(\overline{\eta(\theta)} \cdot \nabla_\theta \eta(\theta)) \). What is the interpretation of this gradient?

**Exercise 11.4 (Complex Baseline).** Prove that \( \mathbb{E}[b(S_t) \cdot \nabla_\theta \log \pi_\theta(A_t \mid S_t)] = 0 \). What does this imply for variance reduction?

**Exercise 11.5 (Complex Advantage).** Define the complex advantage \( A^\pi(s, a) \). How does it encode cost and debt?

**Exercise 11.6 (Hermitian Fisher Information).** Show that \( F(\theta) \) is Hermitian and positive semi-definite. When is it positive definite?

**Exercise 11.7 (Natural Gradient).** Define the natural gradient \( \widetilde{\nabla}_\theta J(\theta) \). Why is it invariant to reparameterization?

**Exercise 11.8 (Automatic Exploration).** In the bandit example, why does the gradient favor the explore action? What is the role of the imaginary component?

---

## Further Reading

- Sutton, R. S., McAllester, D. A., Singh, S. P., & Mansour, Y. (2000). "Policy Gradient Methods for Reinforcement Learning with Function Approximation." *NeurIPS* 13, 1057-1063. — The standard policy gradient theorem.

- Williams, R. J. (1992). "Simple Statistical Gradient-Following Algorithms for Connectionist Reinforcement Learning." *Machine Learning*, 8(3-4):229-256. — REINFORCE.

- Amari, S. (1998). "Natural Gradient Works Efficiently in Learning." *Neural Computation*, 10(2):251-276. — The natural gradient.

- Wirtinger, W. (1927). "Zur formalen Theorie der Funktionen von mehr komplexen Veränderlichen." *Mathematische Annalen*, 97(1):357-375. — Wirtinger calculus.

