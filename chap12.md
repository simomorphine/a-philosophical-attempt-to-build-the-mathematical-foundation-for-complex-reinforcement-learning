# Chapter 12: The Complex Natural Actor-Critic (CNAC) Algorithm

---

## 12.1 Introduction

In Chapter 11, we derived the Complex Policy Gradient Theorem and established the foundation for policy-based methods in complex MDPs. We showed that the gradient of the squared modulus objective can be expressed using the complex advantage, and we introduced the Hermitian Fisher information for natural gradient updates.

Now we assemble these results into a concrete algorithm: the **Complex Natural Actor-Critic (CNAC)** . CNAC combines:
- A **critic** that learns the complex action-value function \( Q^\pi(s, a) \) using complex TD learning.
- An **actor** that updates a parameterized policy \( \pi_\theta \) using the natural gradient of \( |\eta(\theta)|^2 \).
- A **Fisher estimator** that maintains the Hermitian Fisher information matrix.

The key property of CNAC is that it automatically balances cost and debt without external parameters. The imaginary component of the complex advantage drives exploration; as the critic's imaginary \( Q \)-values decay to zero at epistemic equilibrium, the actor gradient reduces automatically to standard real-valued natural policy gradient.

This chapter is organized as follows:
- **Section 12.2:** The CNAC architecture.
- **Section 12.3:** The algorithm pseudocode.
- **Section 12.4:** The critic update.
- **Section 12.5:** The actor update.
- **Section 12.6:** The Fisher estimator.
- **Section 12.7:** Properties of CNAC.
- **Section 12.8:** Automatic exploration-exploitation.
- **Section 12.9:** Examples and interpretations.

---

## 12.2 The CNAC Architecture

### 12.2.1 Overview

CNAC consists of three main components:

1. **Critic:** A function approximator \( Q_\psi: \mathcal{S} \times \mathcal{A} \to \mathbb{C} \) trained to approximate the complex action-value function \( Q^\pi(s, a) \) under the current policy \( \pi_\theta \).

2. **Actor:** A parameterized stochastic policy \( \pi_\theta(a \mid s) \) updated by the natural gradient of \( |\eta(\theta)|^2 \).

3. **Fisher Estimator:** An online estimator of the Hermitian Fisher information matrix \( F(\theta) \).

### 12.2.2 The Critic

**Definition 12.1 (Complex Critic).** The critic is a function \( Q_\psi: \mathcal{S} \times \mathcal{A} \to \mathbb{C} \) with parameters \( \psi \). It approximates:
\[
Q_\psi(s, a) \approx Q^\pi(s, a) = \mathbb{E}^\pi[G_t \mid S_t = s, A_t = a].
\]

**Decomposition:** \( Q_\psi(s, a) = Q_{\psi,R}(s, a) + i \cdot Q_{\psi,I}(s, a) \).

**Training:** The critic is trained by complex temporal difference learning. Since \( T^\pi \) is a \( \lambda \)-contraction (Theorem 8.15), the critic converges with standard TD methods.

### 12.2.3 The Actor

**Definition 12.2 (Complex Actor).** The actor is a parameterized stochastic policy \( \pi_\theta(a \mid s) \) with parameters \( \theta \in \mathbb{R}^n \).

**Objective:** \( J(\theta) = |\eta(\theta)|^2 \), where \( \eta(\theta) = \mathbb{E}^{\pi_\theta}[G_0 \mid S_0 = s_0] \).

**Update:** The actor is updated by the natural gradient of \( J(\theta) \):
\[
\theta \leftarrow \theta - \alpha_\theta \cdot \widetilde{\nabla}_\theta J(\theta),
\]
where \( \widetilde{\nabla}_\theta J(\theta) = F(\theta)^{-1} \nabla_\theta J(\theta) \).

### 12.2.4 The Fisher Estimator

**Definition 12.3 (Fisher Estimator).** The Hermitian Fisher information matrix is:
\[
F(\theta) = \mathbb{E}^{\pi_\theta} \left[ \nabla_\theta \log \pi_\theta(A_t \mid S_t) \cdot \overline{\nabla_\theta \log \pi_\theta(A_t \mid S_t)}^\top \right].
\]

**Estimation:** \( F(\theta) \) is estimated online using the outer product of the score function:
\[
\hat{F} \leftarrow (1 - \beta) \hat{F} + \beta \sum_t \nabla_\theta \log \pi_\theta(a_t \mid s_t) \overline{\nabla_\theta \log \pi_\theta(a_t \mid s_t)}^\top.
\]

---

## 12.3 The Algorithm Pseudocode

### Algorithm 1: Complex Natural Actor-Critic (CNAC)

```
1: Initialize actor parameters θ, critic parameters ψ, Fisher estimate F̂ = I_n
2: Set step sizes αθ > 0, αψ > 0, Fisher learning rate β ∈ (0, 1), regularizer ε > 0
3:
4: for each episode do
5:     Sample trajectory τ = (s₀, a₀, z₀, s₁, a₁, z₁, ...) under πθ
6:     
7:     Compute complex returns:
8:     G_t ← Σ_{k=t}^∞ λ^{k-t} z_k for each t
9:     
10:    Compute complex TD targets:
11:    Y_t ← z_t + λ Qψ(s_{t+1}, πθ(s_{t+1}))
12:    
13:    Critic update (complex TD):
14:    ψ ← ψ - αψ Σ_t ∇ψ |Qψ(s_t, a_t) - Y_t|²
15:    
16:    Compute complex advantages:
17:    Â_t ← G_t - Qψ(s_t, a_t)
18:    
19:    Compute policy gradient:
20:    g ← 2 Re( η̄ · Σ_t λ^t Â_t · ∇θ log πθ(a_t | s_t) )
21:    where η̂ = Σ_t λ^t z_t is the episode return estimate
22:    
23:    Update Fisher estimate:
24:    F̂ ← (1 - β) F̂ + β Σ_t ∇θ log πθ(a_t | s_t) 
                                  · ∇θ log πθ(a_t | s_t)ᵀ
25:    
26:    Actor update (natural gradient descent):
27:    θ ← θ - αθ (F̂ + ε I)^{-1} g
28: end for
```

### 12.3.1 Explanation of Key Steps

**Line 8 (Complex Returns):** Compute the discounted sum of complex utilities from each time step.

**Line 11 (TD Targets):** The complex TD target uses the critic's estimate of the next state's value. Since the actor is stochastic, \( \pi_\theta(s_{t+1}) \) is the expected action under the current policy.

**Line 14 (Critic Update):** The critic minimizes the squared modulus of the TD error. This is a standard complex-valued regression problem.

**Line 17 (Complex Advantage):** The advantage is the difference between the return and the critic's estimate. It is a complex number encoding both cost and debt.

**Line 20 (Policy Gradient):** The gradient \( g \) is the natural gradient of \( J(\theta) \) (before the Fisher inverse). The term \( \bar{\eta} \) conjugates the current performance estimate.

**Line 24 (Fisher Update):** The Fisher estimate is updated using the outer product of the score function. The Hermitian property is maintained.

**Line 27 (Actor Update):** The actor is updated using the natural gradient. The regularizer \( \epsilon I \) ensures numerical stability.

---

## 12.4 The Critic Update

### 12.4.1 Complex TD Learning

**Definition 12.4 (Complex TD Error).** The complex TD error is:
\[
\delta_t = Y_t - Q_\psi(s_t, a_t) \in \mathbb{C},
\]
where \( Y_t = z_t + \lambda Q_\psi(s_{t+1}, \pi_\theta(s_{t+1})) \).

**Loss Function:** The critic minimizes the squared modulus of the TD error:
\[
\mathcal{L}_\text{critic}(\psi) = \mathbb{E} \left[ |\delta_t|^2 \right] = \mathbb{E} \left[ \delta_t \bar{\delta}_t \right].
\]

**Update:** Using Wirtinger calculus:
\[
\nabla_\psi |\delta_t|^2 = 2 \operatorname{Re} \left( \overline{\delta_t} \cdot \nabla_\psi \delta_t \right).
\]

**Interpretation:** The critic learns to predict both the real cost and the imaginary debt components of the return.

### 12.4.2 Convergence

**Proposition 12.5 (Critic Convergence).** Under standard TD learning conditions, the critic converges to \( Q^\pi \) in the limit of infinite samples.

*Proof.* Since \( T^\pi \) is a \( \lambda \)-contraction (Theorem 8.15), the complex TD learning algorithm converges to the unique fixed point \( Q^\pi \). \( \square \)

---

## 12.5 The Actor Update

### 12.5.1 The Policy Gradient

From Chapter 11, the gradient of the squared modulus objective is:
\[
g = \nabla_\theta J(\theta) = 2 \operatorname{Re} \left( \overline{\eta(\theta)} \cdot \nabla_\theta \eta(\theta) \right).
\]

Using the complex advantage:
\[
\nabla_\theta \eta(\theta) = \mathbb{E}^{\pi_\theta} \left[ \sum_{t=0}^\infty \lambda^t A^\pi(S_t, A_t) \cdot \nabla_\theta \log \pi_\theta(A_t \mid S_t) \mid S_0 = s_0 \right].
\]

### 12.5.2 The Natural Gradient

**Definition 12.6 (Natural Gradient Update).** The actor update is:
\[
\theta \leftarrow \theta - \alpha_\theta \cdot (F(\theta) + \epsilon I)^{-1} g,
\]
where:
- \( F(\theta) \) is the Hermitian Fisher information.
- \( \epsilon > 0 \) is a regularizer.
- \( \alpha_\theta > 0 \) is the actor step size.

### 12.5.3 The Role of the Complex Advantage

The complex advantage \( A^\pi(s, a) = Q^\pi(s, a) - V^\pi(s) \) has two components:
- \( \operatorname{Re}(A^\pi(s, a)) \): Excess cost. Negative means the action is cheaper than average.
- \( \operatorname{Im}(A^\pi(s, a)) \): Excess debt. Negative means the action reduces debt more than average.

**Key Insight:** Both components enter the gradient update through \( \bar{\eta} \cdot A^\pi \). The actor naturally balances cost and debt.

---

## 12.6 The Fisher Estimator

### 12.6.1 Online Estimation

**Definition 12.7 (Fisher Update).** The Fisher estimate is updated as:
\[
\hat{F}_{t+1} = (1 - \beta) \hat{F}_t + \beta \cdot \phi_t \phi_t^\dagger,
\]
where:
- \( \phi_t = \nabla_\theta \log \pi_\theta(a_t \mid s_t) \in \mathbb{R}^n \).
- \( \beta \in (0, 1) \) is the Fisher learning rate.
- \( \phi_t \phi_t^\dagger \) is the outer product (Hermitian).

**Properties:**
1. \( \hat{F}_t \) is Hermitian.
2. \( \hat{F}_t \) is positive semi-definite.
3. With regularizer \( \epsilon I \), \( \hat{F}_t + \epsilon I \) is positive definite.

### 12.6.2 Regularization

**Remark 12.8 (Regularization).** The regularizer \( \epsilon I \) ensures numerical stability when \( \hat{F}_t \) is near-singular. In practice, \( \epsilon \) is chosen small (e.g., \( \epsilon = 10^{-6} \)).

---

## 12.7 Properties of CNAC

### 12.7.1 Automatic Exploration-Exploitation

**Proposition 12.9 (Automatic Exploration).** Under Assumption 9.3 (the epistemic potential is a submartingale), the imaginary component of the complex advantage satisfies:
\[
\operatorname{Im}(A^\pi(s, a)) \in \mathbb{R}
\]
and encodes the deviation of action \( a \)'s epistemic debt from the policy average.

*Proof.* By the structural decomposition of \( Q^\pi \):
\[
\operatorname{Im}(A^\pi(s, a)) = Q_I^\pi(s, a) - V_I^\pi(s),
\]
which is the deviation of the action's discounted cumulative mutual information from the state average. \( \square \)

**Interpretation:** Actions that reduce debt more than average have negative \( \operatorname{Im}(A^\pi) \), which contributes negatively to the gradient. The actor is automatically driven toward informative actions.

### 12.7.2 Epistemic Equilibrium as a Fixed Point

**Proposition 12.10 (Equilibrium Fixed Point).** At epistemic equilibrium (Axiom 9.3), \( Q_I^\pi(s, a) \to 0 \) and therefore \( \operatorname{Im}(A^\pi(s, a)) \to 0 \) for all \( (s, a) \). At this point, the actor gradient reduces to:
\[
g = 2 \operatorname{Re} \left( \overline{\eta(\theta)} \cdot \nabla_\theta \eta(\theta) \right),
\]
which is the standard real-valued natural policy gradient applied to the cost component \( Q_R^\pi \) alone.

*Proof.* When \( Q_I^\pi = 0 \), the complex advantage is real-valued. The imaginary component vanishes, and the gradient reduces to the cost-only case. \( \square \)

**Interpretation:** The transition from complex to real gradient is automatic and requires no intervention.

### 12.7.3 No Exploration Schedule

**Remark 12.11 (Parameter-Free Exploration).** CNAC has no explicit exploration schedule. There is no \( \epsilon \) to decay, no entropy bonus to anneal, no \( \beta \) to tune. The imaginary component of the complex advantage is the exploration signal, and it decays to zero automatically as the critic's imaginary \( Q \)-values approach zero at equilibrium.

---

## 12.8 Automatic Exploration-Exploitation

### 12.8.1 The Exploration Signal

The exploration signal in CNAC is the imaginary component of the complex advantage:
\[
\operatorname{Im}(A^\pi(s, a)) = Q_I^\pi(s, a) - V_I^\pi(s).
\]

**Interpretation:**
- If \( \operatorname{Im}(A^\pi(s, a)) < 0 \), action \( a \) reduces debt more than average. The action is **informative**. The gradient drives the actor toward this action.
- If \( \operatorname{Im}(A^\pi(s, a)) > 0 \), action \( a \) increases debt more than average. The action is **uninformative**. The gradient drives the actor away.

### 12.8.2 The Phase Evolution

The phase of the complex action-value function:
\[
\theta(s, a) = \arg Q^\pi(s, a) = \arctan \left( \frac{Q_I^\pi(s, a)}{Q_R^\pi(s, a)} \right)
\]
is the exploration-exploitation angle.

**Evolution:**
1. **Early learning:** \( Q_I^\pi \) is large (high entropy, much to learn). The phase is large. The agent explores.
2. **Middle learning:** \( Q_I^\pi \) decreases as information is acquired. The phase decreases.
3. **Late learning:** \( Q_I^\pi \to 0 \). The phase approaches 0. The agent exploits.

**No manual decay is required.**

### 12.8.3 Comparison with Classical Methods

| Method | Exploration Mechanism | Tuning Required |
|--------|----------------------|-----------------|
| \( \epsilon \)-greedy | Random actions with prob \( \epsilon \) | \( \epsilon \) schedule |
| Entropy regularization | Entropy bonus with weight \( \beta \) | \( \beta \) |
| UCB | Confidence bound with parameter \( \beta \) | \( \beta \) |
| Information-directed | \( \text{reward} + \beta \cdot \text{info} \) | \( \beta \) |
| **CNAC** | **Imaginary advantage** | **None** |

---

## 12.9 Examples and Interpretations

### 12.9.1 Example 1: Bandit with CNAC

Consider a bandit with two actions:
- Action 0: \( z_0 = 0.1 + i \cdot 0 \)
- Action 1: \( z_1 = 0.5 - i \cdot 1 \)

**Initial:** The critic has high uncertainty, so \( Q_I^\pi \) is large. The phase is large. The actor explores.

**Learning:** The critic estimates \( Q_I^\pi \to 0 \). The imaginary advantage decreases. The actor shifts toward the action with smaller modulus.

**Equilibrium:** \( Q_I^\pi = 0 \). The actor is a pure cost minimizer.

### 12.9.2 Example 2: Grid World with Information

Consider a grid world where:
- Moving costs energy (\( c > 0 \)).
- Entering certain cells reveals information (\( d < 0 \), debt paydown).

**Exploration:** The actor is driven to informative cells because \( \operatorname{Im}(A^\pi) < 0 \) for those actions.

**Exploitation:** After the information is acquired, \( Q_I^\pi \to 0 \), and the actor focuses on cost minimization.

**Interpretation:** The agent explores to gather information, then exploits to minimize cost. The transition is automatic.

### 12.9.3 Example 3: The Phase Transition

**Remark 12.12 (Phase Transition).** The transition from exploration to exploitation in CNAC is a continuous phase transition. As \( Q_I^\pi \to 0 \), the system undergoes a continuous transition from a complex-valued (two-dimensional) regime to a real-valued (one-dimensional) regime. This is analogous to a second-order phase transition in statistical physics, where an order parameter (here, the imaginary \( Q \)-value) vanishes continuously.

---

## 12.10 Summary

This chapter has presented the Complex Natural Actor-Critic (CNAC) algorithm:

1. **Architecture:** Critic (complex TD learning), Actor (natural gradient), Fisher estimator (Hermitian).

2. **Algorithm:** Pseudocode for CNAC with complex returns, TD targets, advantage, and natural gradient.

3. **Critic Update:** Complex TD learning with \( \mathcal{L}_\text{critic} = \mathbb{E}[|\delta_t|^2] \). Converges because \( T^\pi \) is a \( \lambda \)-contraction.

4. **Actor Update:** Natural gradient of \( J(\theta) = |\eta(\theta)|^2 \): \( \theta \leftarrow \theta - \alpha_\theta (F + \epsilon I)^{-1} g \).

5. **Fisher Estimator:** Online Hermitian Fisher estimate: \( \hat{F} \leftarrow (1 - \beta) \hat{F} + \beta \phi_t \phi_t^\dagger \).

6. **Automatic Exploration:** \( \operatorname{Im}(A^\pi) \) drives exploration without external parameters.

7. **Equilibrium Fixed Point:** At epistemic equilibrium, \( Q_I^\pi = 0 \), and CNAC reduces to standard real-valued natural policy gradient.

8. **No Exploration Schedule:** The phase \( \arg Q^\pi \) evolves naturally from exploration to exploitation.

---

## Exercises

**Exercise 12.1 (CNAC Pseudocode).** Walk through the CNAC algorithm step by step. What is the role of each step?

**Exercise 12.2 (Complex TD).** Derive the critic update for complex TD learning. What is the loss function?

**Exercise 12.3 (Natural Gradient).** Why is the natural gradient update used instead of the Euclidean gradient? What is the role of the Hermitian Fisher information?

**Exercise 12.4 (Fisher Estimator).** Show that the Fisher estimate \( \hat{F} \) is Hermitian and positive semi-definite. Why is the regularizer \( \epsilon I \) needed?

**Exercise 12.5 (Automatic Exploration).** Explain how \( \operatorname{Im}(A^\pi) \) drives exploration. Why is no explicit exploration bonus needed?

**Exercise 12.6 (Equilibrium Fixed Point).** Show that at epistemic equilibrium, CNAC reduces to standard real-valued natural policy gradient.

**Exercise 12.7 (Phase Transition).** Describe the phase transition from exploration to exploitation in CNAC. How does it differ from classical methods?

---

## Further Reading

- Sutton, R. S. & Barto, A. G. (2018). *Reinforcement Learning: An Introduction*, 2nd ed. MIT Press. — Actor-critic methods.

- Amari, S. (1998). "Natural Gradient Works Efficiently in Learning." *Neural Computation*, 10(2):251-276. — The natural gradient.

- Peters, J. & Schaal, S. (2008). "Natural Actor-Critic." *Neurocomputing*, 71(7-9):1180-1190. — The natural actor-critic algorithm.

- Borkar, V. S. (2009). *Stochastic Approximation: A Dynamical Systems Viewpoint*. Cambridge University Press. — Convergence of stochastic approximation algorithms.
