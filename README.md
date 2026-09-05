## A Note on the Nature of This Work

This work, **“A Humble Philosophical Attempt to Build the Mathematical Foundation for Complex-Valued Reinforcement Learning,”** should **not** be interpreted as a finished scientific discovery, a validated theory, or a complete AI paradigm.

This repository is primarily **my way of learning, exploring, and thinking**. I collect ideas, mathematical formulations, experiments, and references that may help me better understand the subject and, potentially, build a foundation for future work.

I may be **completely wrong about some — or even most — of what is presented here**. The ideas should therefore be treated as exploratory hypotheses and learning material rather than established scientific facts.

### AI Collaboration

This work has been developed in collaboration with **DeepSeek**, which has contributed to the exploration, discussion, development, and refinement of ideas throughout the process.

### Contribution

At the present stage, **I am the sole human contributor to this work**.

### Intellectual Property and Collaboration

This work is subject to **copyright and applicable intellectual-property protections**, including efforts to protect certain aspects through patent-related processes where applicable.

If you find these ideas useful, build upon them, or incorporate concepts from this work into your own research, **please cite the original work appropriately and engage in fair attribution and collaboration**.

At the same time, **collaboration is genuinely welcomed**. If you see an error, a weakness, a better formulation, or a promising direction, I would much rather hear about it than have the idea remain unchallenged.

**This is an invitation to explore, criticize, improve, and build — not a claim of having found the final answer.**
---

# Introduction

## The Whisper Chain: From Universe to Algorithm

What if we have been looking in the wrong direction for inspiration?

For decades, the field of artificial intelligence has looked to biology—to the intricate workings of animal brains, to human cognition, to the evolutionary strategies that shaped nervous systems over millions of years. This approach has yielded remarkable algorithms, from Deep Q-Networks to AlphaGo. Yet perhaps, in our fascination with the **product of evolution**, we have overlooked the evolutionary process itself—and, more fundamentally, the laws that govern that process.

This book proposes a radical shift in perspective. Instead of building algorithms inspired primarily by human and animal intelligence, we will explore the possibility of building algorithms inspired by the universe itself—the physical system that ultimately produced matter, life, brains, and intelligence.

This work is part of **Humble Systems Theory (HST)**, a broader attempt to understand intelligent systems through the concepts of cost, energy, information, uncertainty, equilibrium, and responsibility.

There is, however, an important point that should be made before going any further.

**At the time of writing, the author is the only contributor to Humble Systems Theory.** There is no research team behind this work, no large laboratory, and no established scientific community supporting the theory. The ideas presented here are the product of one person's attempt to connect mathematics, physics, information theory, reinforcement learning, and artificial intelligence.

And the author is not a physicist.

He is not an information theorist.

He does not hold a PhD.

He is therefore fully aware that there is a **high probability that some of what follows is wrong—and a non-negligible possibility that some of it is completely wrong.**

This is not a disclaimer intended to weaken the theory.

It is part of the theory's philosophy.

Humble Systems Theory is called *humble* precisely because it does not begin with the assumption that the author has discovered the truth. It begins with the assumption that **the author may be wrong** and that the only meaningful way forward is to expose the ideas to mathematics, criticism, experimentation, and eventually to people who know much more about these fields than the author does.

Some of the physical interpretations in this book may prove incorrect. Some connections between information theory and physics may turn out to be misunderstood. Some mathematical constructions may fail. Some ideas may survive only as philosophical metaphors rather than scientific statements.

That is acceptable.

The purpose of this work is not to declare that the universe works according to HST.

The purpose is to ask whether it **might**.

The universe, after all, has been solving optimization problems for approximately 13.8 billion years. It has produced stars, planets, chemistry, life, consciousness, and perhaps even the cognitive structures that we now seek to reproduce in silicon. To ignore this enormous cosmic laboratory in favor of studying only its biological products may be to limit ourselves to a fraction of what is possible.

Consider the **whisper chain** of knowledge.

The universe whispers its fundamental principles to us through the laws of physics—through the trajectories of particles, the geometry of spacetime, the irreversible flow of entropy, and the symmetries that govern physical interactions.

We, in turn, whisper to artificial intelligence through our algorithms and architectures.

But to truly understand what we are whispering, perhaps we must learn to listen more carefully to the original voice.

We must attune ourselves to patterns we have previously ignored, to connections we may have dismissed as coincidence, and to structures that might lie beneath the phenomena we already understand.

The whisper is always there, constant and patient, waiting for us to pause our own noise long enough to hear it.

---

## The Universe as Informational Processor

The possibility that physical reality has a deep informational structure has become an important theme across modern physics and theoretical science.

From statistical mechanics and thermodynamics to quantum theory and information theory, information appears repeatedly in our attempts to understand physical reality.

HST explores a particular interpretation of this landscape: perhaps physical systems can be viewed as systems that continuously negotiate between **cost, information, uncertainty, coherence, and constraint**.

The claim explored throughout this book is that what we experience as organized physical behavior may emerge from deeper processes involving the competition between disorder, information, energy, and constraint.

Mechanics may therefore be viewed, from one perspective, as thermodynamics under constraint.

What we experience as the solidity of matter, the passage of time, and the apparent certainty of cause and effect could be understood as emergent behavior arising from a deeper physical and informational substrate.

This perspective aligns with various lines of research investigating relationships between information theory, thermodynamics, stochastic processes, optimal control, and physical dynamics.

But here we take an additional step.

We ask whether these ideas can provide inspiration for artificial learning systems.

The central physical intuition of HST can be expressed in a simple sentence:

> **The universe does not get anything for free. Every physical process carries a cost.**

This leads to the central concept developed throughout this book:

> **The universe does not merely minimize an abstract "action" as a mathematical curiosity—it may be understood as minimizing the cost associated with action.**

Action, in this interpretation, is not treated as a metaphysical quantity but as a representation of accumulated cost along a trajectory.

Every trajectory, every interaction, every transformation carries consequences.

Every physical process consumes resources.

Every interaction changes information.

Every state transition leaves a trace.

The universe is not a free lunch.

It is the most efficient accountant imaginable.

---

## Emergence Phenomenon

The idea that physical systems can be understood computationally or informationally provides a natural bridge toward artificial intelligence.

A physical system receives constraints.

It evolves.

It interacts with its environment.

It dissipates energy.

It changes its internal state.

It leaves information about its previous states in the environment.

And somehow, from these processes, increasingly complex structures emerge.

Life emerges.

Nervous systems emerge.

Learning emerges.

Intelligence emerges.

From the perspective of HST, this sequence is worth studying as a whole.

Rather than beginning with the brain and asking how to imitate it, we can begin with the physical world and ask:

> **What properties of physical systems made intelligence possible in the first place?**

This is where concepts such as entropy, free energy, information, coherence, and optimization become particularly interesting.

Some theoretical frameworks have proposed deep relationships between the minimization of variational free energy, prediction, self-organization, and the persistence of systems.

Whether these ideas ultimately justify a universal theory of intelligence remains an open question.

HST does not claim that this question has already been answered.

It proposes that the question itself deserves to be explored.

Perhaps the universe does not "think" in the biological sense.

Perhaps it does not possess consciousness.

Perhaps it does not have intentions.

But perhaps physical systems nevertheless exhibit an optimization structure that, when sufficiently abstracted, resembles some of the computational principles we associate with intelligence.

In that weaker—and perhaps more defensible—sense, we can speak of the universe as an **informational processor**.

The universe processes states.

It transforms information.

It dissipates energy.

It creates structure.

It explores possibilities through physical evolution.

And eventually, one of those structures—life—began asking questions about the process itself.

We are one such structure.

---

## The Cost of Action: From Physics to Reinforcement Learning

If the universe can be understood as a system in which every transition carries a cost, then perhaps our algorithms should take cost more seriously.

This is not merely an analogy.

It is a research hypothesis.

To build artificial intelligence that is not merely capable but **responsible**, perhaps we need to give our learning systems an explicit representation of what their actions cost—not only immediately, but informationally and structurally.

The Principle of Least Action, first associated with Maupertuis and subsequently developed through the work of Euler, Lagrange, Hamilton, and others, tells us that physical trajectories can be characterized through an extremal principle involving action.

The familiar expression is

$$
S = \int L\,dt,
$$

where \(L\) is the Lagrangian.

In classical mechanics,

$$
L=T-V.
$$

HST asks a simple question:

> **What if reinforcement learning could be formulated around an analogous concept of accumulated cost?**

Standard reinforcement learning generally frames the objective around maximizing cumulative reward:

$$
\max_\pi
\mathbb{E}
\left[
\sum_t \gamma^t R_t
\right].
$$

But reward is only one side of the story.

What if the fundamental objective is instead related to **cost minimization**, where cost encompasses not only energy consumption and negative rewards, but also informational consequences?

What if an action that produces a high immediate reward can nevertheless be considered expensive because it leaves the agent with a large amount of unresolved uncertainty?

What if reward is merely a visible projection of a deeper utility structure?

What if everything we have been optimizing is only the visible portion of an invisible iceberg?

---

## The Free Energy Connection

The Free Energy Principle, developed prominently by Karl Friston and collaborators, provides one possible bridge between these ideas.

In its various formulations, the framework describes systems in terms of variational free energy, inference, prediction, and the maintenance of states within certain constraints.

HST is interested in this connection because it suggests that **persistence, prediction, information, and energetic constraints may not be independent concepts**.

There are, of course, substantial debates surrounding the interpretation and explanatory scope of the Free Energy Principle.

HST does not attempt to settle those debates.

Instead, it asks whether some of the mathematical ideas associated with these frameworks can be repurposed for reinforcement learning.

If an intelligent system must continuously maintain itself within an environment while dealing with incomplete information, then perhaps learning can be understood partly as a process of managing the cost of uncertainty.

This leads naturally to the concept of **informational debt**.

---

## The Probabilistic Decision Tree

To understand how an agent navigates an energy-information surface, we must first understand the probabilistic structure of decision-making itself.

Every decision an agent makes can be understood as traversing a tree of uncertainty—a tree whose branches represent different consequences of acting in an uncertain world.

At each step, the agent maintains a policy, a strategy for choosing actions based on its current understanding of the environment.

With some probability, it follows its current policy, exploiting what it knows.

With the complementary probability, it explores, venturing into the unknown.

But even when it exploits, it is not guaranteed a reward.

And even when it receives a reward, that reward may or may not be informative about the underlying structure of reality.

The first level of the tree concerns **action selection**.

The agent chooses between following its current policy and exploring alternative actions.

This is the classic exploration-exploitation dilemma, but within our framework it takes on another interpretation.

The balance between exploitation and exploration may reflect the agent's **informational debt**—a measure of how much it still needs to learn about the world in which it operates.

The second level concerns **reward realization**.

Given the action chosen, there is some probability that the agent receives a positive reward.

This probability represents the agent's current estimate of the reward landscape, derived from past experiences and updated through learning.

Reward, in this framework, can be interpreted as a proxy for energetic or computational efficiency.

A positive reward indicates that an action produced a favorable outcome relative to the agent's objective.

The third—and most subtle—level concerns **information gain**.

Even when an agent receives a reward, that reward may or may not be informative.

The outcome is informative when it causes a meaningful update to the agent's model of the world.

When an action produces little information, the agent may have spent resources without substantially reducing uncertainty.

It has paid the cost without reducing the debt.

The complete probabilistic structure of a single decision step can therefore be represented as

$$P(A,R,I \mid \theta)=P(A)P(R \mid A,\theta)P(I \mid A,R,\theta)$$

where:

* $A$ represents the action selected,
* $R$ represents the reward received,
* $I$ represents whether the outcome was informative,
* $\theta$ represents the agent's current model parameters.

This factorization reveals a potentially important distinction:

> **Information gain depends on what happens after an action, while the value of the reward and the value of the information need not be identical.**

Probability may therefore be whispering something to us about the deeper nature of utility.

Perhaps $R$ and $I$ are not independent objectives.

Perhaps they are projections of a deeper structure.

Perhaps what we call utility is only a real-valued shadow of something richer.

---

## The Safety Protocol: Mapping Agent State to Energy and Responsibility

This brings us to one of the central technical ideas of this book:

$$
\boxed{
\text{Cost}+i\times\text{Debt}
}
$$

where $i$ is the imaginary unit and **Debt** represents informational debt.

Within HST, this complex quantity is proposed as a representation of two dimensions of intelligent action.

The real component represents **cost**.

The imaginary component represents **debt**.

The motivation for using a complex representation is not arbitrary.

Complex numbers already play a fundamental role in theoretical physics, particularly in quantum mechanics, where complex amplitudes are essential to the mathematical formulation of the theory.

But HST makes a different proposal.

It asks whether complex-valued representations can also provide a useful mathematical language for representing multiple dimensions of responsibility in an artificial agent.

In our framework,

$$
Q=C+iD,
$$

where

$$
C = \mathrm{Re}(Q)
$$

is the action cost and

$$
D=\mathrm{Im}(Q)
$$

is informational debt.

The real component can include quantities such as energy expenditure, computational resources, negative rewards, thermodynamic dissipation, or other measurable costs.

The imaginary component represents the informational consequences of acting under incomplete knowledge.

It can be related to uncertainty, unresolved information, model mismatch, or the discrepancy between what the agent knows and what would be required to fully account for its behavior.

The precise definition of debt is therefore one of the central mathematical problems of the theory.

It must ultimately be measurable.

It must be falsifiable.

And it must produce predictions that differ meaningfully from conventional reinforcement learning.

---

## The Energy–Responsibility Plane

The complex representation gives us a simple geometric picture.

An agent does not exist solely on a one-dimensional reward axis.

It occupies a position in an **energy-responsibility plane**.

One direction represents cost.

The other represents informational debt.

An agent that minimizes the real component alone might become extremely efficient while accumulating substantial unresolved uncertainty.

Such an agent could be efficient but fragile.

An agent that minimizes informational debt alone might become excessively conservative, gathering information indefinitely while refusing to act.

The interesting region lies somewhere between these extremes.

The proposed complex quantity is

$$
Q=C+iD.
$$

Its magnitude is

$$
|Q|=\sqrt{C^2+D^2}.
$$

Therefore,

$$
|Q|^2=C^2+D^2.
$$

This produces a simple but potentially important property:

> **Debt contributes quadratically to the magnitude of the combined quantity.**

In other words, the framework naturally creates increasing pressure against large informational debt.

This could provide a mathematical mechanism for making an agent more cautious about actions whose consequences are difficult to predict.

But this remains a hypothesis.

Whether the complex formulation actually produces safer, more robust, or more efficient agents must be determined experimentally.

---

## A New Foundation for Reinforcement Learning

This book is a **humble philosophical and mathematical attempt to build a foundation for complex reinforcement learning**.

It is an attempt to approach reinforcement learning from a different direction.

Not from the brain outward.

But from the universe inward.

The goal is not to claim that the universe literally runs a reinforcement-learning algorithm.

The goal is to ask whether some of the principles governing physical systems can inspire fundamentally different learning algorithms.

The central hypothesis is that intelligent systems should perhaps be evaluated not only by what they gain, but also by what they spend and what they leave unresolved.

This gives us a conceptual objective:

$$
\boxed{\mathrm{Intelligence}=\mathrm{Effective\ Action}+\mathrm{Responsible\ Information\ Management}}
$$

Or, in the language of HST:

$$
\boxed{
Q=\text{Cost}+i\times\text{Debt}.
}
$$

This is the beginning of **Complex Reinforcement Learning** as explored within HST.

Whether it is the correct beginning is another question.

---

## Humility Before the Universe

The author does not claim to have discovered the algorithm of the universe.

He does not claim to have solved intelligence.

He does not claim to have unified physics and information theory.

He does not claim that the equation

$$
C+iD
$$

is a fundamental equation of nature.

He proposes it.

He explores it.

He experiments with it.

And he waits to see whether reality agrees.

That distinction is essential.

The universe does not care who proposed a theory.

It does not care whether the author has a PhD.

It does not care whether the theory is beautiful.

It does not care whether thousands of people believe it.

If the mathematics is wrong, the mathematics is wrong.

If the experiment fails, the experiment fails.

If the theory is useful, it should survive testing.

If it is useless, it should be discarded.

That is the humility at the heart of HST.

The author is not asking the reader to believe.

He is asking the reader to **test**.

---

## The Whisper Chain

The whisper chain begins with the universe.

It passes through physics.

Physics becomes mathematics.

Mathematics becomes models.

Models become algorithms.

Algorithms become artificial intelligence.

And artificial intelligence may eventually become another way for the universe to examine itself.

We are not necessarily inventing intelligence from nothing.

We may simply be discovering structures that were already present in the world around us.

Perhaps the universe has been whispering all along.

Perhaps cost, information, entropy, coherence, and equilibrium are fragments of a deeper language.

Perhaps intelligence is what happens when a physical system becomes sufficiently capable of modeling the consequences of its own actions.

Perhaps responsibility is simply the informational shadow of action.

Perhaps time is the ledger of irreversible change.

Perhaps intelligence is a system learning how to pay the cost of existing.

Or perhaps all of this is wrong.

The author is prepared for that possibility.

After all, this is **Humble Systems Theory**.

The theory does not ask us to believe that the whisper has been understood.

It asks us to listen.

And then to check whether what we heard was actually there.

---

## Mental Experiment & Simulation Hypothesis

Imagine telling your son: **“If you study, you will succeed.”** From the perspective of an Information Processing System (IPS), this is a measurable piece of information that enters the system and must be processed. The statement describes a relationship between a present action and a possible future state, but the future outcome does not yet exist as an actual state. At the moment the information is received, success and failure remain possible outcomes. The IPS therefore has to process information about a space of possible future states before one of those states becomes realized.

This processing is not free. Your son must spend time and energy to interpret the information, evaluate it, integrate it with previous information, make a decision, learn, remember, and act. The information may be represented abstractly, but its processing is physically instantiated. An IPS cannot simply acquire information and transform it into a future outcome without paying some physical cost. Information enters the system, the internal state changes, energy and time are consumed, and the system interacts with its environment until a new state is produced.

Now consider the possibility that this is not unique to biological intelligence. An atom interacts with another atom. A molecule responds to its environment. A neuron receives signals and changes state. A brain processes sensory information and generates an action. In each case, we can describe the system in terms of states, information, interactions, transitions, and physical costs. This suggests a broader modeling principle: **perhaps physical systems can be represented as Information Processing Systems, with their evolution understood as information processing under physical constraints.**

If this perspective is correct, then the universe itself can be viewed as a vast network of interacting IPS. Physical systems continuously receive information from other systems, update their states, exchange energy, and generate new states. What we describe through physical laws may therefore be understood, at a deeper level, as rules governing the transformation of information between physical states.

This immediately raises the question of possibility. Before an event is realized, the system does not necessarily have access to only one future state. There can be multiple physically possible outcomes. Somehow, the physical description of the present contains enough structure to determine how these possibilities evolve and eventually produce an observed outcome. In quantum mechanics, this idea becomes particularly striking because the formalism explicitly represents superpositions of possible states before measurement produces a definite result. Whether or not this should be interpreted literally as “the universe storing possibilities,” the underlying idea is difficult to ignore: **physical reality appears to contain information about states that are not yet realized.**

This gives us a powerful conceptual connection between information processing and physical reality. An IPS can be thought of as a system that maintains and transforms information about possible states while consuming physical resources as it evolves. The universe appears to do something remarkably similar, except on an incomprehensibly larger scale. It continuously evolves from one physical configuration to another while preserving and transforming information according to physical laws.

This is where the computational interpretation becomes unavoidable. If physical evolution can fundamentally be understood as information transformation, and if physical systems can be modeled as IPS, then perhaps computation is not something that humans invented and then imposed on nature. Perhaps computation is something we discovered because **nature was already doing it.**

The simulation hypothesis then becomes more interesting. We normally imagine a simulation as something running on a computer located outside the simulated world. But if the universe itself is an information-processing system, that distinction becomes unnecessary. The universe would not need to be a simulation running on some external machine. **The universe could itself be the computational process.** Matter would represent physical states, interactions would transform information, energy would impose the cost of those transformations, and time would describe the evolution of the system.

This also gives a possible interpretation of intelligence. An intelligent organism would not be fundamentally different from the rest of nature. It would simply be an IPS that has become sufficiently complex to process information about its environment, construct internal representations of possible futures, and modify its own state through action. The human brain would therefore not be an exception to the computational character of the universe; it would be one of its most obvious examples.

And this leads to the uncomfortable question: if we are IPS operating inside a universe that can itself be modeled as an IPS, what exactly are we?

Perhaps we are **information-processing systems inside an information-processing system, using information processing to discover that we are information-processing systems.**

At which point the universe has either achieved extraordinary self-awareness...

or has written a very elaborate program whose only purpose is to make its own subroutines ask whether they are living in a simulation.

**Either way, the debugging is going to be difficult.**

---

[humble_systems_theorey_cost_geometry_under_investigation](https://github.com/simomorphine/humble_systems_theorey_cost_geometry_under_investigation)

[ELWARDI-TETRIS](https://github.com/simomorphine/ELWARDI-TETRIS)

[probabilistic_decision_tree](https://github.com/simomorphine/probabilistic_decision_tree)

[neural_ips](https://github.com/simomorphine/neural_ips)

[The-Cosmic-Comedy](https://github.com/simomorphine/The-Cosmic-Comedy)
