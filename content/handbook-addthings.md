+++
title = "Things to add to Elara Handbook"
+++

## For knowledge library

$W = \Delta K = -\Delta U$

Distance is denoted $S$, and speed is denoted $|v|$.

"Calculus is the study of the infinitely large and infinitely small. We use limits to tame infinity so it can be reasoned with."

In elara handbook's neural network chapter, mention how you can use first or second-order Newton-Raphson method to minimize loss function of neural network, basically solving the equation $\nabla L = 0$.

Line integral is like "weighted arc length". The "weights" are the value of the (vector or scalar-valued) function at each point along the curve.

Interpretation of parametric arc length - you are taking the speed (norm of the vector-valued velocity function) and integrating it over the enter length of the curve.

Add a mechanics section to GR (General Relativity, Part 4 in the Handbook) - it should have things like the GR version of 4-force, GR version of 4-momentum, GR version of Lorentz force law with Faraday tensor, etc. E.g. force in General Relativity is defined with:

$$
F^\lambda = \frac{dP^\lambda}{d\tau} + \Gamma^\lambda_{\mu \nu} U^\mu P^\nu
$$

Because otherwise GR seems like just a theory of gravity. Which it is, but really it's a complete reformulation of classical mechanics in curved spacetime. That is - it (can technically) describe everything from electromagnetism to fluid dynamics to kinematics, it's just not usually taught that way.

Nice page with physics-motivated calculus problems: <https://math24.net/optimization-problems-physics.html> and <https://math24.net/newtons-second-law-motion.html>

Add to Elara handbook differential equations section: everything from <https://math24.net/topics-differential-equations.html>

[[All about partial differential equations]](@/all-about-pdes.md) should be added to diffeq section (for the analytical parts) and developer hub (for the numerical/simulation parts)

In an analogy to gravity, the field corresponds to the gravitational force pulling an object onto the Earth, while the potential corresponds to the shape of the landscape on which it stands.

A differential equation defines something by how it changes.

Horner's method
<https://math.stackexchange.com/questions/1356981/why-does-the-separation-of-variables-method-for-des-work>

## For ecosystem

Establish the Betancourt Doctrine - project elara will neither recognize nor establish relationships with governments, organizations or groups that have committed, or aim to commit human rights abuses, derive their authority from non-democratic means, or commit gross abuses of power

## For developer hub

Project elara will do open-source hardware like this: <https://sr.ht/~jacqueline/tangara/>. The open-source hardware will be part of developer hub.

Project Elara development guidelines:

- Simple - using minimal abstraction, minimal dependencies, and implementing a minimalist set of features - this is to aid in maintenance and code preservation over centuries
- Fast - high performance, low resource usage, able to run on even low-performance devices
- Robust - well-engineered and able to handle edge-cases and errors and still provide correct behavior

Each verified project elara app or library will be explained in great detail in the Handbook - the implementations are all explained well enough, and enough code examples are given, that someone could conceivably rewrite the code from scratch following the explanations. This is also why they have to be fairly small codebases.
