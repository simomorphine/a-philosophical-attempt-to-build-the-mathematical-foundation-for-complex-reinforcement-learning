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

**Theorem 2.3 (Schauder's Fixed-Point Theorem).** Let $X$ be a Banach space, $K \subset X$ a non-empty compact convex set, and $T: K \to K$ a continuous operator. Then $T$ has a fixed point $x^* \in K$ such that $T(x^*) = x^*$.

**Why This Matters:**

If we can show that the Bellman optimality operator $T$ maps a compact convex set of holomorphic Q-functions into itself, then Schauder's theorem guarantees the existence of a fixed point $Q^*$. This would resolve **OP2** (Existence and Uniqueness) in the affirmative.

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

Is $T$ a contraction (strictly reducing the distance between functions), or merely a continuous map with a fixed point? The compactness of the domain suggests that contraction may hold, but this remains an open problem (OP1).

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
   \left| \max_a f(a) - \max_a g(a) \right| \leq \max_a |f(a) - g(a)|
   $$

2. The transition kernel averages probabilities, which is also non-expansive.

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


