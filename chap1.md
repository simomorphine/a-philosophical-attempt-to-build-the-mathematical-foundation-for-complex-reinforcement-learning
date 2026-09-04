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

*Figure 1.1: Trajectory in the cost-debt plane. The agent starts with high debt (uncertainty) and moves toward the origin as it gathers information and minimizes cost. The path is determined by the geometry of* $\mathbb{C}$, *not by external parameters.*

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

### 1.1.5 Summary of Section 1.1

In this introductory section, we have established the core problem that motivates this book:

1. **Two Fundamental Burdens:** Every adaptive agent faces both real cost ($c$) and information debt ($d$). These are qualitatively different—cost is additive and irreversible, while debt is conservative and reversible.

2. **The Failure of Scalarisation:** Classical RL attempts to combine these burdens into a single scalar reward $r = c + \mu d$. This introduces a free parameter $\mu$ that must be tuned and is environment-dependent.

3. **The Free Parameter Problem:** No single $\mu$ works across all environments, tasks, or states. This is a fundamental limitation, not a practical nuisance.

4. **A Geometric Alternative:** Encoding cost and debt as the real and imaginary parts of a complex number $z = c + id$ eliminates the free parameter. Minimizing the modulus $|z| = \sqrt{c^2 + d^2}$ is principled, parameter-free, and geometrically natural.

5. **Humble Systems Theory:** The HST Equilibrium Axiom asserts that all information processors evolve toward epistemic equilibrium—a state of zero debt. The transition from exploration to exploitation is automatic, driven by geometry.


---

### 1.1.6 Exercises for Section 1.1

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

### 1.1.7 Further Reading for Section 1.1

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

## 1.3 Why Scalarisation Fails

### 1.3.1 The Standard Approach: Linear Scalarisation

When an agent faces multiple objectives, the standard approach in reinforcement learning is to combine them into a single scalar reward. This process is called **scalarisation**.

**Definition 1.6 (Linear Scalarisation).** Given two objectives—real cost $c$ and information debt $d$—the scalarised reward is:

$$
r_{\text{total}} = c + \mu d,
$$

where $\mu \geq 0$ is a weighting parameter.

More generally, for $n$ objectives $r_1, r_2, \ldots, r_n$:

$$
r_{\text{total}} = \sum_{i=1}^n \mu_i r_i,
$$

where $\mu_i \geq 0$ are weights.

**How It Works:**

1. Choose weights $\mu_i$ that reflect the relative importance of each objective.
2. Optimize the scalarised objective using standard RL algorithms.
3. The resulting policy reflects the chosen weights.

**Advantages:**

- **Simplicity:** Uses standard RL algorithms without modification.
- **Interpretability:** Weights can be interpreted as the "value" of each objective.
- **Computational Efficiency:** Single objective optimization is well-understood.

**Disadvantages:**

- **Arbitrary Weights:** The choice of weights is subjective and problem-dependent.
- **Scale Sensitivity:** Weights must account for the relative scales of different objectives.
- **No Universal Solution:** Different environments require different weights.
- **Convexity Assumption:** Linear scalarisation can only find policies on the convex hull of the Pareto frontier.

**The Fundamental Question:** How should we choose $\mu$? There is no principled answer. The choice is often made by trial and error, or based on domain expertise—neither of which is satisfactory for autonomous agents.

---

### 1.3.2 The Free Parameter Problem

**Definition 1.7 (The Free Parameter Problem).** For any $\mu > 0$, the optimal policy $\pi^*_\mu$ for the scalarised objective depends on $\mu$. There is no universal $\mu$ that yields the optimal trade-off across all environments.

**Why This Matters:**

1. **Environment Dependence:** The optimal $\mu$ depends on the environment's structure—its transition probabilities, reward scales, and dynamics.

2. **Scale Sensitivity:** If cost and debt have different scales, the weight must be adjusted. For example, if cost ranges from 0 to 100 and debt ranges from 0 to 1, a small $\mu$ is needed; if the ranges are reversed, a large $\mu$ is needed.

3. **Task Dependence:** Different tasks within the same environment may require different weights. A task with a tight energy budget requires a different $\mu$ than a task where information is critical.

4. **State Dependence:** The optimal balance may differ between early exploration (where debt is high) and late exploitation (where debt is low).

5. **No Ground Truth:** There is no principled way to choose $\mu$ for a new problem. The agent cannot know the optimal $\mu$ without solving the full problem.

**Theorem 1.4 (Non-Existence of Universal $\mu$).** For any fixed $\mu > 0$, there exist MDPs $\mathcal{M}_1$ and $\mathcal{M}_2$ such that the optimal policies for the scalarised objectives are different.

**Proof by Construction:**

Construct $\mathcal{M}_1$ with:
- Cost range $[0, C_1]$, Debt range $[0, D_1]$ where $C_1 \gg D_1$.
- The optimal policy for $\mu$ is the cost-minimizing policy (since cost dominates).

Construct $\mathcal{M}_2$ with:
- Cost range $[0, C_2]$, Debt range $[0, D_2]$ where $D_2 \gg C_2$.
- The optimal policy for the same $\mu$ is the debt-minimizing policy (since debt dominates).

For $\mu$ to yield the same policy in both environments, we would need:

$$
\mu C_1 = \mu C_2 \quad \text{and} \quad \mu D_1 = \mu D_2,
$$

which implies $C_1/D_1 = C_2/D_2$. This is not generally true. Therefore, no single $\mu$ works for both environments. $\square$

**Corollary 1.1.** Linear scalarisation cannot be universally optimal across all environments.

---

### 1.3.3 The Inherent Dependence on Environment

The free parameter problem is not just a theoretical concern—it has practical consequences. Consider two environments with different scales:

**Environment A: Cost Variations Dominate**

- Cost ranges: 0 to 100
- Debt ranges: 0 to 10
- The optimal $\mu$ is small (cost matters more)

**Environment B: Debt Variations Dominate**

- Cost ranges: 0 to 10
- Debt ranges: 0 to 100
- The optimal $\mu$ is large (debt matters more)

No single $\mu$ works for both environments. The agent would need to know the scales in advance to choose the right $\mu$.

**Example 1.4 (Two Different Tasks).** Consider a robot operating in two different scenarios:

**Scenario 1: Time-Critical Mission**
- The robot must complete a task within a strict deadline.
- Cost = time taken (0-100 seconds).
- Debt = uncertainty about the environment (0-10).
- Optimal $\mu$ is small (time is critical).

**Scenario 2: Exploration Mission**
- The robot must map an unknown environment.
- Cost = energy used (0-10 units).
- Debt = unmapped area (0-100 units).
- Optimal $\mu$ is large (information is critical).

The same robot, with the same hardware, would need different $\mu$ values for these two scenarios. No single $\mu$ works for both.

---

### 1.3.4 The Pareto Frontier: The Geometry of the Trade-off

To understand why scalarisation fails, we need to introduce the concept of the **Pareto frontier**.

**Definition 1.8 (Pareto Dominance).** A policy $\pi_1$ dominates $\pi_2$ if:
- $c_{\pi_1} \leq c_{\pi_2}$ and $d_{\pi_1} \leq d_{\pi_2}$, and
- At least one inequality is strict.

**Definition 1.9 (Pareto Frontier).** The Pareto frontier is the set of policies that are not dominated by any other policy. These are the policies where improving one objective (cost) requires worsening the other (debt), and vice versa.

**Visual Representation:**

```
Debt (d)
  ↑
  |    * 
  |   *   ← Pareto Frontier (non-dominated policies)
  |  *    
  | *     ← Dominated policies (worse in both objectives)
  |*      
  | 
  +--------------------→ Cost (c)
```

**Key Insight:** The Pareto frontier represents the optimal trade-offs between cost and debt. Any point on the frontier is "optimal" in the sense that no other policy is better in both objectives.

**The Scalarisation Problem:**

Linear scalarisation $r = c + \mu d$ selects a single point on the Pareto frontier. Different $\mu$ values select different points:

- **$\mu = 0$:** Minimizes cost only ($d$ is ignored). Selects the point with lowest cost.
- **$\mu \to \infty$:** Minimizes debt only ($c$ is ignored). Selects the point with lowest debt.
- **$0 < \mu < \infty$:** Selects a point on the frontier determined by $\mu$.

The scalarisation selects the point where the line $c + \mu d = \text{constant}$ is tangent to the Pareto frontier.

**Example 1.5 (Pareto Frontier in a 2-State MDP).** Consider four policies with cost-debt pairs:

| Policy | Cost $c$ | Debt $d$ |
|--------|----------|----------|
| $\pi_1$ | 1 | 5 |
| $\pi_2$ | 2 | 3 |
| $\pi_3$ | 4 | 1 |
| $\pi_4$ | 5 | 0 |

The Pareto frontier is $\{(1,5), (2,3), (4,1), (5,0)\}$. No policy dominates another on both objectives.

**Scalarisation Analysis:**

- $\mu = 0$: Selects $\pi_4$ (lowest cost).
- $\mu = 0.5$: Values: $\pi_1 = 3.5, \pi_2 = 3.5, \pi_3 = 4.5, \pi_4 = 5$ → selects $\pi_1$ or $\pi_2$.
- $\mu = 1$: Values: $\pi_1 = 6, \pi_2 = 5, \pi_3 = 5, \pi_4 = 5$ → selects $\pi_2, \pi_3, \pi_4$.
- $\mu = 2$: Values: $\pi_1 = 11, \pi_2 = 8, \pi_3 = 6, \pi_4 = 5$ → selects $\pi_4$.
- $\mu \to \infty$: Selects $\pi_1$ (lowest debt).

Different $\mu$ values select different points on the frontier. The optimal $\mu$ depends on the problem.

---

### 1.3.5 A Concrete Counterexample: Grid World with Cost and Debt

Consider a 3x3 grid world that vividly illustrates the failure of scalarisation:

```
[S] [ ] [ ]
[ ] [ ] [ ]
[ ] [ ] [G]
```

- $S$ = Start (top-left), $G$ = Goal (bottom-right).
- Actions: Up, Down, Left, Right.
- **Cost:** -1 per move (energy consumption).
- **Debt:** -2 for visiting a new state (information gain), +1 for revisiting a state (no new information).

**Policies and Their Cost-Debt Pairs:**

| Policy | Description | Cost $c$ | Debt $d$ | $c + \mu d$ |
|--------|-------------|----------|----------|-------------|
| $\pi_1$ | Shortest path (4 moves) | 4 | -2 (visited 1 new state) | $4 - 2\mu$ |
| $\pi_2$ | Path visiting 3 new states (6 moves) | 6 | -6 | $6 - 6\mu$ |
| $\pi_3$ | Path visiting 5 new states (8 moves) | 8 | -10 | $8 - 10\mu$ |
| $\pi_4$ | Random walk (many moves) | 12 | -12 | $12 - 12\mu$ |

**Scalarisation Analysis:**

- $\mu = 0$: $\pi_1$ is optimal (cost only).
- $\mu = 0.5$: Values: $\pi_1 = 3, \pi_2 = 3, \pi_3 = 3, \pi_4 = 6$ → $\pi_1, \pi_2, \pi_3$ are tied.
- $\mu = 1$: Values: $\pi_1 = 2, \pi_2 = 0, \pi_3 = -2, \pi_4 = 0$ → $\pi_3$ is optimal.
- $\mu = 2$: Values: $\pi_1 = 0, \pi_2 = -6, \pi_3 = -12, \pi_4 = -12$ → $\pi_3, \pi_4$ are tied.
- $\mu \to \infty$: $\pi_3$ or $\pi_4$ is optimal (debt only).

**The Problem:**

- If the agent is in a new environment (high uncertainty), it should explore more (high $\mu$).
- If the agent is in a familiar environment, it should exploit more (low $\mu$).
- The agent doesn't know the environment well enough to choose the right $\mu$.
- No single $\mu$ works across all states and phases of learning.

**The Dilemma:** The agent needs to choose $\mu$ before it knows what $\mu$ should be. This is a catch-22.

---

### 1.3.6 Alternative Scalarisation Approaches

Linear scalarisation is not the only way to combine objectives. Several alternatives have been proposed:

**1. Non-Linear Scalarisation:**

$$
r_{\text{total}} = f(c,d),
$$

where $f$ is a non-linear function.

**Examples:**
- **Weighted Product:** $f(c,d) = c^\alpha d^\beta$
- **Tchebycheff:** $f(c,d) = \max(w_c c, w_d d)$
- **Goal Programming:** Minimize deviation from an ideal point $(c^*, d^*)$

**Problems:**
- Still requires choosing parameters ($\alpha, \beta, w_c, w_d$).
- The function $f$ must be chosen a priori.
- The optimal choice depends on the environment.

**2. Lexicographic Ordering:**

Prioritize objectives: optimize the primary objective first, then the secondary objective subject to the primary being optimal.

**Problems:**
- Requires a strict priority ordering.
- Cannot find compromises between objectives.
- The priority order depends on the environment.

**3. Pareto-Based Methods:**

Maintain a set of non-dominated policies (the Pareto frontier) and select among them using a secondary criterion.

**Problems:**
- Computationally expensive (maintaining the frontier).
- Requires storing multiple policies.
- Still requires a secondary criterion for selection.

**4. Threshold-Based Methods:**

Satisfy constraints on secondary objectives: $d \leq D_{\text{max}}$ while optimizing cost.

**Problems:**
- Requires choosing thresholds ($D_{\text{max}}$).
- The optimal threshold depends on the environment.
- May be infeasible if thresholds are too strict.

**The Common Thread:**

All scalarisation approaches require choosing parameters (weights, priorities, thresholds, exponents). These parameters are:

1. **Arbitrary:** No principled way to choose them.
2. **Environment-dependent:** The optimal choice depends on the problem.
3. **Task-dependent:** Different tasks require different parameters.
4. **State-dependent:** The optimal choice may change over time.

**This is the free parameter problem in its most general form.**

---

### 1.3.7 The Geometry of Failure

Why do linear combinations fail geometrically? The answer lies in the shape of the level sets.

**Linear Scalarisation Level Sets:**

The scalarisation $c + \mu d = \text{constant}$ defines lines in the cost-debt plane:

$$
d = -\frac{1}{\mu} c + \text{constant}.
$$

These lines have fixed slope $-1/\mu$. The scalarisation selects the point on the Pareto frontier where a line of this slope is tangent.

**The Problem:**

The Pareto frontier's shape varies across environments. The same slope corresponds to different points on different frontiers. The optimal slope (and hence the optimal $\mu$) depends on the frontier's shape.

**Example 1.6 (Different Pareto Frontiers).**

**Frontier A (Convex):**
- Points: $(1,4), (2,2), (3,1), (4,0.5)$
- The slope that selects the middle point is different from Frontier B.

**Frontier B (Concave):**
- Points: $(1,5), (2,4), (3,3), (4,2)$
- The same slope selects a different point.

**Frontier C (Piecewise Linear):**
- Points: $(1,6), (2,4), (3,2), (4,1)$
- The same slope may select a corner point, which is sensitive to $\mu$.

**The Conclusion:**

No single scalarisation can universally capture the cost-debt trade-off because:
1. The Pareto frontier's shape is environment-dependent.
2. The optimal slope (weight) depends on the frontier's shape.
3. The agent cannot know the frontier's shape in advance.

---

### 1.3.8 The Scalarisation Illusion

There is a deeper problem with scalarisation: it creates the illusion that cost and debt are commensurable—that they can be meaningfully compared and combined.

**The Commensurability Assumption:**

Scalarisation assumes that there exists a common currency for cost and debt. But:

- **Cost is physical:** It has units (joules, seconds, dollars).
- **Debt is informational:** It has units (bits, nats, uncertainty).

These are fundamentally different types of quantity. Comparing them requires a conversion factor $\mu$ that is essentially arbitrary.

**Example 1.7 (The Commensurability Problem).**

How many joules of energy are equivalent to one bit of information? This question has no universal answer. It depends on:

- The physical system (robot, computer, human).
- The environment (terrain, task, constraints).
- The time horizon (short-term vs. long-term).

Scalarisation pretends that such a conversion exists. But the conversion factor $\mu$ is not a physical constant—it's a design parameter that must be chosen by the system designer.

**The Scalarisation Illusion:**

Scalarisation gives the illusion of having solved the multi-objective problem when it has merely hidden the difficulty in a parameter. The agent still needs to know how to balance cost and debt—the scalarisation just pushes the problem to the designer.

**The HST Response:**

Humble Systems Theory (HST) argues that the scalarisation illusion should be abandoned. Instead, we should recognize that cost and debt are orthogonal components of a single geometric object. The geometry of the cost-debt plane provides the balance—no arbitrary conversion factor is needed.

---

### 1.3.9 Summary of Section 1.3

In this section, we have demonstrated why scalarisation fails as a solution to the problem of balancing cost and debt:

1. **Linear Scalarisation:** The standard approach combines objectives using weights $\mu$. This is simple but flawed.

2. **The Free Parameter Problem:** No single $\mu$ works across all environments, tasks, or states. The optimal $\mu$ depends on the problem.

3. **Scale Sensitivity:** If cost and debt have different scales, the weight must be adjusted. The agent cannot know the right scale in advance.

4. **The Pareto Frontier:** Scalarisation selects a single point on the Pareto frontier. Different $\mu$ values select different points. The optimal $\mu$ depends on the frontier's shape.

5. **Alternative Approaches:** Non-linear scalarisation, lexicographic ordering, Pareto-based methods, and threshold-based methods all suffer from the same problem: they require parameters that must be tuned.

6. **The Geometry of Failure:** Linear scalarisation level sets are lines with fixed slope. The Pareto frontier's shape varies across environments. No single slope works for all frontiers.

7. **The Scalarisation Illusion:** Scalarisation creates the illusion that cost and debt are commensurable. In reality, they are fundamentally different types of quantity.

**The Conclusion:**

Scalarisation is not a solution—it's a way of hiding the problem. We need a different approach that does not require arbitrary parameters. This is the subject of the next section.

---

### 1.3.10 Exercises for Section 1.3

**Exercise 1.11 (Scalarisation Failure).** Construct a concrete environment where the optimal $\mu$ changes by a factor of 10 depending on the task. Show that no single $\mu$ can be optimal for both tasks.

**Exercise 1.12 (Pareto Frontier).** For the 2-state MDP in Exercise 1.8, compute the Pareto frontier of cost vs. debt for all deterministic policies. Plot the frontier. For each policy, compute $c + \mu d$ for $\mu = 0, 0.5, 1, 2$. Which policy is selected for each $\mu$?

**Exercise 1.13 (Scale Sensitivity).** Consider two environments:
- Environment A: Cost ranges 0-100, Debt ranges 0-1.
- Environment B: Cost ranges 0-1, Debt ranges 0-100.

For each environment, what $\mu$ would you need to balance cost and debt equally? Show that no single $\mu$ works for both.

**Exercise 1.14 (Non-Linear Scalarisation).** Consider the scalarisation $r = c^\alpha d^\beta$. For $\alpha = \beta = 0.5$, compute the value of each policy in Example 1.5. Does this avoid the free parameter problem? Why or why not?

**Exercise 1.15 (The Scalarisation Illusion).** Explain in your own words why scalarisation creates the illusion of commensurability. Give an example where the conversion between cost and debt is clearly arbitrary.

---

### 1.3.11 Further Reading for Section 1.3

- Roijers, D. M., Vamplew, P., Whiteson, S., & Dazeley, R. (2013). "A Survey of Multi-Objective Sequential Decision-Making." *Journal of Artificial Intelligence Research*, 48:67-113. — A comprehensive survey of multi-objective RL, including scalarisation methods and their limitations.

- Vamplew, P., Dazeley, R., & Foale, C. (2017). "Softmax Exploration Strategies for Multi-Objective Reinforcement Learning." *Neurocomputing*, 263:74-86. — Discusses exploration in multi-objective RL.

- Miettinen, K. (1999). *Nonlinear Multiobjective Optimization*. Springer. — A comprehensive treatment of multi-objective optimization, including scalarisation methods.

- Pareto, V. (1906). *Manual of Political Economy*. — The original formulation of Pareto optimality.

---


## 1.4 A Geometric Alternative: The Complex Number Encoding

### 1.4.1 From Weighting to Geometry

The failure of scalarisation suggests a fundamental question: instead of asking *"How should we weight cost and debt?"*, we should ask a different question entirely:

> **What is the natural geometry of cost and debt?**

This shift in perspective is profound. It moves us from the realm of arbitrary parameters to the realm of geometric structure. The geometry of the cost-debt plane already contains the answer—we just need to recognize it.

**The Key Observation:**

Cost and debt are not two quantities that need to be *combined*—they are two components of a single geometric object. The Euclidean distance between cost and debt is natural, principled, and free of arbitrary weights:

$$
\text{Distance} = \sqrt{c^2 + d^2}.
$$

This is the modulus of the complex number:

$$
z = c + id,
$$

where $i = \sqrt{-1}$ is the imaginary unit.

**Why Complex Numbers?**

1. **Orthogonality:** Real and imaginary parts are independent—cost and debt are orthogonal dimensions.
2. **Natural Metric:** The modulus $|z|$ is the Euclidean distance from the origin.
3. **Phase:** The angle $\theta = \arg(z)$ encodes the trade-off between cost and debt.
4. **No Arbitrary Weights:** The geometry selects the trade-off automatically.
5. **Rich Structure:** Complex analysis provides powerful tools (analytic functions, contour integration, conformal mappings).

**The Central Insight:**

The agent that encodes both cost and debt as a single complex number does not need to be told how to balance them. The geometry already knows. The modulus $|z|$ automatically balances cost and debt based on their magnitudes, and the phase $\theta$ tells the agent the current trade-off.

---

### 1.4.2 The Complex Utility

**Definition 1.10 (Complex Utility).** Let:

$$
z = c + id \in \mathbb{C},
$$

where:
- $c \in \mathbb{R}_{\geq 0}$ is the real cost (non-negative).
- $d \in \mathbb{R}$ is the imaginary debt (sign-unrestricted).

The complex utility $z$ represents the total burden of an action or transition in the cost-debt plane.

**The Cost-Debt Plane:**

The cost-debt plane is a 2D coordinate system where:
- The real axis represents cost $c$.
- The imaginary axis represents debt $d$.

```
      Debt (d)
         ↑
         |    d > 0 (increasing uncertainty)
         |
         |    c > 0 (energy/time)
   ←------+------→ Cost (c)
         |    c > 0
         |
         |    d < 0 (knowledge gained)
         |
         ↓
```

Every action or transition corresponds to a point $(c,d)$ in this plane.

**Geometric Interpretation of Points:**

| Point | Interpretation |
|-------|----------------|
| $(c, 0)$ | Pure cost action (exploitation) |
| $(0, d)$ | Pure debt action (exploration) |
| $(c, d)$ with $c > 0, d > 0$ | Action that both costs and creates debt |
| $(c, d)$ with $c > 0, d < 0$ | Action that pays down debt at a cost |
| $(c, d)$ with $c = 0, d < 0$ | Pure information gathering (no cost) |

**Example 1.8 (Actions in the Cost-Debt Plane).**

| Action | Cost $c$ | Debt $d$ | Complex $z$ | Interpretation |
|--------|----------|----------|-------------|----------------|
| Known path | 5 | 0 | $5 + 0i$ | Pure exploitation |
| Exploration | 2 | -3 | $2 - 3i$ | Paying down debt |
| Wrong turn | 1 | +1 | $1 + i$ | Cost + creating debt |
| Learning task | 0 | -5 | $-5i$ | Pure exploration |

---

### 1.4.3 The Modulus as a Natural Objective

**Definition 1.11 (Modulus Objective).** The performance criterion is:

$$
J(z) = |z| = \sqrt{c^2 + d^2}.
$$

Minimizing $|z|$ has several desirable properties:

**1. Principled:**

Euclidean distance is the natural metric on the cost-debt plane. It is the shortest path to the origin—the point of zero cost and zero debt.

**2. Parameter-Free:**

No weights to tune. No $\mu$, no $\epsilon$, no $\beta$, no schedule. The geometry provides the balance.

**3. Geometric:**

The objective is invariant under rotations in the cost-debt plane. This means the agent's behavior is independent of arbitrary coordinate choices.

**4. Scale-Aware:**

Automatically balances cost and debt based on their magnitudes. If cost is large relative to debt, the agent focuses on reducing cost. If debt is large relative to cost, the agent focuses on gathering information.

**5. Convex:**

The modulus $|z|$ is a convex function in the cost-debt plane. This ensures that minimization problems are well-behaved.

**Why Minimizing $|z|$ Works:**

Consider three actions with different cost-debt trade-offs but the same modulus:

| Action | Cost $c$ | Debt $d$ | $|z|$ |
|--------|----------|----------|-------|
| $\pi_1$ | 5 | 0 | 5 |
| $\pi_2$ | 3 | 4 | 5 |
| $\pi_3$ | 4 | 3 | 5 |

All three actions have the same $|z| = 5$ but different cost-debt trade-offs. The geometry selects the action with the most favorable trade-off for the current situation. As the agent learns, the optimal point on the Pareto frontier shifts, and the geometry automatically tracks it.

**The Circles of Equal $|z|$:**

The level sets of the modulus are circles:

$$
c^2 + d^2 = R^2.
$$

Minimizing $|z|$ means moving toward the origin along the steepest descent path. The geometry naturally pulls the agent toward epistemic equilibrium.

```
Debt (d)
  ↑
  |    (5, 0)  · (3, 4)
  |   /         ·
  |  /   (4, 3) ·
  | /          ·
  |/          ·  ← Circle of radius 5
  +----·------------→ Cost (c)
      Origin (0,0)
```

**Comparison with Scalarisation:**

| Aspect | Scalarisation $c + \mu d$ | Complex $|z| = \sqrt{c^2 + d^2}$ |
|--------|--------------------------|----------------------------------|
| **Weighting** | $\mu$ is arbitrary | No weights |
| **Level Sets** | Lines $c + \mu d = \text{const}$ | Circles $c^2 + d^2 = R^2$ |
| **Phase** | Not present | $\theta = \arctan(d/c)$ |
| **Trade-off** | Determined by $\mu$ | Determined by geometry |
| **Universality** | No | Yes |
| **Parameters** | One ($\mu$) | Zero |

---

### 1.4.4 The Phase as the Exploration-Exploitation Angle

**Definition 1.12 (Phase).** The phase (or argument) of the complex utility is:

$$
\theta = \arg(z) = \arctan\left(\frac{d}{c}\right),
$$

where $\theta \in (-\pi/2, \pi/2)$ for $c > 0$, with special cases:
- $\theta = \pi/2$ when $c = 0, d > 0$.
- $\theta = -\pi/2$ when $c = 0, d < 0$.
- $\theta$ is undefined when $c = 0, d = 0$ (the origin).

**Interpretation:**

The phase $\theta$ represents the instantaneous balance between cost and debt—the exploration-exploitation angle.

| $\theta$ | Interpretation |
|----------|----------------|
| $\theta = 0$ | Pure exploitation (only cost matters, $d = 0$) |
| $\theta = \pi/2$ | Pure exploration (only debt matters, $c = 0, d > 0$) |
| $0 < \theta < \pi/2$ | Balance with debt dominating (more exploration) |
| $-\pi/2 < \theta < 0$ | Paying down debt (gaining information at a cost) |
| $\theta \to 0$ | Converging to exploitation |

**The Exploration Angle in Action:**

```
Debt (d)
  ↑
  |    /
  |   /  θ = arctan(d/c)
  |  /   
  | /    
  |/ θ   
  +----→ Cost (c)
```

The phase $\theta$ provides a continuous measure of the exploration-exploitation balance:

- When $\theta$ is large (close to $\pi/2$), the agent is in exploration mode—debt dominates.
- When $\theta$ is small (close to 0), the agent is in exploitation mode—cost dominates.
- When $\theta$ is negative, the agent is paying down debt (gaining information).

**Example 1.9 (Phase as Learning Progress).**

Consider an agent learning a new environment:

| Time | Cost $c$ | Debt $d$ | $|z|$ | $\theta$ | Interpretation |
|------|----------|----------|-------|----------|----------------|
| $t=0$ | 1 | 10 | 10.05 | 84.3° | Mostly exploration |
| $t=10$ | 3 | 5 | 5.83 | 59.0° | Exploring, some cost |
| $t=50$ | 5 | 2 | 5.39 | 21.8° | Mostly exploiting |
| $t=100$ | 8 | 0.5 | 8.02 | 3.6° | Pure exploitation |
| $t \to \infty$ | 10 | 0 | 10 | 0° | Epistemic equilibrium |

As the agent learns, the phase decreases from exploration toward exploitation. The transition is automatic—no schedule, no tuning.

---

### 1.4.5 The Geometry Already Knows

**The Central Insight:**

The agent that encodes both cost and information debt as a single complex number does not need to be told how to balance them. The geometry already knows.

**Why:**

1. **The modulus $|z|$ automatically balances cost and debt:** When cost is high, the modulus is dominated by cost; when debt is high, it's dominated by debt. The agent naturally focuses on the larger burden.

2. **The phase $\theta$ tells the agent the current trade-off:** The agent can use $\theta$ to guide its decisions, without needing to set a parameter.

3. **No tuning, no parameters, no arbitrary choices:** The geometry of $\mathbb{C}$ provides the trade-off intrinsically.

**A Simple Decision Rule:**

The modulus-greedy policy selects the action that minimizes the modulus:

$$
\pi_Q(s) = \arg\min_{a \in \mathcal{A}} |Q(s,a)|.
$$

This policy automatically balances cost and debt. When $Q_I$ (the debt component) is large, the agent explores (since $|Q|$ is dominated by debt). When $Q_I$ is small, the agent exploits (since $|Q|$ is dominated by cost).

**The Automatic Transition:**

As the agent learns:
1. $Q_I$ decreases (debt is paid down).
2. The modulus $|Q|$ shifts from being debt-dominated to cost-dominated.
3. The policy shifts from exploration to exploitation.
4. No external parameters are required.

**Comparison with Classical Approaches:**

| Classical Approach | Complex Approach |
|--------------------|------------------|
| Choose $\mu$ | No $\mu$ |
| Tune exploration schedule | Automatic exploration |
| Heuristic trade-off | Geometric trade-off |
| Scalar rewards | Complex utility |
| Free parameters | No free parameters |
| Designer must choose balance | Geometry provides balance |

---

### 1.4.6 A Preview of the Complex Framework

This geometric insight leads to a complete framework for complex-valued reinforcement learning. Here is a preview of the key components:

**The Complex MDP (cMDP):**

$$
\mathcal{M} = (\mathcal{S}, \mathcal{A}, p, z, \lambda),
$$

where $z = c + id$ is the complex utility. Unlike the classical MDP with a scalar reward, the cMDP has a complex-valued utility.

**The Complex Return:**

$$
G_t = \sum_{k=0}^{\infty} \lambda^k z(S_{t+k}, A_{t+k}, S_{t+k+1}),
$$

where $G_t \in \mathbb{C}$ is the complex return.

**The Complex Value Functions:**

$$
V^\pi(s) = \mathbb{E}^\pi[G_t | S_t = s],
$$

$$
Q^\pi(s,a) = \mathbb{E}^\pi[G_t | S_t = s, A_t = a].
$$

Both $V^\pi$ and $Q^\pi$ are complex-valued.

**The Performance Criterion:**

$$
J^\pi(s) = |V^\pi(s)| = \sqrt{\text{Re}(V^\pi(s))^2 + \text{Im}(V^\pi(s))^2}.
$$

The agent seeks to minimize $|V^\pi(s)|$ for all states, or equivalently, to reach the origin of the cost-debt plane.

**Key Theoretical Results (Preview):**

1. **Evaluation Contraction:** The Bellman evaluation operator $T^\pi$ is a $\lambda$-contraction on the Banach space of complex value functions. This is proven in Chapter 9.

2. **Telescoping Identity:** Under the potential difference form, the imaginary $Q$-value telescopes to a closed-form expression. When interpreted as negative entropy, this becomes discounted cumulative mutual information.

3. **Mutual Information:** $Q_I^\pi$ equals discounted cumulative mutual information between actions and future states.

4. **Epistemic Equilibrium:** Under the HST Equilibrium Axiom, $|Q|^2$ is a Lyapunov function, guaranteeing convergence to zero debt.

5. **Automatic Exploration:** No exploration schedule is needed. The geometry of $\mathbb{C}$ provides the balance.

---

### 1.4.7 The Philosophical Implications

The complex framework has profound implications for how we think about adaptive systems:

**1. Duality is Fundamental:**

Cost and debt are not two separate objectives to be traded off—they are two sides of the same coin. Every action has both a cost and an information consequence. The complex plane captures this duality naturally.

**2. Geometry is Informative:**

The geometry of the problem space reveals the solution. The Euclidean distance to the origin is the natural objective. The phase tells us the current balance. The contours guide the system toward equilibrium.

**3. Parameters are Arbitrary:**

Free parameters should be eliminated wherever possible. They represent ignorance about the problem structure. The geometry already contains the necessary information.

**4. Emergent Behavior:**

Complex systems naturally find equilibrium without external intervention. The HST Equilibrium Axiom asserts that all information processors are driven toward epistemic equilibrium by the geometry of their problem space.

**5. Exploration is Intrinsic:**

Exploration is not something that must be programmed or incentivized—it emerges naturally from the geometry of cost and debt. When debt is high, the system must explore to reduce it. When debt is low, the system can exploit.

**Implications for AI Design:**

1. **Intrinsic Motivation:** AI agents should have intrinsic drives to reduce debt (information gain) and cost (energy/time). These drives emerge from the geometry.

2. **Autonomous Exploration:** AI agents should explore autonomously, without requiring external exploration schedules.

3. **Parameter-Free Learning:** AI agents should be designed to be as parameter-free as possible. The geometry of the problem should determine the behavior.

4. **Equilibrium-Seeking:** AI agents should seek equilibrium, not optimize a fixed objective. Equilibrium is the natural state of an information processing system.

---

### 1.4.8 Summary of Section 1.4

In this section, we have introduced the geometric alternative to scalarisation:

1. **From Weighting to Geometry:** Instead of asking "how should we weight cost and debt?", we ask "what is the natural geometry?"

2. **Complex Utility:** Encode cost and debt as $z = c + id$. Cost is the real part, debt is the imaginary part.

3. **Modulus Objective:** Minimize $|z| = \sqrt{c^2 + d^2}$. This is principled, parameter-free, and geometrically natural.

4. **Phase:** $\theta = \arctan(d/c)$ is the exploration-exploitation angle. It tells the agent the current balance between cost and debt.

5. **The Geometry Already Knows:** No tuning, no parameters, no arbitrary choices. The geometry provides the trade-off intrinsically.

6. **Preview of the Framework:** Complex MDP, complex return, complex value functions, and key theoretical results (evaluation contraction, telescoping identity, epistemic equilibrium).

7. **Philosophical Implications:** Duality is fundamental, geometry is informative, parameters are arbitrary, behavior is emergent, and exploration is intrinsic.

**The Central Claim:**

> The geometry of the complex plane provides a natural, parameter-free solution to the problem of balancing cost and debt. The agent does not need to be told how to balance them—the geometry already knows.

---

### 1.4.9 Exercises for Section 1.4

**Exercise 1.16 (Complex Utility).** For each of the following actions, compute the complex utility $z$, the modulus $|z|$, and the phase $\theta$:
- Action A: $c = 3, d = 4$
- Action B: $c = 4, d = -3$
- Action C: $c = 5, d = 0$
- Action D: $c = 0, d = -5$

Plot these points in the cost-debt plane. Which action has the smallest modulus?

**Exercise 1.17 (Modulus vs. Scalarisation).** For the actions in Exercise 1.16, compute $c + \mu d$ for $\mu = 0, 0.5, 1, 2$. Compare the ordering of actions under scalarisation with the ordering under the modulus. Under what conditions do they agree?

**Exercise 1.18 (Phase Interpretation).** Provide an intuitive explanation of why $\theta = \arctan(d/c)$ can be interpreted as the "exploration angle." In your explanation:
- Describe what $\theta = 0$ means (pure exploitation).
- Describe what $\theta = \pi/2$ means (pure exploration).
- Describe what intermediate values mean.
- Provide a real-world example of an agent transitioning from $\theta = \pi/2$ to $\theta = 0$ as it learns.

**Exercise 1.19 (Level Sets).** Draw the level sets of the modulus $|z| = R$ for $R = 1, 2, 3$ in the cost-debt plane. Draw the level sets of the scalarisation $c + \mu d = \text{constant}$ for $\mu = 0.5, 1, 2$. Compare the shapes. Why does the geometry of the modulus avoid the free parameter problem?

**Exercise 1.20 (The Modulus-Greedy Policy).** Consider four actions with $Q$-values:
- $Q_1 = 5 + 0i$
- $Q_2 = 4 + 3i$
- $Q_3 = 3 + 4i$
- $Q_4 = 0 + 5i$

Which action does the modulus-greedy policy select? Which action would a scalarisation with $\mu = 1$ select? Which action would be selected for different $\mu$ values?

---

### 1.4.10 Further Reading for Section 1.4

- Needham, T. (1997). *Visual Complex Analysis*. Oxford University Press. — An accessible and visually-oriented introduction to complex analysis.

- Ahlfors, L. V. (1979). *Complex Analysis*, 3rd ed. McGraw-Hill. — The classic textbook on complex analysis.

- Rudin, W. (1987). *Real and Complex Analysis*, 3rd ed. McGraw-Hill. — A rigorous treatment of complex analysis.

- Penrose, R. (2004). *The Road to Reality*. Knopf. — A broad treatment of the role of complex numbers in physics and mathematics.


---

## 1.6 Why Complex Numbers Are Not Arbitrary

### 1.6.1 The Uniqueness of the Complex Numbers

A natural question arises: why complex numbers? Why not vectors, matrices, quaternions, or some other mathematical structure? The answer is that complex numbers are the *unique* two-dimensional structure that satisfies the properties we need.

**Definition 1.15 (Real Algebra).** A real algebra is a vector space over $\mathbb{R}$ equipped with a bilinear multiplication operation.

**Theorem 1.5 (Classification of Two-Dimensional Real Algebras).** Any two-dimensional real algebra that is a field must be isomorphic to $\mathbb{C}$.

**Proof Sketch:**

Let $\mathcal{A}$ be a two-dimensional real algebra that is a field. Choose a basis $\{1, u\}$ where $1$ is the multiplicative identity. Then:

$$
u^2 = \alpha + \beta u
$$

for some $\alpha, \beta \in \mathbb{R}$, since $\mathcal{A}$ is two-dimensional.

Completing the square:

$$
(u - \beta/2)^2 = \alpha + \beta^2/4.
$$

If $\alpha + \beta^2/4 < 0$, then $\mathcal{A} \cong \mathbb{C}$ (with $i = (u - \beta/2)/\sqrt{|\alpha + \beta^2/4|}$).

If $\alpha + \beta^2/4 = 0$, then $\mathcal{A}$ has zero divisors (not a field).

If $\alpha + \beta^2/4 > 0$, then $\mathcal{A}$ has square roots of positive numbers, which implies it is isomorphic to the split-complex numbers, which are not a field (they have zero divisors).

Therefore, the only two-dimensional real algebra that is a field is $\mathbb{C}$. $\square$

**The Only Alternatives:**

1. **Split-Complex Numbers:** Numbers of the form $a + bj$ with $j^2 = 1$. These are not a field because $(1+j)(1-j) = 0$ (zero divisors). They do not have a compatible norm.

2. **Dual Numbers:** Numbers of the form $a + b\epsilon$ with $\epsilon^2 = 0$. These are not a field (zero divisors) and do not have a compatible norm.

3. **Quaternions:** Four-dimensional, not two-dimensional. They are non-commutative ($ij = -ji$), which makes them unsuitable for our purposes.

4. **Vectors:** Two-dimensional vectors lack a natural multiplication operation and a compatible norm that interacts with multiplication.

**The Implication:**

> If we want a two-dimensional structure with addition, multiplication, and a compatible norm, the complex numbers are the *unique* choice. They are not arbitrary—they are mathematically forced.

---

### 1.6.2 Comparison with Other Representations

Let us compare the complex numbers with alternative representations for encoding cost and debt.

**1. Vectors $(c,d)$:**

| Aspect | Vector Representation | Complex Representation |
|--------|----------------------|----------------------|
| **Addition** | Component-wise | Component-wise |
| **Multiplication** | No natural multiplication | Natural multiplication |
| **Distance** | Ad hoc (Euclidean norm) | Modulus (natural) |
| **Phase** | No phase concept | Phase has meaning |
| **Algebraic Structure** | Vector space only | Field |
| **Analytic Functions** | Not applicable | Rich theory |

**Advantages of Vectors:**
- Simple and intuitive.
- Component-wise operations are straightforward.

**Disadvantages of Vectors:**
- No natural multiplication (dot product is not multiplication; cross product is not defined in 2D).
- Distance is ad hoc—why Euclidean and not Manhattan or some other norm?
- No phase or angle concept with algebraic meaning.
- No analytic function theory.

**2. Complex Numbers $c + id$:**

| Aspect | Complex Representation |
|--------|----------------------|
| **Addition** | Component-wise |
| **Multiplication** | $(a+ib)(c+id) = (ac-bd) + i(ad+bc)$ |
| **Distance** | Modulus $|z| = \sqrt{c^2 + d^2}$ (natural) |
| **Phase** | $\theta = \arg(z)$ (meaningful) |
| **Algebraic Structure** | Field |
| **Analytic Functions** | Rich theory (holomorphic functions, conformal maps) |

**Advantages of Complex Numbers:**
- Natural multiplication that interacts with the geometry.
- Modulus is the natural Euclidean norm.
- Phase has a clear interpretation (exploration-exploitation angle).
- Rich mathematical theory (complex analysis, conformal mappings, contour integration).

**Disadvantages of Complex Numbers:**
- Requires familiarity with complex analysis.
- Multiplication may not have an intuitive physical interpretation in all contexts.

**3. Quaternions $q = a + bi + cj + dk$:**

| Aspect | Quaternion Representation |
|--------|--------------------------|
| **Dimension** | Four-dimensional |
| **Multiplication** | Non-commutative |
| **Distance** | Norm (Euclidean) |
| **Phase** | Ambiguous (multiple angles) |

**Advantages of Quaternions:**
- Can represent rotations in 3D.
- Rich algebraic structure.

**Disadvantages of Quaternions:**
- Overkill for two burdens.
- Non-commutativity complicates operations.
- Phase ambiguity (multiple angles).
- Not a field (non-commutative division algebra).

**4. Matrices:**

| Aspect | Matrix Representation |
|--------|----------------------|
| **Dimension** | Any (including 2x2) |
| **Multiplication** | Matrix multiplication |
| **Distance** | Various matrix norms |
| **Phase** | Not naturally defined |

**Advantages of Matrices:**
- Very general.
- Can represent many operations.

**Disadvantages of Matrices:**
- Too general—loses the specific structure we need.
- No natural phase interpretation.
- Many matrix norms to choose from (ad hoc).

**5. Tensors:**

| Aspect | Tensor Representation |
|--------|----------------------|
| **Dimension** | Multi-dimensional |
| **Multiplication** | Various tensor products |
| **Distance** | Various norms |
| **Phase** | Not naturally defined |

**Advantages of Tensors:**
- Can represent high-dimensional structures.
- Flexible.

**Disadvantages of Tensors:**
- Too complex for our purposes.
- No natural metric or phase.
- Computationally expensive.

**Summary Comparison:**

| Representation | Dimension | Field? | Norm | Phase | Natural |
|----------------|-----------|--------|------|-------|---------|
| **Vector** | 2 | No | Ad hoc | No | No |
| **Complex** | 2 | Yes | Natural | Yes | **Yes** |
| **Split-Complex** | 2 | No | No | No | No |
| **Quaternion** | 4 | No (non-commutative) | Natural | Ambiguous | No |
| **Matrix** | Variable | No | Ad hoc | No | No |
| **Tensor** | Variable | No | Ad hoc | No | No |

**Conclusion:** The complex numbers are the unique two-dimensional algebra that is a field with a compatible norm and a natural phase interpretation. They are the *only* representation that satisfies all our requirements.

---

### 1.6.3 Why Complex Numbers Are Better Than Vectors

The difference between complex numbers and vectors is not merely semantic—it is structural.

**1. Multiplication:**

Vectors do not have a natural multiplication. The dot product $c_1 c_2 + d_1 d_2$ produces a scalar, not another vector. The cross product is not defined in 2D.

Complex numbers have natural multiplication:

$$
(c_1 + id_1)(c_2 + id_2) = (c_1 c_2 - d_1 d_2) + i(c_1 d_2 + d_1 c_2).
$$

This multiplication has geometric meaning: it rotates and scales in the complex plane.

**2. The Norm:**

For vectors, the Euclidean norm $\sqrt{c^2 + d^2}$ is one of many possible norms (Manhattan, supremum, etc.). Why choose Euclidean?

For complex numbers, the modulus $|z| = \sqrt{c^2 + d^2}$ is the *unique* norm that satisfies:

- **Multiplicativity:** $|z_1 z_2| = |z_1||z_2|$.
- **Triangle Inequality:** $|z_1 + z_2| \leq |z_1| + |z_2|$.
- **Compatibility:** The norm is compatible with the field structure.

The modulus is not arbitrary—it is the *natural* norm on the complex numbers.

**3. The Phase:**

Vectors can have an angle, but it is defined using trigonometric functions: $\theta = \arctan(d/c)$. This is an external definition.

Complex numbers have an intrinsic phase: $z = |z| e^{i\theta}$. The phase is part of the algebraic structure, not an external addition.

**4. Analytic Functions:**

Vectors do not support a theory of analytic functions. Complex numbers support holomorphic functions, conformal mappings, contour integration, and the rich theory of complex analysis.

**5. Algebraic Closure:**

The complex numbers are algebraically closed—every polynomial has a root in $\mathbb{C}$. Vectors do not have this property.

**The Bottom Line:**

Complex numbers are not just vectors with a special notation. They are a *different kind of structure*—a field with a compatible norm, a natural phase, and a rich analytic theory. The complex plane is not the same as $\mathbb{R}^2$ with a dot product.

---

### 1.6.4 The Geometric Meaning of Complex Operations

The operations on complex numbers have direct geometric interpretations that are relevant to our framework:

**1. Addition:**

$$
(c_1 + id_1) + (c_2 + id_2) = (c_1 + c_2) + i(d_1 + d_2).
$$

Geometric meaning: Vector addition in the complex plane.

**2. Multiplication by a Real Number:**

$$
\alpha(c + id) = \alpha c + i\alpha d.
$$

Geometric meaning: Scaling.

**3. Multiplication:**

$$
(c_1 + id_1)(c_2 + id_2) = (c_1 c_2 - d_1 d_2) + i(c_1 d_2 + d_1 c_2).
$$

Geometric meaning: Rotation and scaling. If $z_1 = r_1 e^{i\theta_1}$ and $z_2 = r_2 e^{i\theta_2}$, then:

$$
z_1 z_2 = r_1 r_2 e^{i(\theta_1 + \theta_2)}.
$$

**4. Conjugation:**

$$
\overline{c + id} = c - id.
$$

Geometric meaning: Reflection across the real axis.

**5. Modulus:**

$$
|z| = \sqrt{c^2 + d^2}.
$$

Geometric meaning: Distance from the origin.

**6. Phase:**

$$
\theta = \arg(z).
$$

Geometric meaning: Angle from the real axis.

**Relevance to Our Framework:**

| Operation | Geometric Meaning | Relevance |
|-----------|-------------------|-----------|
| Addition | Vector addition | Combining costs and debts |
| Multiplication | Rotation and scaling | Combining utilities |
| Conjugation | Reflection | Debt reversal (paying down) |
| Modulus | Distance | Objective function |
| Phase | Angle | Exploration-exploitation balance |

The operations of the complex plane have *direct* interpretations in terms of cost, debt, and their trade-off. This is not true for vectors, which lack natural multiplication and have an ad hoc norm.

---

### 1.6.5 The Phase as a Natural Quantity

The phase $\theta = \arg(z)$ is not an arbitrary construction—it emerges naturally from the complex structure.

**Why Phase Matters:**

1. **Exploration-Exploitation Balance:** $\theta$ directly measures the balance between cost ($c$) and debt ($d$).

2. **Learning Progress:** As the agent learns, $\theta$ decreases from exploration toward exploitation.

3. **State-Dependent:** $\theta$ varies across states, reflecting the different information needs of different states.

4. **Parameter-Free:** $\theta$ is determined by the geometry, not by an external parameter.

**The Phase in Other Representations:**

- **Vectors:** Phase can be defined externally as $\theta = \arctan(d/c)$, but this is an addition to the vector structure, not inherent to it.

- **Matrices:** No natural phase concept.

- **Quaternions:** Multiple phases (Euler angles), leading to ambiguity.

- **Tensors:** No natural phase concept.

**Only Complex Numbers Have a Natural Phase:**

The phase $\theta = \arg(z)$ is part of the algebraic structure of the complex numbers, defined by $z = |z| e^{i\theta}$. It is not an external addition—it emerges from the multiplicative structure.

**This is why phase is meaningful in our framework:**

- It is not an arbitrary parameter.
- It is determined by the geometry of cost and debt.
- It changes automatically as the agent learns.
- It provides a continuous measure of exploration-exploitation balance.

---

### 1.6.6 The Role of the Modulus in a Field

The modulus $|z|$ plays a special role in the complex numbers because it is compatible with the field structure:

1. **Multiplicativity:** $|z_1 z_2| = |z_1||z_2|$.

2. **Triangle Inequality:** $|z_1 + z_2| \leq |z_1| + |z_2|$.

3. **Positive Definiteness:** $|z| \geq 0$, with equality iff $z = 0$.

**Why This Matters:**

- **Convergence:** The contraction properties of the Bellman evaluation operator depend on the compatibility of the norm with the field structure.

- **Stability:** The modulus provides a natural Lyapunov function.

- **Optimality:** The modulus objective is principled because it arises from the field structure.

**In Other Representations:**

- **Vectors:** The Euclidean norm is multiplicative only in the sense that $|z_1||z_2|$ is the product of norms, but there is no multiplication operation to make this meaningful.

- **Quaternions:** The norm is multiplicative, but non-commutativity complicates the algebra.

- **Matrices:** The norm is not generally multiplicative.

**Only Complex Numbers Have:**

1. A field structure (commutative, every element has an inverse).
2. A compatible norm (multiplicative, positive definite).
3. A natural phase (from the polar representation).
4. A rich analytic theory (holomorphic functions).

**This is the mathematical justification for using complex numbers.**

---

### 1.6.7 Summary of Section 1.6

In this section, we have justified the use of complex numbers as the natural framework for encoding cost and debt:

1. **The Uniqueness of $\mathbb{C}$:** Any two-dimensional real algebra that is a field must be isomorphic to $\mathbb{C}$. The only alternatives (split-complex, dual numbers) are not fields.

2. **Comparison with Other Representations:** Vectors lack natural multiplication and have an ad hoc norm. Quaternions are overkill and non-commutative. Matrices and tensors are too general. Complex numbers are the unique two-dimensional field with a compatible norm.

3. **Why Complex Numbers Are Better Than Vectors:** Natural multiplication, unique norm, intrinsic phase, analytic function theory, and algebraic closure.

4. **Geometric Meaning of Complex Operations:** Addition (vector addition), multiplication (rotation and scaling), conjugation (reflection), modulus (distance), phase (angle). All have direct relevance to cost, debt, and their trade-off.

5. **The Phase as a Natural Quantity:** The phase $\theta = \arg(z)$ emerges from the algebraic structure of $\mathbb{C}$, not from an external definition.

6. **The Role of the Modulus in a Field:** The modulus is compatible with the field structure (multiplicative, positive definite), enabling convergence proofs, stability guarantees, and optimality results.

**The Central Claim:**

> The complex numbers are not an arbitrary choice. They are the *unique* two-dimensional real algebra that is a field with a compatible norm. The geometry of $\mathbb{C}$—modulus, phase, multiplication, conjugation—provides a natural and principled framework for encoding cost and debt and balancing them without free parameters.

---

### 1.6.8 Exercises for Section 1.6

**Exercise 1.26 (Why Not Vectors?).** Explain why two-dimensional vectors $(c,d)$ are insufficient for our purposes. What operations are missing? What aspects of the complex framework cannot be reproduced with vectors?

**Exercise 1.27 (Why Not Quaternions?).** Quaternions are a four-dimensional division algebra. Why would quaternions be overkill for encoding cost and debt? What problems would arise from non-commutativity?

**Exercise 1.28 (The Uniqueness of $\mathbb{C}$).** Prove that any two-dimensional real algebra that is a field must be isomorphic to $\mathbb{C}$. Follow the proof sketch in Section 1.6.1.

**Exercise 1.29 (Modulus Multiplicativity).** Prove that $|z_1 z_2| = |z_1||z_2|$ for complex numbers. Why is this property important for the contraction properties of the Bellman evaluation operator?

**Exercise 1.30 (Phase Interpretation).** For the complex utility $z = c + id$:
- What does $\theta = 0$ mean?
- What does $\theta = \pi/2$ mean?
- What does $\theta = -\pi/2$ mean?
- What does $\theta$ measure as an agent learns?

**Exercise 1.31 (Split-Complex Numbers).** Split-complex numbers are of the form $a + bj$ with $j^2 = 1$. Show that they have zero divisors (non-zero elements whose product is zero). Why does this make them unsuitable for our framework?

---

### 1.6.9 Further Reading for Section 1.6

- Needham, T. (1997). *Visual Complex Analysis*. Oxford University Press. — An accessible introduction to complex analysis with a geometric focus.

- Ahlfors, L. V. (1979). *Complex Analysis*, 3rd ed. McGraw-Hill. — The classic textbook on complex analysis.

- Conway, J. B. (1978). *Functions of One Complex Variable*, 2nd ed. Springer. — A rigorous treatment of complex analysis.

- Kantor, I. L. & Solodovnikov, A. S. (1989). *Hypercomplex Numbers: An Elementary Introduction to Algebras*. Springer. — An introduction to algebras beyond the complex numbers.

- Baez, J. C. (2002). "The Octonions." *Bulletin of the American Mathematical Society*, 39(2):145-205. — A comprehensive treatment of division algebras, including the classification theorem.

---

## 1.5 Historical Context and Related Work

### 1.5.1 Multi-Objective Reinforcement Learning (MORL)

The problem of balancing multiple objectives in sequential decision-making has a long history in reinforcement learning. **Multi-Objective Reinforcement Learning (MORL)** extends classical RL to problems with vector-valued rewards.

**Definition 1.13 (Multi-Objective MDP).** A Multi-Objective MDP (MOMDP) is a tuple $(\mathcal{S}, \mathcal{A}, p, \mathbf{r}, \lambda)$ where $\mathbf{r}: \mathcal{S} \times \mathcal{A} \to \mathbb{R}^n$ is a vector-valued reward function with $n$ objectives.

The agent's goal is to find policies that achieve desirable trade-offs between the $n$ objectives.

**Main Approaches in MORL:**

**1. Linear Scalarisation:**

$$
r_{\text{total}} = \sum_{i=1}^n w_i r_i,
$$

where $w_i \geq 0$ are weights.

- **Advantages:** Simple, uses standard RL algorithms.
- **Limitations:** Requires choosing weights; can only find policies on the convex hull of the Pareto frontier; weights are environment-dependent.

**2. Lexicographic Ordering:**

Objectives are ordered by priority. The agent optimizes the primary objective first, then the secondary objective subject to the primary being optimal, and so on.

- **Advantages:** No weights needed; respects priority ordering.
- **Limitations:** Requires a priority ordering; cannot find compromises.

**3. Pareto-Based Methods:**

Maintain a set of non-dominated policies (the Pareto frontier). The agent selects among them using a secondary criterion.

- **Advantages:** Can find the entire Pareto frontier; no weights needed.
- **Limitations:** Computationally expensive; requires storing multiple policies; still needs a selection criterion.

**4. Threshold-Based Methods:**

Satisfy constraints on secondary objectives: $r_i \geq \tau_i$ for $i \neq j$, while optimizing the primary objective.

- **Advantages:** Respects constraints; no weights needed.
- **Limitations:** Requires choosing thresholds; may be infeasible.

**5. Multi-Objective Policy Gradients:**

Extend policy gradient methods to vector-valued rewards using multi-objective optimization techniques.

- **Advantages:** Can handle continuous action spaces; scalable.
- **Limitations:** Still requires choosing weights or priorities.

**The Common Limitation:**

All MORL methods require choosing parameters—weights, priorities, thresholds, or selection criteria. These parameters are:

- **Arbitrary:** No principled way to choose them.
- **Environment-dependent:** The optimal choice depends on the problem.
- **Task-dependent:** Different tasks require different parameters.

**How Our Approach Differs:**

The complex framework eliminates the need for parameters by recognizing that cost and debt are not two separate objectives to be weighted—they are two components of a single geometric object. The geometry of the complex plane provides the trade-off intrinsically.

**Key Differences from MORL:**

| Aspect | MORL | Complex Framework |
|--------|------|-------------------|
| **Number of Objectives** | $n \geq 2$ | Exactly 2 (cost and debt) |
| **Parameterization** | Requires weights, priorities, or thresholds | No parameters |
| **Trade-off Mechanism** | External (designer chooses) | Internal (geometry provides) |
| **Pareto Frontier** | Must be computed or approximated | Emerges from geometry |
| **Exploration** | Must be handled separately | Emerges from debt minimization |

---

### 1.5.2 Information-Theoretic RL

Information-theoretic approaches to RL use concepts from information theory to guide learning and decision-making.

**Key Ideas:**

- Balance reward maximization with information gain.
- Use information-theoretic measures (entropy, mutual information, KL divergence) to quantify uncertainty and exploration.
- Examples: Information-directed sampling, maximum entropy RL, Bayesian RL.

**Russo and Van Roy (2018): Information-Directed Sampling**

Information-directed sampling (IDS) selects actions to maximize:

$$
\text{IDS}(a) = \frac{\text{Expected Reward}}{\text{Information Gain}}.
$$

Or more precisely, it minimizes the "information ratio":

$$
\rho(a) = \frac{\Delta(a)^2}{I(a)},
$$

where $\Delta(a)$ is the expected regret and $I(a)$ is the information gain.

**The Problem with IDS:**

IDS requires balancing reward and information. The information ratio implicitly uses a weight that must be chosen. The optimal balance depends on the environment and the time horizon.

**Russo and Van Roy (2018) acknowledge this limitation:**

> "The information ratio is a heuristic that must be tuned for each environment. There is no universal information ratio that works for all problems."

**Maximum Entropy RL:**

Maximum entropy RL (e.g., Soft Actor-Critic) adds an entropy bonus to the reward:

$$
r_{\text{total}} = r + \beta \mathcal{H}(\pi),
$$

where $\mathcal{H}(\pi)$ is the policy entropy and $\beta$ is a temperature parameter.

**The Problem with Maximum Entropy RL:**

The temperature parameter $\beta$ must be tuned. The optimal $\beta$ depends on the environment and the task. Recent work has proposed adaptive $\beta$, but this introduces additional complexity.

**Bayesian RL:**

Bayesian RL maintains a belief over the environment dynamics and uses this belief to guide exploration. It is parameter-free in principle but computationally intractable for most problems.

**How Our Approach Differs:**

The complex framework provides a parameter-free approach to information-theoretic RL. The imaginary component $Q_I$ naturally encodes information value, and the modulus $|Q|$ automatically balances reward (cost minimization) and information (debt minimization).

**Key Differences:**

| Aspect | Information-Theoretic RL | Complex Framework |
|--------|--------------------------|-------------------|
| **Balance Parameter** | Requires $\beta$, $\rho$, or schedule | No parameters |
| **Information Measure** | Entropy, mutual information | Debt (imaginary component) |
| **Exploration** | Encouraged via bonus or constraint | Emerges from geometry |
| **Adaptivity** | Requires adaptive parameters | Automatically adaptive |

---

### 1.5.3 Complex-Valued Neural Networks

Complex-valued neural networks (CVNNs) use complex numbers as building blocks, with applications in signal processing, quantum computing, and more recently, deep learning.

**Key Ideas:**

- Complex-valued neurons: $y = \sigma(Wz + b)$ where $W, z, b \in \mathbb{C}$.
- Wirtinger calculus for backpropagation.
- Applications: radar, speech processing, communications, image processing.

**Hirose (2012): Complex-Valued Neural Networks**

Hirose's comprehensive treatment covers:
- Fundamentals of complex-valued neural networks.
- Wirtinger calculus and complex backpropagation.
- Applications in adaptive signal processing.

**Trabelsi et al. (2018): Deep Complex Networks**

Trabelsi et al. extended complex-valued networks to deep learning:
- Complex-valued convolutions.
- Complex batch normalization.
- Complex-valued activation functions.
- Applications: speech enhancement, music transcription, image reconstruction.

**Limitations of CVNNs for RL:**

- **Not Applied to RL:** CVNNs have not been applied to reinforcement learning in a principled way.
- **Focus on Representation:** CVNNs focus on representation learning, not sequential decision-making.
- **No MDP Framework:** CVNNs lack the theoretical framework of MDPs and Bellman equations.
- **No Exploration:** CVNNs do not address the exploration-exploitation trade-off.

**How Our Approach Builds on CVNNs:**

We leverage complex numbers not just for representation but for decision-making. The complex framework provides:

1. A theoretical foundation (complex MDP, complex Bellman equations).
2. A natural exploration mechanism (debt minimization).
3. A parameter-free trade-off (modulus objective).
4. Rich geometric structure (phase, modulus, analyticity).

---

### 1.5.4 Information Geometry and Natural Gradient

Information geometry uses differential geometry to study probability distributions and statistical manifolds.

**Key Ideas:**

- Statistical manifold: a Riemannian manifold whose points are probability distributions.
- Fisher information metric: the natural metric on statistical manifolds.
- Natural gradient: steepest descent in the Fisher metric.

**Amari (2016): Information Geometry**

Amari's comprehensive treatment covers:
- The Fisher information metric.
- Natural gradient descent.
- Applications in machine learning and neural networks.

**Connections to Our Work:**

The complex framework has deep connections to information geometry:

1. **The Debt Function as a Potential:** The imaginary component $Q_I$ can be interpreted as a potential function on the state space.

2. **The Epistemic Potential:** Under the telescoping identity, $Q_I$ equals discounted cumulative mutual information, which has a natural information-geometric interpretation.

3. **The Natural Gradient in the Complex Plane:** The Complex Natural Actor-Critic (CNAC) algorithm uses a natural gradient in the complex plane, reminiscent of Amari's natural gradient.

4. **The Kähler Conjecture:** The connection to Kähler geometry suggests a deeper link between information geometry and complex analysis.

**Open Questions:**

- Can the complex framework be formulated as a flow on a statistical manifold?
- Is the Hermitian Fisher metric Kähler? (Open Problem 6)
- What is the relationship between epistemic equilibrium and information-geometric geodesics?

---

### 1.5.5 Bitopological Spaces and Asymmetric Metrics

Bitopological spaces and asymmetric metrics provide a mathematical framework for studying systems with two topologies or asymmetric distances.

**Key Ideas:**

- **Bitopological Space:** A set with two topologies.
- **Quasi-Metric:** A metric that is not necessarily symmetric.
- **Asymmetric Distance:** A distance $d(x,y)$ that may differ from $d(y,x)$.

**Kelly (1963): Bitopological Spaces**

Kelly introduced bitopological spaces as a generalization of topological spaces with two topologies. This framework is useful for studying asymmetric structures.

**Fletcher and Lindgren (1982): Quasi-Uniform Spaces**

Quasi-uniform spaces generalize uniform spaces to asymmetric settings. They provide a framework for studying asymmetric metrics.

**Connections to Our Work:**

The complex framework has natural connections to bitopological spaces and asymmetric metrics:

1. **The Energy Quasi-Metric:** The cost function $c(s,s')$ is a quasi-metric—it is not symmetric (going from $s$ to $s'$ may cost more than going from $s'$ to $s$).

2. **The Forward/Backward Topologies:** The cost and debt functions induce two topologies on the state space, reminiscent of bitopological spaces.

3. **The Complex Quasi-Metric:** The complex utility $z = c + id$ can be interpreted as a complex-valued quasi-metric.

4. **Equilibrium as a Fixed Point:** Epistemic equilibrium corresponds to a fixed point of the complex dynamics, similar to fixed points in bitopological spaces.

**Open Questions:**

- Can the complex framework be formalized as a bitopological space?
- What is the relationship between epistemic equilibrium and bitopological fixed points?
- Can asymmetric metrics provide a foundation for the complex framework?

---

### 1.5.6 Humble Systems Theory (HST)

**Humble Systems Theory (HST)** is the overarching framework that unifies the complex approach to RL.

**Definition 1.14 (Humble Systems Theory).** HST proposes that every Information Processing System (IPS) evolves toward epistemic equilibrium—a state of minimum computational cost subject to informational constraints.

**Key Principles of HST:**

1. **Equilibrium-Seeking:** Systems are naturally driven toward equilibrium by the geometry of their problem space.

2. **Duality:** Cost and debt are the two fundamental burdens of an IPS. They are orthogonal components of a single geometric object.

3. **Geometry:** The geometry of the cost-debt plane determines the system's behavior. The complex plane provides the natural framework.

4. **Automaticity:** Exploration and exploitation emerge naturally from the geometry. No external parameters are needed.

5. **Humble:** Systems are "humble" in the sense that they do not require external tuning or supervision to find their balance point.

**Relationship to Other Work:**

| Framework | Relationship to HST |
|-----------|---------------------|
| **MORL** | HST extends MORL by eliminating parameters |
| **Information-Theoretic RL** | HST deepens by deriving trade-offs from geometry |
| **CVNNs** | HST provides a decision-making framework for complex numbers |
| **Information Geometry** | HST connects RL to information geometry |
| **Bitopological Spaces** | HST provides a possible foundation in asymmetric spaces |

**The HST Equilibrium Axiom:**

**Axiom 1.1 (HST Equilibrium Axiom).** Every Information Processing System evolves toward epistemic equilibrium. Formally, for any system with complex value function $Q^\pi$:

$$
\lim_{t \to \infty} Q_t^{\pi^*}(S_t, A_t) = 0 \quad \text{almost surely}.
$$

This axiom is the foundational claim of the theory. It asserts that all information processors are naturally driven to balance cost and information, and that this balance point is the origin of the cost-debt plane.

---

### 1.5.7 Related Mathematical Fields

Several mathematical fields provide tools and insights for the complex framework:

**1. Complex Analysis:**

- Analytic functions, holomorphic maps, contour integration.
- Provides the tools for studying complex-valued value functions.
- **Applications:** Wirtinger calculus for policy gradients; Kähler geometry for the Bergman conjecture.

**2. Differential Geometry:**

- Riemannian manifolds, geodesics, curvature.
- Provides the framework for information geometry and natural gradients.
- **Applications:** Natural gradient in the complex plane; Kähler geometry.

**3. Dynamic Programming:**

- Bellman equations, contraction mappings, fixed point theory.
- Provides the foundation for value iteration and policy iteration.
- **Applications:** Complex Bellman equations; contraction of the evaluation operator.

**4. Potential Theory:**

- Harmonic functions, potentials, Dirichlet problems.
- Provides tools for studying conservative (path-independent) functions.
- **Applications:** The debt function as a potential; telescoping identity.

**5. Category Theory:**

- Functors, natural transformations, adjunctions.
- Provides a high-level framework for relating different structures.
- **Applications:** Formalization of the complex framework; open problems.

---

### 1.5.8 Summary of Section 1.5

In this section, we have placed the complex framework in historical and intellectual context:

1. **Multi-Objective RL:** MORL methods all require parameters (weights, priorities, thresholds). The complex framework eliminates these parameters by recognizing cost and debt as components of a single geometric object.

2. **Information-Theoretic RL:** Methods like IDS and maximum entropy RL require balance parameters. The complex framework provides a parameter-free approach where exploration emerges from geometry.

3. **Complex-Valued Neural Networks:** CVNNs provide tools for complex-valued computation but lack a decision-making framework. The complex framework provides an RL foundation for complex numbers.

4. **Information Geometry:** Provides tools for natural gradient and statistical manifolds. The complex framework connects RL to information geometry via the natural gradient and the Kähler conjecture.

5. **Bitopological Spaces:** Provide a foundation for asymmetric metrics and two topologies. The complex framework has natural connections to quasi-metrics and bitopological spaces.

6. **Humble Systems Theory:** The overarching framework that unifies the complex approach. HST proposes that all information processors evolve toward epistemic equilibrium.

7. **Mathematical Fields:** Complex analysis, differential geometry, dynamic programming, potential theory, and category theory all provide tools and insights for the complex framework.

**The Big Picture:**

The complex framework is not developed in isolation—it builds on and connects to a rich intellectual tradition. It extends MORL by eliminating parameters, deepens information-theoretic RL by deriving trade-offs from geometry, provides a decision-making framework for complex numbers, and connects RL to information geometry, bitopological spaces, and Humble Systems Theory.

---

### 1.5.9 Exercises for Section 1.5

**Exercise 1.21 (Comparison with MORL).** Identify the key differences between the complex approach and standard MORL methods. What problems does the complex approach solve that MORL doesn't? What new challenges does it introduce?

**Exercise 1.22 (Information-Theoretic RL).** Compare information-directed sampling with the complex approach. How does each balance reward and information? What are the advantages and disadvantages of each?

**Exercise 1.23 (CVNNs vs. Complex RL).** Explain the difference between using complex numbers for representation learning (CVNNs) and using them for decision-making (complex RL). How do the two approaches differ in their assumptions and goals?

**Exercise 1.24 (The HST Equilibrium Axiom).** Explain the HST Equilibrium Axiom in your own words. What does it mean for an information processing system to evolve toward epistemic equilibrium? What evidence supports this claim?

**Exercise 1.25 (Mathematical Foundations).** Identify at least three mathematical fields that contribute to the complex framework. For each field, explain one tool or insight it provides and how it applies to the framework.

---

### 1.5.10 Further Reading for Section 1.5

**Multi-Objective RL:**
- Roijers, D. M., Vamplew, P., Whiteson, S., & Dazeley, R. (2013). "A Survey of Multi-Objective Sequential Decision-Making." *Journal of Artificial Intelligence Research*, 48:67-113.

- Hayes, C. F., Rădulescu, R., Bargiacchi, E., et al. (2022). "A Practical Guide to Multi-Objective Reinforcement Learning and Planning." *Autonomous Agents and Multi-Agent Systems*, 36(1):26.

**Information-Theoretic RL:**
- Russo, D. & Van Roy, B. (2018). "Learning to Optimize with Information-Directed Sampling." *Operations Research*, 66(1):230-252.

- Ziebart, B. D. (2010). *Modeling Purposeful Adaptive Behavior with the Principle of Maximum Causal Entropy*. Ph.D. Thesis, Carnegie Mellon University.

**Complex-Valued Neural Networks:**
- Hirose, A. (2012). *Complex-Valued Neural Networks*, 2nd ed. Springer.

- Trabelsi, C., Bilaniuk, O., Zhang, Y., et al. (2018). "Deep Complex Networks." *International Conference on Learning Representations (ICLR)*.

**Information Geometry:**
- Amari, S. (2016). *Information Geometry and Its Applications*. Springer.

**Bitopological Spaces:**
- Kelly, J. C. (1963). "Bitopological Spaces." *Proceedings of the London Mathematical Society*, 13(1):71-89.

- Fletcher, P. & Lindgren, W. F. (1982). *Quasi-Uniform Spaces*. Marcel Dekker.




