# Chapter 8: The Complex MDP (cMDP)

---

## 8.1 Introduction

We have now completed the geometric foundations of the book. In Part I (Chapters 2-7), we developed a comprehensive framework for understanding belief spaces equipped with:
- An energy quasi-metric \( d \) (real cost).
- A debt function \( \text{debt}(b_i, b_j) = \psi(b_j) - \psi(b_i) \) (informational burden).
- A complex quasi-metric \( Q = d + i \cdot \text{debt} \) (unified geometry).
- A \( \gamma \)-distance family \( d_\gamma \) (interpolating geometries).
- A hierarchy of equilibrium concepts (from strong to weak balance).

Now we transition from **geometry** to **dynamics**. In Part II, we instantiate the belief space as the state space of a Markov decision process, and we study how an agent learns to navigate this space while balancing cost and debt.

This chapter introduces the **Complex Markov Decision Process (cMDP)** —the foundational framework for complex-valued reinforcement learning. The cMDP extends the standard MDP by replacing the scalar reward with a complex utility \( z = c + id \). The agent's objective is to minimize the modulus of the expected cumulative complex utility.

The chapter is organized as follows:
- **Section 8.2:** Definition of the cMDP.
- **Section 8.3:** The complex return and performance criteria.
- **Section 8.4:** The two criteria: modulus of expectation vs. expectation of modulus.
- **Section 8.5:** The complex value functions.
- **Section 8.6:** The Bellman equations for cMDPs.
- **Section 8.7:** The modulus-greedy policy.
- **Section 8.8:** Examples and interpretations.
- **Section 8.9:** Relationship to the geometric framework.

---

## 8.2 Definition of the cMDP

### 8.2.1 The Standard MDP

We begin by recalling the standard Markov decision process.

**Definition 8.1 (Standard MDP).** A finite Markov decision process is a tuple \( (\mathcal{S}, \mathcal{A}, p, r, \lambda) \), where:
- \( \mathcal{S} \) is a finite set of states.
- \( \mathcal{A} \) is a finite set of actions.
- \( p(s' \mid s, a) \) is the transition probability from state \( s \) to state \( s' \) under action \( a \).
- \( r: \mathcal{S} \times \mathcal{A} \to \mathbb{R} \) is the reward function.
- \( \lambda \in [0, 1) \) is the discount factor.

The agent's objective is to maximize the expected discounted cumulative reward:
\[
J^\pi(s) = \mathbb{E}^\pi \left[ \sum_{t=0}^\infty \lambda^t r(S_t, A_t) \mid S_0 = s \right].
\]

### 8.2.2 The Complex MDP

**Definition 8.2 (Complex MDP).** A **complex Markov decision process (cMDP)** is a tuple
\[
\mathcal{M} = (\mathcal{S}, \mathcal{A}, p, z, \lambda),
\]
where:
- \( \mathcal{S} \) is a finite set of states.
- \( \mathcal{A} \) is a finite set of actions.
- \( p(s' \mid s, a) \) is the transition probability.
- \( z: \mathcal{S} \times \mathcal{A} \times \mathcal{S} \to \mathbb{C} \) is the **complex utility function**, decomposing as:
  \[
  z(s, a, s') = c(s, a, s') + i \cdot d(s, a, s'),
  \]
  where \( c \geq 0 \) is the real cost and \( d \in \mathbb{R} \) is the real information debt.
- \( \lambda \in [0, 1) \) is the discount factor.

**Interpretation:**
- **Real part \( c \):** The immediate cost of the transition. Always non-negative.
- **Imaginary part \( d \):** The immediate information debt of the transition. Can be positive (debt incurred), negative (debt paid down), or zero.

### 8.2.3 The Utility as a Vector

The complex utility can be viewed as a two-dimensional vector:
\[
z(s, a, s') = \begin{pmatrix} c(s, a, s') \\ d(s, a, s') \end{pmatrix} \in \mathbb{R}^2.
\]

The complex structure provides:
1. **Orthogonality:** Cost and debt are orthogonal components.
2. **Geometry:** The modulus \( |z| = \sqrt{c^2 + d^2} \) is the Euclidean norm.
3. **Phase:** \( \arg(z) = \arctan(d/c) \) is the exploration-exploitation ratio.

### 8.2.4 Boundedness Assumption

**Assumption 8.3 (Bounded Utility).** The complex utility is bounded:
\[
Z_{\max} = \max_{s, a, s'} |z(s, a, s')| < \infty.
\]

This ensures that the discounted cumulative utility converges.

---

## 8.3 The Complex Return and Performance

### 8.3.1 The Complex Return

**Definition 8.4 (Complex Return).** Under a deterministic policy \( \pi: \mathcal{S} \to \mathcal{A} \), the complex return from time \( t \) is:
\[
G_t^\pi = \sum_{k=0}^\infty \lambda^k \cdot z(S_{t+k}, A_{t+k}, S_{t+k+1}),
\]
where \( A_{t+k} = \pi(S_{t+k}) \).

**Convergence:** The return converges absolutely since:
\[
|G_t^\pi| \leq \sum_{k=0}^\infty \lambda^k Z_{\max} = \frac{Z_{\max}}{1 - \lambda} < \infty.
\]

### 8.3.2 The Complex Expected Return

**Definition 8.5 (Complex Expected Return).** The complex expected return under policy \( \pi \) from state \( s \) is:
\[
J^\pi(s) = \mathbb{E}^\pi[G_t^\pi \mid S_t = s] \in \mathbb{C}.
\]

This is the expected value of the complex return—a complex number whose real part is the expected cost and whose imaginary part is the expected debt.

### 8.3.3 The Performance Objective

**Definition 8.6 (Performance Objective).** The agent's performance under policy \( \pi \) from state \( s \) is:
\[
J_\gamma^\pi(s) = \sqrt{\Re(J^\pi(s))^2 + \gamma^2 \cdot \Im(J^\pi(s))^2},
\]
where \( \gamma \in [0, 1] \) is the \( \gamma \)-parameter from Chapter 6.

**Special Cases:**
- \( \gamma = 0 \): \( J_0^\pi(s) = \Re(J^\pi(s)) \) (cost-only objective).
- \( \gamma = 1 \): \( J_1^\pi(s) = |J^\pi(s)| \) (full modulus objective).

**Interpretation:** The agent minimizes \( J_\gamma^\pi(s) \). This is the dynamic version of the \( \gamma \)-distance from Chapter 6.

---

## 8.4 Two Criteria: Modulus of Expectation vs. Expectation of Modulus

### 8.4.1 The Distinction

There are two natural ways to define the performance of a complex-valued return:

**Criterion 1: Modulus of Expectation**
\[
J^\pi(s) = |\mathbb{E}^\pi[G_t^\pi \mid S_t = s]|.
\]

**Criterion 2: Expectation of Modulus**
\[
\tilde{J}^\pi(s) = \mathbb{E}^\pi[|G_t^\pi| \mid S_t = s].
\]

### 8.4.2 Why the Modulus of Expectation?

**Lemma 8.7 (Modulus of Expectation Inequality).** For any complex-valued random variable \( Z \) with \( \mathbb{E}[|Z|] < \infty \):
\[
|\mathbb{E}[Z]| \leq \mathbb{E}[|Z|].
\]

*Proof.* This is the triangle inequality for integrals:
\[
|\mathbb{E}[Z]| = \left| \int Z \, d\mathbb{P} \right| \leq \int |Z| \, d\mathbb{P} = \mathbb{E}[|Z|].
\]
\( \square \)

**Implication:** The modulus of expectation is a **lower bound** on the expectation of modulus:
\[
J^\pi(s) \leq \tilde{J}^\pi(s).
\]

### 8.4.3 Why We Focus on Modulus of Expectation

We focus on the modulus of expectation for three reasons:

1. **Tractability:** The modulus of expectation leads to a **linear** Bellman equation (Section 8.6). The expectation of modulus leads to a **nonlinear** distributional problem.

2. **Geometry:** The modulus of expectation is the natural geometric objective in the cost-debt plane. It corresponds to minimizing the distance to the origin in expectation space.

3. **Practicality:** The modulus of expectation is computable using standard RL techniques. The expectation of modulus requires full distributional information, which is significantly harder.

### 8.4.4 The Expected-Modulus Problem

**Remark 8.8 (Expected Modulus is Open).** The expected-modulus criterion \( \tilde{J}^\pi(s) = \mathbb{E}[|G_t^\pi| \mid S_t = s] \) is the subject of ongoing research. It requires a **distributional** treatment of the complex return and is currently an open problem. All formal results in this book pertain to the modulus-of-expectation criterion.

---

## 8.5 The Complex Value Functions

### 8.5.1 The Complex Action-Value Function

**Definition 8.9 (Complex Action-Value Function).** Under policy \( \pi \), the complex action-value function is:
\[
Q^\pi(s, a) = \mathbb{E}^\pi[G_t^\pi \mid S_t = s, A_t = a] \in \mathbb{C}.
\]

**Decomposition:** The complex action-value function decomposes into real and imaginary parts:
\[
Q^\pi(s, a) = Q_R^\pi(s, a) + i \cdot Q_I^\pi(s, a),
\]
where:
- \( Q_R^\pi(s, a) = \mathbb{E}^\pi[\sum_{k=0}^\infty \lambda^k c(S_{t+k}, A_{t+k}, S_{t+k+1}) \mid S_t = s, A_t = a] \).
- \( Q_I^\pi(s, a) = \mathbb{E}^\pi[\sum_{k=0}^\infty \lambda^k d(S_{t+k}, A_{t+k}, S_{t+k+1}) \mid S_t = s, A_t = a] \).

### 8.5.2 The Complex State-Value Function

**Definition 8.10 (Complex State-Value Function).** Under policy \( \pi \), the complex state-value function is:
\[
V^\pi(s) = \mathbb{E}^\pi[G_t^\pi \mid S_t = s] \in \mathbb{C}.
\]

**Relationship:** \( V^\pi(s) = Q^\pi(s, \pi(s)) \).

### 8.5.3 The Function Spaces

**Definition 8.11 (Action-Value Function Space).** The space of bounded complex action-value functions is:
\[
\mathcal{Q} = \mathcal{B}(\mathcal{S} \times \mathcal{A}, \mathbb{C}),
\]
with the supremum norm:
\[
\| Q \|_\infty = \max_{s, a} |Q(s, a)|.
\]

**Proposition 8.12 (\( \mathcal{Q} \) is a Banach Space).** \( (\mathcal{Q}, \|\cdot\|_\infty) \) is a Banach space over \( \mathbb{C} \).

*Proof.* The space of bounded complex-valued functions on a finite set is complete under the supremum norm. \( \square \)

---

## 8.6 The Bellman Equations for cMDPs

### 8.6.1 The Bellman Evaluation Equation

**Theorem 8.13 (Bellman Evaluation for cMDPs).** For any deterministic policy \( \pi \), the complex action-value function \( Q^\pi \) satisfies:
\[
Q^\pi(s, a) = \sum_{s' \in \mathcal{S}} p(s' \mid s, a) \left[ z(s, a, s') + \lambda \cdot Q^\pi(s', \pi(s')) \right].
\]

*Proof.* This follows from the definition of \( Q^\pi \) and the Markov property:
\[
Q^\pi(s, a) = \mathbb{E}^\pi[G_t^\pi \mid S_t = s, A_t = a]
\]
\[
= \mathbb{E}^\pi \left[ z(S_t, A_t, S_{t+1}) + \lambda G_{t+1}^\pi \mid S_t = s, A_t = a \right]
\]
\[
= \sum_{s'} p(s' \mid s, a) \left[ z(s, a, s') + \lambda \mathbb{E}^\pi[G_{t+1}^\pi \mid S_{t+1} = s'] \right]
\]
\[
= \sum_{s'} p(s' \mid s, a) \left[ z(s, a, s') + \lambda Q^\pi(s', \pi(s')) \right].
\]
\( \square \)

### 8.6.2 The Evaluation Operator

**Definition 8.14 (Evaluation Operator).** For a fixed policy \( \pi \), the evaluation operator \( T^\pi: \mathcal{Q} \to \mathcal{Q} \) is:
\[
(T^\pi Q)(s, a) = \sum_{s'} p(s' \mid s, a) \left[ z(s, a, s') + \lambda \cdot Q(s', \pi(s')) \right].
\]

**Theorem 8.15 (Evaluation Contraction).** \( T^\pi \) is a \( \lambda \)-contraction on \( (\mathcal{Q}, \|\cdot\|_\infty) \).

*Proof.* For any \( Q_1, Q_2 \in \mathcal{Q} \):
\[
|(T^\pi Q_1)(s, a) - (T^\pi Q_2)(s, a)| = \lambda \left| \sum_{s'} p(s' \mid s, a) [Q_1(s', \pi(s')) - Q_2(s', \pi(s'))] \right|
\]
\[
\leq \lambda \sum_{s'} p(s' \mid s, a) |Q_1(s', \pi(s')) - Q_2(s', \pi(s'))|
\]
\[
\leq \lambda \| Q_1 - Q_2 \|_\infty.
\]
Taking the maximum over \( (s, a) \):
\[
\| T^\pi Q_1 - T^\pi Q_2 \|_\infty \leq \lambda \| Q_1 - Q_2 \|_\infty.
\]
Since \( \lambda < 1 \), \( T^\pi \) is a contraction. By the Banach fixed-point theorem, it has a unique fixed point \( Q^\pi \). \( \square \)

### 8.6.3 Structural Decomposition

**Corollary 8.16 (Independent Bellman Equations).** The real and imaginary parts of \( Q^\pi \) satisfy independent Bellman equations:
\[
Q_R^\pi(s, a) = \sum_{s'} p(s' \mid s, a) \left[ c(s, a, s') + \lambda \cdot Q_R^\pi(s', \pi(s')) \right],
\]
\[
Q_I^\pi(s, a) = \sum_{s'} p(s' \mid s, a) \left[ d(s, a, s') + \lambda \cdot Q_I^\pi(s', \pi(s')) \right].
\]

*Proof.* This follows from the linearity of expectation and the fact that the Bellman equation is linear over \( \mathbb{C} \). \( \square \)

**Interpretation:** The real and imaginary parts are **decoupled** in evaluation. The agent can compute the expected cost and expected debt separately. However, they are **coupled** in action selection (Section 8.7).

---

## 8.7 The Modulus-Greedy Policy

### 8.7.1 Definition

**Definition 8.17 (Modulus-Greedy Policy).** Given a complex action-value function \( Q \in \mathcal{Q} \), the **modulus-greedy policy** is:
\[
\pi_Q(s) = \arg\min_{a \in \mathcal{A}} |Q(s, a)|,
\]
with ties broken by a fixed deterministic rule (e.g., smallest action index).

**Interpretation:** The agent selects the action that minimizes the modulus \( |Q(s, a)| = \sqrt{Q_R(s, a)^2 + Q_I(s, a)^2} \). This is the dynamic version of the geometric principle from Chapter 5: minimize the distance to the origin in the cost-debt plane.

### 8.7.2 The Coupling of Cost and Debt

**Remark 8.18 (Coupling in Action Selection).** Although the Bellman evaluation equations for \( Q_R \) and \( Q_I \) are decoupled, they are **coupled** in action selection through the modulus:
\[
\arg\min_a |Q(s, a)| = \arg\min_a \sqrt{Q_R(s, a)^2 + Q_I(s, a)^2}.
\]

**Interpretation:** The agent cannot separately minimize cost and debt. It must balance them, and the modulus provides the natural balance.

### 8.7.3 The \( \gamma \)-Greedy Policy

**Definition 8.19 (\( \gamma \)-Greedy Policy).** For \( \gamma \in [0, 1] \), the \( \gamma \)-greedy policy is:
\[
\pi_\gamma(s) = \arg\min_a \sqrt{Q_R(s, a)^2 + \gamma^2 \cdot Q_I(s, a)^2}.
\]

**Special Cases:**
- \( \gamma = 0 \): \( \pi_0(s) = \arg\min_a Q_R(s, a) \) (cost-minimizing, pure exploitation).
- \( \gamma = 1 \): \( \pi_1(s) = \arg\min_a |Q(s, a)| \) (full modulus, balanced).

**Interpretation:** The \( \gamma \)-greedy policy interpolates between cost-only and balanced decision-making.

---

## 8.8 Examples and Interpretations

### 8.8.1 Example 1: A Simple cMDP

Consider a cMDP with:
- Two states: \( \mathcal{S} = \{0, 1\} \).
- Two actions: \( \mathcal{A} = \{a_0, a_1\} \).
- Deterministic transitions: \( p(1 \mid 0, a_0) = 1 \), \( p(0 \mid 0, a_1) = 1 \), \( p(0 \mid 1, a_0) = 1 \), \( p(1 \mid 1, a_1) = 1 \).
- Utilities:
  - From 0: \( z(0, a_0, 1) = 1 + i \), \( z(0, a_1, 0) = 0 + i \cdot 0 \).
  - From 1: \( z(1, a_0, 0) = 1 - i \), \( z(1, a_1, 1) = 0 + i \cdot 0 \).

**Interpretation:** Action \( a_0 \) costs 1 unit of energy and changes the state. Action \( a_1 \) costs 0 and stays in the same state.

**Value Functions:** For \( \lambda = 0.9 \):
- \( Q(0, a_0) = 1 + i + \lambda Q(1, a_0) \).
- \( Q(1, a_0) = 1 - i + \lambda Q(0, a_0) \).

Solving:
\[
Q(0, a_0) = \frac{1 + i + \lambda(1 - i)}{1 - \lambda^2} = \frac{(1 + \lambda) + i(1 - \lambda)}{1 - \lambda^2}.
\]
For \( \lambda = 0.9 \): \( Q(0, a_0) = \frac{1.9 + i(0.1)}{0.19} = 10 + i(0.526) \).

**Modulus-Greedy Policy:** \( |Q(0, a_0)| = \sqrt{100 + 0.277} \approx 10.014 \), \( |Q(0, a_1)| = 0 \). The agent chooses \( a_1 \) (stay in state) because it has zero cost and zero debt.

### 8.8.2 Example 2: Exploration-Exploitation

Consider a cMDP where:
- Action \( a_E \) (explore): cost \( c = 0.1 \), debt \( d = 1 \) (information gain).
- Action \( a_X \) (exploit): cost \( c = 1 \), debt \( d = -1 \) (debt paydown).

The complex utilities are:
- \( z_E = 0.1 + i \cdot 1 \).
- \( z_X = 1 - i \cdot 1 \).

The moduli:
- \( |z_E| = \sqrt{0.01 + 1} \approx 1.005 \).
- \( |z_X| = \sqrt{1 + 1} \approx 1.414 \).

**Interpretation:** The explore action has a smaller modulus because the debt gain (information) outweighs the cost. The agent naturally chooses to explore.

### 8.8.3 Example 3: The Phase as Exploration-Exploitation Ratio

The phase of the complex utility:
- \( \arg(z_E) = \arctan(1/0.1) \approx 84.3^\circ \) (high exploration).
- \( \arg(z_X) = \arctan(-1/1) = -45^\circ \) (exploitation).

**Interpretation:** The phase gives the instantaneous exploration-exploitation ratio. The agent's phase evolves as it learns.

---

## 8.9 Relationship to the Geometric Framework

### 8.9.1 From Belief Space to MDP

The cMDP instantiates the geometric framework from Part I:

| Geometric Concept | cMDP Instantiation |
|-------------------|-------------------|
| Belief space \( \mathcal{B} \) | State space \( \mathcal{S} \) |
| Energy quasi-metric \( d \) | Discounted cost \( Q_R^\pi \) |
| Debt function \( \text{debt} \) | Discounted debt \( Q_I^\pi \) |
| Complex quasi-metric \( Q \) | Complex action-value \( Q^\pi = Q_R^\pi + i Q_I^\pi \) |
| \( \gamma \)-distance \( d_\gamma \) | \( J_\gamma^\pi = \sqrt{Q_R^2 + \gamma^2 Q_I^2} \) |
| Equilibrium \( E \) | Zero value \( |Q^\pi| = 0 \) |

### 8.9.2 The HST Equilibrium in cMDPs

The HST Equilibrium Axiom (Axiom 7.19) in the cMDP setting:

**Axiom 8.20 (HST Equilibrium Axiom for cMDPs).** The agent's complex value function converges to zero:
\[
\lim_{t \to \infty} Q_t^{\pi^*}(S_t, A_t) = 0 \quad \text{almost surely}.
\]

**Interpretation:** The agent reaches epistemic equilibrium when both the real cost and imaginary debt components of the value function vanish.

---

## 8.10 Summary

This chapter has introduced the Complex Markov Decision Process (cMDP):

1. **cMDP Definition:** \( \mathcal{M} = (\mathcal{S}, \mathcal{A}, p, z, \lambda) \), where \( z = c + id \) is a complex utility.

2. **Complex Return:** \( G_t^\pi = \sum_{k=0}^\infty \lambda^k z(S_{t+k}, A_{t+k}, S_{t+k+1}) \).

3. **Performance Criterion:** \( J_\gamma^\pi(s) = \sqrt{\Re(J^\pi(s))^2 + \gamma^2 \cdot \Im(J^\pi(s))^2} \).

4. **Two Criteria:**
   - Modulus of expectation: \( J^\pi(s) = |\mathbb{E}[G_t^\pi \mid S_t = s]| \) (tractable, focus of this book).
   - Expectation of modulus: \( \tilde{J}^\pi(s) = \mathbb{E}[|G_t^\pi| \mid S_t = s] \) (open problem).

5. **Complex Value Functions:**
   - \( Q^\pi(s, a) = Q_R^\pi(s, a) + i \cdot Q_I^\pi(s, a) \).
   - \( V^\pi(s) = Q^\pi(s, \pi(s)) \).

6. **Bellman Evaluation:** \( T^\pi \) is a \( \lambda \)-contraction with unique fixed point \( Q^\pi \).

7. **Structural Decomposition:** \( Q_R^\pi \) and \( Q_I^\pi \) satisfy independent Bellman equations.

8. **Modulus-Greedy Policy:** \( \pi_Q(s) = \arg\min_a |Q(s, a)| \). Cost and debt are coupled in action selection.

9. **\( \gamma \)-Greedy Policy:** \( \pi_\gamma(s) = \arg\min_a \sqrt{Q_R(s, a)^2 + \gamma^2 Q_I(s, a)^2} \).

10. **Relationship to Geometry:** The cMDP instantiates the geometric framework from Part I.

The cMDP provides the foundational framework for complex-valued reinforcement learning. In Chapter 9, we study the Bellman evaluation equation in detail and derive the telescoping identity for the imaginary \( Q \)-value.

---

## Exercises

**Exercise 8.1 (cMDP Definition).** Define a cMDP with two states and two actions. Specify the transition probabilities and the complex utility function.

**Exercise 8.2 (Complex Return).** For the cMDP from Exercise 8.1, compute the complex return for a given policy. What is the expected return?

**Exercise 8.3 (Modulus of Expectation vs. Expectation of Modulus).** Give an example of a complex-valued random variable \( Z \) where \( |\mathbb{E}[Z]| < \mathbb{E}[|Z|] \). What does this imply for the cMDP?

**Exercise 8.4 (Bellman Evaluation).** Prove that the Bellman evaluation operator \( T^\pi \) is a \( \lambda \)-contraction for the cMDP. What is the contraction constant?

**Exercise 8.5 (Structural Decomposition).** Show that the real and imaginary parts of \( Q^\pi \) satisfy independent Bellman equations. What does this imply about the separability of cost and debt?

**Exercise 8.6 (Modulus-Greedy Policy).** For the cMDP from Exercise 8.1, compute the modulus-greedy policy. How does it balance cost and debt?

**Exercise 8.7 (\( \gamma \)-Greedy Policy).** For the same cMDP, compute the \( \gamma \)-greedy policy for \( \gamma = 0, 0.5, 1 \). How does the policy change with \( \gamma \)?

**Exercise 8.8 (Phase Interpretation).** For the complex utility \( z = c + id \), what is the phase? How does the phase relate to exploration and exploitation?

---

## Further Reading

- Puterman, M. L. (2014). *Markov Decision Processes*. Wiley. — The standard textbook on MDPs.

- Sutton, R. S. & Barto, A. G. (2018). *Reinforcement Learning: An Introduction*, 2nd ed. MIT Press. — The standard RL textbook.

- Altman, E. (1999). *Constrained Markov Decision Processes*. Chapman & Hall/CRC. — MDPs with constraints and multiple objectives.

- Russo, D. & Van Roy, B. (2018). "Learning to Optimize with Information-Directed Sampling." *Operations Research*, 66(1):230-252. — Information-directed exploration.

