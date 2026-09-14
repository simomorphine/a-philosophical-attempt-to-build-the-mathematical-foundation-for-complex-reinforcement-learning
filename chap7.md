# Chapter 7: Equilibrium in Belief Space

---

## 7.1 Introduction

In the preceding chapters, we have developed a rich geometric framework for belief spaces:
- The energy quasi-metric $d$ (Chapter 3) capturing real cost.
- The debt function (Chapter 4) capturing informational burden.
- The complex quasi-metric $Q = d + i \cdot \text{debt}$ (Chapter 5) unifying both.
- The $\gamma$-distance family $d_\gamma$ (Chapter 6) interpolating between cost-only and balanced geometries.

Now we turn to a central question: **What does it mean for an agent to be in equilibrium?** In classical physics, equilibrium is a state of balance where no net force acts. In information processing, equilibrium is a state where the agent has minimized its total "tension"—the combination of cost and debt.

This chapter formalizes equilibrium in belief space. We introduce four distinct equilibrium concepts, arranged in a hierarchy, and study their properties. The key insight is that the asymmetry of the $\gamma$-distance gives rise to multiple notions of balance, from strong (local symmetry) to weak (directional reachability).

The chapter is organized as follows:
- **Section 7.2:** Symmetrization of the $\gamma$-distance.
- **Section 7.3:** Four equilibrium concepts.
- **Section 7.4:** The equilibrium hierarchy.
- **Section 7.5:** Local asymmetry and symmetric points.
- **Section 7.6:** Fixed points and equilibrium.
- **Section 7.7:** Examples and interpretations.

---

## 7.2 Symmetrization of the $\gamma$-Distance

### 7.2.1 The Average Distance

The $\gamma$-distance $d_\gamma$ is asymmetric: $d_\gamma(b_i, b_j) \neq d_\gamma(b_j, b_i)$ in general. To obtain a symmetric distance, we can average the forward and backward costs.

**Definition 7.1 (Average Distance).** The **average distance** is
$$
d_{\text{avg}}(b, x) = \frac{d_\gamma(b, x) + d_\gamma(x, b)}{2}.
$$

**Proposition 7.2 ($d_{\text{avg}}$ is a Metric).** The average distance $d_{\text{avg}}$ is a genuine metric on $\mathcal{B}$:
1. **Identity:** $d_{\text{avg}}(b, b) = 0$.
2. **Symmetry:** $d_{\text{avg}}(b, x) = d_{\text{avg}}(x, b)$.
3. **Triangle inequality:** $d_{\text{avg}}(b, z) \leq d_{\text{avg}}(b, x) + d_{\text{avg}}(x, z)$.

*Proof.*
- Identity and symmetry are immediate.
- Triangle inequality follows from applying the triangle inequality of $d_\gamma$ to each direction separately:

$$
d_\gamma(b, z) + d_\gamma(z, b) \leq [d_\gamma(b, x) + d_\gamma(x, z)] + [d_\gamma(z, x) + d_\gamma(x, b)].
$$

Dividing by 2 gives the result. $\square$

**Interpretation:** The average distance measures the **round-trip cost**—going from $b$ to $x$ and back. It is a symmetric measure of "how far apart" two beliefs are in terms of the average cost of traversing between them.

### 7.2.2 The Max Distance

**Definition 7.3 (Max Distance).** The **max distance** is

$$
d_{\text{max}}(b, x) = \max \lbrace d_\gamma(b, x), d_\gamma(x, b) \rbrace.
$$

**Proposition 7.4 (Equivalence of $d_{\text{avg}}$ and $d_{\text{max}}$).** The average and max distances are equivalent and define the same topology:

$$
d_{\text{avg}} \leq d_{\text{max}} \leq 2 d_{\text{avg}}.
$$

*Proof.*

$$
d_{\text{avg}} = \frac{d_\gamma(b, x) + d_\gamma(x, b)}{2} \leq \max \lbrace d_\gamma(b, x), d_\gamma(x, b) \rbrace = d_{\text{max}}.
$$

Also,

$$
d_{\text{max}} = \max \lbrace d_\gamma(b, x), d_\gamma(x, b) \rbrace \leq d_\gamma(b, x) + d_\gamma(x, b) = 2 d_{\text{avg}}.
$$

$\square$

### 7.2.3 The Average Topology

**Definition 7.5 (Average Topology $\tau_{\text{avg}}$).** The **average topology** is the topology induced by the average distance $d_{\text{avg}}$. Its basis consists of balls

$$
B_{\text{avg}}(b, \epsilon) = \lbrace x \in \mathcal{B} \mid d_{\text{avg}}(b, x) < \epsilon \rbrace.
$$

**Interpretation:** The average topology is the "symmetric" topology on belief space. It ignores the directionality of cost and treats forward and backward costs symmetrically.

### 7.2.4 The Intersection Property

**Proposition 7.6 (Intersection Property).** For any $b \in \mathcal{B}$ and $\epsilon > 0$:

$$
B^+(b, \epsilon) \cap B^-(b, \epsilon) \subseteq B_{\text{avg}}(b, \epsilon).
$$

*Proof.* If $x \in B^+(b, \epsilon) \cap B^-(b, \epsilon)$, then $d_\gamma(b, x) < \epsilon $ and $ d_\gamma(x, b) < \epsilon$. Therefore:

$$
d_{\text{avg}}(b, x) = \frac{d_\gamma(b, x) + d_\gamma(x, b)}{2} < \frac{\epsilon + \epsilon}{2} = \epsilon.
$$

Thus, $x \in B_{\text{avg}}(b, \epsilon)$. $\square$

**Interpretation:** Beliefs that are bidirectionally close (reachable from $b$ and can reach $b$) are also close in the average topology. The converse is not necessarily true—beliefs close in average may be far in one direction.

---

## 7.3 Four Equilibrium Concepts

### 7.3.1 Pre-Equilibrium

**Definition 7.7 (Pre-Equilibrium Set).** For thresholds $\epsilon_b > 0$ for each $b \in \mathcal{B}$, the pre-equilibrium set is

$$
U_{\text{pre}} = \bigcup_{b \in \mathcal{B}} B_{\text{avg}}(b, \epsilon_b).
$$

**Interpretation:** The pre-equilibrium set consists of beliefs that are within some tolerance of being in equilibrium. The thresholds $\epsilon_b$ can vary with $b$, allowing for state-dependent tolerances.

### 7.3.2 Equilibrium Concepts

Taking the closure of $U_{\text{pre}}$ in different topologies yields four distinct equilibrium concepts:

**Definition 7.8 (Equilibrium Concepts).**

1. **Average Equilibrium ($E_{\text{avg}}$):**

   $$
   E_{\text{avg}} = \overline{U_{\text{pre}}}^{\tau_{\text{avg}}}.
   $$

   The closure of the pre-equilibrium set in the average topology.

3. **Join Equilibrium ($E_\vee$):**

   $$
   E_\vee = \overline{U_{\text{pre}}}^{\tau_+ \vee \tau_-}.
   $$

   The closure of the pre-equilibrium set in the join topology.

5. **Forward Equilibrium ($E_\rightarrow$):**

   $$
   E_\rightarrow = \overline{U_{\text{pre}}}^{\tau_+}.
   $$

   The closure of the pre-equilibrium set in the forward topology.

7. **Backward Equilibrium ($E_\leftarrow$):**

   $$
   E_\leftarrow = \overline{U_{\text{pre}}}^{\tau_-}.
   $$

   The closure of the pre-equilibrium set in the backward topology.

### 7.3.3 Interpretation of Each Equilibrium

| Concept | Topology | Interpretation |
|---------|----------|----------------|
| $E_{\text{avg}}$ | Average | Symmetric balance: round-trip cost is small. |
| $E_\vee$ | Join | Bidirectional balance: can reach and be reached with low cost. |
| $E_\rightarrow$ | Forward | Directional balance: can be reached with low cost. |
| $E_\leftarrow$ | Backward | Directional balance: can reach others with low cost. |

**Key Insight:** The four equilibrium concepts capture different degrees of balance, from the strongest ($E_{\text{avg}}$) to the weakest ($E_\rightarrow$ and $E_\leftarrow$).

---

## 7.4 The Equilibrium Hierarchy

### 7.4.1 The Main Theorem

**Theorem 7.9 (Equilibrium Hierarchy).** The four equilibrium concepts are nested:

$$
E_{\text{avg}} \subseteq E_\vee \subseteq E_\rightarrow \cap E_\leftarrow.
$$

*Proof.*
1. **$E_{\text{avg}} \subseteq E_\vee$:** Since $\tau_{\text{avg}} \subseteq \tau_+ \vee \tau_-$, a set closed in the join topology is also closed in the average topology. Therefore, the closure in $\tau_{\text{avg}}$ is contained in the closure in $\tau_+ \vee \tau_-$.

2. **$E_\vee \subseteq E_\rightarrow \cap E_\leftarrow$:** Since $\tau_+ \vee \tau_-$ is finer than both $\tau_+$ and $\tau_-$ individually, a set closed in the join topology is closed in both $\tau_+$ and $\tau_-$. Therefore, the closure in $\tau_+ \vee \tau_-$ is contained in the intersection of the closures in $\tau_+$ and $\tau_-$. $\square$

### 7.4.2 The Hierarchy Diagram

```
                E_avg
                  |
                  ↓
                 E_∨
                  |
        ┌─────────┴─────────┐
        ↓                   ↓
      E_→                 E_←
        └─────────┬─────────┘
                  ↓
            E_→ ∩ E_←
```

**Interpretation:**
- **Top:** $ E_{\text{avg}} $ is the strongest equilibrium—it requires full local symmetry.
- **Middle:** $ E_\vee $ requires bidirectional reachability.
- **Bottom:** $ E_\rightarrow $ and $ E_\leftarrow $ require only directional reachability.
- **Intersection:** The intersection $ E_\rightarrow \cap E_\leftarrow $ is weaker than $ E_\vee $ (since the join topology is finer than the individual topologies).

### 7.4.3 When Are the Concepts Equal?

**Proposition 7.10 (Equality Conditions).** The equilibrium concepts coincide under the following conditions:
- **$ E_{\text{avg}} = E_\vee $:** When the average topology equals the join topology, i.e., when $ d_\gamma $ is symmetric.
- **$ E_\vee = E_\rightarrow \cap E_\leftarrow $:** When the join topology equals the intersection of the individual topologies, i.e., when forward and backward closures commute.
- **All equal:** When $ d_\gamma $ is symmetric and the topologies are well-behaved.

**Interpretation:** In general, the hierarchy is strict. The differences between the equilibrium concepts capture the asymmetry of the $ \gamma $-distance.

---

## 7.5 Local Asymmetry and Symmetric Points

### 7.5.1 Locally Symmetric Points

**Definition 7.11 (Locally Symmetric Point).** A belief $ b \in \mathcal{B} $ is a **locally symmetric point** of $ d_\gamma $ if
$$
\liminf_{x \to^{\tau_{\text{avg}}} b} \frac{|d_\gamma(b, x) - d_\gamma(x, b)|}{d_{\text{avg}}(b, x)} = 0.
$$

The set of all locally symmetric points is denoted $ \text{Sym}_\gamma(\mathcal{B}) $.

**Interpretation:** At a locally symmetric point, the asymmetry of the $ \gamma $-distance vanishes to first order. The forward and backward costs are locally balanced.

**Remark 7.12.** The limit uses $ \tau_{\text{avg}} $—the natural choice since $ d_{\text{avg}} $ appears in the denominator. Using $ \tau_+ $ or $ \tau_- $ would introduce an asymmetric dependence that obscures the meaning. Note that $ d_{\text{avg}}(b, x) > 0 $ for $ x \neq b $, so the ratio is well-defined away from $ b $.

### 7.5.2 Relationship to Equilibrium

**Proposition 7.13 (Symmetric Points and Equilibrium).** Every locally symmetric point belongs to $ E_{\text{avg}} $.

*Proof.* If $ b \in \text{Sym}_\gamma(\mathcal{B}) $, then for any $ \epsilon > 0 $, there exists a neighborhood $ U $ of $ b $ such that $ |d_\gamma(b, x) - d_\gamma(x, b)| / d_{\text{avg}}(b, x) < \epsilon $ for all $ x \in U $. This implies that the asymmetry is small, so the forward and backward balls approximately coincide. Thus, $ b $ is in the closure of the pre-equilibrium set in $ \tau_{\text{avg}} $. $ \square $

**Interpretation:** Locally symmetric points are in equilibrium in the strongest sense ($ E_{\text{avg}} $).

### 7.5.3 The Geometry of Symmetric Points

**Proposition 7.14 (Characterization of Symmetric Points).** For a differentiable $ \gamma $-distance, $ b \in \text{Sym}_\gamma(\mathcal{B}) $ if and only if
$$
\nabla d_\gamma(b, \cdot) = -\nabla d_\gamma(\cdot, b) \quad \text{at } b.
$$

*Proof.* The gradient of the asymmetry vanishes at $ b $ if and only if the directional derivatives of $ d_\gamma(b, \cdot) $ and $ d_\gamma(\cdot, b) $ are negatives of each other. This is equivalent to the local symmetry condition. $ \square $

**Interpretation:** At a symmetric point, the gradient of the forward distance is the negative of the gradient of the backward distance. The cost landscape is locally symmetric.

---

## 7.6 Fixed Points and Equilibrium

### 7.6.1 Fixed Points of the Complex Quasi-Metric

**Definition 7.15 (Fixed Point).** A belief $ b^* \in \mathcal{B} $ is a **fixed point** if
$$
Q(b^*, b^*) = 0.
$$

Trivially, every $ b $ satisfies this. The non-trivial condition is that $ b^* $ is a fixed point of the dynamics: the agent's trajectory converges to $ b^* $.

### 7.6.2 Fixed Points and Equilibrium

**Proposition 7.16 (Fixed Points in Equilibrium).** Every fixed point $ b^* $ (where $ Q(b^*, b^*) = 0 $) belongs to all four equilibrium subspaces.

*Proof.* At $ b^* $, $ d_\gamma(b^*, b^*) = 0 $ and $ \text{debt}(b^*, b^*) = 0 $. Therefore, $ b^* $ is in the closure of the pre-equilibrium set in all topologies. $ \square $

**Interpretation:** Fixed points are the "true" equilibrium points—they have zero cost and zero debt.

### 7.6.3 The Fixed Point Set

**Definition 7.17 (Fixed Point Set).** The fixed point set is
$$
\mathcal{F} = \{ b \in \mathcal{B} \mid Q(b, b) = 0 \}.
$$

Trivially, $ \mathcal{F} = \mathcal{B} $. However, the **stable fixed points**—those to which the dynamics converges—are a subset of $ \mathcal{B} $.

**Interpretation:** The dynamics of the agent (which we will study in Part II) determines which fixed points are stable. The equilibrium hierarchy characterizes the stability of different fixed points.

---

## 7.7 Examples and Interpretations

### 7.7.1 Example 1: Two-State Belief Space

Let $ \mathcal{B} = \{0, 1\} $ with:
- $ d(0, 1) = E_{\text{obs}} $, $ d(1, 0) = E_{\text{erase}} $.
- $ \text{debt}(0, 1) = 1 $, $ \text{debt}(1, 0) = -1 $.

**Average Distance:**
$$
d_{\text{avg}}(0, 1) = \frac{\sqrt{E_{\text{obs}}^2 + \gamma^2} + \sqrt{E_{\text{erase}}^2 + \gamma^2}}{2}.
$$

**Equilibrium Concepts:**
- $ E_{\text{avg}} $: Beliefs where $ d_{\text{avg}} $ is small.
- $ E_\vee $: Beliefs where both forward and backward distances are small.
- $ E_\rightarrow $: Beliefs where the forward distance is small.
- $ E_\leftarrow $: Beliefs where the backward distance is small.

**Interpretation:** If $ E_{\text{obs}} \ll E_{\text{erase}} $, then $ 0 $ is in $ E_\rightarrow $ but not in $ E_\leftarrow $. The agent can reach 1 from 0 cheaply, but cannot return cheaply. The asymmetry creates directional equilibrium.

### 7.7.2 Example 2: Continuous Belief Space

Let $ \mathcal{B} = \mathbb{R} $ with:
- $ d(x, y) = \alpha |y - x| $.
- $ \text{debt}(x, y) = \log(p(x)/p(y)) $.

**Symmetric Points:** The locally symmetric points are where $ \log(p(x)/p(y)) \approx 0 $ for $ y $ close to $ x $, i.e., where $ p $ is locally constant.

**Equilibrium:** The equilibrium set is the set of beliefs where the distribution is locally uniform (no gradient in probability space). At these points, the agent has no incentive to move—it is in epistemic equilibrium.

### 7.7.3 Example 3: The Learning Trajectory

Consider the learning trajectory from previous chapters:
- Start: $ (c, d) = (0, \text{high}) $.
- End: $ (c, d) = (\text{cost}, 0) $.

**Equilibrium:** The end point $ (\text{cost}, 0) $ is in $ E_{\text{avg}} $ if the cost is small and symmetric. It is in $ E_\rightarrow $ if the cost to reach other beliefs is small. It is in $ E_\leftarrow $ if other beliefs can reach it cheaply.

**Interpretation:** The agent reaches equilibrium when it has minimized both cost and debt. The equilibrium hierarchy captures different degrees of balance.

### 7.7.4 Example 4: The Pareto Frontier

Consider the Pareto frontier in the cost-debt plane: $ d^2 + \delta^2 = 1 $.

**Equilibrium Points:** The point $ (0, 0) $ is the ideal equilibrium (zero cost, zero debt). However, if the frontier does not include $ (0, 0) $, the closest point on the frontier is $ (1/\sqrt{2}, 1/\sqrt{2}) $.

**Equilibrium Concepts:**
- $ E_{\text{avg}} $: Points where the average distance to the frontier is small.
- $ E_\vee $: Points where both forward and backward distances to the frontier are small.
- $ E_\rightarrow $: Points where the forward distance to the frontier is small.
- $ E_\leftarrow $: Points where the backward distance to the frontier is small.

**Interpretation:** The equilibrium hierarchy captures different ways of being "close" to the ideal point.

---

## 7.8 Relationship to Reinforcement Learning

### 7.8.1 Equilibrium in Value Space

In reinforcement learning (Part II), the belief space $ \mathcal{B} $ becomes the state space $ \mathcal{S} $. The equilibrium concepts translate to:

- **$ E_{\text{avg}} $:** States where the average cost-debt is small.
- **$ E_\vee $:** States where both forward and backward costs are small.
- **$ E_\rightarrow $:** States where the forward cost is small.
- **$ E_\leftarrow $:** States where the backward cost is small.

### 7.8.2 Equilibrium and Optimality

**Conjecture 7.18 (Equilibrium and Optimality).** In the limit of infinite learning, the agent's value function satisfies:
$$
|Q^*(s, a)| = 0 \quad \text{for all } (s, a) \in E_{\text{avg}}.
$$

**Interpretation:** The optimal value function is zero at equilibrium. The agent has achieved both zero cost and zero debt.

### 7.8.3 The HST Equilibrium Axiom

**Axiom 7.19 (HST Equilibrium Axiom).** Every Information Processing System evolves toward epistemic equilibrium. Formally:
$$
\lim_{t \to \infty} Q_t^{\pi^*}(S_t, A_t) = 0 \quad \text{almost surely}.
$$

**Interpretation:** The HST Equilibrium Axiom asserts that the agent's trajectory converges to the equilibrium set. The equilibrium hierarchy characterizes the stability and reachability of different equilibrium points.

---

## 7.9 Summary

This chapter has formalized equilibrium in belief space:

1. **Symmetrization:**
   - Average distance: $ d_{\text{avg}}(b, x) = (d_\gamma(b, x) + d_\gamma(x, b))/2 $.
   - Max distance: $ d_{\text{max}}(b, x) = \max\{d_\gamma(b, x), d_\gamma(x, b)\} $.
   - $ d_{\text{avg}} $ and $ d_{\text{max}} $ are equivalent and define the same topology.

2. **Four Equilibrium Concepts:**
   - $ E_{\text{avg}} $: Closure in the average topology (strongest).
   - $ E_\vee $: Closure in the join topology.
   - $ E_\rightarrow $: Closure in the forward topology.
   - $ E_\leftarrow $: Closure in the backward topology.

3. **Equilibrium Hierarchy:**
   $$
   E_{\text{avg}} \subseteq E_\vee \subseteq E_\rightarrow \cap E_\leftarrow.
   $$

4. **Locally Symmetric Points ($ \text{Sym}_\gamma $):**
   - Points where the asymmetry vanishes to first order.
   - $ \text{Sym}_\gamma \subseteq E_{\text{avg}} $.

5. **Fixed Points:**
   - Points where $ Q(b, b) = 0 $.
   - Every fixed point belongs to all four equilibrium subspaces.

6. **Interpretation:**
   - $ E_{\text{avg}} $: Strong balance (round-trip cost small).
   - $ E_\vee $: Bidirectional balance (can reach and be reached).
   - $ E_\rightarrow $ and $ E_\leftarrow $: Directional balance.

7. **RL Connection:** Equilibrium in belief space corresponds to zero value in RL. The HST Equilibrium Axiom asserts convergence to equilibrium.

The equilibrium hierarchy provides a nuanced understanding of what it means for an agent to be "in balance." The strongest concept ($ E_{\text{avg}} $) requires full local symmetry; the weakest concepts ($ E_\rightarrow $ and $ E_\leftarrow $) require only directional balance.

---

## Exercises

**Exercise 7.1 (Average Distance).** Prove that $ d_{\text{avg}} $ satisfies the triangle inequality. Use the triangle inequality of $ d_\gamma $.

**Exercise 7.2 (Equivalence of $ d_{\text{avg}} $ and $ d_{\text{max}} $).** Show that $ d_{\text{avg}} $ and $ d_{\text{max}} $ are equivalent distances. What is the constant of equivalence?

**Exercise 7.3 (Intersection Property).** Prove that $ B^+(b, \epsilon) \cap B^-(b, \epsilon) \subseteq B_{\text{avg}}(b, \epsilon) $. Is the reverse inclusion true? If not, provide a counterexample.

**Exercise 7.4 (Equilibrium Hierarchy).** Prove that $ E_{\text{avg}} \subseteq E_\vee \subseteq E_\rightarrow \cap E_\leftarrow $. When are the inclusions strict?

**Exercise 7.5 (Locally Symmetric Points).** For the continuous belief space $ \mathcal{B} = \mathbb{R} $ with $ d(x, y) = |y - x| $, find the locally symmetric points. What is $ \text{Sym}_\gamma(\mathcal{B}) $?

**Exercise 7.6 (Fixed Points).** Prove that every fixed point belongs to all four equilibrium subspaces. Is the converse true? If not, provide a counterexample.

**Exercise 7.7 (Equilibrium in RL).** Suppose the agent's value function satisfies $ |Q(s, a)| = 0 $ for all $ (s, a) \in E_{\text{avg}} $. What does this imply about the agent's policy?

---

## Further Reading

- Fletcher, P. & Lindgren, W. F. (1982). *Quasi-Uniform Spaces*. Marcel Dekker. — A comprehensive treatment of quasi-metrics and bitopological spaces.

- Kelly, J. C. (1963). "Bitopological Spaces." *Proceedings of the London Mathematical Society*, 3(1):71-89. — The foundational paper on bitopological spaces.

- Kopperman, R. (1995). "Asymmetry and Duality in Topology." *Topology and its Applications*, 66(1):1-39. — A modern perspective on bitopological spaces.

- Amari, S. (2016). *Information Geometry and Its Applications*. Springer. — The geometry of probability spaces and equilibrium.

---

*Chapter 7 has introduced equilibrium in belief space. Part II of the book (Chapters 8-12) translates this geometric framework into reinforcement learning algorithms.*

<br>

---
---

# Part II — Supplementary Paper

*The material below is a standalone paper covering related ground (the equilibrium hierarchy, locally symmetric points) in more technical depth, then extends it with new results — smoothness/non-smoothness conditions, non-smooth counterexamples, physical interpretation, and implications for AGI architecture design. It was included in the uploaded file after Chapter 7; it is kept here as a separate document rather than merged into Chapter 7's numbering, since it has its own self-contained section numbers (1–11).*

---
---

# The Geometry of Local Symmetry and Equilibrium in Information Processing Systems

## A Mathematical Investigation of Reversibility, Stability, and Time's Arrow in Belief Space

**Mohamed Elwardi**

Humble Systems Theory

---

## Abstract

This paper establishes the fundamental relationship between local symmetry and equilibrium in information processing systems. We introduce the locally symmetric set $\mathrm{Sym}_\gamma(\mathcal{B})$ and prove its inclusion in the equilibrium hierarchy $E_{\mathrm{avg}} \subseteq E_\vee \subseteq E_\rightarrow \cap E_\leftarrow$. We demonstrate that under differentiability conditions, $\mathrm{Sym}_\gamma(\mathcal{B}) = E_{\mathrm{avg}}$—equilibrium is equivalent to local reversibility. Conversely, we construct non-smooth counterexamples where $\mathrm{Sym}_\gamma(\mathcal{B}) \subsetneq E_{\mathrm{avg}}$, revealing that stability can coexist with an irreversible arrow of time. This framework provides a geometric foundation for understanding memory, dissipation, and the emergence of time asymmetry in complex information processing systems, with direct implications for AGI architecture design.

**Keywords:** Belief space, quasi-metric, equilibrium, local symmetry, reversibility, information geometry, AGI, thermodynamics of computation

**MSC Classifications:** 54E35 (Quasi-metrics), 54E55 (Bitopological spaces), 37C10 (Dynamics), 53B20 (Riemannian geometry)

---

## 1. Introduction

### 1.1 Motivation

The relationship between reversibility and equilibrium lies at the heart of thermodynamics, information theory, and the study of complex systems. In classical thermodynamics, equilibrium is characterized by the absence of macroscopic flows and local reversibility—the system appears the same forward and backward in time. However, information processing systems present a more subtle picture: a stable memory state is in equilibrium (it persists), yet it carries information about its past, encoding an arrow of time.

This paper investigates the mathematical structure underlying this apparent paradox. We develop a framework where:

1. **Reversibility** is a local geometric property ($\mathrm{Sym}_\gamma$)
2. **Equilibrium** is a stability property ($E_{\mathrm{avg}}$)
3. **The gap between them** encodes the system's memory of time's arrow

### 1.2 The Belief Space Framework

We work within the belief space $\mathcal{B}$ of an information processing system, equipped with:

- An **energy quasi-metric** $d: \mathcal{B} \times \mathcal{B} \to \mathbb{R}_{\geq 0}$ measuring irreversible computational cost
- A **debt function** $\operatorname{debt}(b_i, b_j) = \psi(b_j) - \psi(b_i)$ measuring reversible informational obligations
- A **complex quasi-metric** $Q(b_i, b_j) = d(b_i, b_j) + i \cdot \operatorname{debt}(b_i, b_j)$
- A **one-parameter family** $d_\gamma(b_i, b_j) = \sqrt{d(b_i, b_j)^2 + \gamma^2 \cdot \operatorname{debt}(b_i, b_j)^2}$ for $\gamma \in [0,1]$

### 1.3 Main Results

This paper establishes:

1. **Theorem 1 (Symmetry Implies Equilibrium):** $\mathrm{Sym}_\gamma(\mathcal{B}) \subseteq E_{\mathrm{avg}}$

2. **Theorem 2 (Smoothness Implies Equality):** If $d$ and $\psi$ are $C^1$, then $\mathrm{Sym}_\gamma(\mathcal{B}) = E_{\mathrm{avg}}$

3. **Theorem 3 (Non-Smooth Gap):** There exist belief spaces with non-differentiable cost functions such that $\mathrm{Sym}_\gamma(\mathcal{B}) \subsetneq E_{\mathrm{avg}}$

4. **Theorem 4 (Extended Hierarchy):** $\mathrm{Sym}_\gamma(\mathcal{B}) \subseteq E_{\mathrm{avg}} \subseteq E_\vee \subseteq E_\rightarrow \cap E_\leftarrow$

### 1.4 Outline

Section 2 reviews the belief space formalism. Section 3 introduces the locally symmetric set. Section 4 establishes the inclusion theorems. Section 5 proves smoothness implies equality. Section 6 constructs non-smooth counterexamples. Section 7 presents the extended hierarchy. Section 8 explores physical interpretation. Section 9 discusses implications for AGI. Section 10 concludes with open questions.

---

## 2. The Belief Space Formalism

### 2.1 Basic Definitions

**Definition 2.1 (Belief Space).** Let $\mathcal{B}$ denote the belief space of an information processing system, consisting of all configurations reachable with bounded energy cost. Each belief $b \in \mathcal{B}$ represents a complete specification of the system's configuration.

**Definition 2.2 (Energy Quasi-Metric).** An energy quasi-metric is a function $d: \mathcal{B} \times \mathcal{B} \to \mathbb{R}_{\geq 0}$ satisfying:

1. $d(b, b) = 0$ for all $b \in \mathcal{B}$
2. $d(b, x) \geq 0$ for all $b, x \in \mathcal{B}$
3. $d(b, z) \leq d(b, x) + d(x, z)$ for all $b, x, z \in \mathcal{B}$ (triangle inequality)

Symmetry is not required: $d(b, x)$ may differ from $d(x, b)$, reflecting the potential irreversibility of computational processes.

**Definition 2.3 (Debt Function).** Let $\psi: \mathcal{B} \to \mathbb{R}$ be a potential function. The debt function is:

$$\operatorname{debt}(b, x) = \psi(x) - \psi(b)$$

The debt function satisfies:
1. $\operatorname{debt}(b, b) = 0$
2. $\operatorname{debt}(b, z) = \operatorname{debt}(b, x) + \operatorname{debt}(x, z)$ (telescoping)
3. $\operatorname{debt}(b, x) = -\operatorname{debt}(x, b)$ (antisymmetry)
4. Cycle invariance: $\sum_{k=1}^n \operatorname{debt}(b_k, b_{k+1}) = 0$ for closed paths

**Definition 2.4 ($\gamma$-Distance).** For $\gamma \in [0,1]$, the $\gamma$-distance is:

$$d_\gamma(b, x) = \sqrt{d(b, x)^2 + \gamma^2 \cdot \operatorname{debt}(b, x)^2}$$

At the extremes:
- $\gamma = 0$: $d_0 = d$ (pure energy)
- $\gamma = 1$: $d_1 = |Q|$ (full energy-debt metric)

### 2.2 Topological Structures

**Definition 2.5 (Average Distance).**

$$d_{\mathrm{avg}}(b, x) = \frac{d_\gamma(b, x) + d_\gamma(x, b)}{2}$$

The average distance is a genuine metric on $\mathcal{B}$.

**Definition 2.6 (Forward and Backward Topologies).**

- **Forward topology** $\tau_+$: generated by forward balls $B^+(b, \epsilon) = \{x \in \mathcal{B} : d_\gamma(b, x) < \epsilon\}$

- **Backward topology** $\tau_-$: generated by backward balls $B^-(b, \epsilon) = \{x \in \mathcal{B} : d_\gamma(x, b) < \epsilon\}$

- **Join topology** $\tau_+ \vee \tau_-$: coarsest topology containing both, with basis $\{B^+(b, \epsilon) \cap B^-(b, \delta) : b \in \mathcal{B}, \epsilon, \delta > 0\}$

**Definition 2.7 (Equilibrium Hierarchy).**

1. $E_{\mathrm{avg}}$: sets closed in $\tau_{\mathrm{avg}}$
2. $E_\vee$: sets closed in $\tau_+ \vee \tau_-$
3. $E_\rightarrow$: sets closed in $\tau_+$
4. $E_\leftarrow$: sets closed in $\tau_-$

**Theorem 2.8 (Equilibrium Hierarchy from [1]).**

$$E_{\mathrm{avg}} \subseteq E_\vee \subseteq E_\rightarrow \cap E_\leftarrow$$

---

## 3. Local Symmetry in Belief Space

### 3.1 Definition and Motivation

The asymmetry $d_\gamma(b, x) \neq d_\gamma(x, b)$ encodes the arrow of time in belief space. However, this asymmetry may vanish locally at certain points, indicating local reversibility.

**Definition 3.1 (Locally Symmetric Point).** A belief $b \in \mathcal{B}$ is called a locally symmetric point of $d_\gamma$ if:

$$\liminf_{x \to \tau_{\mathrm{avg}} b} \frac{|d_\gamma(b, x) - d_\gamma(x, b)|}{d_{\mathrm{avg}}(b, x)} = 0$$

The set of all locally symmetric points is denoted $\mathrm{Sym}_\gamma(\mathcal{B})$.

**Remark 3.2.** The limit uses $\tau_{\mathrm{avg}}$—the natural choice since $d_{\mathrm{avg}}$ appears in the denominator. Using $\tau_+$ or $\tau_-$ would introduce an asymmetric dependence that obscures the meaning. Note that $d_{\mathrm{avg}}(b, x) > 0$ for $x \neq b$, so the ratio is well-defined away from $b$.

### 3.2 Interpretation

The locally symmetric set has several intuitive interpretations:

| Perspective | Meaning |
|-------------|---------|
| **Thermodynamic** | Points where the arrow of time vanishes locally |
| **Geometric** | Points where the forward and backward distances are asymptotically equal |
| **Information-theoretic** | Points where the system has no local memory of direction |
| **Computational** | Points where processes are locally reversible |

### 3.3 Basic Properties

**Proposition 3.3 (Fixed Points).** Every fixed point $b^*$ (where $Q(b^*, b^*) = 0$) belongs to $\mathrm{Sym}_\gamma(\mathcal{B})$.

**Proof.** At a fixed point, $d_\gamma(b^*, b^*) = 0$ and $d_{\mathrm{avg}}(b^*, b^*) = 0$. The ratio is trivially zero. $\square$

**Proposition 3.4 (Gauge Invariance).** $\mathrm{Sym}_\gamma(\mathcal{B})$ is invariant under gauge transformations $\psi \mapsto \psi + c$ for $c \in \mathbb{R}$.

**Proof.** The debt function is gauge-invariant (Proposition 2.2, property 5), so $d_\gamma$ and $d_{\mathrm{avg}}$ are unchanged. $\square$

---

## 4. Symmetry Implies Equilibrium

We now prove the central inclusion theorem.

### 4.1 The Inclusion Result

**Theorem 4.1 (Symmetry Implies Equilibrium).**

$$\mathrm{Sym}_\gamma(\mathcal{B}) \subseteq E_{\mathrm{avg}}$$

**Proof.** Let $b \in \mathrm{Sym}_\gamma(\mathcal{B})$. By Definition 3.1:

$$\liminf_{x \to \tau_{\mathrm{avg}} b} \frac{|d_\gamma(b, x) - d_\gamma(x, b)|}{d_{\mathrm{avg}}(b, x)} = 0$$

This means that for any sequence $x_n \to b$ in $\tau_{\mathrm{avg}}$, we have:

$$|d_\gamma(b, x_n) - d_\gamma(x_n, b)| = o(d_{\mathrm{avg}}(b, x_n))$$

Suppose for contradiction that $b \notin E_{\mathrm{avg}}$. Then there exists a sequence $x_n \to b$ in $\tau_{\mathrm{avg}}$ such that $x_n \notin E_{\mathrm{avg}}$ for all $n$.

Since $x_n \to b$ in $\tau_{\mathrm{avg}}$, we have $d_{\mathrm{avg}}(b, x_n) \to 0$. By the symmetry condition:

$$\frac{|d_\gamma(b, x_n) - d_\gamma(x_n, b)|}{d_{\mathrm{avg}}(b, x_n)} \to 0$$

This implies that $d_\gamma(b, x_n)$ and $d_\gamma(x_n, b)$ are asymptotically equal.

Now, for any $\epsilon > 0$ and sufficiently large $n$:

$$d_\gamma(b, x_n) < \frac{\epsilon}{2} \quad \text{and} \quad d_\gamma(x_n, b) < \frac{\epsilon}{2}$$

Thus $x_n \in B^+(b, \epsilon/2) \cap B^-(b, \epsilon/2)$. For $\epsilon$ small enough, this intersection is contained in $B_{\mathrm{avg}}(b, \epsilon)$, so $x_n$ is arbitrarily close to $b$ in $\tau_{\mathrm{avg}}$.

But if $E_{\mathrm{avg}}$ is closed and contains $b$, then $x_n$ must eventually lie in $E_{\mathrm{avg}}$—contradiction.

Therefore, $b \in E_{\mathrm{avg}}$. $\square$

### 4.2 Discussion

Theorem 4.1 establishes that local reversibility is a sufficient condition for equilibrium in belief space. This aligns with physical intuition: if a system is locally reversible, it cannot sustain directional flows, so it must be in equilibrium.

**Corollary 4.2.** $\mathrm{Sym}_\gamma(\mathcal{B}) \subseteq E_\vee \subseteq E_\rightarrow \cap E_\leftarrow$.

**Proof.** Immediate from Theorem 4.1 and Theorem 2.8. $\square$

---

## 5. Smoothness Implies Equality

Under differentiability conditions, the inclusion becomes equality.

### 5.1 The Smoothness Theorem

**Theorem 5.1 (Smoothness Implies Equality).** If $d$ and $\psi$ are $C^1$ (continuously differentiable) on $\mathcal{B}$, then:

$$\mathrm{Sym}_\gamma(\mathcal{B}) = E_{\mathrm{avg}}$$

**Proof.** We already have $\mathrm{Sym}_\gamma(\mathcal{B}) \subseteq E_{\mathrm{avg}}$ from Theorem 4.1. We need to prove $E_{\mathrm{avg}} \subseteq \mathrm{Sym}_\gamma(\mathcal{B})$.

Let $b \in E_{\mathrm{avg}}$. By definition, $b$ is closed in the average topology $\tau_{\mathrm{avg}}$.

Define the asymmetry function:

$$A(x) = |d_\gamma(b, x) - d_\gamma(x, b)|$$

Since $d$ and $\psi$ are $C^1$, and $d_\gamma$ is constructed from them, $A(x)$ is $C^1$ near $b$.

Note that $A(b) = 0$ because $d_\gamma(b, b) = 0$.

By Taylor expansion around $b$:

$$A(x) = |\nabla A(b) \cdot (x - b)| + O(\|x - b\|^2)$$

But the linear term must vanish because $A$ is non-negative and has a minimum at $b$. Therefore:

$$A(x) = O(\|x - b\|^2)$$

Now, since $d_\gamma(b, x) > 0$ for $x \neq b$ and is $C^1$, by the positive definiteness of the cost function, there exists a constant $c > 0$ such that:

$$d_{\mathrm{avg}}(b, x) \geq c \cdot \|x - b\|$$

for $x$ sufficiently close to $b$.

Therefore:

$$\frac{A(x)}{d_{\mathrm{avg}}(b, x)} \leq \frac{O(\|x - b\|^2)}{c \cdot \|x - b\|} = O(\|x - b\|) \to 0$$

as $x \to b$ in $\tau_{\mathrm{avg}}$.

Thus:

$$\liminf_{x \to \tau_{\mathrm{avg}} b} \frac{|d_\gamma(b, x) - d_\gamma(x, b)|}{d_{\mathrm{avg}}(b, x)} = 0$$

So $b \in \mathrm{Sym}_\gamma(\mathcal{B})$.

**Conclusion:** $E_{\mathrm{avg}} \subseteq \mathrm{Sym}_\gamma(\mathcal{B})$. Combined with Theorem 4.1: $\mathrm{Sym}_\gamma(\mathcal{B}) = E_{\mathrm{avg}}$. $\square$

### 5.2 The Regularity Condition

**Remark 5.2.** The proof of Theorem 5.1 requires $d$ and $\psi$ to be $C^1$ at $b$. Is $C^1$ necessary? Consider weaker regularity conditions:

| Regularity | Sym = E_avg? | Reason |
|------------|--------------|--------|
| $C^1$ | ✅ Yes | Taylor expansion gives quadratic asymmetry |
| Lipschitz | ❌ Not necessarily | Asymmetry can decay linearly, not quadratically |
| Hölder | ❌ Not necessarily | Asymmetry can decay slower than quadratically |
| Continuous | ❌ Generally false | Counterexamples exist (Section 6) |

**Open Question 5.3.** Is there a weaker condition than $C^1$ that still implies $\mathrm{Sym} = E_{\mathrm{avg}}$? Perhaps $C^{1,\alpha}$ (Hölder continuous derivatives) with $\alpha > 0$?

---

## 6. Non-Smooth Counterexamples

We now demonstrate that without differentiability, the inclusion $\mathrm{Sym} \subseteq E_{\mathrm{avg}}$ can be strict.

### 6.1 A 1D Counterexample

**Construction 6.1.** Let $\mathcal{B} = \mathbb{R}$. Define the energy quasi-metric:

$$d(b, x) = |\Delta| + |\Delta|^{1/2}$$

where $\Delta = x - b$. Define the debt function:

$$\operatorname{debt}(b, x) = \Delta$$

Define the $\gamma$-distance for $\gamma = 1$:

$$d_1(b, x) = \sqrt{(|\Delta| + |\Delta|^{1/2})^2 + \Delta^2}$$

And the reverse distance:

$$d_1(x, b) = \sqrt{(|\Delta| - |\Delta|^{1/2})^2 + \Delta^2}$$

**Note:** The reverse distance uses the fact that $\operatorname{debt}(x, b) = -\Delta$.

### 6.2 Analysis

**Lemma 6.2.** For $b = 0$, we have $b \in E_{\mathrm{avg}}$.

**Proof.** The average distance is:

$$d_{\mathrm{avg}}(0, \Delta) = \frac{1}{2}\left[\sqrt{(|\Delta| + |\Delta|^{1/2})^2 + \Delta^2} + \sqrt{(|\Delta| - |\Delta|^{1/2})^2 + \Delta^2}\right]$$

For small $|\Delta|$, this is approximately:

$$d_{\mathrm{avg}}(0, \Delta) \approx |\Delta| + O(|\Delta|^{3/2})$$

This is positive for all $\Delta \neq 0$ and vanishes only as $\Delta \to 0$. Therefore, $0$ is closed in the average topology, so $0 \in E_{\mathrm{avg}}$. $\square$

**Lemma 6.3.** $0 \notin \mathrm{Sym}_\gamma(\mathcal{B})$ for $\gamma > 0$.

**Proof.** Compute the asymmetry:

$$|d_1(0, \Delta) - d_1(\Delta, 0)|$$

For small $|\Delta|$, we have:

$$d_1(0, \Delta) \approx |\Delta| + |\Delta|^{1/2} + \frac{|\Delta|^{3/2}}{2} + O(|\Delta|^2)$$

$$d_1(\Delta, 0) \approx |\Delta| - |\Delta|^{1/2} + \frac{|\Delta|^{3/2}}{2} + O(|\Delta|^2)$$

So:

$$|d_1(0, \Delta) - d_1(\Delta, 0)| \approx 2|\Delta|^{1/2} + O(|\Delta|^{3/2})$$

Therefore:

$$\frac{|d_1(0, \Delta) - d_1(\Delta, 0)|}{d_{\mathrm{avg}}(0, \Delta)} \approx \frac{2|\Delta|^{1/2}}{|\Delta| + O(|\Delta|^{3/2})} = \frac{2}{\sqrt{|\Delta|}} \to \infty$$

Thus the limit infimum is infinite (or diverges), so $0 \notin \mathrm{Sym}_\gamma(\mathcal{B})$. $\square$

### 6.3 The Gap

**Theorem 6.4 (Non-Smooth Gap).** There exists a belief space such that:

$$\mathrm{Sym}_\gamma(\mathcal{B}) \subsetneq E_{\mathrm{avg}}$$

**Proof.** The construction in Section 6.1 provides a counterexample where $0 \in E_{\mathrm{avg}}$ but $0 \notin \mathrm{Sym}_\gamma(\mathcal{B})$. Therefore, the inclusion is strict. $\square$

### 6.4 Generalization

**Proposition 6.5.** The gap $\mathrm{Sym} \subsetneq E_{\mathrm{avg}}$ occurs whenever the cost function has a singularity at a point $b$ of the form:

$$d(b, x) = \|x - b\| + \|x - b\|^\alpha \quad \text{for } 0 < \alpha < 1$$

At such points, $b \in E_{\mathrm{avg}}$ but $b \notin \mathrm{Sym}_\gamma(\mathcal{B})$.

**Proof.** Follows the same argument as Theorem 6.4. $\square$

---

## 7. The Extended Equilibrium Hierarchy

### 7.1 The Complete Hierarchy

**Theorem 7.1 (Extended Equilibrium Hierarchy).**

$$\mathrm{Sym}_\gamma(\mathcal{B}) \subseteq E_{\mathrm{avg}} \subseteq E_\vee \subseteq E_\rightarrow \cap E_\leftarrow$$

**Proof.** 
- $\mathrm{Sym} \subseteq E_{\mathrm{avg}}$: Theorem 4.1
- $E_{\mathrm{avg}} \subseteq E_\vee$: From [1], Theorem 6.8
- $E_\vee \subseteq E_\rightarrow \cap E_\leftarrow$: From [1], Theorem 6.8

$\square$

### 7.2 Summary Table

| Level | Set | Meaning | Condition for Equality |
|-------|-----|---------|------------------------|
| 0 | $\mathrm{Sym}_\gamma$ | Local reversibility | N/A |
| 1 | $E_{\mathrm{avg}}$ | Average equilibrium | $\mathrm{Sym} = E_{\mathrm{avg}}$ if $C^1$ |
| 2 | $E_\vee$ | Bidirectional equilibrium | $E_{\mathrm{avg}} = E_\vee$ if global symmetry |
| 3 | $E_\rightarrow \cap E_\leftarrow$ | Directional stability | $E_\vee = E_\rightarrow \cap E_\leftarrow$ if local symmetry |

### 7.3 When Are Inclusions Strict?

**Proposition 7.2.** The inclusions are strict in the following cases:

1. **$\mathrm{Sym} \subsetneq E_{\mathrm{avg}}$**: Non-smooth cost functions (Section 6)

2. **$E_{\mathrm{avg}} \subsetneq E_\vee$**: When the asymmetry does not vanish to first order, but the system is still bidirectionally stable

3. **$E_\vee \subsetneq E_\rightarrow \cap E_\leftarrow$**: When the system is stable in both directions but not simultaneously (e.g., one-way paths)

---

## 8. Physical Interpretation

### 8.1 The Sym = E_avg Case (Smooth Systems)

In smooth systems, $\mathrm{Sym} = E_{\mathrm{avg}}$. This means:

- **Equilibrium = Local Reversibility**
- The system is in equilibrium exactly when the arrow of time vanishes locally
- This corresponds to the **thermodynamic ideal**: systems in equilibrium are locally time-reversible

**Example:** A gas in thermal equilibrium. At the microscopic level, collisions are reversible. The arrow of time emerges only at the macroscopic level through entropy increase.

### 8.2 The Sym ⊊ E_avg Case (Non-Smooth Systems)

In non-smooth systems, there are equilibrium points that are not locally reversible. This means:

- **Stability without Reversibility**
- The system is in equilibrium (persists in time) but retains a memory of the arrow of time
- This corresponds to **hysteresis**, **memory**, or **learning**

**Example:** A magnet below its Curie temperature. It is in equilibrium (stable magnetic domains) but retains a memory of its past magnetization. The arrow of time is encoded in the domain structure.

**Example:** An AGI system with learned weights. The weights are in equilibrium (stable) but encode the history of training. The system has memory but is stable.

### 8.3 The Gap as Memory

**Proposition 8.1.** The gap $E_{\mathrm{avg}} \setminus \mathrm{Sym}_\gamma(\mathcal{B})$ represents the **memory** of the system:

- The system is stable (in $E_{\mathrm{avg}}$)
- But it is not locally reversible (not in $\mathrm{Sym}_\gamma$)
- Therefore, it retains information about its past

**Interpretation:** The gap $\mathrm{Sym} \subsetneq E_{\mathrm{avg}}$ quantifies the amount of "irreversible stability" in the system—its capacity to store information without being destabilized.

### 8.4 Connection to Thermodynamics

| Concept | Mathematical Expression | Physical Meaning |
|---------|------------------------|------------------|
| Local reversibility | $\mathrm{Sym}_\gamma$ | Microscopic time symmetry |
| Equilibrium | $E_{\mathrm{avg}}$ | Macroscopic stability |
| Memory | $E_{\mathrm{avg}} \setminus \mathrm{Sym}_\gamma$ | Stable information storage |
| Arrow of time | $d_\gamma(b, x) \neq d_\gamma(x, b)$ | Directional asymmetry |

---

## 9. Implications for AGI Architecture Design

> **Editor's note:** The source document lost its headers at this point (likely a copy/paste break). The subsection headers below (9.1, 9.3) have been reconstructed from context to restore the section's logical flow — please verify against the original if precision matters.

### 9.1 The Necessity of the Gap

For a learning system to retain information about its history, it must satisfy:

$$\mathrm{Sym}_\gamma(\mathcal{B}) \subsetneq E_{\mathrm{avg}}$$

This means:
- The system must be in equilibrium (stable)
- But not locally reversible (must retain memory)

### 9.2 The Gap as Learning Capacity

**Proposition 9.2.** The size of the gap $E_{\mathrm{avg}} \setminus \mathrm{Sym}_\gamma(\mathcal{B})$ is a measure of the system's **learning capacity**:

- Large gap → More capacity to store irreversible information
- Small gap → Less capacity; system tends to forget

### 9.3 Design Principles

1. **Maintain equilibrium**: Ensure stability through the cost-debt dynamics
2. **Allow non-smoothness**: Create conditions for $\mathrm{Sym} \subsetneq E_{\mathrm{avg}}$
3. **Control the gap**: Tune $\gamma$ to balance reversibility and memory

**Design Principle:** Systems should operate at non-smooth points in belief space to enable learning while maintaining stability.

---

## 10. Open Questions

1. **Exact Regularity Condition:** Is $C^1$ necessary for $\mathrm{Sym} = E_{\mathrm{avg}}$? Or is Lipschitz continuity sufficient? What about $C^{1,\alpha}$?

2. **Topological Structure:** What is the topology of $\mathrm{Sym}_\gamma(\mathcal{B})$? Is it a submanifold? Dense? Open? Closed? What are its connected components?

3. **Gap Geometry:** What is the geometry of $E_{\mathrm{avg}} \setminus \mathrm{Sym}_\gamma(\mathcal{B})$? Is it a stratified space? A manifold with boundary?

4. **Phase Transitions:** Are there phase transitions as $\gamma$ varies where $\mathrm{Sym}$ changes discontinuously?

5. **Information Theory:** Can we relate the size of the gap to information-theoretic quantities (entropy, mutual information)?

6. **Dynamics:** What are the dynamics of $\mathrm{Sym}$ under gradient flow? Does the system evolve toward $\mathrm{Sym}$ or away from it?

7. **Conservation Laws:** Are there conserved quantities on $\mathrm{Sym}$ (e.g., energy, momentum)?

8. **Statistical Mechanics:** Can we define a $\gamma$-temperature and formulate statistical mechanics on $\mathrm{Sym}$?

9. **Category Theory:** Does $\mathrm{Sym}$ have a categorical interpretation? Is it a limit or colimit in some category?

10. **Connection to Physics:** Is the gap $\mathrm{Sym} \subsetneq E_{\mathrm{avg}}$ related to the second law of thermodynamics? Does it provide a mechanism for entropy production?

---

## 11. Conclusion

We have established the fundamental relationship between local symmetry and equilibrium in belief space. The hierarchy:

$$\mathrm{Sym}_\gamma(\mathcal{B}) \subseteq E_{\mathrm{avg}} \subseteq E_\vee \subseteq E_\rightarrow \cap E_\leftarrow$$

captures distinct notions of stability and reversibility.

Key results include:

1. **Theorem 1:** $\mathrm{Sym} \subseteq E_{\mathrm{avg}}$ — local reversibility implies equilibrium

2. **Theorem 2:** Under $C^1$ conditions, $\mathrm{Sym} = E_{\mathrm{avg}}$ — smooth systems have equilibrium = reversibility

3. **Theorem 3:** Non-smooth systems can have $\mathrm{Sym} \subsetneq E_{\mathrm{avg}}$ — stability without reversibility

4. **Theorem 4:** Complete hierarchy: $\mathrm{Sym} \subseteq E_{\mathrm{avg}} \subseteq E_\vee \subseteq E_\rightarrow \cap E_\leftarrow$

---

## Appendix A: Technical Lemmas

### A.1 Positive Definiteness of Cost

**Lemma A.1.** For any $b \in \mathcal{B}$, there exists a constant $c > 0$ such that for sufficiently small $\epsilon$:

$$d_\gamma(b, x) \geq c \cdot \|x - b\|$$

for all $x$ with $\|x - b\| < \epsilon$.

**Proof.** By Definition 2.2, $d(b, b) = 0$ and $d$ is continuous at $b$ (for $C^1$ functions). The result follows from the positive definiteness of the cost function. $\square$

### A.2 Smoothness of Asymmetry

**Lemma A.2.** If $d$ and $\psi$ are $C^1$, then $A(x) = |d_\gamma(b, x) - d_\gamma(x, b)|$ is $C^1$ near $b$.

**Proof.** $d_\gamma$ is constructed from $d$ and $\psi$ via smooth operations (square root, addition, multiplication). Therefore, $d_\gamma$ is $C^1$ wherever $d$ and $\psi$ are $C^1$ and $d_\gamma > 0$. Near $b$, $d_\gamma > 0$ for $x \neq b$. $\square$

### A.3 Vanishing Linear Term

**Lemma A.3.** If $A$ is $C^1$, $A \geq 0$, and $A(b) = 0$, then $\nabla A(b) = 0$.

**Proof.** Since $A$ has a minimum at $b$, the gradient must vanish. $\square$

---

## References

[1] M. Elwardi, "Humble Systems Theory: A Unified Framework for Information Processing Systems," 2026.

[2] R. Landauer, "Irreversibility and heat generation in the computing process," *IBM Journal of Research and Development*, vol. 5, no. 3, pp. 183-191, 1961.

[3] C. H. Bennett, "The thermodynamics of computation — a review," *International Journal of Theoretical Physics*, vol. 21, no. 12, pp. 905-940, 1982.

[4] S. Amari, *Information Geometry and Its Applications*. Springer, 2016.

[5] J. C. Kelly, "Bitopological spaces," *Proceedings of the London Mathematical Society*, 3(1):71-89, 1963.

[6] P. Fletcher and W. F. Lindgren, *Quasi-Uniform Spaces*. Marcel Dekker, New York, 1982.

[7] R. Kopperman, "Asymmetry and duality in topology," *Topology and its Applications*, 66(1):1-39, 1995.

[8] A. Turing, "Computing machinery and intelligence," *Mind*, vol. 59, no. 236, pp. 433-460, 1950.

[9] J. von Neumann, *The Computer and the Brain*. Yale University Press, 1958.

[10] D. J. C. MacKay, *Information Theory, Inference, and Learning Algorithms*. Cambridge University Press, 2003.

---

## Index

| Symbol | Meaning | First Appearance |
|--------|---------|------------------|
| $\mathcal{B}$ | Belief space | Definition 2.1 |
| $d$ | Energy quasi-metric | Definition 2.2 |
| $\operatorname{debt}$ | Debt function | Definition 2.3 |
| $\psi$ | Potential function | Definition 2.3 |
| $d_\gamma$ | $\gamma$-distance | Definition 2.4 |
| $d_{\mathrm{avg}}$ | Average distance | Definition 2.5 |
| $\tau_+$ | Forward topology | Definition 2.6 |
| $\tau_-$ | Backward topology | Definition 2.6 |
| $E_{\mathrm{avg}}$ | Average equilibrium | Definition 2.7 |
| $E_\vee$ | Join equilibrium | Definition 2.7 |
| $E_\rightarrow$ | Forward equilibrium | Definition 2.7 |
| $E_\leftarrow$ | Backward equilibrium | Definition 2.7 |
| $\mathrm{Sym}_\gamma$ | Locally symmetric set | Definition 3.1 |

