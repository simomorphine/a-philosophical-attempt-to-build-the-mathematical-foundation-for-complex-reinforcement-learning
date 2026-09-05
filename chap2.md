# Chapter 2: Mathematical Preliminaries

---

## 2.1 Introduction

### 2.1.1 The Purpose of This Chapter

Chapter 1 established the central problem of this book: every adaptive agent faces two qualitatively distinct burdens—real cost ($c$) and information debt ($d$)—and classical reinforcement learning's scalarisation approach fails to balance them naturally. We proposed a geometric alternative: encode cost and debt as the real and imaginary parts of a complex number $z = c + id$, and minimize the modulus $|z| = \sqrt{c^2 + d^2}$.

This geometric insight, while powerful, requires mathematical tools that go beyond standard reinforcement learning. The complex plane is not just a convenient notation—it is a *field* with a rich algebraic and analytic structure. To fully develop the complex framework, we need:

- **Complex analysis:** To understand holomorphic functions, their properties, and why the Bellman optimality operator fails to contract.
- **Wirtinger calculus:** To differentiate non-holomorphic functions—essential for policy gradients and optimization.
- **Information theory:** To interpret debt as entropy reduction and mutual information gain.
- **Geometry and topology:** To understand the structure of state spaces, quasi-metrics, and bitopological spaces.
- **Functional analysis:** To prove contraction properties and establish convergence guarantees.

This chapter provides these mathematical foundations. The presentation is self-contained but concise; readers familiar with these topics may skip ahead.

---

### 2.1.2 What We Assume the Reader Already Knows

We assume the reader has completed Chapter 1 and is familiar with:

1. **The two burdens:** Real cost ($c$) and information debt ($d$), their properties, and their qualitative differences.
2. **The cost-debt plane:** The geometric interpretation of cost and debt as orthogonal axes.
3. **The failure of scalarisation:** The free parameter problem, the Pareto frontier, and why no single $\mu$ works universally.
4. **The complex encoding:** $z = c + id$, the modulus $|z|$, and the phase $\theta = \arctan(d/c)$.
5. **Basic RL concepts:** MDPs, policies, value functions, Bellman equations, and the scalar max operator.

We do *not* assume prior knowledge of complex analysis, Wirtinger calculus, information theory beyond basic definitions, quasi-metrics, bitopological spaces, or functional analysis beyond basic normed spaces. These are introduced in this chapter.

---

### 2.1.3 How This Chapter Is Organized

This chapter is organized into eight sections:

| Section | Topic | Why It Matters |
|---------|-------|----------------|
| **2.2** | Complex Numbers: A Refresher | The foundational object of the entire book |
| **2.3** | The Extended Complex Plane | Compactness for the Bergman conjecture |
| **2.4** | Holomorphic Functions | The obstruction to contraction (OP1) |
| **2.5** | Wirtinger Calculus | Differentiating non-holomorphic functions |
| **2.6** | Information Theory | Debt as entropy reduction (Chapter 9) |
| **2.7** | Geometry and Topology | Quasi-metrics and bitopological spaces (Chapters 3-6) |
| **2.8** | Functional Analysis | Contraction mappings and fixed points (Chapter 9) |
| **2.9** | Summary and Roadmap | Connecting the math to the rest of the book |

---

### 2.1.4 Who Should Read What

- **Familiar with complex numbers?** You can skip Sections 2.2.1-2.2.3, but read 2.2.4-2.2.5.
- **Familiar with complex analysis?** You can skip most of Sections 2.2 and 2.3, but read 2.3.3 (why compactness matters) and 2.4.4 (why holomorphic functions matter for RL).
- **Familiar with information theory?** You can skip Section 2.6.1-2.6.3, but read 2.6.4-2.6.5 (the epistemic potential).
- **Familiar with geometry?** You can skip Section 2.7.1, but read 2.7.2-2.7.6 (quasi-metrics and bitopological spaces).
- **Familiar with functional analysis?** You can skip Section 2.8.1-2.8.3, but read 2.8.4-2.8.5 (Bellman operators and the obstruction).

For readers new to these topics, we recommend reading the entire chapter in order.

---

### 2.1.5 Key Takeaways

1. **Purpose:** This chapter provides the mathematical tools for the complex framework.
2. **Scope:** Complex analysis, Wirtinger calculus, information theory, geometry, topology, and functional analysis.
3. **Assumptions:** Familiarity with Chapter 1 and basic RL concepts.
4. **Organization:** Eight sections, each motivated by a specific need in later chapters.
5. **Flexibility:** Readers can skip sections they already know.

---

## 2.2 Complex Numbers: A Refresher

### 2.2.1 Definition and Basic Arithmetic

**Definition 2.1 (Complex Number).** A complex number is an expression of the form:

$$
z = a + ib
$$

where:
- $a, b \in \mathbb{R}$ are real numbers
- $i = \sqrt{-1}$ is the imaginary unit, satisfying $i^2 = -1$

**Terminology:**
- $\mathrm{Re}(z) = a$ is the **real part**
- $\mathrm{Im}(z) = b$ is the **imaginary part** (a real number!)

**Examples:**
- $z = 3 + 4i$: $\mathrm{Re}(z) = 3$, $\mathrm{Im}(z) = 4$
- $z = -2 + i$: $\mathrm{Re}(z) = -2$, $\mathrm{Im}(z) = 1$
- $z = 5$: $\mathrm{Re}(z) = 5$, $\mathrm{Im}(z) = 0$ (purely real)
- $z = 3i$: $\mathrm{Re}(z) = 0$, $\mathrm{Im}(z) = 3$ (purely imaginary)

**Arithmetic Operations:**

1. **Addition:**
   
$$
(a + ib) + (c + id) = (a + c) + i(b + d)
$$

2. **Subtraction:**
   
$$
(a + ib) - (c + id) = (a - c) + i(b - d)
$$

3. **Multiplication:**
   
$$
(a + ib)(c + id) = (ac - bd) + i(ad + bc)
$$

4. **Division:**
   
$$
\frac{a + ib}{c + id} = \frac{(a + ib)(c - id)}{(c + id)(c - id)} = \frac{(ac + bd) + i(bc - ad)}{c^2 + d^2}
$$

**Example 2.1 (Complex Arithmetic).**

Compute $(3 + 4i) + (2 - i)$:

$$
(3 + 4i) + (2 - i) = (3 + 2) + i(4 - 1) = 5 + 3i
$$

Compute $(3 + 4i)(2 - i)$:

$$
(3 + 4i)(2 - i) = 6 - 3i + 8i - 4i^2 = 6 + 5i + 4 = 10 + 5i
$$

Compute $(3 + 4i)/(2 - i)$:

$$
\frac{3 + 4i}{2 - i} = \frac{(3 + 4i)(2 + i)}{(2 - i)(2 + i)} = \frac{6 + 3i + 8i + 4i^2}{4 + 1} = \frac{2 + 11i}{5} = \frac{2}{5} + \frac{11}{5}i
$$

---

### 2.2.2 The Complex Conjugate

**Definition 2.2 (Complex Conjugate).** The complex conjugate of $z = a + ib$ is:

$$
\bar{z} = a - ib
$$

**Geometric Meaning:** Conjugation reflects the point $z$ across the real axis in the complex plane.

**Properties of the Conjugate:**

1. $\overline{z_1 + z_2} = \bar{z}_1 + \bar{z}_2$
2. $\overline{z_1 z_2} = \bar{z}_1 \bar{z}_2$
3. $\overline{z_1 / z_2} = \bar{z}_1 / \bar{z}_2$
4. $\overline{\bar{z}} = z$
5. $z + \bar{z} = 2\mathrm{Re}(z)$
6. $z - \bar{z} = 2i\mathrm{Im}(z)$
7. $z\bar{z} = \|z\|^2$ (see Section 2.2.3)

**Example 2.2 (Conjugation).**

For $z = 3 + 4i$:

$$
\bar{z} = 3 - 4i
$$

Verify properties:

$$
z + \bar{z} = 6 = 2\mathrm{Re}(z)
$$

$$
z - \bar{z} = 8i = 2i\mathrm{Im}(z)
$$

$$
z\bar{z} = (3+4i)(3-4i) = 9 + 16 = 25 = |z|^2
$$

---

### 2.2.3 Modulus and Argument

**Definition 2.3 (Modulus).** The modulus (or magnitude) of $z = a + ib$ is:

$$
\|z\| = \sqrt{a^2 + b^2}
$$

The modulus is the Euclidean distance from the origin to the point $z$ in the complex plane.

**Definition 2.4 (Argument).** The argument of $z = a + ib$ is the angle $\theta$ such that:

$$
\cos \theta = \frac{a}{\|z\|}, \quad \sin \theta = \frac{b}{\|z\|}
$$

The **principal argument** is $\mathrm{Arg}(z) \in (-\pi, \pi]$.

**Properties of the Modulus:**

1. **Non-negativity:** $|z| \geq 0$, with equality iff $z = 0$.
2. **Multiplicativity:** $|z_1 z_2| = |z_1| \cdot |z_2|$.
3. **Division:** $|z_1 / z_2| = |z_1| / |z_2|$ for $z_2 \neq 0$.
4. **Triangle Inequality:** $|z + w| \leq |z| + |w|$.
5. **Reverse Triangle Inequality:** $\bigl||z| - |w|\bigr| \leq |z - w|$.
6. **Conjugate Product:** $z\bar{z} = |z|^2$.

**Properties of the Argument:**

1. $\arg(z_1 z_2) = \arg(z_1) + \arg(z_2)$ (mod $2\pi$).
2. $\arg(z_1 / z_2) = \arg(z_1) - \arg(z_2)$ (mod $2\pi$).
3. $\arg(\bar{z}) = -\arg(z)$ (mod $2\pi$).

**Example 2.3 (Modulus and Argument).**

For $z = 3 + 4i$:

$$
\|z\| = \sqrt{3^2 + 4^2} = \sqrt{9 + 16} = \sqrt{25} = 5
$$

$$
\theta = \arctan\left(\frac{4}{3}\right) \approx 53.13^\circ
$$

Verification:

$$
\cos(53.13^\circ) = 3/5, \quad \sin(53.13^\circ) = 4/5
$$

---

### 2.2.4 Polar Form and Euler's Formula

**Definition 2.5 (Polar Form).** Any complex number $z \neq 0$ can be written in polar form:

$$
z = |z|(\cos \theta + i \sin \theta) = |z| e^{i\theta}
$$

where $\theta = \arg(z)$ and $e^{i\theta} = \cos \theta + i \sin \theta$ is **Euler's formula**.

**Euler's Formula:**

$$
e^{i\theta} = \cos \theta + i \sin \theta
$$

**Special Cases:**
- $e^{i\pi} = -1$
- $e^{i\pi/2} = i$
- $e^{i\pi/4} = \frac{1}{\sqrt{2}} + \frac{i}{\sqrt{2}}$
- $e^{i0} = 1$
- $e^{i2\pi} = 1$

**Multiplication in Polar Form:**

If $z_1 = r_1 e^{i\theta_1}$ and $z_2 = r_2 e^{i\theta_2}$, then:

$$
z_1 z_2 = r_1 r_2 e^{i(\theta_1 + \theta_2)}
$$

**Geometric Interpretation:** Multiplication of complex numbers corresponds to scaling by $r_1 r_2$ and rotation by $\theta_1 + \theta_2$.

**Division in Polar Form:**

$$
\frac{z_1}{z_2} = \frac{r_1}{r_2} e^{i(\theta_1 - \theta_2)}
$$

**Powers (De Moivre's Formula):**

$$
z^n = r^n e^{in\theta} = r^n(\cos n\theta + i\sin n\theta)
$$

**Example 2.4 (Polar Form).**

For $z = 3 + 4i$:

$$
z = 5 \cdot e^{i\arctan(4/3)} = 5e^{i53.13^\circ}
$$

Compute $z^2$ in polar form:

$$
z^2 = 25 \cdot e^{i106.26^\circ} = 25(\cos 106.26^\circ + i\sin 106.26^\circ) = -7 + 24i
$$

Verify by direct multiplication:

$$
(3+4i)^2 = 9 + 24i + 16i^2 = -7 + 24i \quad \checkmark
$$

---

### 2.2.5 The Modulus of Expectation

A crucial lemma for the complex framework:

**Lemma 2.1 (Modulus of Expectation).** For any $\mathbb{C}$-valued random variable $Z$ with $\mathbb{E}[|Z|] < \infty$:

$$
|\mathbb{E}[Z]| \leq \mathbb{E}[|Z|]
$$

**Proof:**

Let $Z = X + iY$. Then:

$$
|\mathbb{E}[Z]| = \sqrt{(\mathbb{E}[X])^2 + (\mathbb{E}[Y])^2}
$$

By Jensen's inequality (since $t \mapsto \sqrt{t}$ is concave):

$$
\sqrt{(\mathbb{E}[X])^2 + (\mathbb{E}[Y])^2} \leq \mathbb{E}\left[\sqrt{X^2 + Y^2}\right] = \mathbb{E}[|Z|]
$$

$\square$

**Why This Matters:**

Our primary performance criterion will be $|\mathbb{E}[G_t]|$—the modulus of the expected complex return. Lemma 2.1 tells us that:

$$
|\mathbb{E}[G_t]| \leq \mathbb{E}[|G_t|]
$$

This means minimizing $|\mathbb{E}[G_t]|$ is a *lower bound* on the harder criterion $\mathbb{E}[|G_t|]$. This makes the problem tractable—we can optimize the expected return rather than the expected modulus.

---

### 2.2.6 Key Takeaways

1. **Complex Numbers:** $z = a + ib$, with real part $a$ and imaginary part $b$.
2. **Arithmetic:** Addition, subtraction, multiplication, and division follow natural rules.
3. **Conjugate:** $\bar{z} = a - ib$, useful for computing modulus and real/imaginary parts.
4. **Modulus:** $|z| = \sqrt{a^2 + b^2}$, the Euclidean distance from the origin.
5. **Argument:** $\theta = \arg(z)$, the angle from the real axis.
6. **Polar Form:** $z = |z| e^{i\theta}$, useful for multiplication and powers.
7. **Modulus of Expectation:** $|\mathbb{E}[Z]| \leq \mathbb{E}[|Z|]$, crucial for the complex framework.

---

### 2.2.7 Exercises for Section 2.2

**Exercise 2.1 (Complex Arithmetic).** Compute:
1. $(3 + 4i) + (2 - i)$
2. $(3 + 4i)(2 - i)$
3. $(3 + 4i)/(2 - i)$

**Exercise 2.2 (Modulus and Argument).** For $z = -3 + 4i$:
1. Compute $|z|$ and $\arg(z)$.
2. Write $z$ in polar form.
3. Compute $z^2$ using polar form and verify by direct multiplication.

**Exercise 2.3 (Conjugate Properties).** Prove that $z\bar{z} = |z|^2$ for any complex number $z$. Use this to derive the formula for division: $1/(a+ib) = (a-ib)/(a^2+b^2)$.

**Exercise 2.4 (Triangle Inequality).** Prove the triangle inequality for complex numbers: $|z + w| \leq |z| + |w|$. *Hint: Use* $\|z+w\|^2 = (z+w)(\bar{z}+\bar{w})$.

**Exercise 2.5 (Modulus of Expectation).** Let $Z$ be a complex random variable taking values $\{1+i, 2-3i, -1+2i\}$ with probabilities $1/3$ each. Compute $\mathbb{E}[Z]$, $|\mathbb{E}[Z]|$, and $\mathbb{E}[|Z|]$. Verify that $|\mathbb{E}[Z]| \leq \mathbb{E}[|Z|]$.

---

## 2.3 The Extended Complex Plane and the Riemann Sphere

### 2.3.1 Why We Need the Extended Plane

In standard complex analysis, we work with the complex plane $\mathbb{C}$. However, for certain theoretical results—particularly the Bergman conjecture—we need a **compact** space. The complex plane $\mathbb{C}$ is not compact: sequences can "escape to infinity" without converging.

To make $\mathbb{C}$ compact, we add a single point at infinity:

$$
\hat{\mathbb{C}} = \mathbb{C} \cup \{\infty\}
$$

This is called the **extended complex plane** or the **Riemann sphere**.

**Why Compactness Matters:**

1. **Montel's Theorem:** On a compact domain, bounded families of holomorphic functions are precompact (every sequence has a convergent subsequence).

2. **Schauder's Fixed-Point Theorem:** A continuous operator mapping a compact convex subset of a Banach space into itself has a fixed point.

3. **The Bergman Conjecture:** The Bellman optimality operator $T$ may be a contraction on the Bergman space $\mathcal{A}^2(\Omega)$ because the domain is bounded (hence compact in the appropriate topology).

4. **Convergence Guarantees:** Compactness ensures that iterative algorithms converge to a fixed point.

---

### 2.3.2 Definition of the Extended Complex Plane

**Definition 2.6 (Extended Complex Plane).** The extended complex plane is:

$$
\hat{\mathbb{C}} = \mathbb{C} \cup \{\infty\}
$$

where $\infty$ is a single point at infinity, not a complex number.

**Operations with Infinity:**

For $z \in \mathbb{C}$:

1. $z + \infty = \infty$
2. $z \cdot \infty = \infty$ (for $z \neq 0$)
3. $z / 0 = \infty$ (for $z \neq 0$)
4. $z / \infty = 0$ (for $z \neq \infty$)
5. $\infty / z = \infty$ (for $z \neq 0$)

**Undefined Operations:**

1. $\infty + \infty$ (undefined)
2. $\infty - \infty$ (undefined)
3. $0 \cdot \infty$ (undefined)
4. $\infty / \infty$ (undefined)
5. $0 / 0$ (undefined)

---

### 2.3.3 The Riemann Sphere

**Definition 2.7 (Riemann Sphere).** The Riemann sphere is a geometric representation of the extended complex plane $\hat{\mathbb{C}}$ as a sphere.

**Construction:**

1. Place a unit sphere on the complex plane, tangent at the origin.
2. The **south pole** is at the bottom of the sphere (touching the origin).
3. The **north pole** is at the top of the sphere.
4. **Stereographic projection** maps points from the sphere to the complex plane (and vice versa).

```
          North Pole (∞)
               ▲
              /|\
             / | \
            /  |  \
           /   |   \
          /    |    \
         /     |     \
        /      |      \
       /       |       \
      /        |        \
     /         |         \
    /          |          \
   /           |           \
  /            |            \
 /             |             \
South Pole (0) +-------------→ Complex Plane
```

**Stereographic Projection Formula:**

For a point $(x, y, z)$ on the unit sphere (with $x^2 + y^2 + z^2 = 1$), the stereographic projection to the complex plane is:

$$
w = \frac{x + iy}{1 - z}
$$

Conversely, for a complex number $w = u + iv$, the corresponding point on the sphere is:

$$
x = \frac{2u}{1 + |w|^2}, \quad y = \frac{2v}{1 + |w|^2}, \quad z = \frac{|w|^2 - 1}{|w|^2 + 1}
$$

**Properties of Stereographic Projection:**

1. **Conformal:** Preserves angles (but not distances).
2. **Bijective:** One-to-one correspondence between $\hat{\mathbb{C}}$ and the sphere.
3. **Continuous:** The mapping and its inverse are continuous.
4. **Circle-Preserving:** Circles on the sphere map to circles or lines in the plane.

---

### 2.3.4 Compactness of $\hat{\mathbb{C}}$

**Theorem 2.1 (Compactness of the Riemann Sphere).** The extended complex plane $\hat{\mathbb{C}}$ is compact.

**Proof Sketch:**

The Riemann sphere is a closed and bounded subset of $\mathbb{R}^3$ (the unit sphere). By the Heine-Borel theorem, it is compact. Since $\hat{\mathbb{C}}$ is homeomorphic to the Riemann sphere via stereographic projection, $\hat{\mathbb{C}}$ is also compact. $\square$

**Definition 2.8 (Compact Set).** A set $K$ is compact if every open cover has a finite subcover.

**Intuition for Compactness:**

- **Boundedness:** No sequence can "escape to infinity" because infinity is included as a point.
- **Closedness:** The set contains all its limit points.
- **Finite Subcover Property:** Any open cover has a finite subcover (the formal definition).

**Why Compactness Matters for RL:**

The Bergman space $\mathcal{A}^2(\Omega)$ on a bounded domain $\Omega \subset \mathbb{C}$ has a compact inclusion into the space of continuous functions on $\Omega$. This compactness is essential for:

1. **Montel's Theorem:** Bounded families of holomorphic functions are normal (have convergent subsequences).
2. **Schauder's Fixed-Point Theorem:** Compact operators on convex sets have fixed points.
3. **The Bergman Conjecture:** The Bellman optimality operator $T$ may be a contraction on $\mathcal{A}^2(\Omega)$ due to the compactness of the domain.

---

### 2.3.5 Montel's Theorem

**Theorem 2.2 (Montel's Theorem).** A family $\mathcal{F}$ of holomorphic functions on a domain $\Omega \subset \mathbb{C}$ is **normal** (every sequence has a subsequence that converges uniformly on compact subsets) if and only if it is **locally uniformly bounded** (for every compact $K \subset \Omega$, there exists $M_K$ such that $|f(z)| \leq M_K$ for all $f \in \mathcal{F}$ and $z \in K$).

**Why This Matters:**

In later chapters, we will consider the space of holomorphic Q-functions on a bounded domain $\Omega$. Montel's theorem ensures that bounded sequences of such functions have convergent subsequences, which is essential for proving the existence of fixed points.

---

### 2.3.6 Schauder's Fixed-Point Theorem

**Theorem 2.3 (Schauder's Fixed-Point Theorem).** Let $X$ be a Banach space, $K \subset X$ a non-empty compact convex set, and $T: K \to K$ a continuous operator. Then $T$ has a fixed point $x^\* \in K$ such that $T(x^\*) = x^\*$.

**Why This Matters:**

If we can show that the Bellman optimality operator $T$ maps a compact convex set of holomorphic Q-functions into itself, then Schauder's theorem guarantees the existence of a fixed point $Q^*$. This would resolve (Existence and Uniqueness of the Bellman operator) in the affirmative.

**The Challenge:**

Showing that $T$ maps the set into itself and is continuous requires:

1. **Compactness of the domain:** The Bergman space on a bounded domain has compact inclusion into $C(\Omega)$.
2. **Holomorphic closure:** The modulus-greedy policy $\pi_Q(s) = \arg\min_a |Q(s,a)|$ must preserve holomorphicity.
3. **Continuity:** $T$ must be continuous in the Bergman norm.

---

### 2.3.7 Connection to the Bergman Conjecture 

The Bergman conjecture:

> **Conjecture (Bergman).** The Bellman optimality operator $T$ is a contraction on the Bergman space $\mathcal{A}^2(\Omega)$ for a suitable domain $\Omega \subset \mathbb{C}$.

**How Compactness Supports the Conjecture:**

1. **Bounded Domain:** The Bergman space is defined on a bounded domain $\Omega$, which is compact in the appropriate topology.

2. **Compact Inclusion:** The inclusion $\mathcal{A}^2(\Omega) \hookrightarrow C(\Omega)$ is compact, meaning bounded sets in $\mathcal{A}^2$ are precompact in $C(\Omega)$.

3. **Montel's Theorem:** Bounded families of holomorphic functions on $\Omega$ are normal.

4. **Schauder's Theorem:** If $T$ maps a compact convex set into itself, it has a fixed point.

**The Open Question:**

Is $T$ a contraction (strictly reducing the distance between functions), or merely a continuous map with a fixed point? The compactness of the domain suggests that contraction may hold, but this remains an open problem.

---

### 2.3.8 Key Takeaways

1. **Extended Complex Plane:** $\hat{\mathbb{C}} = \mathbb{C} \cup \{\infty\}$, adding a point at infinity.
2. **Riemann Sphere:** A geometric representation of $\hat{\mathbb{C}}$ as a sphere.
3. **Compactness:** $\hat{\mathbb{C}}$ is compact, unlike $\mathbb{C}$.
4. **Montel's Theorem:** Bounded families of holomorphic functions are normal on compact domains.
5. **Schauder's Fixed-Point Theorem:** Continuous operators on compact convex sets have fixed points.
6. **Bergman conjecture:** Compactness supports the Bergman conjecture that $T$ is a contraction on $\mathcal{A}^2(\Omega)$.
7. **Schauder's theorem:** Schauder's theorem may resolve the existence question for $Q^*$.

---

### 2.3.9 Exercises for Section 2.3

**Exercise 2.6 (Stereographic Projection).** For the complex number $w = 1 + i$, compute its stereographic projection onto the Riemann sphere. That is, find $(x, y, z)$ on the unit sphere such that the projection is $w$.

**Exercise 2.7 (Compactness).** Explain why $\mathbb{C}$ is not compact but $\hat{\mathbb{C}}$ is compact. Give an example of a sequence in $\mathbb{C}$ that does not converge but converges in $\hat{\mathbb{C}}$.

**Exercise 2.8 (Montel's Theorem).** State Montel's theorem in your own words. Why is it useful for proving the existence of fixed points in the Bergman space?

**Exercise 2.9 (Schauder's Theorem).** Let $X = \mathbb{R}$, $K = [0, 1]$, and $T(x) = x/2$. Show that $T$ maps $K$ into itself and has a fixed point. Does this example satisfy the conditions of Schauder's theorem? What is the fixed point?

**Exercise 2.10 (Extended Plane Operations).** Determine whether the following operations are defined in $\hat{\mathbb{C}}$:
1. $\infty + 5$
2. $\infty - \infty$
3. $0 \cdot \infty$
4. $3/0$
5. $\infty/2$
6. $\infty/\infty$

---

### 2.3.10 Further Reading for Section 2.3

- Ahlfors, L. V. (1979). *Complex Analysis*, 3rd ed. McGraw-Hill. — Sections on the extended plane and Riemann sphere.

- Needham, T. (1997). *Visual Complex Analysis*. Oxford University Press. — Chapter 3 covers stereographic projection and the Riemann sphere with beautiful visual explanations.

- Conway, J. B. (1978). *Functions of One Complex Variable*, 2nd ed. Springer. — Chapter 1 covers the extended plane and compactness.

- Rudin, W. (1987). *Real and Complex Analysis*, 3rd ed. McGraw-Hill. — Chapter 10 covers Montel's theorem and normal families.

- Kreyszig, E. (1989). *Introductory Functional Analysis with Applications*. Wiley. — Chapter 9 covers Schauder's fixed-point theorem.

---

## 2.4 Holomorphic Functions

### 2.4.1 Why Holomorphic Functions Matter for RL

In classical reinforcement learning with scalar rewards, the Bellman optimality operator:

$$
(TQ)(s,a) = \sum_{s'} p(s'|s,a) \left[ r(s,a,s') + \lambda \max_{a'} Q(s',a') \right]
$$

is a contraction because:

1. The max operator is non-expansive in $\mathbb{R}$:

$$
\| \max_a f(a) - \max_a g(a) \| \leq \max_a \|f(a) - g(a)\|
$$

3. The transition kernel averages probabilities, which is also non-expansive.

In the complex framework, the Bellman optimality operator becomes:

$$
(TQ)(s,a) = \sum_{s'} p(s'|s,a) \left[ z(s,a,s') + \lambda Q(s', \pi_Q(s')) \right]
$$

where $\pi_Q(s') = \arg\min_{a'} |Q(s',a')|$.

**The Problem:** The modulus-greedy selector $\arg\min_a |Q(s,a)|$ is **discontinuous** in general. The contraction proof fails because $\mathbb{C}$ is not totally ordered.

**The Key Insight:** The obstruction to contraction is exactly the **non-holomorphic component** of the Q-function. If Q-functions were holomorphic, the modulus-greedy selector would be well-behaved.

**This is why we need to understand holomorphic functions.**

---

### 2.4.2 Definition of Holomorphic Functions

**Definition 2.9 (Holomorphic Function).** A function $f: \Omega \to \mathbb{C}$ on an open domain $\Omega \subset \mathbb{C}$ is **holomorphic** (or **complex-differentiable**) if the complex derivative:

$$
f'(z_0) = \lim_{z \to z_0} \frac{f(z) - f(z_0)}{z - z_0}
$$

exists for all $z_0 \in \Omega$.

**Alternative Names:**
- **Analytic:** Equivalent to holomorphic for complex functions (they have power series expansions).
- **Regular:** Another term for holomorphic.
- **Complex-Differentiable:** The defining property.

**Examples of Holomorphic Functions:**

| Function | Domain | Derivative |
|----------|--------|------------|
| $f(z) = c$ (constant) | $\mathbb{C}$ | $f'(z) = 0$ |
| $f(z) = z^n$ | $\mathbb{C}$ | $f'(z) = n z^{n-1}$ |
| $f(z) = e^z$ | $\mathbb{C}$ | $f'(z) = e^z$ |
| $f(z) = \sin z$ | $\mathbb{C}$ | $f'(z) = \cos z$ |
| $f(z) = \cos z$ | $\mathbb{C}$ | $f'(z) = -\sin z$ |
| $f(z) = \log z$ | $\mathbb{C} \setminus (-\infty, 0]$ | $f'(z) = 1/z$ |
| $f(z) = 1/z$ | $\mathbb{C} \setminus \{0\}$ | $f'(z) = -1/z^2$ |

**Non-Examples (Not Holomorphic):**

| Function | Why Not Holomorphic |
|----------|---------------------|
| $f(z) = \bar{z}$ | Depends on $\bar{z}$, not just $z$ |
| $f(z) = \|z\|^2$ | Depends on $\bar{z}$ |
| $f(z) = \mathrm{Re}(z)$ | Depends on $\bar{z}$ |
| $f(z) = \mathrm{Im}(z)$ | Depends on $\bar{z}$ |

**Example 2.5 (Checking Holomorphicity).**

For $f(z) = z^2$:

$$
\lim_{h \to 0} \frac{(z+h)^2 - z^2}{h} = \lim_{h \to 0} \frac{2zh + h^2}{h} = \lim_{h \to 0} (2z + h) = 2z
$$

So $f'(z) = 2z$ exists for all $z \in \mathbb{C}$. Therefore, $f(z) = z^2$ is holomorphic.

For $f(z) = \bar{z}$:

$$
\lim_{h \to 0} \frac{\overline{z+h} - \bar{z}}{h} = \lim_{h \to 0} \frac{\bar{h}}{h}
$$

This limit does not exist because it depends on the direction of approach:
- If $h \in \mathbb{R}$, then $\bar{h}/h = 1$.
- If $h \in i\mathbb{R}$, then $\bar{h}/h = -1$.

Therefore, $f(z) = \bar{z}$ is **not** holomorphic.

---

### 2.4.3 The Cauchy-Riemann Equations

**Theorem 2.4 (Cauchy-Riemann Equations).** Let $f(z) = u(x,y) + iv(x,y)$ where $z = x + iy$ and $u, v: \mathbb{R}^2 \to \mathbb{R}$ are real-valued functions. Then $f$ is holomorphic on $\Omega$ if and only if $u$ and $v$ are continuously differentiable and satisfy the **Cauchy-Riemann equations**:

$$
\frac{\partial u}{\partial x} = \frac{\partial v}{\partial y}, \qquad \frac{\partial u}{\partial y} = -\frac{\partial v}{\partial x}
$$

**Derivation:**

The complex derivative must be independent of the direction of approach.

**Approach along the x-axis (real direction):**

$$
f'(z) = \frac{\partial f}{\partial x} = \frac{\partial u}{\partial x} + i\frac{\partial v}{\partial x}
$$

**Approach along the y-axis (imaginary direction):**

$$
f'(z) = \frac{1}{i}\frac{\partial f}{\partial y} = -i\left(\frac{\partial u}{\partial y} + i\frac{\partial v}{\partial y}\right) = \frac{\partial v}{\partial y} - i\frac{\partial u}{\partial y}
$$

For these to be equal, we need:

$$
\frac{\partial u}{\partial x} = \frac{\partial v}{\partial y}, \qquad \frac{\partial v}{\partial x} = -\frac{\partial u}{\partial y}
$$

which are exactly the Cauchy-Riemann equations.

**Example 2.6 (Verifying Cauchy-Riemann).**

For $f(z) = z^2 = (x+iy)^2 = (x^2 - y^2) + i(2xy)$:
- $u(x,y) = x^2 - y^2$
- $v(x,y) = 2xy$

Check Cauchy-Riemann:

$$
\frac{\partial u}{\partial x} = 2x, \qquad \frac{\partial v}{\partial y} = 2x \quad \checkmark
$$

$$
\frac{\partial u}{\partial y} = -2y, \qquad -\frac{\partial v}{\partial x} = -2y \quad \checkmark
$$

So $f(z) = z^2$ is holomorphic.

For $f(z) = \bar{z} = x - iy$:
- $u(x,y) = x$
- $v(x,y) = -y$

Check Cauchy-Riemann:

$$
\frac{\partial u}{\partial x} = 1, \qquad \frac{\partial v}{\partial y} = -1 \quad \text{NOT equal}
$$

So $f(z) = \bar{z}$ is **not** holomorphic.

**The Wirtinger Form of Cauchy-Riemann:**

A function is holomorphic if and only if:

$$
\frac{\partial f}{\partial \bar{z}} = 0
$$

This will be useful when we discuss Wirtinger calculus in Section 2.5.

---

### 2.4.4 Key Properties of Holomorphic Functions

Holomorphic functions have remarkable properties that make them fundamentally different from real-differentiable functions:

**1. Power Series Representation:**

Every holomorphic function has a power series expansion around any point in its domain:

$$
f(z) = \sum_{n=0}^{\infty} a_n (z - z_0)^n
$$

This converges in a neighborhood of $z_0$. This is why holomorphic functions are also called **analytic**.

**2. Open Mapping Theorem:**

A non-constant holomorphic function maps open sets to open sets. This means holomorphic functions cannot "flatten" regions—they preserve openness.

**3. Maximum Modulus Principle:**

If $f$ is holomorphic on a domain $\Omega$ and $|f|$ attains a maximum at an interior point, then $f$ is constant. The maximum of $|f|$ occurs on the boundary of $\Omega$.

**4. Identity Theorem:**

If two holomorphic functions agree on a set with an accumulation point in $\Omega$, they are identical everywhere in $\Omega$. This means holomorphic functions are determined by their values on any set with a limit point.

**5. Liouville's Theorem:**

A bounded entire function (holomorphic on all of $\mathbb{C}$) is constant. This is a powerful result with many consequences.

**6. Argument Principle:**

The change in $\arg f(z)$ around a closed curve equals the number of zeros minus the number of poles (counted with multiplicity).

**7. Cauchy's Integral Formula:**

$$
f(z_0) = \frac{1}{2\pi i} \oint_{\gamma} \frac{f(z)}{z - z_0} dz
$$

This shows that the values of a holomorphic function on the boundary of a region determine its values in the interior.

---

### 2.4.5 Why Holomorphic Functions Matter for RL

**The Bellman Optimality Operator Obstruction:**

Recall the Bellman optimality operator for the complex case:

$$
(TQ)(s,a) = \sum_{s'} p(s'|s,a) \left[ z(s,a,s') + \lambda Q(s', \pi_Q(s')) \right]
$$

where $\pi_Q(s') = \arg\min_{a'} |Q(s',a')|$.

**Why the Standard Proof Fails:**

In the scalar case, the proof of contraction uses:

$$
\left| \max_a f(a) - \max_a g(a) \right| \leq \max_a |f(a) - g(a)|
$$

This relies on $\mathbb{R}$ being totally ordered. In $\mathbb{C}$, there is no total order. The modulus-greedy selector $\arg\min_a |Q(s,a)|$ is **discontinuous** in general.

**The Non-Holomorphic Component:**

The obstruction can be expressed in terms of the non-holomorphic component of $Q$:

$$
\frac{\partial Q}{\partial \bar{z}}
$$

When $Q$ is holomorphic, $\partial Q/\partial \bar{z} = 0$, and the modulus-greedy selector becomes well-behaved. When $Q$ is not holomorphic, the cross-terms between the holomorphic and anti-holomorphic components cause the contraction gap.

**The Bergman Conjecture:**

> **Conjecture (Bergman, OP1).** If Q-functions are restricted to the Bergman space $\mathcal{A}^2(\Omega)$ of holomorphic functions on a bounded domain $\Omega \subset \mathbb{C}$, then the Bellman optimality operator $T$ is a contraction.

**Why This Might Work:**

1. **Holomorphic Q-functions** have $\partial Q/\partial \bar{z} = 0$, eliminating the non-holomorphic obstruction.

2. **The Bergman norm** $\|\cdot\|_{\mathcal{A}^2}$ is compatible with the complex structure and may make $T$ contractive.

3. **Compactness** of the domain (via the Riemann sphere) ensures convergence.

**Connection to Open Problems:**

| Open Problem | Connection to Holomorphic Functions |
|--------------|-------------------------------------|
| **OP1** (Contraction of Optimality Operator) | The non-holomorphic component $\partial Q/\partial \bar{z}$ causes the contraction gap. Holomorphic restriction may resolve this. |
| **OP2** (Existence and Uniqueness) | If $T$ is a contraction on $\mathcal{A}^2(\Omega)$, existence and uniqueness follow from the Banach fixed-point theorem. |
| **OP3** (Convergence of Q-Learning) | Contraction of $T$ would imply convergence of complex Q-learning. |

---

### 2.4.6 Visualizing the Obstruction

Consider two complex Q-functions $Q_1$ and $Q_2$ that differ only in their non-holomorphic component:

$$
Q_1 = Q_h + \epsilon, \qquad Q_2 = Q_h - \epsilon
$$

where $Q_h$ is holomorphic and $\epsilon$ is a small non-holomorphic perturbation.

The modulus-greedy policies may differ:

$$
\pi_{Q_1}(s) = \arg\min_a |Q_h(s,a) + \epsilon(s,a)|
$$
$$
\pi_{Q_2}(s) = \arg\min_a |Q_h(s,a) - \epsilon(s,a)|
$$

If $\epsilon$ is non-holomorphic, these policies can be **discontinuous** functions of $Q$. The Bellman operator $T$ amplifies this discontinuity, leading to:

$$
\|TQ_1 - TQ_2\| \nleq \lambda \|Q_1 - Q_2\|
$$

**The Resolution (Conjecture):** When $Q$ is holomorphic ($\epsilon = 0$), the modulus-greedy selector is continuous, and $T$ becomes a contraction.

---

### 2.4.7 Key Takeaways

1. **Holomorphic:** Complex-differentiable functions with power series expansions.

2. **Cauchy-Riemann:** $u_x = v_y$, $u_y = -v_x$; equivalently, $\partial f/\partial \bar{z} = 0$.

3. **Properties:** Power series, open mapping, maximum modulus, identity theorem, Liouville's theorem.

4. **The Obstruction:** The non-holomorphic component $\partial Q/\partial \bar{z}$ prevents the Bellman optimality operator from contracting.

5. **The Bergman Conjecture:** Restricting Q-functions to the Bergman space of holomorphic functions may resolve OP1.

6. **Connection to OP1, OP2, OP3:** Holomorphic restriction may prove contraction, existence, uniqueness, and convergence.

---

### 2.4.8 Exercises for Section 2.4

**Exercise 2.11 (Checking Holomorphicity).** Determine whether the following functions are holomorphic on $\mathbb{C}$. Justify your answer.
1. $f(z) = z^3$
2. $f(z) = \mathrm{Re}(z) + i\mathrm{Im}(z)$
3. $f(z) = e^z$
4. $f(z) = \|z\|^2$
5. $f(z) = \sin z$

**Exercise 2.12 (Cauchy-Riemann).** For $f(z) = z^3$, identify $u(x,y)$ and $v(x,y)$, verify the Cauchy-Riemann equations, and compute $f'(z)$.

**Exercise 2.13 (Non-Holomorphic Example).** Show that $f(z) = \|z\|^2$ does not satisfy the Cauchy-Riemann equations (except at $z = 0$). Where is $f$ holomorphic?

**Exercise 2.14 (The Obstruction).** Explain in your own words why the non-holomorphic component of the Q-function prevents the Bellman optimality operator from being a contraction. What would happen if Q-functions were holomorphic?

**Exercise 2.15 (Maximum Modulus Principle).** Let $f(z) = z$ on the unit disk $\|z\| \leq 1$. Where does $\|f(z)\|$ attain its maximum? Does this violate the maximum modulus principle? Explain.

---

### 2.4.9 Further Reading for Section 2.4

- Ahlfors, L. V. (1979). *Complex Analysis*, 3rd ed. McGraw-Hill. — Chapters 1-3 cover holomorphic functions, Cauchy-Riemann equations, and key properties.

- Needham, T. (1997). *Visual Complex Analysis*. Oxford University Press. — Chapters 1-4 provide visual intuition for holomorphic functions.

- Conway, J. B. (1978). *Functions of One Complex Variable*, 2nd ed. Springer. — Chapters 1-4 cover the fundamentals.

- Rudin, W. (1987). *Real and Complex Analysis*, 3rd ed. McGraw-Hill. — Chapters 10-13 cover holomorphic functions and their properties.

---


## 2.5 Wirtinger Calculus

### 2.5.1 Motivation: The Problem with Non-Holomorphic Functions

In standard complex analysis, differentiation is defined only for holomorphic functions. However, many of the functions we encounter in the complex framework are **not holomorphic**:

1. **The Squared Modulus:** $f(z) = |z|^2 = z\bar{z}$ is not holomorphic (as shown in Section 2.4.3).
2. **The Real Part:** $f(z) = \mathrm{Re}(z) = (z + \bar{z})/2$ is not holomorphic.
3. **The Imaginary Part:** $f(z) = \mathrm{Im}(z) = (z - \bar{z})/(2i)$ is not holomorphic.
4. **The Objective Function:** In Chapter 11, we will minimize $J(\theta) = |\eta(\theta)|^2$, which is not holomorphic in $\eta$.

**The Challenge:** We need to differentiate these functions to compute gradients for optimization and policy improvement. Standard complex differentiation doesn't work.

**The Solution: Wirtinger Calculus**

Wirtinger calculus treats $z$ and $\bar{z}$ as **independent variables**. This allows us to differentiate non-holomorphic functions by applying the chain rule to $f(z, \bar{z})$.

**The Key Insight:** Although $z$ and $\bar{z}$ are not truly independent (since $\bar{z}$ is determined by $z$), treating them as independent for the purpose of differentiation gives a consistent calculus that works for all complex functions.

---

### 2.5.2 The Wirtinger Derivatives

**Definition 2.10 (Wirtinger Derivatives).** For a function $f(z, \bar{z})$ that is differentiable as a function of $x = \mathrm{Re}(z)$ and $y = \mathrm{Im}(z)$, the **Wirtinger derivatives** are:

$$
\frac{\partial f}{\partial z} = \frac{1}{2}\left(\frac{\partial f}{\partial x} - i\frac{\partial f}{\partial y}\right)
$$

$$
\frac{\partial f}{\partial \bar{z}} = \frac{1}{2}\left(\frac{\partial f}{\partial x} + i\frac{\partial f}{\partial y}\right)
$$

**Why These Definitions?**

Recall that $z = x + iy$ and $\bar{z} = x - iy$. We can solve for $x$ and $y$:

$$
x = \frac{z + \bar{z}}{2}, \qquad y = \frac{z - \bar{z}}{2i}
$$

The chain rule gives:

$$
\frac{\partial}{\partial z} = \frac{\partial x}{\partial z}\frac{\partial}{\partial x} + \frac{\partial y}{\partial z}\frac{\partial}{\partial y}
= \frac{1}{2}\frac{\partial}{\partial x} + \frac{1}{2i}\frac{\partial}{\partial y}
= \frac{1}{2}\left(\frac{\partial}{\partial x} - i\frac{\partial}{\partial y}\right)
$$

Similarly for $\partial/\partial \bar{z}$.

**Key Properties:**

1. **Holomorphic Condition:** A function is holomorphic if and only if $\partial f/\partial \bar{z} = 0$.

2. **Anti-Holomorphic Condition:** A function is anti-holomorphic if and only if $\partial f/\partial z = 0$.

3. **Linearity:** Wirtinger derivatives are linear operators.

4. **Product Rule:** Wirtinger derivatives satisfy the product rule.

5. **Chain Rule:** Wirtinger derivatives satisfy the chain rule (see Section 2.5.4).

---

### 2.5.3 Basic Computations

Let's compute the Wirtinger derivatives for some basic functions.

**Example 2.7 (Derivatives of $z$ and $\bar{z}$).**

For $f(z, \bar{z}) = z$:

$$
\frac{\partial z}{\partial z} = 1, \qquad \frac{\partial z}{\partial \bar{z}} = 0
$$

For $f(z, \bar{z}) = \bar{z}$:

$$
\frac{\partial \bar{z}}{\partial z} = 0, \qquad \frac{\partial \bar{z}}{\partial \bar{z}} = 1
$$

**Example 2.8 (Derivatives of $z^2$ and $\bar{z}^2$).**

For $f(z) = z^2$:

$$
\frac{\partial z^2}{\partial z} = 2z, \qquad \frac{\partial z^2}{\partial \bar{z}} = 0
$$

For $f(z) = \bar{z}^2$:

$$
\frac{\partial \bar{z}^2}{\partial z} = 0, \qquad \frac{\partial \bar{z}^2}{\partial \bar{z}} = 2\bar{z}
$$

**Example 2.9 (Derivatives of $|z|^2$).**

For $f(z, \bar{z}) = |z|^2 = z\bar{z}$:

$$
\frac{\partial |z|^2}{\partial z} = \bar{z}, \qquad \frac{\partial |z|^2}{\partial \bar{z}} = z
$$

**Verification:**

Using the definition:

$$
\frac{\partial |z|^2}{\partial z} = \frac{1}{2}\left(\frac{\partial (x^2+y^2)}{\partial x} - i\frac{\partial (x^2+y^2)}{\partial y}\right)
= \frac{1}{2}(2x - i2y) = x - iy = \bar{z}
$$

Similarly:

$$
\frac{\partial |z|^2}{\partial \bar{z}} = \frac{1}{2}(2x + i2y) = x + iy = z
$$

**Example 2.10 (Derivatives of Real and Imaginary Parts).**

For $f(z, \bar{z}) = \mathrm{Re}(z) = (z + \bar{z})/2$:

$$
\frac{\partial \mathrm{Re}(z)}{\partial z} = \frac{1}{2}, \qquad \frac{\partial \mathrm{Re}(z)}{\partial \bar{z}} = \frac{1}{2}
$$

For $f(z, \bar{z}) = \mathrm{Im}(z) = (z - \bar{z})/(2i)$:

$$
\frac{\partial \mathrm{Im}(z)}{\partial z} = \frac{1}{2i}, \qquad \frac{\partial \mathrm{Im}(z)}{\partial \bar{z}} = -\frac{1}{2i}
$$

---

### 2.5.4 The Chain Rule for Wirtinger Derivatives

**Theorem 2.5 (Wirtinger Chain Rule).** Let $f: \mathbb{C} \to \mathbb{C}$ and $g: \mathbb{C} \to \mathbb{C}$ be differentiable (in the real sense). Then:

$$
\frac{\partial (f \circ g)}{\partial z} = \frac{\partial f}{\partial g} \frac{\partial g}{\partial z} + \frac{\partial f}{\partial \bar{g}} \frac{\partial \bar{g}}{\partial z}
$$

$$
\frac{\partial (f \circ g)}{\partial \bar{z}} = \frac{\partial f}{\partial g} \frac{\partial g}{\partial \bar{z}} + \frac{\partial f}{\partial \bar{g}} \frac{\partial \bar{g}}{\partial \bar{z}}
$$

**Important:** The chain rule has two terms because $f$ depends on both $g$ and $\bar{g}$.

**Example 2.11 (Chain Rule for $|g|^2$).**

Let $f(g) = |g|^2 = g\bar{g}$. Then:

$$
\frac{\partial f}{\partial g} = \bar{g}, \qquad \frac{\partial f}{\partial \bar{g}} = g
$$

Using the chain rule:

$$
\frac{\partial |g|^2}{\partial z} = \bar{g} \frac{\partial g}{\partial z} + g \frac{\partial \bar{g}}{\partial z}
$$

This is a useful formula for computing gradients of squared moduli.

---

### 2.5.5 Gradients for Real Parameters

In reinforcement learning, we often parameterize policies by real parameters $\theta \in \mathbb{R}^n$. The value function $\eta(\theta)$ is complex-valued, and we need to compute gradients of objectives like $|\eta(\theta)|^2$.

**Definition 2.11 (Gradient for Real Parameters).** For a function $f: \mathbb{R}^n \to \mathbb{C}$, the gradient is:

$$
\nabla_\theta f = \begin{pmatrix}
\frac{\partial f}{\partial \theta_1} \\
\frac{\partial f}{\partial \theta_2} \\
\vdots \\
\frac{\partial f}{\partial \theta_n}
\end{pmatrix}
$$

**Wirtinger Chain Rule for Real Parameters:**

If $f(\theta) = g(z(\theta), \bar{z}(\theta))$, then:

$$
\frac{\partial f}{\partial \theta_j} = \frac{\partial g}{\partial z} \frac{\partial z}{\partial \theta_j} + \frac{\partial g}{\partial \bar{z}} \frac{\partial \bar{z}}{\partial \theta_j}
$$

---

### 2.5.6 Application: Gradient of the Squared Modulus

**Theorem 2.6 (Gradient of Squared Modulus).** For $\eta: \mathbb{R}^n \to \mathbb{C}$:

$$
\nabla_\theta |\eta(\theta)|^2 = 2\mathrm{Re}\left( \overline{\eta(\theta)} \cdot \nabla_\theta \eta(\theta) \right)
$$

**Proof:**

Let $f(\theta) = |\eta(\theta)|^2 = \eta(\theta)\overline{\eta(\theta)}$.

Using the Wirtinger chain rule for each component $\theta_j$:

$$
\frac{\partial f}{\partial \theta_j} = \frac{\partial f}{\partial \eta} \frac{\partial \eta}{\partial \theta_j} + \frac{\partial f}{\partial \bar{\eta}} \frac{\partial \bar{\eta}}{\partial \theta_j}
$$

From Example 2.9:

$$
\frac{\partial f}{\partial \eta} = \bar{\eta}, \qquad \frac{\partial f}{\partial \bar{\eta}} = \eta
$$

So:

$$
\frac{\partial f}{\partial \theta_j} = \bar{\eta} \frac{\partial \eta}{\partial \theta_j} + \eta \overline{\frac{\partial \eta}{\partial \theta_j}}
$$

Taking the real part:

$$
\frac{\partial f}{\partial \theta_j} = 2\mathrm{Re}\left( \bar{\eta} \frac{\partial \eta}{\partial \theta_j} \right)
$$

Summing over $j$:

$$
\nabla_\theta f = 2\mathrm{Re}\left( \bar{\eta} \nabla_\theta \eta \right)
$$

$\square$

**Geometric Interpretation:**

The gradient $\nabla_\theta |\eta(\theta)|^2$ points in the direction that moves $\eta(\theta)$ toward the origin in the complex plane. This is because:

1. $\bar{\eta} \nabla_\theta \eta$ is the projection of the gradient of $\eta$ onto the direction of $\eta$.
2. Taking the real part ensures we move in the direction that reduces $|\eta|$.
3. The factor of 2 comes from the derivative of the square.

This geometric interpretation is crucial for Chapter ??, where we derive policy gradient algorithms that minimize $|Q^\pi|$.

---

### 2.5.7 Application: The Bellman Operator Obstruction

Recall from Section 2.4.5 that the obstruction to the Bellman optimality operator being a contraction is the non-holomorphic component of the Q-function.

**Proposition 2.1 (Non-Holomorphic Component as Obstruction).** The obstruction to the Bellman optimality operator $T$ being a contraction is exactly:

$$
\left\|\frac{\partial Q}{\partial \bar{z}}\right\|_\infty
$$

**Proof Sketch:**

1. Decompose $Q = Q_h + Q_{\bar{h}}$ into holomorphic and anti-holomorphic parts:
   - $Q_h$: holomorphic component ($\partial Q_h/\partial \bar{z} = 0$)
   - $Q_{\bar{h}}$: anti-holomorphic component ($\partial Q_{\bar{h}}/\partial z = 0$)

2. The Bellman optimality operator acts on $Q$ and produces:

$$
TQ = TQ_h + TQ_{\bar{h}} + \text{cross-terms}
$$

4. The cross-terms involve products of $Q_h$ and $Q_{\bar{h}}$, which are non-zero only when $\partial Q/\partial \bar{z} \neq 0$.

5. The contraction gap is:

$$
\|TQ_1 - TQ_2\| = \lambda \|Q_1 - Q_2\| + O\left(\left\|\frac{\partial Q}{\partial \bar{z}}\right\|_\infty\right)
$$

7. When $Q$ is holomorphic ($\partial Q/\partial \bar{z} = 0$), the cross-terms vanish and $T$ becomes a contraction.

**Interpretation:** The contraction gap is precisely the "non-holomorphicity" of the Q-function. This is why restricting Q-functions to the Bergman space (holomorphic functions) may resolve OP1.

---

### 2.5.8 Application: Policy Gradients (Preview)

In Chapter 11, we will derive the Complex Policy Gradient Theorem. The key result is:

**Theorem 2.7 (Complex Policy Gradient Theorem — Preview).** For a parameterized policy $\pi_\theta$ and complex return $G_t$:

$$
\nabla_\theta |\eta(\theta)|^2 = 2\mathrm{Re}\left( \overline{\eta(\theta)} \cdot \mathbb{E}\left[ \sum_{t=0}^\infty \nabla_\theta \log \pi_\theta(A_t|S_t) G_t \right] \right)
$$

where $\eta(\theta) = \mathbb{E}[G_t]$ is the complex expected return.

This theorem uses the Wirtinger gradient formula from Theorem 2.6 and the standard REINFORCE identity for the gradient of the expected return.

The proof will be developed in Chapter ??.

---

### 2.5.9 Key Takeaways

1. **Motivation:** Many objectives (like $|z|^2$) are not holomorphic and cannot be differentiated using standard complex analysis.

2. **Wirtinger Derivatives:** $\partial/\partial z$ and $\partial/\partial \bar{z}$ treat $z$ and $\bar{z}$ as independent variables.

3. **Holomorphic Condition:** $f$ is holomorphic iff $\partial f/\partial \bar{z} = 0$.

4. **Chain Rule:** $\partial(f \circ g)/\partial z = (\partial f/\partial g)(\partial g/\partial z) + (\partial f/\partial \bar{g})(\partial \bar{g}/\partial z)$.

5. **Gradient of Squared Modulus:** $\nabla_\theta |\eta(\theta)|^2 = 2\mathrm{Re}(\bar{\eta} \nabla_\theta \eta)$.

6. **Geometric Interpretation:** The gradient points toward the origin in $\mathbb{C}$.

7. **Bellman Obstruction:** The non-holomorphic component $\partial Q/\partial \bar{z}$ causes the contraction gap.

8. **Policy Gradients:** Wirtinger calculus enables complex policy gradients (Chapter ??).

---

### 2.5.10 Exercises for Section 2.5

**Exercise 2.16 (Wirtinger Derivatives).** Compute $\partial f/\partial z$ and $\partial f/\partial \bar{z}$ for:
1. $f(z) = z^3$
2. $f(z) = \bar{z}^3$
3. $f(z) = z\bar{z}^2$
4. $f(z) = \mathrm{Re}(z)^2 + \mathrm{Im}(z)^2$

**Exercise 2.17 (Holomorphic Check).** For each function in Exercise 2.16, determine whether it is holomorphic. Justify your answer.

**Exercise 2.18 (Gradient of Squared Modulus).** Let $\eta(\theta) = \theta^2 + i\theta$ for $\theta \in \mathbb{R}$. Compute $\nabla_\theta |\eta(\theta)|^2$ using:
1. Direct differentiation (treat $\theta$ as real)
2. The Wirtinger formula: $\nabla_\theta |\eta|^2 = 2\mathrm{Re}(\bar{\eta} \nabla_\theta \eta)$

Verify that both methods give the same result.

**Exercise 2.19 (Chain Rule).** Let $g(z) = z^2$ and $f(g) = |g|^2$. Compute $\partial(f \circ g)/\partial z$ and $\partial(f \circ g)/\partial \bar{z}$ using the Wirtinger chain rule. Verify by direct computation.

**Exercise 2.20 (The Obstruction).** Let $Q(z) = z + \bar{z}$ (a non-holomorphic function). Compute $\partial Q/\partial \bar{z}$. Explain why this non-holomorphic component would cause the Bellman optimality operator to fail to be a contraction.

---

### 2.5.11 Further Reading for Section 2.5

- Wirtinger, W. (1927). "Zur formalen Theorie der Funktionen von mehr komplexen Veränderlichen." *Mathematische Annalen*, 97(1):357-375. — The original paper on Wirtinger calculus.

- Kreutz-Delgado, K. (2009). "The Complex Gradient Operator and the CR-Calculus." arXiv:0906.4835. — A comprehensive tutorial on Wirtinger calculus.

- Remmert, R. (1991). *Theory of Complex Functions*. Springer. — Chapter 1 covers Wirtinger calculus.

- Ahlfors, L. V. (1979). *Complex Analysis*, 3rd ed. McGraw-Hill. — Appendix covers Wirtinger derivatives.

---

## 2.6 Information Theory

### 2.6.1 Introduction

Information theory, founded by Claude Shannon in 1948, provides a mathematical framework for quantifying information, uncertainty, and the transmission of messages. It is fundamental to many areas of science and engineering, including communication systems, data compression, statistical inference, and machine learning.

This section provides a refresher on the core concepts of information theory that will be used throughout the book. Readers familiar with these concepts may skip ahead.

---

### 2.6.2 Entropy

**Definition 2.12 (Entropy — Discrete).** For a discrete random variable $X$ taking values in $\mathcal{X}$ with probability mass function $p(x) = P(X = x)$, the **entropy** is:

$$
H(X) = -\sum_{x \in \mathcal{X}} p(x) \log p(x)
$$

where we define $0 \log 0 = 0$.

**Base of the Logarithm:**

| Base | Unit |
|------|------|
| 2 | Bits |
| $e$ | Nats |
| 10 | Dits (or Hartleys) |

In this book, we use **nats** (natural logarithm) unless otherwise specified, as they are mathematically convenient for calculus.

**Interpretation:** Entropy measures the average uncertainty or surprise associated with the outcomes of a random variable. Higher entropy means more uncertainty (more "information" is needed on average to describe the outcome).

**Properties of Entropy:**

1. **Non-Negativity:** $H(X) \geq 0$ for all discrete $X$.

2. **Maximum Entropy:** For a random variable with $\|\mathcal{X}\| = n$ possible values, $H(X) \leq \log n$, with equality when $X$ is uniformly distributed.

3. **Minimum Entropy:** $H(X) = 0$ if and only if $X$ is deterministic (takes a single value with probability 1).

4. **Concavity:** $H(X)$ is a concave function of the probability distribution $p$.

5. **Invariance:** Entropy depends only on the probabilities, not on the actual values of the outcomes.

**Example 2.12 (Computing Entropy).**

**Coin Toss (Bernoulli):** $X \in \{0, 1\}$ with $P(X=1) = p$.

$$
H(X) = -p \log p - (1-p) \log(1-p)
$$

- If $p = 0.5$ : $H(X) = \log 2 \approx 0.693$ nats (or 1 bit)
- If $p = 0$ or $p = 1$ : $H(X) = 0$ (deterministic)

**Uniform Distribution on 4 Outcomes:**

\( X \in \{1, 2, 3, 4\} \) with \( p(x) = 1/4 \) for all \( x \).

$$
H(X) = -4 \cdot \frac{1}{4} \log \frac{1}{4} = \log 4 \approx 1.386 \text{ nats (or 2 bits)}
$$

**Definition 2.13 (Differential Entropy — Continuous).** For a continuous random variable \( X \) with probability density function \( f(x) \), the **differential entropy** is:

$$
H(X) = -\int_{\mathcal{X}} f(x) \log f(x) \, dx
$$

**Important Note:** Differential entropy can be negative (unlike discrete entropy) and is not invariant under change of variables. It is used primarily for its relationship to mutual information, which is well-defined and non-negative.

**Example 2.13 (Differential Entropy).**

**Uniform Distribution on \( [0, a] \):** \( f(x) = 1/a \) for \( x \in [0, a] \).

$$
H(X) = -\int_0^a \frac{1}{a} \log \frac{1}{a} \, dx = \log a
$$

- If \( a < 1 \), \( H(X) < 0 \) (negative differential entropy).
- If \( a = 1 \), \( H(X) = 0 \).
- If \( a > 1 \), \( H(X) > 0 \).

**Normal Distribution \( \mathcal{N}(\mu, \sigma^2) \):**

$$
H(X) = \frac{1}{2} \log(2\pi e \sigma^2)
$$

---

### 2.6.3 Joint Entropy and Conditional Entropy

**Definition 2.14 (Joint Entropy).** For two discrete random variables \( X \) and \( Y \) with joint distribution \( p(x,y) \), the **joint entropy** is:

$$
H(X, Y) = -\sum_{x \in \mathcal{X}} \sum_{y \in \mathcal{Y}} p(x,y) \log p(x,y)
$$

**Interpretation:** Joint entropy measures the total uncertainty associated with the pair \( (X, Y) \).

**Definition 2.15 (Conditional Entropy).** For random variables \( X \) and \( Y \) with joint distribution \( p(x,y) \), the **conditional entropy** of \( X \) given \( Y \) is:

$$
H(X \mid Y) = \mathbb{E}_Y[H(X \mid Y=y)] = -\sum_{x,y} p(x,y) \log p(x \mid y)
$$

**Interpretation:** \( H(X \mid Y) \) is the expected uncertainty about \( X \) after observing \( Y \). It measures the remaining uncertainty in \( X \) given knowledge of \( Y \).

**Properties of Conditional Entropy:**

1. **Non-Negativity:** \( H(X \mid Y) \geq 0 \).

2. **Chain Rule:** \( H(X, Y) = H(X) + H(Y \mid X) = H(Y) + H(X \mid Y) \).

3. **Reduction of Entropy:** \( H(X \mid Y) \leq H(X) \) with equality iff \( X \) and \( Y \) are independent.

4. **Conditioning Reduces Entropy:** \( H(X \mid Y, Z) \leq H(X \mid Y) \).

**Example 2.14 (Conditional Entropy).**

Let \( X \) be the outcome of a fair coin toss, and \( Y \) be a perfect observation of \( X \). Then:
- \( H(X) = \log 2 \approx 0.693 \) nats
- \( H(X \mid Y) = 0 \) (no uncertainty remains after observing \( Y \))

Let \( X \) be the outcome of a fair coin toss, and \( Y \) be independent noise. Then:
- \( H(X \mid Y) = H(X) = \log 2 \) (observing \( Y \) provides no information)

---

### 2.6.4 Mutual Information

**Definition 2.16 (Mutual Information).** The **mutual information** between random variables \( X \) and \( Y \) is:

$$
I(X; Y) = H(X) - H(X \mid Y) = H(Y) - H(Y \mid X)
$$

**Alternative Formulations:**

1. **In Terms of Joint Distribution:**
   $$
   I(X; Y) = \sum_{x,y} p(x,y) \log \frac{p(x,y)}{p(x)p(y)}
   $$

2. **In Terms of KL Divergence:**
   $$
   I(X; Y) = D_{\text{KL}}(p(x,y) \parallel p(x)p(y))
   $$

3. **In Terms of Entropies:**
   $$
   I(X; Y) = H(X) + H(Y) - H(X, Y)
   $$

**Properties of Mutual Information:**

1. **Non-Negativity:** \( I(X; Y) \geq 0 \), with equality iff \( X \) and \( Y \) are independent.

2. **Symmetry:** \( I(X; Y) = I(Y; X) \).

3. **Chain Rule:** \( I(X_1, X_2; Y) = I(X_1; Y) + I(X_2; Y \mid X_1) \).

4. **Data Processing Inequality:** If \( X \to Y \to Z \) forms a Markov chain, then \( I(X; Z) \leq I(X; Y) \).

5. **Relation to Correlation:** \( I(X; Y) = 0 \) iff \( X \) and \( Y \) are independent (stronger than zero correlation).

**Interpretation:** Mutual information measures the amount of information one random variable provides about another. It is the reduction in uncertainty about \( X \) after observing \( Y \) (or vice versa).

**Example 2.15 (Mutual Information).**

**Perfect Correlation:** Let \( X \in \{0,1\} \) be fair, and \( Y = X \). Then:
$$
I(X; Y) = H(X) = \log 2 \approx 0.693 \text{ nats}
$$

**Independence:** Let \( X \) and \( Y \) be independent fair coin tosses. Then:
$$
I(X; Y) = 0
$$

**Partial Correlation:** Let \( X \) be fair, and \( Y = X \oplus Z \) where \( Z \) is fair noise independent of \( X \). Then:
$$
I(X; Y) = H(Y) - H(Y \mid X) = \log 2 - \log 2 = 0.307 \text{ nats}
$$

---

### 2.6.5 Relative Entropy (KL Divergence)

**Definition 2.17 (Kullback-Leibler Divergence).** The **KL divergence** (or **relative entropy**) between two probability distributions \( p \) and \( q \) on the same alphabet \( \mathcal{X} \) is:

$$
D_{\text{KL}}(p \parallel q) = \sum_{x \in \mathcal{X}} p(x) \log \frac{p(x)}{q(x)}
$$

**Properties of KL Divergence:**

1. **Non-Negativity:** \( D_{\text{KL}}(p \parallel q) \geq 0 \), with equality iff \( p = q \).

2. **Not a Metric:** KL divergence is not symmetric and does not satisfy the triangle inequality.

3. **Interpretation:** KL divergence measures the inefficiency of using \( q \) to approximate \( p \). It is the expected extra number of bits (or nats) needed when using \( q \) instead of \( p \).

**Example 2.16 (KL Divergence).**

Let \( p \) be a Bernoulli distribution with \( p(1) = 0.9, p(0) = 0.1 \), and \( q \) be a Bernoulli distribution with \( q(1) = 0.5, q(0) = 0.5 \).

$$
D_{\text{KL}}(p \parallel q) = 0.9 \log \frac{0.9}{0.5} + 0.1 \log \frac{0.1}{0.5} \approx 0.9(0.588) + 0.1(-1.609) \approx 0.529 - 0.161 = 0.368 \text{ nats}
$$

---

### 2.6.6 The Chain Rule for Entropy and Mutual Information

**Theorem 2.9 (Chain Rule for Entropy).**
$$
H(X_1, X_2, \ldots, X_n) = \sum_{i=1}^n H(X_i \mid X_1, \ldots, X_{i-1})
$$

**Theorem 2.10 (Chain Rule for Mutual Information).**
$$
I(X_1, X_2, \ldots, X_n; Y) = \sum_{i=1}^n I(X_i; Y \mid X_1, \ldots, X_{i-1})
$$

**Interpretation:** The chain rule allows us to decompose joint entropy (or mutual information) into a sum of conditional terms. This is useful for sequential decision-making where information is acquired over time.

---

### 2.6.7 Data Processing Inequality

**Theorem 2.11 (Data Processing Inequality).** If \( X \to Y \to Z \) forms a Markov chain (i.e., \( Z \) depends on \( X \) only through \( Y \)), then:

$$
I(X; Z) \leq I(X; Y)
$$

**Interpretation:** Post-processing cannot increase information. Once you have observed \( Y \), any further processing \( Z = f(Y) \) cannot provide more information about \( X \) than \( Y \) already provides.

**Corollary:** For any function \( f \):
$$
I(X; f(Y)) \leq I(X; Y)
$$

---

### 2.6.8 Key Takeaways

1. **Entropy:** \( H(X) = -\sum_x p(x) \log p(x) \), measures uncertainty.

2. **Joint Entropy:** \( H(X, Y) \) measures total uncertainty of a pair.

3. **Conditional Entropy:** \( H(X \mid Y) \) measures remaining uncertainty after observing \( Y \).

4. **Mutual Information:** \( I(X; Y) = H(X) - H(X \mid Y) \), measures information gain.

5. **KL Divergence:** \( D_{\text{KL}}(p \parallel q) = \sum_x p(x) \log(p(x)/q(x)) \), measures distance between distributions.

6. **Chain Rules:** Decompose joint entropy and mutual information into sums.

7. **Data Processing Inequality:** \( I(X; Z) \leq I(X; Y) \) for Markov chains \( X \to Y \to Z \).

---

### 2.6.9 Exercises for Section 2.6

**Exercise 2.21 (Computing Entropy).** Let $X$ be a random variable taking values in ${1, 2, 3, 4}$ with probabilities $[0.1, 0.2, 0.3, 0.4]$. Compute:
1. $H(X)$
2. The maximum possible entropy for a 4-outcome variable
3. The minimum possible entropy

**Exercise 2.22 (Conditional Entropy).** Let $X \in \{0,1\}$ be fair, and let $Y$ be a noisy observation of $X$ such that $P(Y = X) = 0.8$ and $P(Y \neq X) = 0.2$. Compute:
1. $H(X)$
2. $H(X \mid Y)$
3. $I(X; Y)$

**Exercise 2.23 (Mutual Information).** Prove that $I(X; Y) \geq 0$ with equality iff $X$ and $Y$ are independent. *Hint: Use the KL divergence formulation or Jensen's inequality.*

**Exercise 2.24 (KL Divergence).** Let $p$ be a Bernoulli distribution with $p(1) = 0.8$, and $q$ with $q(1) = 0.6$. Compute $D_{\text{KL}}(p \parallel q)$ and $D_{\text{KL}}(q \parallel p)$. Are they equal?

**Exercise 2.25 (Chain Rule).** Prove the chain rule for entropy: $H(X, Y) = H(X) + H(Y \mid X)$. Use the definitions of joint and conditional entropy.

---

### 2.6.10 Further Reading for Section 2.6

- Cover, T. M. & Thomas, J. A. (2006). *Elements of Information Theory*, 2nd ed. Wiley. — The definitive text on information theory.

- Shannon, C. E. (1948). "A Mathematical Theory of Communication." *Bell System Technical Journal*, 27:379-423, 623-656. — The foundational paper.

- MacKay, D. J. C. (2003). *Information Theory, Inference, and Learning Algorithms*. Cambridge University Press. — An accessible introduction with connections to machine learning.

- Gray, R. M. (2011). *Entropy and Information Theory*, 2nd ed. Springer. — A concise treatment of information theory.

---


