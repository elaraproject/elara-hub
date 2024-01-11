+++
title = "Speculative future extensions"
+++

The following includes a number of ideas that may make their way into the project at some point in the future.

## Spaceplanes

A new way to create SSTO-like transport spaceplanes:

- Lift becomes insufficient ~30km into atmosphere
- So we will design a jet (turbofan) engine that will transition to a ramjet, and then turn off at 30km, and rely on external rocket boosters the rest of the way
	- A modified version of the aircraft can be launched from an electromagnetic catapult and does not need the turbofan; it can just use its ramjet
- The external rocket boosters then detach (for this concept design they are non-reusable, but they are relatively cheap and inexpensive because they don't need to be as big as a typical rocket given they are given a head start by the jet engine)
- If the spaceplane is configured for suborbital mode, then its payload (with attached third stage) is released at this point; if the spaceplane is configured for pure orbital mode (such as docking with ISS or an interplanetary spacecraft in orbit), then its rocket boosters burn for a little while longer
- Upon re-entry the SSTO would slow down in the upper atmosphere, then glide, then finally at ~5-10km altitude it turns its jet engine back on to fly to its runway

Why do this? It integrates the best things about both SSTOs and conventional rockets:

- Ability to operate from runways rather than extremely complex rocket launch pads
- Theoretically safer than conventional rocket (given it takes off like a normal jet) and cheaper than (non-reusable) rockets
- Isn't as limited by payload constraints of SSTOs because it uses lift and efficient jet engines to gain speed at low altitudes, then uses expendable rockets for the second stage - in fact it should be able to carry more payload than a comparable 2-stage rocket given the boosting effect of the jet engine at low altitudes, allowing the second stage expendable boosters to be smaller
- It works well with economies of scale, which is the best way to achieve inexpensive spaceflight

Active cooling for reentry systems to prevent need for thermal tiling.

Engine stages:

- Takeoff-Cruise: turbofan with afterburner (or alternatively electromagnetic catapult, it must get to 300 km/h or above for its ramjet to work)
- Cruise-15km: ramjet
- 15km-70km: scramjet, from 30km onwards boosted by rocket

The same intake is used by both the turbofan and ram/scramjet. However, the air from the intake is rerouted to the appropriate engine at the appropriate stage.

## Elara-powered vertical farms

Vertical farms that use hydroponics for massively increased food growth efficiency, to feed the world.

## Elara-powered water splitting

- Project elara - using photosynthetic water splitting into H2 and O2 to make rocket fuel (LH2 and LOX)
- Idea: ion engines powered by hydrogen fuel cells are used for second-generation elara arrays
- Idea: project elara can store a lot of energy for peak demand times by using electrolysis to split water into hydrogen gas which can be then burned to generate electricity via steam turbines and generators (fuel cells work too but are less efficient) and the resulting water vapour collected and condensed during peak demand

## Fuel-efficient airplanes

These are blended wing bodies with very very high lift-to-drag ratios (up to 600:1). So they can quickly take off to a high altitude (with high-thrust engines), then turn off their engines and just glide to their destination. This allows them to take advantage of jet streams in Earth's upper atmosphere and use minimal fuel.

### Mass driver propulsion

The idea is to have two mass drivers that accelerate solid spherical fuel pellets so fast that when they hit each other the pellets vaporize. The mass drivers would be powered by nuclear reactors.

### Oberth effect launches

For interplanetary missions in project elara, the standard practice is to let a spacecraft perform a powered fall into a gravity well to gain speed.

$$
K = \frac{1}{2} mv^2
$$
$$
\frac{dK}{dt} = mv \frac{dv}{dt} = mav = F v
$$
$$
V = \Delta v \sqrt{1 + \frac{2v_{esc}}{\Delta v}}
$$

Thus with a delta-v of 100 km/s as can be accomplished with very long firings of an ion engine, then you can get to as much as 360 km/s. That being said, the ion engine would need to be fired for decades to get to actually get to 100 km/s from rest. Thus, conventional chemical engines with a high enough delta-v would work as well, as would a nuclear thermal engine (which should be able to run for much longer and thus have a high delta-v)
### Magsail in interstellar medium

### Black hole reactor

Non rotating black hole - Dyson array around it to extract mass-energy of accretion disk - can extract 6% mass energy. Method of simulating this would be to conduct an N-body simulation of millions of particles distributed randomly within a defined accretion disk with my geodesic integrator and finding the average particle flux around a Gaussian surface around the BH. Then use the flux distribution to find the optimal placement and geometry of the black hole mirror array.

Plan for black hole spacecraft: it will be a photonic rocket powered by a black hole reactor. The black hole reactor uses a microscopic rotating black hole to which a tiny but constant stream of fuel is supplied. A dyson sphere envelops the black hole (which also serves as a magnetic cage). The black hole converts up to 29% of the mass-energy of the fuel to energy - that means 1 kg of fuel would be able to release $9 \times 10^{16}$ Joules of energy - which can then be used to power a conventional thermal rocket or mass driver for spacecraft propulsion.

Derivation: the maximum energy that can be extracted from a rotational black hole is given by:

$$
E_{max} = Mc^2 \left[1 - \frac{1}{\sqrt{2}}\left(1 + \sqrt{1 - \frac{a^2}{M^2}}\right)^{1/2}\right]
$$
The total energy efficiency is given by:

$$
\lim_{a \to 1} \frac{E_{max}}{Mc^2} \approx 0.29
$$

### SSTOs

Idea is for spaceplane with detachable rocket boosters. The spaceplane uses a ramjet up to about 20km of altitude, then shuts off its ramjet and uses its rocket boosters to quickly and rapidly gain thrust before the boosters detach and fall back to earth (they land with parachutes and big airbags, like pathfinder). This allows its main rocket engine and thrusters to be fairly small.

### High thrust, high delta-v propulsion

High thrust, high delta-v rockets are rockets that both produce enough thrust and can run for a long time, from months to years on end. This excludes chemical rockets (which cannot run for long) and electrical propulsion (which produces too little thrust). The ones primarily investigated are:

- Mass driver propulsion
- Thermal rocket
- Antimatter rocket
- Plasma/other magnetohydrodynamic rocket

Note: of interest also is interstellar magsail designs

### Methods to reduce nuclear waste

- Fission-catalyzed fusion
- Artificial nuclear transmutation
