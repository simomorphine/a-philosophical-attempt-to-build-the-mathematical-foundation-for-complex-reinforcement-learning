# The Search for Cost in Nature's Blueprint

## From Least Action to the Thermodynamics of Information

> *What if the quantity nature minimizes is not “action” in the abstract, but a deeper notion of cost associated with changing, maintaining, and organizing physical states?*

This question sits at the center of my developing work on **Humble Systems Theory (HST)**.

The claim is deliberately ambitious:

> **Perhaps physical action is not the fundamental quantity being minimized. Perhaps action is an emergent mathematical representation of a deeper cost of transition.**

I am not presenting this as an established fact of physics. It is a hypothesis, and there are many ways in which the hypothesis could be wrong.

I am also not a physicist or an information theorist. The purpose of this article is therefore not to claim authority over these fields, but to collect observations from existing physics that make the question worth investigating.

What is particularly interesting is that several apparently different areas of modern physics repeatedly encounter the same broad structure:

$$
\boxed{
\text{physical evolution}
\longrightarrow
\text{optimization}
\longrightarrow
\text{cost}
}
$$

Sometimes the relevant quantity is action.

Sometimes it is dissipated work.

Sometimes entropy production.

Sometimes information-theoretic distance.

Sometimes free energy.

Sometimes a geometric distance between probability distributions.

The terminology changes, the mathematical frameworks change, and the physical systems change.

But an intriguing pattern remains:

> **Physical processes appear to be strongly constrained by the cost of transforming one state into another.**

This article explores that pattern.

---

# 1. The Original Meaning of Action

The modern Principle of Least Action can sound deceptively abstract.

We write

$$
S[\gamma]
=
\int_{t_0}^{t_1} L(q,\dot q,t)\,dt
$$

and state that the physical trajectory is stationary with respect to variations of the action:

$$
\delta S = 0.
$$

The mathematical formalism is extraordinarily powerful.

But historically, the concept of action was not introduced merely as an abstract functional.

Pierre-Louis Moreau de Maupertuis associated action with ideas such as **effort, expense, and economy in nature**. The quantity he proposed was interpreted in terms of what nature “spends” in producing motion.

This historical interpretation is interesting for HST because it suggests that the language of **cost** is not a modern metaphor artificially imposed on mechanics.

The idea that nature somehow “economizes” is almost as old as the variational principle itself.

However, there is an important distinction.

Historical interpretation is not experimental proof.

Maupertuis' conception does **not** establish that the universe literally minimizes an energetic cost functional in the modern thermodynamic sense.

What it establishes is something more modest but still important:

> The conceptual relationship between action and economy has existed since the earliest formulations of the principle.

That makes the question worth reopening.

---

# 2. Action Is Not Energy

A major obstacle appears immediately.

Action is not simply energy.

The dimensions of action are

$$
[S] = \text{energy}\times\text{time},
$$

whereas energy has dimensions

$$
[E] = \text{energy}.
$$

For a classical system,

$$
L = T-V,
$$

and therefore

$$
S = \int (T-V)\,dt.
$$

This is not equivalent to saying that the system minimizes the total energy consumed.

Indeed, the Principle of Stationary Action applies naturally to conservative systems, while genuinely dissipative systems require additional mathematical structures.

This distinction matters enormously.

A careless version of the HST argument would be:

> “Least action doesn't include dissipation, therefore physics is wrong and HST fixes it.”

That would be too strong.

Physics already possesses sophisticated descriptions of dissipative systems, including generalized variational principles, Rayleigh dissipation, Onsager-type formulations, stochastic thermodynamics, optimal control, and other frameworks.

The more interesting question is therefore different:

> **Can action itself be understood as one particular representation of a more general cost associated with physical evolution?**

This is a much more open question.

---

# 3. The Emergence of a Cost Language in Nonequilibrium Physics

The equilibrium world described by elementary textbook mechanics is deceptively clean.

Real physical systems dissipate energy.

They fluctuate.

They exchange heat.

They erase information.

They operate away from equilibrium.

They have finite resources and finite timescales.

And once we leave the idealized conservative world, the concept of **cost** becomes increasingly explicit.

Modern nonequilibrium thermodynamics gives us quantities such as:

* work,
* heat,
* entropy production,
* dissipated work,
* irreversible entropy production,
* free-energy differences,
* thermodynamic length,
* information-theoretic divergences,
* and optimal-transport distances.

The remarkable development is that these quantities are not merely philosophical concepts.

They can be measured experimentally.

This is where the Cost-of-Action hypothesis becomes more interesting.

---

# 4. Information Is Not Free

One of the clearest examples comes from information thermodynamics.

Landauer's principle establishes a thermodynamic relationship between logically irreversible information processing and physical dissipation.

For an equilibrium memory, erasing one bit has a minimum energetic scale proportional to

$$
k_B T\ln 2.
$$

The important conceptual message is simple:

$$
\boxed{
\text{information processing has physical consequences.}
}
$$

Information is not merely an abstract mathematical object floating outside physics.

It is encoded in physical states.

Changing those states requires physical processes.

And physical processes can have thermodynamic costs.

But modern experiments are showing that even the phrase “the cost of one bit” requires considerably more care than the simple textbook picture suggests.

---

# 5. Going Beyond the Traditional Landauer Picture

In December 2025, Ciampini and collaborators reported an experiment using a levitated optomechanical two-state memory to investigate information erasure from a **nonequilibrium initial state**.

The experiment demonstrated that information could be erased with thermodynamic quantities below the conventional equilibrium Landauer value, including situations in which the measured dissipated heat could become negative.

This does **not** violate thermodynamics.

The crucial point is that the standard Landauer bound is formulated for a particular equilibrium preparation. If the memory begins in a nonequilibrium state, that initial state itself contains thermodynamic resources.

The “missing cost” has not disappeared.

The initial state has effectively supplied a resource.

This leads to a remarkably important conceptual observation:

$$
\boxed{
\text{The cost of an operation depends on the state from which the operation begins.}
}
$$

That statement resonates strongly with the motivation behind a generalized cost framework.

An action is not necessarily an isolated event with a universal price.

Its cost can depend on:

* the initial state,
* the available information,
* the physical resources already stored,
* the desired final state,
* the duration of the transformation,
* and the constraints imposed on the process.

This begins to look less like a simple scalar “price of an operation” and more like a **geometry of state transitions**.

---

# 6. Cost as Geometry

One of the most striking developments comes from optimal transport.

In December 2025, Oikawa, Nakayama, Ito, Sagawa, Toyabe and collaborators reported an experimental realization of **thermodynamically optimal transport** using optically trapped microparticles.

The experiment investigated finite-time transformations between probability distributions.

The central mathematical object was the **Wasserstein distance**.

In optimal transport, a transformation between distributions is not characterized only by where the system starts and where it ends.

The geometry of the space of probability distributions matters.

The experiment demonstrated that the excess dissipation associated with finite-time information erasure can be related to the Wasserstein distance between probability distributions. The researchers experimentally achieved the theoretical lower bound on dissipation within experimental uncertainty.

This is extraordinarily relevant to the Cost-of-Action hypothesis.

Consider the abstract transformation

$$
s_i \rightarrow s_f.
$$

A naive interpretation says:

> There is a beginning and an end.

Optimal transport suggests something deeper:

> **There is a geometry of possible transformations between the beginning and the end.**

Different paths have different costs.

Some paths are thermodynamically expensive.

Some are cheaper.

And under appropriate conditions, there exists an optimal path.

Schematically,

$$
s_i
\xrightarrow{\text{many possible paths}}
s_f
$$

with

$$
C[\gamma_1] >
C[\gamma_2] >
C[\gamma_*].
$$

The optimal trajectory satisfies something resembling

$$
\gamma_*
=
\arg\min_\gamma C[\gamma].
$$

That structure is remarkably close to the mathematical architecture of variational mechanics.

---

# 7. From Least Action to Least Dissipation

This creates an intriguing analogy.

Classical mechanics asks:

$$
\delta S = 0.
$$

Thermodynamic optimization can ask:

$$
\delta C = 0
$$

or, in appropriate formulations,

$$
C[\gamma_*]
=
\min_\gamma C[\gamma].
$$

The quantities are not identical.

The physical interpretations are not identical.

And one should absolutely not simply replace \(S\) with \(C\) and declare victory.

But the structural similarity is difficult to ignore.

Both involve:

1. a space of possible trajectories,
2. a functional assigned to trajectories,
3. physical constraints,
4. and a distinguished trajectory selected by an optimization principle.

The question then becomes:

> **Could classical action be one member of a larger family of physical cost functionals?**

That is a much more interesting question than merely renaming the Lagrangian.

---

# 8. The Cost of Going Fast

The 2025 experiments also reveal another important feature.

In the thermodynamic erasure experiments, speed, dissipation, and accuracy cannot generally be optimized independently. The Nature Communications experiment explicitly demonstrated trade-offs between speed, dissipation, and accuracy in finite-time information processing.

This is intuitive from an engineering perspective.

Suppose a system must transform

$$
s_i \rightarrow s_f.
$$

It can potentially do so:

* slowly and reversibly,
* quickly but dissipatively,
* accurately but expensively,
* or approximately with lower cost.

The physical world therefore does not merely ask:

> “Can the transition occur?”

It asks:

> **“At what cost can the transition occur under these constraints?”**

That is a much richer optimization problem.

---

# 9. Quantum Information Engines

A similar structure appears at the quantum scale.

In 2025, Aggarwal and collaborators experimentally implemented optimized work-extraction protocols in a quantum-dot Szilard engine over more than two orders of magnitude in driving speed.

The experiment investigated how optimized protocols behave from slow to fast driving regimes.

Importantly, the work showed that optimization involves trade-offs involving:

* efficiency,
* power,
* driving speed,
* and fluctuations.

Increasing useful power comes with increased fluctuations under the conditions studied.

Again, we encounter a physical optimization problem:

$$
\text{maximize useful output}
$$

subject to

$$
\text{thermodynamic constraints}.
$$

The system is not simply following an arbitrary trajectory.

The protocol itself can be optimized.

This suggests a broader pattern:

$$
\boxed{
\text{physical dynamics}
+
\text{constraints}
+
\text{resources}
\rightarrow
\text{optimal transformation}.
}
$$

That structure is increasingly difficult to dismiss as merely an engineering metaphor.

---

# 10. Information, Entropy and Quantum Fluctuations

A related 2025 experiment investigated information thermodynamics in a quantum-dot Szilard engine, specifically testing fluctuation theorems and thermodynamic uncertainty relations. The authors found that entropy production inferred from measurement can provide a more relevant information measure than mutual information in the context they studied.

This is conceptually important.

It suggests that “information” itself is not necessarily represented by one universal scalar.

Different operational questions can make different information measures relevant.

That observation matters for HST.

If one wants to introduce something such as **informational debt**, it cannot simply be assumed that debt is synonymous with Shannon entropy, mutual information, or any other existing quantity.

Instead, one has to ask:

> **What physical or operational phenomenon is the proposed information quantity measuring?**

A useful theory should eventually answer that question mathematically and experimentally.

---

# 11. Maxwell's Demon Returns

The old Maxwell's demon thought experiment provides perhaps the most intuitive bridge between information and physical cost.

In December 2025, researchers experimentally implemented a thermodynamically controlled stochastic-resetting process using a time-dependent optical trapping potential. They described the setup as a Maxwell's demon that continuously erases information and experimentally evaluated its energetic cost.

Their analysis found that, using an appropriate measure of available information, the energetic cost could approach a reversible minimum while remaining constrained by the Landauer limit.

The historical thought experiment has therefore become a laboratory problem.

The demon is no longer merely a philosophical character.

The information it processes is physically embodied.

The resetting protocol has an energetic cost.

And that cost can be experimentally investigated.

This is precisely the type of evidence that motivates the Cost-of-Action hypothesis.

Not because it proves HST.

It doesn't.

Rather, because it demonstrates experimentally that:

$$
\boxed{
\text{information}
\rightarrow
\text{physical transformation}
\rightarrow
\text{thermodynamic cost}.
}
$$

---

# 12. The Quantum Question: Is Action More Fundamental Than We Think?

The connection between action and quantum mechanics provides another fascinating piece of the puzzle.

In April 2026, Winfried Lohmiller and Jean-Jacques Slotine published *On computing quantum waves exactly from classical action* in the Proceedings of the Royal Society A. They argue that the Schrödinger equation can be constructed from classical least-action dynamics combined with classical density evolution, and they extend their construction to other equations including Klein–Gordon, Pauli and Dirac equations.

Their construction uses classical stationary-action paths and associated density information to construct the quantum wavefunction, with the authors arguing that this can avoid the enormous path-space calculation associated with the conventional Feynman path-integral picture.

If correct in its full generality, this would be an extremely interesting connection between classical variational mechanics and quantum dynamics.

It would suggest that the mathematical role of action reaches deeper into physics than one might naively expect.

But this result requires an important warning.

---

# 13. A Scientific Hypothesis Must Survive Criticism

A May 2026 comment by Gábor Vattay challenged the claimed exact equivalence. The criticism argues that the derivation neglects spatial derivatives of the probability-density amplitude and therefore omits the quantum potential, making the claimed equivalence non-exact in general.

This disagreement is extremely important.

It means that the Lohmiller–Slotine result should **not** currently be presented as established evidence that classical action generates quantum mechanics.

The responsible interpretation is:

> A recent peer-reviewed paper proposes a potentially deep connection between classical least action and quantum wave equations, while a subsequent technical critique argues that the claimed exact equivalence contains a foundational mathematical error.

That is precisely how scientific evidence should be handled.

For HST, this is not an inconvenience.

It is a methodological lesson.

A theory becomes stronger when it explicitly records both:

$$
\text{evidence supporting hypothesis}
$$

and

$$
\text{evidence challenging hypothesis}.
$$

The goal is not to collect arguments that confirm HST.

The goal is to discover whether HST survives attempts to falsify it.

---

# 14. From Physical Action to Computational Cost

There is another direction that deserves attention.

Modern physics increasingly allows us to describe physical processes in informational and computational terms.

A physical system can be viewed as moving through a state space.

A transformation

$$
s_i\rightarrow s_f
$$

requires some physical process.

That process may involve:

* energy,
* time,
* information,
* entropy production,
* control,
* precision,
* and environmental interaction.

From this perspective, the physical trajectory resembles a computation constrained by available resources.

One does not need to assume that the universe is literally a computer to ask whether **computational cost** provides a useful mathematical description of physical dynamics.

This distinction is important.

The statement

> “The universe is a computer”

is a metaphysical hypothesis.

The statement

> “Certain physical transformations can be described using computational and information-theoretic costs”

is an experimentally supported feature of modern physics.

The latter does not require the former.

---

# 15. A Possible Generalization

This is where HST enters the discussion.

Suppose that a physical system has a state space

$$
\mathcal S.
$$

For two states

$$
s_i,s_j\in\mathcal S,
$$

define a generalized transition quantity

$$
Q(s_i,s_j).
$$

Instead of assuming that this quantity is simply a conventional distance, HST explores a decomposition of the form

$$
\boxed{
Q(s_i,s_j)
=
C(s_i,s_j)
+
iD(s_i,s_j)
}
$$

where:

* \(C\) represents a **cost** associated with the transition,
* \(D\) represents a form of **informational debt**, history dependence, or informational imbalance.

The complex notation is not meant to claim that physical energy is literally imaginary.

It is a mathematical device for carrying two coupled quantities in a single object.

The corresponding geometry can then be explored through a metric-like construction such as

$$
d_\gamma(s_i,s_j)
=
\sqrt{
d(s_i,s_j)^2
+
\gamma^2D(s_i,s_j)^2
}.
$$

The important question is not whether this particular equation is aesthetically appealing.

The important question is:

> **Can such a decomposition reproduce known physical limits?**

If it cannot, HST should be rejected or substantially revised.

If it can, the next question becomes whether it predicts something new.

---

# 16. Why “Debt” Might Be More Than Another Name for Entropy

The concept of informational debt is deliberately different from simply calling entropy “debt.”

Consider a system that possesses information or structure acquired in the past.

A later transition may depend on that history.

The same physical state transition could therefore have different costs depending on:

$$
\text{what resources were already accumulated},
$$

$$
\text{what information was available},
$$

or

$$
\text{what constraints were inherited from previous states}.
$$

This resembles the nonequilibrium memory experiment discussed earlier.

The apparent energetic cost of erasing information depends on the initial nonequilibrium preparation.

That does not prove informational debt.

But it demonstrates an experimentally relevant phenomenon:

$$
\boxed{
\text{initial informational/thermodynamic structure changes transition cost}.
}
$$

This is exactly the kind of phenomenon that a generalized cost theory would need to explain.

---

# 17. The Geometry of Probability

The Wasserstein experiment makes the connection even more concrete.

Suppose

$$
p_i(x)
$$

is an initial probability distribution and

$$
p_f(x)
$$

is the final distribution.

The transition is not represented merely by

$$
p_i\rightarrow p_f.
$$

There exists a space of possible paths connecting these distributions.

The Wasserstein metric provides a geometry on this space.

The thermodynamically optimal protocol corresponds to a geodesic-like motion through probability space, and the experiment demonstrated the corresponding minimum-dissipation bound for finite-time transformations.

This produces a fascinating conceptual bridge:

$$
\boxed{
\text{geometry}
\rightarrow
\text{optimal path}
\rightarrow
\text{physical dissipation}.
}
$$

In other words, geometry is not merely describing the system.

Under the appropriate thermodynamic framework, it constrains the physical cost of moving through state space.

This is one of the strongest reasons I think the relationship between **cost and geometry** deserves further investigation.

---

# 18. The Broader Pattern

At this point, we can place the different examples next to one another.

| Field                         | Physical/Mathematical Object         | Optimization or Constraint                                  |
| ----------------------------- | ------------------------------------ | ----------------------------------------------------------- |
| Classical mechanics           | Action \(S\)                         | Stationary action                                           |
| Nonequilibrium thermodynamics | Dissipation                          | Minimize irreversible cost under constraints                |
| Information thermodynamics    | Erasure cost                         | Thermodynamic lower bounds                                  |
| Optimal transport             | Wasserstein geometry                 | Minimum transport cost                                      |
| Quantum information           | Work extraction                      | Optimize work/efficiency under fluctuations                 |
| Maxwell's demon               | Information + work                   | Information processing constrained by thermodynamics        |
| Quantum dynamics              | Classical action constructions       | Proposed connection to quantum wave equations               |
| Biological systems            | Variational/free-energy formulations | Minimize variational quantities under dynamical constraints |

These are not the same theory.

They should not be merged carelessly.

But the recurring mathematical structure is striking.

---

# 19. The Cost-of-Action Hypothesis

This motivates a more precise formulation of the central HST hypothesis.

Instead of saying:

> “The universe minimizes action.”

we ask:

> **What if action is an effective representation of a more general physical cost functional?**

Symbolically,

$$
\boxed{
S_{\mathrm{action}}
\sim
\mathcal F[\text{physical cost}]
}
$$

in appropriate physical limits.

Under this hypothesis, classical mechanics would not necessarily be the fundamental level.

It could represent a particular regime in which the generalized cost reduces to the familiar action.

For example,

$$
\mathcal C
\longrightarrow
S
$$

under conservative classical assumptions.

Meanwhile, in nonequilibrium systems,

$$
\mathcal C
\longrightarrow
\text{dissipation / thermodynamic cost},
$$

and in information-processing systems,

$$
\mathcal C
\longrightarrow
\text{information-processing cost}.
$$

The same underlying principle would therefore manifest differently depending on the physical regime.

This is speculative.

But it is a testable form of speculation.

---

# 20. What Would Count as Evidence?

This is perhaps the most important question.

Finding papers containing the word “cost” is not enough.

Finding optimization everywhere is not enough.

Finding mathematical analogies is not enough.

For HST to become physics rather than philosophy, it needs increasingly difficult tests.

### Test 1 — Recover known physics

A generalized cost functional should reproduce known results in appropriate limits.

For example,

$$
\mathcal C
\rightarrow
S
$$

should recover classical mechanics where expected.

### Test 2 — Recover thermodynamic bounds

It should reproduce known relations involving:

$$
\Delta F,\qquad
W_{\mathrm{diss}},\qquad
\Sigma,
$$

and information-theoretic quantities where appropriate.

### Test 3 — Handle nonequilibrium states

It should naturally explain why the cost of an operation can depend on the initial nonequilibrium state.

The 2025 levitated-memory experiment provides an excellent benchmark for such a theory.

### Test 4 — Recover geometric costs

It should explain why Wasserstein geometry appears in finite-time thermodynamic optimization.

The 2025 optimal-transport experiment provides another benchmark.

### Test 5 — Produce new predictions

This is the decisive step.

A theory that merely redescribes existing results is interesting philosophically but limited scientifically.

A successful HST would eventually need to predict something that existing theories do not already predict.

---

# 21. What HST Should Not Claim Yet

There are several claims that I deliberately do **not** think the current evidence establishes.

### Claim 1: “Physics has proven that the universe minimizes energy cost.”

No.

The evidence does not establish this universal statement.

### Claim 2: “Least action is wrong.”

No.

Least action remains one of the most successful formulations in theoretical physics.

### Claim 3: “Action is just energy consumption.”

No.

Action and energy have different dimensions and mathematical roles.

### Claim 4: “Information is literally energy.”

Not in that simplistic sense.

Information can be physically embodied and processing information can have thermodynamic consequences, but information and energy are not interchangeable quantities.

### Claim 5: “Recent experiments prove HST.”

Absolutely not.

They don't test HST.

They provide **independent physical phenomena that motivate questions HST is attempting to formalize.**

That distinction is essential.

---

# 22. The Evidence Map

The evidence can therefore be organized into four levels.

## Level I — Historical evidence

Maupertuis and the early development of least action provide historical precedent for interpreting action through concepts of economy, effort, and expense.

This supports the *language* of cost.

It does not establish a new physical law.

---

## Level II — Established physical phenomena

Modern thermodynamics demonstrates that:

$$
\text{information processing}
$$

can have measurable physical consequences.

Experiments have directly investigated:

* information erasure,
* work extraction,
* stochastic resetting,
* thermodynamic uncertainty,
* and finite-time dissipation.

This establishes that **physical cost associated with information processing is real and measurable**.

---

## Level III — Mathematical convergence

Several areas use geometric or variational structures to identify optimal physical processes.

Examples include:

$$
\text{least action},
$$

$$
\text{optimal transport},
$$

$$
\text{thermodynamic length},
$$

and

$$
\text{information geometry}.
$$

These do not constitute one unified theory.

But they suggest that **geometry and optimization are deeply intertwined with physical cost.**

---

## Level IV — HST hypothesis

HST proposes that these observations may reflect a deeper principle:

$$
\boxed{
\text{physical evolution is fundamentally constrained by cost of action.}
}
$$

The exact definition of that cost remains the central theoretical problem.

That is where the actual research begins.

---

# 23. The Most Interesting Possibility

Perhaps the most interesting version of the hypothesis is not:

$$
\text{Action} \rightarrow \text{Cost}.
$$

It may instead be:

$$
\boxed{
\text{Cost}
\rightarrow
\text{Action}
}
$$

In this interpretation, action is not fundamental.

It emerges as the appropriate mathematical quantity under a particular set of assumptions.

For conservative classical mechanics, we obtain the familiar action:

$$
S=\int L\,dt.
$$

For nonequilibrium processes, other cost functionals become more natural.

For information-processing systems, information-theoretic quantities become relevant.

For probability distributions, optimal-transport geometry becomes relevant.

The different theories would then be viewed not as competing descriptions, but as different projections of a deeper optimization structure.

This is, in my view, the strongest form of the idea.

---

# 24. A Possible Unification

The dream would be to construct something like

$$
\boxed{
\mathcal C[\gamma]
=
\text{physical cost of realizing trajectory }\gamma
}
$$

and derive known principles as limiting cases.

Perhaps

$$
\mathcal C
=
\mathcal C_{\mathrm{energy}}
+
\lambda_1\mathcal C_{\mathrm{information}}
+
\lambda_2\mathcal C_{\mathrm{irreversibility}}
+
\lambda_3\mathcal C_{\mathrm{constraint}}
+\cdots
$$

with the coefficients and terms determined by physical theory rather than chosen arbitrarily.

Then a physical trajectory would satisfy something like

$$
\gamma_*
=
\operatorname*{arg\,min}_{\gamma\in\Gamma}
\mathcal C[\gamma].
$$

Classical action could emerge in one limit.

Thermodynamic optimal transport could emerge in another.

Information-processing bounds could emerge in another.

Whether such a construction is possible is completely unknown.

But it provides a concrete mathematical target.

---

# 25. Why the 2025–2026 Experiments Matter

The recent experimental literature does not establish the Cost-of-Action hypothesis.

What it does establish is arguably more useful at this stage.

It shows that physicists can experimentally manipulate and measure relationships among:

$$
\boxed{
\text{information}
\leftrightarrow
\text{work}
\leftrightarrow
\text{entropy}
\leftrightarrow
\text{dissipation}
\leftrightarrow
\text{geometry}
}
$$

At the same time, contemporary theoretical work continues to investigate deep relationships between variational principles and quantum dynamics.

This creates a fertile environment for asking whether these structures can be understood through a more general concept of physical cost.

The hypothesis is therefore not:

> “I discovered that nature optimizes things.”

Nature's optimization structure is already deeply embedded in physics.

The more ambitious question is:

> **Are the different quantities that nature appears to optimize manifestations of a common underlying notion of cost?**

That is the question HST is attempting to investigate.

---

# 26. From Physics to Intelligent Systems

This question becomes especially interesting when moving from physics to artificial intelligence.

An intelligent system also transforms states.

It observes a state:

$$
s_t,
$$

takes an action:

$$
a_t,
$$

and reaches a new state:

$$
s_{t+1}.
$$

Traditional reinforcement learning generally asks the agent to maximize expected cumulative reward:

$$
\max_\pi
\mathbb E_\pi
\left[
\sum_t \gamma^t r_t
\right].
$$

HST instead asks whether we can formulate intelligent behavior around the complementary question:

$$
\boxed{
\text{What is the cost of producing this transition?}
}
$$

This leads naturally toward quantities such as

$$
C(s_t,s_{t+1})
$$

and potentially

$$
D(s_t,s_{t+1}).
$$

The motivation is not that an artificial agent literally follows the laws of quantum mechanics.

It is that biological and physical systems operate under resource constraints.

If intelligence is ultimately physical, then intelligence cannot escape physics.

Every computation occurs on matter.

Every state transition requires physical resources.

Every memory has a physical substrate.

Every update has some energetic and informational context.

This makes the relationship between **intelligence and cost** worth investigating.

---

# 27. The Humble Position

There is an important philosophical component to this work.

The hypothesis should remain humble.

I may be completely wrong.

The proposed decomposition

$$
Q=C+iD
$$

may turn out to be mathematically unnecessary.

The generalized geometry may collapse into an existing theory.

The proposed physical interpretation of “cost” may prove to be only metaphorical.

The connection between action and computational cost may fail.

Or HST may simply rediscover mathematics that already exists under different names.

All of these are legitimate outcomes.

The goal is therefore not to protect HST from criticism.

The goal is to expose it to enough mathematics and enough physics that we eventually discover whether anything survives.

---

# 28. The Real Research Program

The most productive interpretation of the current evidence is therefore not:

> “Physics has confirmed HST.”

It is:

> **Modern physics provides several independent examples in which physical transformations are constrained by measurable energetic, informational, geometric, or thermodynamic costs. HST proposes investigating whether these structures can be placed inside a common theory of cost of action.**

That research program has several concrete directions:

1. **Formalize the cost functional.**
2. **Determine its physical dimensions and invariances.**
3. **Recover classical mechanics as a limiting case.**
4. **Connect it to stochastic thermodynamics.**
5. **Investigate its relationship with optimal transport.**
6. **Clarify the meaning of informational debt.**
7. **Determine whether the complex representation \(C+iD\) has mathematical necessity or is merely convenient notation.**
8. **Search for experimentally distinguishable predictions.**
9. **Attempt to falsify the framework.**
10. **Only then consider applications to artificial intelligence.**

---

# 29. Conclusion

The history of physics contains a recurring idea:

$$
\boxed{
\text{Nature does not appear to realize arbitrary trajectories.}
}
$$

Classical mechanics expresses this through stationary action.

Thermodynamics expresses constraints through free energy and entropy production.

Information thermodynamics reveals physical costs associated with manipulating information.

Optimal transport reveals a geometry in which different transformations between the same states have different thermodynamic costs.

Quantum information experiments demonstrate that information can be directly connected to work, fluctuations, and thermodynamic constraints.

Recent experiments have even begun to realize these optimal processes directly in microscopic physical systems.

At the same time, recent theoretical work has reopened questions about the relationship between classical action and quantum dynamics—although those claims are actively disputed and therefore should be treated cautiously.

None of this proves that the universe fundamentally minimizes a quantity called **cost of action**.

But it establishes something that I believe is worth taking seriously:

> **The physical cost of transforming a state is not merely a philosophical metaphor. In modern physics, energetic and informational costs can be defined, bounded, geometrized, optimized, and experimentally measured.**

This motivates the central HST question:

$$
\boxed{
\textbf{Is action fundamental, or is action an emergent representation of a deeper cost functional?}
}
$$

Perhaps the answer is no.

Perhaps conventional action is already fundamental.

Perhaps “cost” is merely a useful language for several unrelated physical phenomena.

Or perhaps the apparently different principles of mechanics, thermodynamics, information theory, and computation are different shadows cast by a deeper variational structure.

At present, I do not know.

And that uncertainty is precisely why the question is worth investigating.

---

# References and Further Reading

### Classical action and quantum mechanics

**Lohmiller, W. & Slotine, J.-J. (2026).**
*On computing quantum waves exactly from classical action.*
Proceedings of the Royal Society A, 482(2336), 20250413. Published April 22, 2026.
[Royal Society A — On computing quantum waves exactly from classical action](https://doi.org/10.1098/rspa.2025.0413?utm_source=chatgpt.com)

**Vattay, G. (2026).**
*Comment on “On computing quantum waves exactly from classical action”.*
arXiv:2605.02621.
[arXiv — Critical comment on the classical-action/quantum-wave claim](https://arxiv.org/abs/2605.02621?utm_source=chatgpt.com)

---

### Information thermodynamics and Landauer's principle

**Ciampini, M. A. et al. (2025).**
*Erasure of a nonequilibrium memory beyond Landauer's bound using levitated optomechanics with spatio-temporal optical control.*
Physical Review Research 7, 043321.
[Aspelmeyer Group — Breaking Landauer's bound using a nonequilibrium memory](https://aspelmeyer.quantum.at/details/news/breaking-landauers-bound-using-a-nonequilibrium-memory/?utm_source=chatgpt.com)

---

### Thermodynamic optimal transport

**Oikawa, S. et al. (2025).**
*Experimentally achieving minimal dissipation via thermodynamically optimal transport.*
Nature Communications 16, 10424. Published December 1, 2025.
[Nature Communications — Experimentally achieving minimal dissipation via thermodynamically optimal transport](https://www.nature.com/articles/s41467-025-66519-9?utm_source=chatgpt.com)

---

### Quantum-dot information engines

**Aggarwal, K. et al. (2025).**
*Rapid optimal work extraction from a quantum-dot information engine.*
Physical Review Research 7, L032017. Published July 15, 2025.
[Physical Review Research — Quantum-dot information engine experiment](https://journals.aps.org/prresearch/recent?page=90&utm_source=chatgpt.com)

---

### Information thermodynamics, fluctuation theorems and quantum engines

**Barker, D. et al. (2025).**
*Information Thermodynamics in a Quantum Dot Szilard Engine — Experimentally Investigating Fluctuation Theorems and Thermodynamic Uncertainty Relations.*
arXiv:2511.08541.
[arXiv — Information thermodynamics in a quantum-dot Szilard engine](https://arxiv.org/abs/2511.08541?utm_source=chatgpt.com)

---

### Maxwell's demon and stochastic resetting

**Taming a Maxwell's demon for experimental stochastic resetting. (2025).**
Physical Review E 112, 064116. Published December 11, 2025.
[Physical Review E — Taming a Maxwell's demon for experimental stochastic resetting](https://journals.aps.org/pre/abstract/10.1103/jcvp-6yw2?utm_source=chatgpt.com)
