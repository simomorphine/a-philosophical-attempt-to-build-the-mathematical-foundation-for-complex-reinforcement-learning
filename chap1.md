# Chapter 1: The Problem of Two Burdens

---

## 1.1 Introduction: The Two Burdens of an Adaptive Agent

### 1.1.1 A Tale of Two Burdens

Every adaptive agent navigating an uncertain world faces two qualitatively distinct burdens *simultaneously*. These burdens are not merely different in degree—they are different in *kind*. They have different mathematical properties, different physical interpretations, and different implications for decision-making. Yet classical reinforcement learning treats them as if they were the same.

The first burden is **real cost** ($c$)—the measurable, immediate price of action. This is the tangible currency of physical and computational systems: joules of energy consumed, seconds of time elapsed, dollars spent, bits processed, errors made. Real cost is:

- **Additive and cumulative:** Costs sum along a trajectory. The total cost of a path is the sum of its parts.
- **Path-dependent:** The cost of going from A to C via B is generally greater than going from A to C directly. The path matters.
- **Irreversible:** Once energy is spent or time is elapsed, it cannot be recovered.
- **Non-negative:** $c \geq 0$ for all transitions.

The second burden is **information debt** ($d$)—the epistemic burden of operating with incomplete or miscalibrated models. It is the gap between what the agent knows and what the world demands it know. This debt accrues when actions are taken without sufficient information; it can only be reduced by actions that acquire knowledge. Information debt is:

- **Conservative:** Debt behaves like a potential difference. The debt of a transition depends only on the endpoints, not on the path taken.
- **Path-independent:** For any states $s_i, s_j, s_k$, we have $d(s_i, s_k) = d(s_i, s_j) + d(s_j, s_k)$.
- **Reversible:** Debt can be paid down by gaining information. An agent that explores can reduce its uncertainty.
- **Sign-unrestricted:** Debt can be positive (taking on obligations, increasing uncertainty) or negative (paying them down, gaining knowledge).

**The Contrast:**

| Property | Real Cost ($c$) | Information Debt ($d$) |
|----------|----------------|----------------------|
| **Additivity** | Additive | Conservative (potential difference) |
| **Path-dependence** | Path-dependent | Path-independent |
| **Sign** | Always non-negative ($c \geq 0$) | Sign-unrestricted ($d \in \mathbb{R}$) |
| **Reversibility** | Irreversible | Reversible |
| **Units** | Joules, seconds, dollars, bits | Nats, bits, uncertainty |

These are not two dimensions of the same quantity. They are two *fundamentally different types* of burden. A decision-making framework that cannot distinguish between them—that forces them into a single scalar—is missing something essential.

---

### 1.1.2 The Limits of a Single Number

Classical reinforcement learning (RL) models an agent interacting with an environment through a Markov Decision Process (MDP). The agent receives a scalar reward $r(s,a)$ and seeks to maximize expected cumulative reward:

$$
J^\pi(s) = \mathbb{E}^\pi \left[ \sum_{t=0}^\infty \gamma^t r(S_t, A_t) \mid S_0 = s \right].
$$

This framework has achieved remarkable success in domains ranging from game playing to robotics to resource allocation. But it rests on a hidden assumption: that all objectives can be meaningfully *combined* into a single scalar.

In practice, this assumption often leads to **scalarisation**: the combination of multiple objectives into a weighted sum. If an agent cares about both cost and debt, the standard approach is to define:

$$
r_{\text{total}} = c + \mu d,
$$

where $\mu \geq 0$ is a weighting parameter that must be chosen. Then the agent optimizes this scalar reward using standard RL.

But this approach is deeply problematic. The choice of $\mu$ is:

1. **Arbitrary:** There is no principled way to choose $\mu$ for a new problem.
2. **Environment-dependent:** The optimal $\mu$ for one environment is different from the optimal $\mu$ for another.
3. **Task-dependent:** Even within the same environment, different tasks may require different $\mu$ values.
4. **State-dependent:** The optimal balance between cost and debt may differ between exploration and exploitation phases.

**This is the Free Parameter Problem.** No single $\mu$ works across all environments, tasks, or states. And this is not merely a practical nuisance—it is a *fundamental limitation* of the scalarisation approach.

> **The central thesis of this book:** The response to this parameter sensitivity is not to search for a universal $\mu$, but to recognize that the problem is inherently two-dimensional. Cost and debt are not two quantities that need to be weighted—they are two components of a single geometric object.

---

### 1.1.3 A Motivating Example: The Two-Armed Bandit with Cost

Consider a simple two-armed bandit problem with an energy cost for pulling each arm. The agent must balance the cost of pulling arms against the value of information gained.

**Environment:**
- **Arm A (Known):** Reward $R_A = 0.5$, cost $c_A = 0.1$ per pull.
- **Arm B (Unknown):** Reward $R_B \sim \text{Bernoulli}(p)$ with $p \in \{0.2, 0.8\}$ equally likely, cost $c_B = 0.2$ per pull.

The agent's objective is to minimize total cost while maximizing reward over a finite horizon. This is an exploration-exploitation problem with an explicit cost for information.

**Classical Approaches and Their Failures:**

1. **Pure exploitation:** Always pull the arm with the best *expected* reward. Since Arm A gives 0.5 and Arm B gives $0.5$ (expected), the agent pulls Arm A. This fails if the true $p = 0.8$.

2. **$\epsilon$-greedy:** Pull a random arm with probability $\epsilon$. This explores but requires tuning $\epsilon$. What is the optimal $\epsilon$? It depends on the horizon, the costs, and the prior uncertainty.

3. **UCB (Upper Confidence Bound):** Pull the arm with the highest upper confidence bound. This requires a confidence parameter that must be tuned. The optimal parameter depends on the problem structure.

4. **Thompson sampling:** Sample from the posterior and pull the best sampled arm. This is Bayesian and automatic for reward, but it doesn't naturally incorporate the *cost* of pulling arms.

**The Complex Approach (Preview):**

We encode the agent's utility as a complex number:

$$
z = c + id,
$$

where $c$ is the cost and $d$ is a measure of *epistemic debt*—the uncertainty about which arm is optimal. Initially, $d$ is large (the agent doesn't know if Arm B has $p = 0.2$ or $p = 0.8$). As the agent gathers information, $d$ decreases.

The agent minimizes the modulus:

$$
|z| = \sqrt{c^2 + d^2}.
$$

- **Initially:** $d$ is large, so the modulus is dominated by debt. The agent explores—pulling Arm B to reduce uncertainty.
- **As information accumulates:** $d$ decreases. The modulus becomes dominated by cost. The agent exploits—pulling the now-known best arm.

**The key insight:** The transition from exploration to exploitation is **automatic**. No $\epsilon$, no $\beta$, no schedule, no tuning. The geometry of the complex plane does the work.

```
    Debt (d)
      ↑
      |   *  (Initial state: high debt, low cost)
      |  / 
      | /   The trajectory moves toward the origin
      |/    as the agent learns
      *-----→ Cost (c)
     (Epistemic equilibrium: zero debt, minimal cost)
```

*Figure 1.1: Trajectory in the cost-debt plane. The agent starts with high debt (uncertainty) and moves toward the origin as it gathers information and minimizes cost. The path is determined by the geometry of $\mathbb{C}$, not by external parameters.*

This simple example illustrates the core idea: the geometry of the complex plane provides the balance between cost and debt. No weighting parameter is required.

---

### 1.1.4 The HST Framework and Epistemic Equilibrium

This book is grounded in **Humble Systems Theory (HST)** , a framework for understanding information processing systems as seeking equilibrium between cost and informational constraints.

**The Central Axiom:**

> **Axiom 1.1 (HST Equilibrium Axiom).** Every Information Processing System evolves toward epistemic equilibrium—a state of minimum computational cost subject to informational constraints.

At epistemic equilibrium:

- The system has no remaining epistemic debt: $d = 0$ for all relevant quantities.
- The system acts as a pure cost minimizer: all information worth acquiring has been acquired.
- The system has achieved the minimum possible computational cost given its environment.

**Why "Humble"?**

The name reflects a central principle: systems are "humble" in the sense that they do not require external tuning or supervision to find their balance point. The geometry of their problem space drives them to equilibrium. They do not need a designer to choose $\mu$ or schedule $\epsilon$—the geometry already knows.

The HST Equilibrium Axiom implies a remarkable property: **the transition from exploration to exploitation is automatic.** As the system gathers information, debt decreases. The modulus-greedy policy $\pi_Q(s) = \arg\min_a |Q(s,a)|$ automatically shifts from exploring (when debt is large) to exploiting (when debt is small).

No exploration schedule is required. No entropy bonus is needed. No annealing of $\epsilon$ is necessary. The geometry of $\mathbb{C}$ does the work.

---

### 1.1.5 What This Book Is About

This book is about the consequences of taking the geometry of cost and debt seriously. We will:

1. **Develop the geometric foundation:** Belief spaces, energy quasi-metrics, the debt function, the complex quasi-metric, and equilibrium concepts. (Chapters 2-7)

2. **Build complex-valued reinforcement learning algorithms:** The Complex MDP (cMDP), Bellman evaluation (proven contraction), Bellman optimality (the central open problem), policy gradients in the complex plane, and the Complex Natural Actor-Critic (CNAC) algorithm. (Chapters 8-12)

3. **Explore deeper structure:** Harmonic analysis of the gradient flow, Kähler geometry and the Bergman conjecture, epistemic equilibrium and the HST Axiom, and the open problems that define the frontier. (Chapters 13-16)

**Key Results We Will Prove:**

- **Evaluation Contraction (Chapter 9):** The Bellman evaluation operator $T^\pi$ is a $\lambda$-contraction on the Banach space of complex value functions.

- **Telescoping Identity (Chapter 9):** Under the potential difference form, the imaginary $Q$-value telescopes to a closed-form expression. When interpreted as negative entropy, this becomes discounted cumulative mutual information.

- **Policy Gradient Theorem (Chapter 11):** The gradient of the complex expected return is given by a REINFORCE-style identity.

- **Lyapunov Function (Chapter 14):** Under the HST Equilibrium Axiom, $|Q|^2$ is a Lyapunov function.

**Open Problems We Will Identify:**

1. **Contraction of the Optimality Operator:** Is the Bellman optimality operator $T$ a contraction? The geometric obstruction is identified; the Bergman Conjecture proposes a resolution.

2. **Existence and Uniqueness:** Does $Q^* = TQ^*$ have a unique solution?

3. **Convergence of Q-Learning:** Does complex Q-learning converge?

4. **Convergence of CNAC:** Does the CNAC algorithm converge?

5. **Kähler Condition:** When is the Hermitian Fisher metric Kähler?

6. **Physics Connections:** Can we formulate a statistical mechanics of learning?

---

### 1.1.6 Summary of Section 1.1

In this introductory section, we have established the core problem that motivates this book:

1. **Two Fundamental Burdens:** Every adaptive agent faces both real cost ($c$) and information debt ($d$). These are qualitatively different—cost is additive and irreversible, while debt is conservative and reversible.

2. **The Failure of Scalarisation:** Classical RL attempts to combine these burdens into a single scalar reward $r = c + \mu d$. This introduces a free parameter $\mu$ that must be tuned and is environment-dependent.

3. **The Free Parameter Problem:** No single $\mu$ works across all environments, tasks, or states. This is a fundamental limitation, not a practical nuisance.

4. **A Geometric Alternative:** Encoding cost and debt as the real and imaginary parts of a complex number $z = c + id$ eliminates the free parameter. Minimizing the modulus $|z| = \sqrt{c^2 + d^2}$ is principled, parameter-free, and geometrically natural.

5. **Humble Systems Theory:** The HST Equilibrium Axiom asserts that all information processors evolve toward epistemic equilibrium—a state of zero debt. The transition from exploration to exploitation is automatic, driven by geometry.

6. **A Roadmap:** This book develops the mathematical framework, algorithms, and deeper structure that arise from this simple geometric insight.

---

### 1.1.7 Exercises for Section 1.1

**Exercise 1.1 (Cost vs. Debt).** Give three real-world examples of systems that face both cost and debt. For each, identify:
- What constitutes real cost ($c$)
- What constitutes information debt ($d$)
- How cost and debt interact in that system

**Exercise 1.2 (The Free Parameter Problem).** Consider an environment where cost is 10 times larger than debt. What weight $\mu$ would be needed to balance them? What if the environment changes and debt becomes 10 times larger? Show that no fixed $\mu$ works.

**Exercise 1.3 (Cost-Debt Trajectory).** For the two-armed bandit example in Section 1.1.3, plot the trajectory of the agent in the cost-debt plane as it learns about Arm B. What does the trajectory look like if the agent uses $\epsilon$-greedy with $\epsilon = 0.1$? What if $\epsilon$ is annealed?

**Exercise 1.4 (Epistemic Equilibrium).** Define a simple MDP with two states and two actions. What would it mean for this system to be in epistemic equilibrium? What conditions must hold?

**Exercise 1.5 (The Phase).** For the complex utility $z = c + id$, the phase is $\theta = \arctan(d/c)$. Explain in your own words:
- What $\theta = 0$ means
- What $\theta = \pi/2$ means
- What $\theta < 0$ means
- How $\theta$ changes as an agent learns

---

### 1.1.8 Further Reading for Section 1.1

- Sutton, R. S. & Barto, A. G. (2018). *Reinforcement Learning: An Introduction*, 2nd ed. MIT Press. — The standard textbook on RL, covering the scalar reward assumption and its limitations.

- Russo, D. & Van Roy, B. (2018). "Learning to Optimize with Information-Directed Sampling." *Operations Research*, 66(1):230-252. — Information-directed sampling and the exploration-exploitation trade-off with explicit parameters.

- Landauer, R. (1961). "Irreversibility and Heat Generation in the Computing Process." *IBM Journal of Research and Development*, 5(3):183-191. — The physical cost of computation, relevant to real cost $c$.

- Cover, T. M. & Thomas, J. A. (2006). *Elements of Information Theory*, 2nd ed. Wiley. — Information theory fundamentals, relevant to information debt $d$.

---


## 1.2 The Classical Reinforcement Learning Paradigm

### 1.2.1 What is Reinforcement Learning?

Reinforcement Learning (RL) is a computational approach to learning from interaction. Unlike supervised learning, where the learner is told the correct answer for each input, or unsupervised learning, where the learner must find structure in unlabeled data, RL involves an agent learning through trial and error, receiving feedback in the form of rewards or penalties.

**The Core Idea:** An agent learns to achieve a goal by interacting with an environment. It takes actions, observes the consequences, receives feedback, and adapts its behavior to maximize cumulative reward over time.

**The Core Loop:**

```
1. Agent observes state S_t of the environment
2. Agent selects action A_t based on policy π
3. Environment transitions to new state S_{t+1}
4. Agent receives reward R_{t+1}
5. Agent updates its knowledge (policy, value function, or model)
6. Repeat
```

This simple loop underlies all of RL. The challenge is that the agent does not know the environment's dynamics in advance—it must learn through experience. This is what makes RL both powerful and difficult.

**Key Characteristics of RL:**

1. **Sequential Decision-Making:** Decisions have consequences that unfold over time.
2. **Delayed Feedback:** The effect of an action may not be immediately apparent.
3. **Exploration-Exploitation Trade-off:** The agent must balance trying new actions (to learn) and using known good actions (to get reward).
4. **Generalization:** The agent must generalize from past experience to new situations.

**Why RL Matters:**

RL provides a unified framework for understanding and building intelligent systems that learn from interaction. It has been applied to:
- Game playing (AlphaGo, Atari, Chess, Poker)
- Robotics (locomotion, manipulation, navigation)
- Resource management (energy grids, inventory, supply chains)
- Recommender systems (content, products, advertisements)
- Autonomous vehicles (planning, control, decision-making)

---

### 1.2.2 The Markov Decision Process (MDP)

The mathematical foundation of RL is the **Markov Decision Process (MDP)** . An MDP provides a formal framework for modeling sequential decision-making under uncertainty.

**Definition 1.1 (Markov Decision Process).** A finite Markov Decision Process is a tuple $(\mathcal{S}, \mathcal{A}, p, r, \lambda)$ where:

| Component | Symbol | Description |
|-----------|--------|-------------|
| **State Space** | $\mathcal{S}$ | A finite set of possible states the environment can be in |
| **Action Space** | $\mathcal{A}$ | A finite set of actions the agent can take |
| **Transition Probability** | $p(s'|s,a)$ | Probability of transitioning to state $s'$ after taking action $a$ in state $s$ |
| **Reward Function** | $r(s,a,s')$ | Immediate numerical reward received after transitioning from $s$ to $s'$ via $a$ |
| **Discount Factor** | $\lambda \in [0,1)$ | Determines the weight of future rewards |

**The Transition Probability:**

The transition probability $p(s'|s,a)$ satisfies:

$$
\sum_{s' \in \mathcal{S}} p(s'|s,a) = 1 \quad \forall s \in \mathcal{S}, a \in \mathcal{A}.
$$

This ensures that for any state and action, the agent will transition to *some* next state with probability 1.

**The Reward Function:**

The reward function $r(s,a,s')$ returns a scalar representing the immediate benefit (or cost) of transitioning from $s$ to $s'$ via action $a$. In many formulations, the reward is written as $r(s,a)$, but the full form $r(s,a,s')$ captures potential dependence on the next state.

**The Markov Property:**

The key assumption of an MDP is the **Markov property**:

$$
p(s_{t+1} | s_t, a_t, s_{t-1}, a_{t-1}, \ldots) = p(s_{t+1} | s_t, a_t).
$$

This means the future depends only on the current state and action, not on the history of past states and actions. The Markov property is what makes RL mathematically tractable—it allows us to use dynamic programming.

**Why Discounting?**

The discount factor $\lambda \in [0,1)$ serves two purposes:

1. **Mathematical:** It ensures that the infinite sum of rewards converges to a finite value.
2. **Intuitive:** It represents that immediate rewards are more valuable than distant ones (like interest rates in economics).

When $\lambda$ is close to 1, the agent cares strongly about long-term consequences. When $\lambda$ is close to 0, the agent is myopic and cares only about immediate rewards.

**Example 1.1 (A Simple MDP).** Consider a 2x2 grid world:

```
[S] [ ]
[ ] [G]
```

- $S$ is the start state (top-left)
- $G$ is the goal state (bottom-right)
- Actions: Up, Down, Left, Right
- If the agent attempts to move outside the grid, it stays in place
- Reward: -1 for each move, +10 for reaching $G$
- $\lambda = 0.9$

This simple MDP captures the essence of navigation: the agent must find the shortest path to the goal while avoiding unnecessary moves.

---

### 1.2.3 Policies and Returns

**Definition 1.2 (Policy).** A policy $\pi$ is the agent's behavior strategy:

- **Deterministic Policy:** $\pi: \mathcal{S} \to \mathcal{A}$ maps each state to a single action.
- **Stochastic Policy:** $\pi(\cdot|s)$ maps each state to a probability distribution over actions, where $\sum_{a \in \mathcal{A}} \pi(a|s) = 1$.

A policy defines how the agent chooses actions. It may be fixed (the agent uses the same policy throughout) or learned (the agent improves its policy over time).

**Definition 1.3 (Return).** The discounted return from time $t$ is:

$$
G_t = \sum_{k=0}^{\infty} \lambda^k R_{t+k+1},
$$

where $R_{t+k+1}$ is the reward received at time $t+k+1$.

The return is the total discounted reward that the agent receives starting from time $t$. The discount factor $\lambda$ ensures that rewards far in the future contribute less to the total.

**Example 1.2 (Computing Returns).** Consider an episode with rewards $[5, 3, 2]$ and $\lambda = 0.9$:

$$
G_0 = 5 + 0.9(3) + 0.9^2(2) = 5 + 2.7 + 1.62 = 9.32
$$

$$
G_1 = 3 + 0.9(2) = 4.8
$$

$$
G_2 = 2
$$

The return $G_0$ weights immediate rewards more heavily than distant ones, as expected with $\lambda < 1$.

**Performance Criterion:** The agent's performance is measured by the expected return:

$$
J^\pi(s) = \mathbb{E}^\pi[G_t | S_t = s].
$$

The agent's objective is to find the policy that maximizes this expected return for all states (or for a specific starting state distribution).

---

### 1.2.4 Value Functions

Value functions are the central objects of RL. They estimate the expected return from a state (or state-action pair) under a given policy.

**Definition 1.4 (State-Value Function).** The value of a state under policy $\pi$ is:

$$
V^\pi(s) = \mathbb{E}^\pi[G_t | S_t = s].
$$

This represents the expected cumulative reward starting from state $s$ and following policy $\pi$ thereafter.

**Definition 1.5 (Action-Value Function).** The value of taking action $a$ in state $s$ under policy $\pi$ is:

$$
Q^\pi(s,a) = \mathbb{E}^\pi[G_t | S_t = s, A_t = a].
$$

This represents the expected cumulative reward starting from state $s$, taking action $a$, and then following policy $\pi$ thereafter.

**Relationship Between $V$ and $Q$:**

The state-value function can be expressed in terms of the action-value function:

$$
V^\pi(s) = \sum_{a \in \mathcal{A}} \pi(a|s) Q^\pi(s,a).
$$

This says that the value of a state is the expected value of the action taken in that state, averaged according to the policy.

**Example 1.3 (Computing Value Functions).** Consider the grid world from Example 1.1. If $\pi$ is a random policy (equal probability for each action), we can compute $V^\pi$ by solving a system of equations. For state $S$:

$$
V^\pi(S) = \frac{1}{4} \sum_{a \in \{\text{Up},\text{Down},\text{Left},\text{Right}\}} \sum_{s'} p(s'|S,a) [r(S,a,s') + \lambda V^\pi(s')].
$$

Each action leads to a different next state with different rewards. Solving this system gives the expected return from each state under the random policy.

---

### 1.2.5 Bellman Equations

The Bellman equations express value functions recursively. They are the foundation of dynamic programming and RL algorithms.

**Bellman Equation for $V^\pi$:**

$$
V^\pi(s) = \sum_{a \in \mathcal{A}} \pi(a|s) \sum_{s' \in \mathcal{S}} p(s'|s,a) [r(s,a,s') + \lambda V^\pi(s')].
$$

**Bellman Equation for $Q^\pi$:**

$$
Q^\pi(s,a) = \sum_{s' \in \mathcal{S}} p(s'|s,a) [r(s,a,s') + \lambda Q^\pi(s', \pi(s'))].
$$

For a deterministic policy $\pi$, this simplifies to:

$$
Q^\pi(s,a) = \sum_{s' \in \mathcal{S}} p(s'|s,a) [r(s,a,s') + \lambda Q^\pi(s', \pi(s'))].
$$

**Derivation of the Bellman Equation for $V^\pi$:**

1. Start with the definition: $V^\pi(s) = \mathbb{E}^\pi[G_t | S_t = s]$.
2. Expand $G_t$: $V^\pi(s) = \mathbb{E}^\pi[R_{t+1} + \lambda G_{t+1} | S_t = s]$.
3. Use linearity of expectation: $V^\pi(s) = \mathbb{E}^\pi[R_{t+1} | S_t = s] + \lambda \mathbb{E}^\pi[G_{t+1} | S_t = s]$.
4. Recognize that $\mathbb{E}^\pi[G_{t+1} | S_{t+1} = s'] = V^\pi(s')$.
5. Average over actions and next states to get the Bellman equation.

**Interpretation:** The Bellman equation says that the value of a state is the immediate reward plus the discounted value of the next state, averaged over all possible actions and next states.

**The Bellman Evaluation Operator:**

We can define the Bellman evaluation operator $T^\pi$ acting on a value function $V$:

$$
(T^\pi V)(s) = \sum_{a} \pi(a|s) \sum_{s'} p(s'|s,a) [r(s,a,s') + \lambda V(s')].
$$

The value function $V^\pi$ is the unique fixed point of this operator:

$$
V^\pi = T^\pi V^\pi.
$$

**Theorem 1.1 (Contraction of the Bellman Evaluation Operator).** $T^\pi$ is a $\lambda$-contraction in the sup-norm:

$$
\|T^\pi V_1 - T^\pi V_2\|_\infty \leq \lambda \|V_1 - V_2\|_\infty.
$$

This ensures that iterative application of $T^\pi$ converges to $V^\pi$ from any starting point.

---

### 1.2.6 Bellman Optimality

The optimal policy $\pi^*$ is the policy that maximizes the expected return from all states. The optimal value functions are:

$$
V^*(s) = \max_\pi V^\pi(s),
$$

$$
Q^*(s,a) = \max_\pi Q^\pi(s,a).
$$

**Bellman Optimality Equations:**

$$
V^*(s) = \max_{a \in \mathcal{A}} \sum_{s' \in \mathcal{S}} p(s'|s,a) [r(s,a,s') + \lambda V^*(s')],
$$

$$
Q^*(s,a) = \sum_{s' \in \mathcal{S}} p(s'|s,a) [r(s,a,s') + \lambda \max_{a' \in \mathcal{A}} Q^*(s',a')].
$$

The Bellman optimality equations are not linear—they contain the $\max$ operator—but they have a unique solution.

**The Bellman Optimality Operator:**

We can define the Bellman optimality operator $T$ acting on a $Q$-function:

$$
(TQ)(s,a) = \sum_{s'} p(s'|s,a) \left[ r(s,a,s') + \lambda \max_{a'} Q(s',a') \right].
$$

The optimal $Q$-function $Q^*$ is the unique fixed point of this operator:

$$
Q^* = TQ^*.
$$

**Theorem 1.2 (Contraction of the Bellman Optimality Operator).** $T$ is a $\lambda$-contraction in the sup-norm:

$$
\|TQ_1 - TQ_2\|_\infty \leq \lambda \|Q_1 - Q_2\|_\infty.
$$

**Proof:** For any $Q_1, Q_2$ and any $(s,a)$:

$$
|(TQ_1)(s,a) - (TQ_2)(s,a)| = \lambda \left| \sum_{s'} p(s'|s,a) [\max_{a'} Q_1(s',a') - \max_{a'} Q_2(s',a')] \right|.
$$

Using the fact that $|\max_a f(a) - \max_a g(a)| \leq \max_a |f(a) - g(a)|$:

$$
|(TQ_1)(s,a) - (TQ_2)(s,a)| \leq \lambda \sum_{s'} p(s'|s,a) \max_{a'} |Q_1(s',a') - Q_2(s',a')|.
$$

$$
\leq \lambda \|Q_1 - Q_2\|_\infty.
$$

Taking the supremum over $(s,a)$ gives the result. $\square$

This contraction property guarantees that value iteration converges to $Q^*$ from any starting point.

---

### 1.2.7 Classical RL Algorithms

**Value Iteration:**

Value iteration uses the Bellman optimality operator to iteratively improve the $Q$-function:

```
Algorithm 1.1: Value Iteration
1: Initialize Q(s,a) arbitrarily for all s ∈ S, a ∈ A
2: Repeat:
3:   Δ ← 0
4:   For each (s,a):
5:     Q_new(s,a) ← Σ_{s'} p(s'|s,a)[r(s,a,s') + λ max_{a'} Q(s',a')]
6:     Δ ← max(Δ, |Q_new(s,a) - Q(s,a)|)
7:     Q(s,a) ← Q_new(s,a)
8: Until Δ < ε (convergence tolerance)
9: Return Q
```

**Policy Iteration:**

Policy iteration alternates between policy evaluation (computing $Q^\pi$) and policy improvement (making the policy greedy with respect to $Q^\pi$):

```
Algorithm 1.2: Policy Iteration
1: Initialize policy π arbitrarily
2: Repeat:
3:   // Policy Evaluation
4:   Repeat:
5:     For each (s,a):
6:       Q(s,a) ← Σ_{s'} p(s'|s,a)[r(s,a,s') + λ Q(s', π(s'))]
7:   Until Q converges to Q^π
8:   // Policy Improvement
9:   π_new(s) ← argmax_a Q(s,a) for all s
10:  If π_new = π: break
11:  π ← π_new
12: Return π
```

**Q-Learning (Model-Free):**

Q-learning is a model-free algorithm that learns $Q^*$ directly from experience, without requiring knowledge of $p$ or $r$:

```
Algorithm 1.3: Q-Learning
1: Initialize Q(s,a) arbitrarily for all s ∈ S, a ∈ A
2: For each episode:
3:   Initialize state s
4:   Repeat:
5:     Choose a using policy derived from Q (e.g., ε-greedy)
6:     Take action a, observe r, s'
7:     Q(s,a) ← Q(s,a) + α[r + λ max_{a'} Q(s',a') - Q(s,a)]
8:     s ← s'
9:   Until s is terminal
10: Return Q
```

**Convergence of Q-Learning:**

**Theorem 1.3 (Convergence of Q-Learning).** Under the following conditions:
- The state and action spaces are finite.
- The learning rate $\alpha_t$ satisfies $\sum_t \alpha_t = \infty$ and $\sum_t \alpha_t^2 < \infty$.
- Every state-action pair is visited infinitely often.
- The rewards are bounded.

Then Q-learning converges to $Q^*$ with probability 1.

---

### 1.2.8 The Success and Limitations of Classical RL

**Successes of Classical RL:**

1. **Theoretical Foundations:** Bellman equations, contraction properties, convergence guarantees.
2. **Practical Successes:** Game playing (Atari, Go, Chess), robotics, recommender systems.
3. **General Framework:** Applicable to a wide range of domains and problems.
4. **Scalability:** Modern deep RL methods scale to high-dimensional state and action spaces.

**Limitations of Classical RL:**

1. **Single Objective:** Assumes a single scalar reward captures all relevant objectives.
2. **Reward Engineering:** Requires careful design of reward functions, which is often difficult.
3. **Exploration:** Requires manual tuning of exploration parameters ($\epsilon$, $\beta$, etc.).
4. **Multi-Objective:** Cannot naturally handle multiple, competing objectives.
5. **Parameter Sensitivity:** Performance depends critically on hyperparameter choices.

**The Central Limitation for This Book:**

Classical RL treats rewards as scalar quantities. When an agent faces multiple objectives—like cost and debt—they must be combined into a single scalar. This scalarisation introduces the free parameter problem (Section 1.1.2). The agent cannot naturally balance cost and debt without external tuning.

**This is the problem we will solve in the remainder of this book.**

---

### 1.2.9 Key Takeaways

1. **RL Framework:** Agent-environment interaction, Markov property, policies, rewards.
2. **MDP:** $(\mathcal{S}, \mathcal{A}, p, r, \lambda)$ provides the mathematical foundation.
3. **Value Functions:** $V^\pi(s)$ and $Q^\pi(s,a)$ capture expected returns.
4. **Bellman Equations:** Recursive relationships that enable dynamic programming.
5. **Bellman Optimality:** The Bellman optimality operator is a $\lambda$-contraction, ensuring convergence.
6. **Classical Algorithms:** Value iteration, policy iteration, Q-learning all rely on scalar rewards.
7. **Limitation:** Single scalar reward is insufficient for complex real-world problems with multiple objectives.

---

### 1.2.10 Exercises for Section 1.2

**Exercise 1.6 (MDP Definition).** Consider a simple grid world with 3x3 cells, where the agent can move Up, Down, Left, or Right. If the agent attempts to move outside the grid, it stays in place. The agent receives -1 for each move and +10 for reaching the goal. Define this as an MDP by specifying $\mathcal{S}, \mathcal{A}, p, r, \lambda$. Assume $\lambda = 0.9$.

**Exercise 1.7 (Computing Returns).** Consider an episode with rewards $[2, 4, 6, 8]$ and $\lambda = 0.8$. Compute $G_0, G_1, G_2, G_3$.

**Exercise 1.8 (Bellman Equation for a Simple MDP).** Consider a 2-state MDP with:
- States: $s_1, s_2$
- Actions: $a_1$ (stay in the same state), $a_2$ (transition to the other state)
- Transitions:
  - $p(s_1|s_1, a_1) = 1, p(s_2|s_2, a_1) = 1$
  - $p(s_2|s_1, a_2) = 1, p(s_1|s_2, a_2) = 1$
- Rewards: $r(s_1, a_1) = 0, r(s_2, a_1) = 5, r(s_1, a_2) = 10, r(s_2, a_2) = 0$
- $\lambda = 0.9$

Write the Bellman equations for $V^\pi$ for the policy $\pi(s_1) = a_1, \pi(s_2) = a_1$. Solve for $V^\pi(s_1)$ and $V^\pi(s_2)$.

**Exercise 1.9 (Contraction Property).** Prove that the Bellman evaluation operator $T^\pi$ is a $\lambda$-contraction. Follow the proof structure in Section 1.2.5.

**Exercise 1.10 (Q-Learning Update).** For the MDP in Exercise 1.8, perform one Q-learning update from state $s_1$ taking action $a_2$, receiving reward 10, and transitioning to state $s_2$. Assume initial $Q(s,a) = 0$ for all $(s,a)$, $\lambda = 0.9$, $\alpha = 0.1$.

---

### 1.2.11 Further Reading for Section 1.2

- Sutton, R. S. & Barto, A. G. (2018). *Reinforcement Learning: An Introduction*, 2nd ed. MIT Press. — The definitive textbook on RL, covering MDPs, Bellman equations, and classical algorithms in detail.

- Puterman, M. L. (1994). *Markov Decision Processes: Discrete Stochastic Dynamic Programming*. Wiley. — A comprehensive mathematical treatment of MDPs.

- Bertsekas, D. P. & Tsitsiklis, J. N. (1996). *Neuro-Dynamic Programming*. Athena Scientific. — Covers the theoretical foundations of RL and dynamic programming.

- Szepesvári, C. (2010). *Algorithms for Reinforcement Learning*. Morgan & Claypool. — A concise introduction to RL algorithms with a focus on theory.

---

