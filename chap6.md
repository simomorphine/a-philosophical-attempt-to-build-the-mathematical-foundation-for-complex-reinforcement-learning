# Chapter 6: The $\gamma$-Distance Family and the Geometry of Exploration

---

## 6.1 Introduction

In Chapter 5, we introduced the complex quasi-metric $Q(b_i, b_j) = d(b_i, b_j) + i \cdot \text{debt}(b_i, b_j)$ as a unified geometric object. The modulus $|Q| = \sqrt{d^2 + \text{debt}^2}$ provides a natural, parameter-free measure of total distance.

However, there are situations where we may want to **tune** the relative importance of cost and debt. For example:
- An agent with abundant energy may prioritize debt reduction over cost minimization.
- An agent with limited energy may prioritize cost minimization over debt reduction.
- Different environments may require different balances.

Rather than introducing an arbitrary weighting parameter $\mu$ (which would reintroduce the free parameter problem), we introduce a **one-parameter family of distances** $d_\gamma$ that continuously interpolates between pure cost and full cost-debt geometry. The parameter $\gamma \in [0, 1]$ has a natural geometric interpretation: it controls the "weight" of debt in the distance measure.

This chapter is organized as follows:
- **Section 6.2:** Definition of the $\gamma$-distance family.
- **Section 6.3:** The extremes: $\gamma = 0$ and $\gamma = 1$.
- **Section 6.4:** Metric properties (triangle inequality).
- **Section 6.5:** Geometric interpretation of varying $\gamma$.
- **Section 6.6:** The derivative with respect to $\gamma$.
- **Section 6.7:** Topological dependence on $\gamma$.
- **Section 6.8:** The bitopological structure from asymmetry.
- **Section 6.9:** Examples and interpretations.
- **Section 6.10:** Relationship to reinforcement learning.
- **Section 6.11:** Summary.

---

## 6.2 Definition of the $\gamma$-Distance

### 6.2.1 The $\gamma$-Distance

**Definition 6.1 $\gamma$-Distance).** For $\gamma \in [0, 1]$, the $\gamma$-distance is defined as

$$
d_\gamma(b_i, b_j) = \sqrt{d(b_i, b_j)^2 + \gamma^2 \cdot \text{debt}(b_i, b_j)^2},
$$

where:
- $d(b_i, b_j)$ is the energy quasi-metric (real cost).
- $\text{debt}(b_i, b_j)$ is the debt function.

Equivalently, in terms of the complex quasi-metric:

$$
d_\gamma(b_i, b_j) = \sqrt{\Re(Q)^2 + \gamma^2 \cdot \Im(Q)^2}.
$$

### 6.2.2 Interpretation of $\gamma$

The parameter $\gamma$ controls the relative importance of debt in the distance:
- $\gamma = 0$: Debt is ignored entirely.
- $\gamma = 1$: Debt is fully included (the full modulus).
- $0 < \gamma < 1$: Debt is partially included.

**Key Insight:** Unlike the classical weighting parameter $\mu$, $\gamma$ is not an arbitrary weight. It is a **geometric parameter** that continuously deforms the metric structure of belief space. It can be interpreted as:
- **A trade-off parameter:** How much does the agent care about debt relative to cost?
- **An environmental parameter:** What is the relative importance of cost and debt in the environment?
- **A learning parameter:** As the agent learns, $\gamma$ may change (e.g., high $\gamma$ early, low $\gamma$ late).

### 6.2.3 The $\gamma$-Distance as a Weighted Euclidean Norm

The $\gamma$-distance can be written as:

$$
d_\gamma(b_i, b_j) = \left\| \bigl( d(b_i, b_j),\; \gamma \cdot \text{debt}(b_i, b_j) \bigr) \right\|_2.
$$

This is a weighted Euclidean norm in the cost-debt plane. The weight $\gamma$ controls the relative importance of debt.

**Comparison with Classical Weighting:**
- Classical weighting: $c + \mu d$ (linear combination, parameter $\mu$).
- $\gamma$-distance: $\sqrt{c^2 + \gamma^2 d^2}$ (Euclidean norm, parameter $\gamma$).

The $\gamma$-distance preserves the geometric structure of the cost-debt plane. The classical weighting destroys it (it is a projection onto a line).

---

## 6.3 The Extremes

### 6.3.1 $\gamma = 0$: Pure Energy Geometry

When $\gamma = 0$:

$$
d_0(b_i, b_j) = \sqrt{d(b_i, b_j)^2 + 0} = d(b_i, b_j).
$$

**Interpretation:** The agent ignores debt entirely. The belief space has the geometry of the energy quasi-metric. The agent minimizes cost only.

**Properties:**
- The distance is the energy cost.
- The geometry is asymmetric (if cost is asymmetric).
- Debt has no influence on decision-making.

**When is this appropriate?**
- When information is free (debt is irrelevant).
- When the agent has perfect information (debt is zero).
- When cost dominates all other considerations.

### 6.3.2 $\gamma = 1$: Full Energy-Debt Geometry

When $\gamma = 1$:

$$
d_1(b_i, b_j) = \sqrt{d(b_i, b_j)^2 + \text{debt}(b_i, b_j)^2} = |Q(b_i, b_j)|.
$$

**Interpretation:** The agent fully considers both cost and debt. The distance is the modulus of the complex quasi-metric.

**Properties:**
- The distance is the full Euclidean distance in the cost-debt plane.
- The geometry is the full complex geometry.
- Both cost and debt influence decision-making equally (in the geometric sense).

**When is this appropriate?**
- When cost and debt are equally important.
- When the agent needs to balance cost and debt optimally.
- When the agent has no prior reason to favor one over the other.

### 6.3.3 The Interpolation

For $0 < \gamma < 1$:

$$
d_\gamma(b_i, b_j) = \sqrt{d(b_i, b_j)^2 + \gamma^2 \cdot \text{debt}(b_i, b_j)^2}.
$$

**Interpretation:** The agent partially considers debt. The geometry is a deformation of the full complex geometry.

**Properties:**
- The distance is a weighted Euclidean norm in the cost-debt plane.
- The geometry interpolates between pure cost and full cost-debt.
- The parameter $\gamma$ controls the deformation.

---

## 6.4 Metric Properties

### 6.4.1 Identity and Positivity

**Proposition 6.2 (Identity and Positivity).** For any $\gamma \in [0, 1]$:
- $d_\gamma(b, b) = 0$.
- $d_\gamma(b_i, b_j) \geq 0$, with equality if and only if $b_i = b_j$ (assuming cost is positive definite).

*Proof.* These follow directly from the properties of $d$ and debt. 

### 6.4.2 Asymmetry

**Proposition 6.3 (Asymmetry).** For $\gamma > 0$, $d_\gamma$ is asymmetric if cost is asymmetric or debt is non-zero:

$$
d_\gamma(b_i, b_j) \neq d_\gamma(b_j, b_i)
$$

in general.

*Proof.*

$$
d_\gamma(b_i, b_j)^2 - d_\gamma(b_j, b_i)^2 = [d(b_i, b_j)^2 - d(b_j, b_i)^2] + \gamma^2[\text{debt}(b_i, b_j)^2 - \text{debt}(b_j, b_i)^2].
$$

Since $\text{debt}(b_i, b_j) = -\text{debt}(b_j, b_i)$, the debt terms cancel:

$$
d_\gamma(b_i, b_j)^2 - d_\gamma(b_j, b_i)^2 = d(b_i, b_j)^2 - d(b_j, b_i)^2.
$$

Thus, asymmetry arises entirely from the asymmetry of the energy quasi-metric. 

**Interpretation:** The $\gamma$-distance inherits the asymmetry of the energy quasi-metric. Debt does not contribute to asymmetry (it is antisymmetric).

### 6.4.3 The Triangle Inequality

**Theorem 6.4 (Triangle Inequality for $d_\gamma$).** For any $\gamma \in [0, 1]$ and any beliefs $b_i, b_j, b_k \in \mathcal{B}$:

$$
d_\gamma(b_i, b_k) \leq d_\gamma(b_i, b_j) + d_\gamma(b_j, b_k).
$$

*Proof.* We prove for $\gamma = 1$; the result extends to all $\gamma \in [0, 1]$ by continuity.

For $\gamma = 1$:

$$
d_1(b_i, b_k)^2 = d(b_i, b_k)^2 + \text{debt}(b_i, b_k)^2.
$$

By the triangle inequality for the energy quasi-metric:

$$
d(b_i, b_k) \leq d(b_i, b_j) + d(b_j, b_k).
$$

By the telescoping property of debt:

$$
\text{debt}(b_i, b_k) = \text{debt}(b_i, b_j) + \text{debt}(b_j, b_k).
$$

Therefore, in $\mathbb{R}^2$:

$$
(d(b_i, b_k), \text{debt}(b_i, b_k)) \leq_{\text{componentwise}} (d(b_i, b_j) + d(b_j, b_k), \text{debt}(b_i, b_j) + \text{debt}(b_j, b_k)).
$$

By the Minkowski inequality:

$$
d_1(b_i, b_k) \leq d_1(b_i, b_j) + d_1(b_j, b_k).
$$

For $\gamma < 1$, the inequality follows from the fact that $d_\gamma$ is the norm $\| (d, \gamma \cdot \text{debt}) \|_2$, and the Minkowski inequality holds for all $\gamma \in [0, 1]$. 

**Interpretation:** The $\gamma$-distance satisfies the triangle inequality for all $\gamma \in [0, 1]$. It is a genuine quasi-metric (or metric if cost is symmetric).

### 6.4.4 Summary of Metric Properties

| Property | Status | Notes |
|----------|--------|-------|
| Identity | $d_\gamma(b, b) = 0$ | Always holds |
| Positivity | $d_\gamma(b_i, b_j) \geq 0$ | Equality iff $b_i = b_j$ |
| Symmetry | Generally asymmetric | Inherited from cost |
| Triangle inequality | Holds for all $\gamma \in [0, 1]$ | Via Minkowski |
| Debt antisymmetry | $\text{debt}(b_i, b_j) = -\text{debt}(b_j, b_i)$ | Cancels in asymmetry |

---

## 6.5 Geometric Interpretation

### 6.5.1 Deformation of the Metric

As $\gamma$ varies from 0 to 1, the metric structure of belief space continuously deforms:

1. **$\gamma = 0$:** The geometry is the energy cost geometry. Metric balls are determined by cost alone.

2. **$0 < \gamma < 1$:** The geometry is a deformed version. Debt contributes to distance, but less than cost.

3. **$\gamma = 1$:** The geometry is the full cost-debt geometry. Cost and debt contribute equally (in the Euclidean sense).

The deformation is smooth and continuous in $\gamma$.

### 6.5.2 Metric Balls

**Definition 6.5 (Metric Ball).** For $\gamma \in [0, 1]$, $r > 0$, and belief $b \in \mathcal{B}$, the metric ball is

$$
B_\gamma(b, r) = \lbrace b' \in \mathcal{B} \mid d_\gamma(b, b') < r \rbrace.
$$

**Effect of $\gamma$ on Balls:**
- For $\gamma = 0$, balls are determined by cost alone.
- For $\gamma > 0$, balls shrink in the debt direction: beliefs with high debt are farther away.
- For $\gamma = 1$, balls are Euclidean balls in the cost-debt plane.

**Observation:** As $\gamma$ increases, the balls deform. Beliefs that are close in the $\gamma = 0$ geometry may be far apart for $\gamma > 0$ if their debt difference is large.

### 6.5.3 Geodesics

**Definition 6.6 (Geodesic).** A geodesic in the $\gamma$-geometry is a path $b(t)$ that minimizes the length:

$$
L_\gamma(b) = \int_0^1 d_\gamma(b(t), b(t+dt)) \, dt.
$$

**Effect of $\gamma$ on Geodesics:**
- For $\gamma = 0$, geodesics minimize cost.
- For $\gamma > 0$, geodesics trade off cost and debt.
- For $\gamma = 1$, geodesics minimize the full cost-debt distance.

**Interpretation:** As $\gamma$ increases, optimal paths shift from cost-efficient to debt-aware. An energy-efficient but debt-accumulating path becomes disfavored; a path that pays down debt may become preferred despite higher energy cost.

### 6.5.4 The Geometry of the Cost-Debt Plane

The $\gamma$-distance induces a family of concentric ellipses in the cost-debt plane. For a fixed distance $r$, the level set is:

$$
\lbrace (c, \delta) \in \mathbb{R}^2 \mid c^2 + \gamma^2 \delta^2 = r^2 \rbrace.
$$

This is an ellipse with:
- Semi-major axis $r$ along the cost direction.
- Semi-minor axis $r/\gamma$ along the debt direction.

As $\gamma$ increases:
- The ellipse becomes narrower in the debt direction.
- The geometry becomes more sensitive to debt.
- The level sets deform continuously.

**Visual Interpretation:**
- $\gamma = 0$: The level set degenerates to two vertical lines $c = \pm r$ (debt ignored).
- $\gamma = 1$: The level set is a circle of radius $r$ (cost and debt equal).
- $0 < \gamma < 1$: The level set is an ellipse stretched along the cost axis.

### 6.5.5 The Pareto Frontier and $\gamma$

The Pareto frontier in the cost-debt plane is the set of points where cost cannot be reduced without increasing debt, and vice versa.

For different $\gamma$, the $\gamma$-distance selects different points on the Pareto frontier:
- $\gamma = 0$: The point with minimum cost (regardless of debt).
- $\gamma = 1$: The point with minimum Euclidean distance to the origin.
- $0 < \gamma < 1$: An intermediate point.

**Interpretation:** The parameter $\gamma$ is a **preference parameter** that selects different trade-offs on the Pareto frontier. Unlike the classical weighting $\mu$, $\gamma$ has a geometric interpretation.

---

## 6.6 The Derivative with Respect to $\gamma$

### 6.6.1 Definition

The $\gamma$-distance depends smoothly on $\gamma$ for $\gamma > 0$. We can therefore differentiate it with respect to $\gamma$ to understand how the geometry responds to changes in the debt weighting.

**Definition 6.7 (Gamma Derivative).** For $\gamma > 0$ and $b_i \neq b_j$, the derivative of $d_\gamma$ with respect to $\gamma$ is

$$
\frac{\partial d_\gamma}{\partial \gamma}(b_i, b_j) = \frac{\gamma \cdot \text{debt}(b_i, b_j)^2}{\sqrt{d(b_i, b_j)^2 + \gamma^2 \cdot \text{debt}(b_i, b_j)^2}}.
$$

Equivalently, in terms of $d_\gamma$ itself:

$$
\frac{\partial d_\gamma}{\partial \gamma}(b_i, b_j) = \frac{\gamma \cdot \text{debt}(b_i, b_j)^2}{d_\gamma(b_i, b_j)}.
$$

*Proof.* Differentiate $d_\gamma^2 = d^2 + \gamma^2 \cdot \text{debt}^2$ with respect to $\gamma$:

$$
2 d_\gamma \cdot \frac{\partial d_\gamma}{\partial \gamma} = 2\gamma \cdot \text{debt}^2,
$$

so

$$
\frac{\partial d_\gamma}{\partial \gamma} = \frac{\gamma \cdot \text{debt}^2}{d_\gamma}.
$$


### 6.6.2 Sign and Monotonicity

**Proposition 6.8 (Monotonicity in $\gamma$).** For any $b_i, b_j \in \mathcal{B}$:

$$
\frac{\partial d_\gamma}{\partial \gamma}(b_i, b_j) \geq 0,
$$

with equality if and only if $\text{debt}(b_i, b_j) = 0$ or $\gamma = 0$.

*Proof.* The numerator $\gamma \cdot \text{debt}^2$ is non-negative for $\gamma \geq 0$, and the denominator $d_\gamma > 0$ for $b_i \neq b_j$. Equality holds precisely when $\gamma = 0$ or $\text{debt} = 0$. 

**Interpretation:** The $\gamma$-distance is non-decreasing in $\gamma$. Increasing $\gamma$ can only increase (or leave unchanged) the distance between any two beliefs. This confirms that $\gamma$ acts as a "debt amplifier": the more weight placed on debt, the larger the distance between beliefs with non-zero debt.

### 6.6.3 The Second Derivative

**Proposition 6.9 (Convexity in $\gamma$).** The $\gamma$-distance is convex in $\gamma$ for $\gamma > 0$:

$$
\frac{\partial^2 d_\gamma}{\partial \gamma^2}(b_i, b_j) = \frac{d(b_i, b_j)^2 \cdot \text{debt}(b_i, b_j)^2}{d_\gamma(b_i, b_j)^3} \geq 0.
$$

*Proof.* Differentiate $\partial d_\gamma / \partial \gamma = \gamma \cdot \text{debt}^2 / d_\gamma$ with respect to $\gamma$:

$$
\frac{\partial^2 d_\gamma}{\partial \gamma^2} = \frac{\text{debt}^2 \cdot d_\gamma - \gamma \cdot \text{debt}^2 \cdot (\partial d_\gamma / \partial \gamma)}{d_\gamma^2}.
$$

Substituting $\partial d_\gamma / \partial \gamma = \gamma \cdot \text{debt}^2 / d_\gamma$:

$$
\frac{\partial^2 d_\gamma}{\partial \gamma^2} = \frac{\text{debt}^2}{d_\gamma} - \frac{\gamma^2 \cdot \text{debt}^4}{d_\gamma^3} = \frac{\text{debt}^2 \cdot d_\gamma^2 - \gamma^2 \cdot \text{debt}^4}{d_\gamma^3} = \frac{d^2 \cdot \text{debt}^2}{d_\gamma^3} \geq 0.
$$


**Interpretation:** The marginal effect of increasing $\gamma$ grows with $\gamma$. The first unit of debt weight has less impact than the second, and so on. This reflects the fact that the Euclidean norm is convex: the debt contribution accelerates as $\gamma$ increases.

### 6.6.4 The Elasticity of $d_\gamma$ with Respect to $\gamma$

**Definition 6.10 (Gamma Elasticity).** The elasticity of $d_\gamma$ with respect to $\gamma$ is

$$
\mathcal{E}_\gamma(b_i, b_j) = \frac{\partial d_\gamma}{\partial \gamma} \cdot \frac{\gamma}{d_\gamma} = \frac{\gamma^2 \cdot \text{debt}(b_i, b_j)^2}{d_\gamma(b_i, b_j)^2}.
$$

**Interpretation:** The elasticity measures the percentage change in distance per percentage change in $\gamma$. It satisfies:

$$
0 \leq \mathcal{E}_\gamma \leq 1.
$$

The elasticity is:
- $0$ when $\gamma = 0$ or $\text{debt} = 0$.
- Approaches $1$ as $\gamma \to \infty$ (debt dominates).
- Equals $1/2$ when $d = \gamma \cdot \text{debt}$ (cost and debt contribute equally).

### 6.6.5 The Gamma Derivative and the Phase

Recall from Chapter 5 that the phase is

$$
\theta = \arctan\left(\frac{\text{debt}}{d}\right).
$$

For the $\gamma$-distance, the effective phase is

$$
\theta_\gamma = \arctan\left(\frac{\gamma \cdot \text{debt}}{d}\right).
$$

The derivative of $\theta_\gamma$ with respect to $\gamma$ is

$$
\frac{\partial \theta_\gamma}{\partial \gamma} = \frac{\text{debt} \cdot d}{d^2 + \gamma^2 \cdot \text{debt}^2} = \frac{\text{debt} \cdot d}{d_\gamma^2}.
$$

**Interpretation:** The phase increases with $\gamma$ when $\text{debt} > 0$ (and decreases when $\text{debt} < 0$). Increasing  $\gamma$ rotates the effective direction of the complex distance toward the debt axis.

### 6.6.6 The Gamma Derivative and the Triangle Inequality

**Proposition 6.11 (Derivative of the Triangle Inequality).** For any $b_i, b_j, b_k \in \mathcal{B}$:

$$
\frac{\partial}{\partial \gamma} d_\gamma(b_i, b_k) \leq \frac{\partial}{\partial \gamma} d_\gamma(b_i, b_j) + \frac{\partial}{\partial \gamma} d_\gamma(b_j, b_k).
$$

*Proof.* This follows from differentiating the triangle inequality $d_\gamma(b_i, b_k) \leq d_\gamma(b_i, b_j) + d_\gamma(b_j, b_k)$ with respect to $\gamma$. 

**Interpretation:** The triangle inequality is preserved under differentiation. The "debt sensitivity" of the direct path is bounded by the sum of the debt sensitivities of the intermediate paths.

### 6.6.7 The Gamma Derivative in the Two-State Example

For the two-state belief space $\mathcal{B} = \{0, 1\}$ with:
- $d(0, 1) = E_{\text{obs}}$, $d(1, 0) = E_{\text{erase}}$.
- $\text{debt}(0, 1) = 1$, $\text{debt}(1, 0) = -1$.

The gamma derivatives are:

$$
\frac{\partial d_\gamma}{\partial \gamma}(0, 1) = \frac{\gamma}{\sqrt{E_{\text{obs}}^2 + \gamma^2}}, \quad
\frac{\partial d_\gamma}{\partial \gamma}(1, 0) = \frac{\gamma}{\sqrt{E_{\text{erase}}^2 + \gamma^2}}.
$$

**Observations:**
- At $\gamma = 0$: both derivatives are $0$ (debt has no effect).
- As $\gamma \to \infty$: both derivatives approach $1$ (debt dominates).
- The derivative is larger for the direction with smaller cost (since the denominator is smaller). Thus, increasing $\gamma$ affects the cheaper direction more.

### 6.6.8 The Gamma Derivative and Learning

In the learning context, $\gamma$ may change over time. Let $\gamma(t)$ be the debt weight at time $t$. The rate of change of the $\gamma$-distance along a learning trajectory is

$$
\frac{d}{dt} d_{\gamma(t)}(b_i, b_j) = \frac{\partial d_\gamma}{\partial \gamma} \cdot \dot{\gamma}(t) + \nabla d_\gamma \cdot \dot{b}(t).
$$

**Interpretation:**
- The first term captures the effect of changing the debt weight.
- The second term captures the effect of changing beliefs.
- If \( \dot{\gamma} > 0 \) (increasing debt weight), distances increase for beliefs with non-zero debt.
- If \( \dot{\gamma} < 0 \) (decreasing debt weight), distances decrease.

**Learning Schedule:** A natural learning schedule is to start with high \( \gamma \) (emphasize debt reduction) and decrease \( \gamma \) over time (focus on cost minimization once debt is under control):
\[
\gamma(t) = \gamma_0 \cdot e^{-\lambda t} + \gamma_\infty,
\]
where \( \gamma_0 \) is the initial debt weight, \( \gamma_\infty \) is the asymptotic debt weight, and \( \lambda \) is the decay rate.

### 6.6.9 Summary of Gamma Derivative Properties

| Property | Expression | Interpretation |
|----------|------------|----------------|
| First derivative | \( \dfrac{\gamma \cdot \text{debt}^2}{d_\gamma} \) | Non-negative; debt amplifies distance |
| Second derivative | \( \dfrac{d^2 \cdot \text{debt}^2}{d_\gamma^3} \) | Non-negative; convex in \( \gamma \) |
| Elasticity | \( \dfrac{\gamma^2 \cdot \text{debt}^2}{d_\gamma^2} \) | Between 0 and 1; percentage sensitivity |
| Phase derivative | \( \dfrac{\text{debt} \cdot d}{d_\gamma^2} \) | Rotates toward debt axis |
| Sign | \( \geq 0 \) | Distance never decreases with \( \gamma \) |
| Zero condition | \( \text{debt} = 0 \) or \( \gamma = 0 \) | No debt or no debt weight |

---

## 6.7 Topological Dependence on \( \gamma \)

### 6.7.1 Topology Induced by \( d_\gamma \)

Each \( \gamma \) induces a topology \( \tau_\gamma \) on \( \mathcal{B} \). The basis of \( \tau_\gamma \) is the set of metric balls \( B_\gamma(b, r) \).

**Proposition 6.12 (Continuity of Topology).** The topology \( \tau_\gamma \) depends continuously on \( \gamma \).

*Proof.* The metric \( d_\gamma \) is continuous in \( \gamma \), and the topology induced by a continuous family of metrics is continuous. \( \square \)

### 6.7.2 Topological Comparison

**Proposition 6.13 (Topological Comparison).** For \( 0 \leq \gamma_1 < \gamma_2 \leq 1 \):
- \( \tau_{\gamma_1} \subseteq \tau_{\gamma_2} \) (the topology becomes finer as \( \gamma \) increases).
- Equivalently, open sets in \( \tau_{\gamma_1} \) are also open in \( \tau_{\gamma_2} \).

*Proof.* For any \( b \in \mathcal{B} \) and \( r > 0 \), \( B_{\gamma_1}(b, r) \supseteq B_{\gamma_2}(b, r) \) because the distance increases with \( \gamma \). Thus, the basis for \( \tau_{\gamma_1} \) is coarser. \( \square \)

**Interpretation:** As \( \gamma \) increases, the metric becomes more sensitive to debt, so the topology becomes finer (more open sets). Beliefs are distinguished by their debt as well as their cost.

### 6.7.3 Separation of Beliefs

**Proposition 6.14 (Debt Separation).** For \( \gamma_1 < \gamma_2 \), there exist beliefs \( b_i, b_j \in \mathcal{B} \) such that
\[
d_{\gamma_1}(b_i, b_j) < d_{\gamma_2}(b_i, b_j)
\]
if and only if \( \text{debt}(b_i, b_j) \neq 0 \).

*Proof.*
\[
d_{\gamma_2}^2 - d_{\gamma_1}^2 = (\gamma_2^2 - \gamma_1^2) \cdot \text{debt}(b_i, b_j)^2.
\]
The difference is positive if and only if \( \text{debt}(b_i, b_j) \neq 0 \). \( \square \)

**Interpretation:** Beliefs with non-zero debt are distinguished by \( \gamma \). Increasing \( \gamma \) increases their separation in the metric.

### 6.7.4 The Topological Lattice

The family of topologies \( \{\tau_\gamma\}_{\gamma \in [0,1]} \) forms a lattice under inclusion:
- \( \tau_0 \) is the coarsest topology (pure cost).
- \( \tau_1 \) is the finest topology (full cost-debt).
- For \( \gamma_1 < \gamma_2 \), \( \tau_{\gamma_1} \subseteq \tau_{\gamma_2} \).

This lattice structure reflects the monotonicity of the \( \gamma \)-distance in \( \gamma \).

---

## 6.8 Bitopological Structure

### 6.8.1 Two Topologies from One Distance

The asymmetry of \( d_\gamma \) naturally generates two distinct topologies on \( \mathcal{B} \):

**Definition 6.15 (Forward Topology \( \tau_+ \)).** The forward topology has basis
\[
B^+(b, \epsilon) = \{ x \in \mathcal{B} \mid d_\gamma(b, x) < \epsilon \}.
\]
The forward ball \( B^+(b, \epsilon) \) contains beliefs reachable from \( b \) with cost less than \( \epsilon \).

**Definition 6.16 (Backward Topology \( \tau_- \)).** The backward topology has basis
\[
B^-(b, \epsilon) = \{ x \in \mathcal{B} \mid d_\gamma(x, b) < \epsilon \}.
\]
The backward ball \( B^-(b, \epsilon) \) contains beliefs from which \( b \) is reachable with cost less than \( \epsilon \).

### 6.8.2 Bitopological Space

**Definition 6.17 (Bitopological Space).** The triple \( (\mathcal{B}, \tau_+, \tau_-) \) is the **bitopological space** of the system.

**Interpretation:**
- \( (\mathcal{B}, \tau_+) \): The "forward" space—where can the agent go?
- \( (\mathcal{B}, \tau_-) \): The "backward" space—where could the agent have come from?
- The asymmetry of \( d_\gamma \) means these topologies are generally different.

### 6.8.3 The Join Topology

**Definition 6.18 (Join Topology).** The join topology \( \tau_+ \vee \tau_- \) is the coarsest topology containing both \( \tau_+ \) and \( \tau_- \). Its basis is
\[
\mathcal{B}^\vee = \{ B^+(b, \epsilon) \cap B^-(b, \delta) \mid b \in \mathcal{B}, \epsilon, \delta > 0 \}.
\]

**Interpretation:** A neighborhood in the join topology requires both forward and backward proximity simultaneously. Beliefs in the intersection are **bidirectionally close**—they are both reachable from \( b \) and can reach \( b \) with low cost.

### 6.8.4 The Average Topology

**Definition 6.19 (Average Topology).** The average topology \( \tau_{\text{avg}} \) is induced by the average distance
\[
d_{\text{avg}}(b, x) = \frac{d_\gamma(b, x) + d_\gamma(x, b)}{2}.
\]

**Proposition 6.20 (Topological Hierarchy).** The average topology is coarser than the join topology:
\[
\tau_{\text{avg}} \subseteq \tau_+ \vee \tau_-.
\]

*Proof.* Every set open in \( \tau_{\text{avg}} \) is open in \( \tau_+ \vee \tau_- \), but the reverse is not necessarily true. \( \square \)

**Interpretation:** The average topology is the "symmetric" version of the bitopological space. The join topology is richer, capturing both forward and backward information.

### 6.8.5 The Bitopological Structure and \( \gamma \)

The bitopological structure depends on \( \gamma \):
- For \( \gamma = 0 \), the forward and backward topologies are determined by cost alone.
- For \( \gamma > 0 \), debt influences both topologies.
- For \( \gamma = 1 \), the full cost-debt geometry determines the topologies.

As \( \gamma \) increases, both \( \tau_+ \) and \( \tau_- \) become finer, and the join topology becomes richer.

---

## 6.9 Examples and Interpretations

### 6.9.1 Example 1: Two-State Belief Space

Let \( \mathcal{B} = \{0, 1\} \) with:
- \( d(0, 1) = E_{\text{obs}} \), \( d(1, 0) = E_{\text{erase}} \).
- \( \text{debt}(0, 1) = 1 \), \( \text{debt}(1, 0) = -1 \).

Then:
\[
d_\gamma(0, 1) = \sqrt{E_{\text{obs}}^2 + \gamma^2}, \quad d_\gamma(1, 0) = \sqrt{E_{\text{erase}}^2 + \gamma^2}.
\]

**Effect of \( \gamma \):**
- \( \gamma = 0 \): \( d_0(0, 1) = E_{\text{obs}} \), \( d_0(1, 0) = E_{\text{erase}} \).
- \( \gamma = 1 \): \( d_1(0, 1) = \sqrt{E_{\text{obs}}^2 + 1} \), \( d_1(1, 0) = \sqrt{E_{\text{erase}}^2 + 1} \).

**Topology:** For \( \gamma > 0 \), the forward ball \( B^+(0, \epsilon) \) contains 1 if \( \sqrt{E_{\text{obs}}^2 + \gamma^2} < \epsilon \). The backward ball \( B^-(0, \epsilon) \) contains 1 if \( \sqrt{E_{\text{erase}}^2 + \gamma^2} < \epsilon \). The join topology captures both directions.

### 6.9.2 Example 2: Continuous Belief Space with Debt

Let \( \mathcal{B} = \mathbb{R} \), with:
- \( d(x, y) = \alpha |y - x| \).
- \( \text{debt}(x, y) = \log(p(x)/p(y)) \) (entropic debt).

Then:
\[
d_\gamma(x, y) = \sqrt{\alpha^2 (y - x)^2 + \gamma^2 \left( \log \frac{p(x)}{p(y)} \right)^2}.
\]

**Interpretation:**
- \( \gamma = 0 \): The agent moves in real space only.
- \( \gamma > 0 \): The agent also moves in probability space.
- The geodesics minimize both real distance and probability distance.

### 6.9.3 Example 3: The Learning Trajectory Revisited

Consider the learning trajectory from Chapter 5:
- Start: \( (c, d) = (0, \text{high debt}) \).
- End: \( (c, d) = (\text{cost}, 0) \).

For different \( \gamma \), the optimal path changes:
- \( \gamma = 0 \): The path minimizes cost only (vertical descent).
- \( \gamma = 1 \): The path minimizes the Euclidean distance (diagonal descent).
- \( 0 < \gamma < 1 \): The path is between vertical and diagonal.

**Interpretation:** \( \gamma \) controls how much the agent values debt reduction relative to cost minimization. Higher \( \gamma \) means the agent takes a more "diagonal" path that pays down debt earlier.

### 6.9.4 Example 4: The Pareto Frontier for Different \( \gamma \)

The Pareto frontier in the cost-debt plane is the set of points where cost cannot be reduced without increasing debt, and vice versa.

For different \( \gamma \), the \( \gamma \)-distance selects different points on the Pareto frontier:
- \( \gamma = 0 \): The point with minimum cost (regardless of debt).
- \( \gamma = 1 \): The point with minimum Euclidean distance to the origin.
- \( 0 < \gamma < 1 \): An intermediate point.

**Interpretation:** The parameter \( \gamma \) is a **preference parameter** that selects different trade-offs on the Pareto frontier. Unlike the classical weighting \( \mu \), \( \gamma \) has a geometric interpretation.

---

## 6.10 Relationship to Reinforcement Learning

### 6.10.1 The \( \gamma \)-Distance for Action-Value Functions

In reinforcement learning (Part II), the \( \gamma \)-distance becomes the objective:
\[
J_\gamma^\pi(s, a) = \sqrt{Q_R^\pi(s, a)^2 + \gamma^2 \cdot Q_I^\pi(s, a)^2}.
\]

The agent minimizes \( J_\gamma^\pi \) for a given \( \gamma \).

### 6.10.2 The \( \gamma \)-Greedy Policy

**Definition 6.21 (\( \gamma \)-Greedy Policy).** Given \( \gamma \in [0, 1] \) and a complex value function \( Q \), the \( \gamma \)-greedy policy is
\[
\pi_\gamma(s) = \arg\min_a \sqrt{Q_R(s, a)^2 + \gamma^2 \cdot Q_I(s, a)^2}.
\]

**Interpretation:**
- \( \gamma = 0 \): The policy minimizes cost only (pure exploitation).
- \( \gamma = 1 \): The policy minimizes the full modulus (balanced).
- \( 0 < \gamma < 1 \): The policy minimizes a weighted combination.

### 6.10.3 The Phase and $\gamma$

The phase for the $\gamma$-distance is:

$$
\theta_\gamma(s, a) = \arctan \left( \frac{\gamma \cdot Q_I(s, a)}{Q_R(s, a)} \right).
$$

**Interpretation:** The parameter $\gamma$ scales the debt component of the phase. Increasing $\gamma$ increases the phase for a given debt-to-cost ratio.

### 6.10.4 The Gamma Derivative in RL

The derivative of the objective with respect to $\gamma$ is:

$$
\frac{\partial J_\gamma^\pi}{\partial \gamma}(s, a) = \frac{\gamma \cdot Q_I^\pi(s, a)^2}{J_\gamma^\pi(s, a)}.
$$

**Interpretation:** The sensitivity of the objective to $\gamma$ is proportional to the square of the debt value. Actions with high debt have higher sensitivity. This can be used to adapt $\gamma$ during learning:
- If the agent wants to reduce debt, increase $\gamma$.
- If the agent wants to reduce cost, decrease $\gamma$.

---

## 6.11 Summary

This chapter has introduced the $\gamma$-distance family:

1. **Definition:** $d_\gamma(b_i, b_j) = \sqrt{d(b_i, b_j)^2 + \gamma^2 \cdot \text{debt}(b_i, b_j)^2}$.

2. **Extremes:**
   - $\gamma = 0$: Pure cost geometry.
   - $\gamma = 1$: Full cost-debt geometry.

3. **Metric Properties:**
   - Identity, positivity.
   - Asymmetry (inherited from cost).
   - Triangle inequality (for all $\gamma \in [0, 1]$).

4. **Geometric Interpretation:**
   - $\gamma$ deforms the metric.
   - Metric balls expand/contract.
   - Geodesics shift from cost-efficient to debt-aware.
   - Level sets are ellipses in the cost-debt plane.

5. **Gamma Derivative:**
   - First derivative: $\partial d_\gamma / \partial \gamma = \gamma \cdot \text{debt}^2 / d_\gamma \geq 0$.
   - Second derivative: $\partial^2 d_\gamma / \partial \gamma^2 = d^2 \cdot \text{debt}^2 / d_\gamma^3 \geq 0$ (convex).
   - Elasticity: $\mathcal{E}_\gamma = \gamma^2 \cdot \text{debt}^2 / d_\gamma^2 \in [0, 1]$.
   - Phase derivative: $\partial \theta_\gamma / \partial \gamma = \text{debt} \cdot d / d_\gamma^2$.
   - Learning schedule: $\gamma(t) = \gamma_0 e^{-\lambda t} + \gamma_\infty$.

6. **Topological Dependence:**
   - The topology $\tau_\gamma$ depends continuously on $\gamma$.
   - $\tau_{\gamma_1} \subseteq \tau_{\gamma_2}$ for $\gamma_1 < \gamma_2$.
   - Beliefs with non-zero debt are separated by $\gamma$.

7. **Bitopological Structure:**
   - Forward topology $\tau_+$: reachable from $b$.
   - Backward topology $\tau_-$: can reach $b$.
   - Join topology $\tau_+ \vee \tau_-$: bidirectional reachability.
   - Average topology $\tau_{\text{avg}}$: symmetric version.

8. **RL Connection:** $d_\gamma$ becomes the objective $J_\gamma^\pi(s, a)$, and the $\gamma$-greedy policy selects actions based on the $\gamma$-distance. The gamma derivative informs adaptive scheduling of $\gamma$.

The $\gamma$-distance family provides a principled way to interpolate between cost-only and balanced decision-making. The derivative with respect to $\gamma$ quantifies how the geometry responds to changes in debt weighting, enabling adaptive learning schedules and sensitivity analysis. In Chapter 7, we study the equilibrium structure of this geometry.

---

## Exercises

**Exercise 6.1 ($\gamma$-Distance Properties).** Verify that $d_\gamma$ satisfies identity, positivity, and the triangle inequality for $\gamma = 0.5$.

**Exercise 6.2 (Metric Balls).** For the two-state belief space, compute $B_\gamma(0, \epsilon)$ for $\gamma = 0, 0.5, 1$. How does the ball change with $\gamma$?

**Exercise 6.3 (Topological Comparison).** Prove that $\tau_{\gamma_1} \subseteq \tau_{\gamma_2}$ for $\gamma_1 < \gamma_2$. Construct an example of a set that is open in $\tau_{\gamma_2}$ but not in $\tau_{\gamma_1}$.

**Exercise 6.4 (Bitopological Spaces).** For the two-state belief space, what are the forward and backward topologies? What is the join topology? What is the average topology?

**Exercise 6.5 (Pareto Frontier).** Consider the Pareto frontier $d^2 + \delta^2 = 1$. For $\gamma = 0.5$, find the point on the frontier that minimizes $d_\gamma$. Compare with $\gamma = 0$ and $\gamma = 1$.

**Exercise 6.6 (Geodesics).** For the continuous belief space with $d(x, y) = |y - x|$ and $\text{debt}(x, y) = y - x$, find the geodesic from $x = 0$ to $y = 1$ for $\gamma = 0, 0.5, 1$. How does the geodesic change?

**Exercise 6.7 (Gamma Derivative).** Compute $\partial d_\gamma / \partial \gamma$ for the two-state belief space with $ E_{\text{obs}} = 1$, $E_{\text{erase}} = 2$, and $\gamma = 0.5$. Interpret the result.

**Exercise 6.8 (Gamma Elasticity).** For the continuous belief space with $d(x, y) = |y - x|$ and $\text{debt}(x, y) = \log(p(x)/p(y))$, compute the elasticity $\mathcal{E}_\gamma$. For what values of $\gamma$ is the elasticity $1/2$?

**Exercise 6.9 (Learning Schedule).** Suppose $\gamma(t) = e^{-t} + 0.1$. Compute $\partial d_\gamma / \partial \gamma$ at $t = 0, 1, 2$ for a pair of beliefs with $d = 1$, $\text{debt} = 1$. How does the sensitivity change over time?

**Exercise 6.10 (Optimal $\gamma$).** Given a fixed pair of beliefs with $d = 1$, $\text{debt} = 2$, find the value of $ \gamma \in [0, 1]$ that minimizes $d_\gamma$. What is the interpretation?

---

## Further Reading

- Fletcher, P. & Lindgren, W. F. (1982). *Quasi-Uniform Spaces*. Marcel Dekker. — A comprehensive treatment of quasi-metrics and bitopological spaces.

- Kelly, J. C. (1963). "Bitopological Spaces." *Proceedings of the London Mathematical Society*, 3(1):71-89. — The foundational paper on bitopological spaces.

- Kopperman, R. (1995). "Asymmetry and Duality in Topology." *Topology and its Applications*, 66(1):1-39. — A modern perspective on bitopological spaces.

- Amari, S. (2016). *Information Geometry and Its Applications*. Springer. — The geometry of probability spaces.

- Rockafellar, R. T. (1970). *Convex Analysis*. Princeton University Press. — Background on convexity and derivatives.


