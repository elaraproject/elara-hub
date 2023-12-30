+++
title = "Black hole raytracing"
+++

The general algorithm is this:

- Emit a bunch of rays from the camera at $(x, y)$ positions for each pixel in the final image
- Calculate where they end up by numerically integrating along geodesics:
	- If they end up inside the event horizon, color should be black
	- Otherwise, color should be a sample color of whichever patch of night sky is in the direction they hit
- Set the color of the pixel located at position $(x, y)$ based on the previous step

Note that this has no global illumination as it is **not** a path tracer. A path tracer would actually generate photorealistic images, but this is just a proof of concept.

I will write it in numpy first, then port it to Rust once I get it working.

---

First, we position our black hole at the origin, $(0, 0, 0)$. We want to position our camera at $(6M, \pi / 2, \pi / 4)$ which should give a good view of the black hole.

Then, we want to produce our rays. Recall that a ray is typically given by:

$$
\vec R(t) = \vec O + \vec t D
$$

Here, $\vec O$ is the origin, and $\vec D$ is the direction vector of the ray. Note that the normalized velocity vector is exactly the same thing as the direction vector, so $\vec D$ can also be interpreted as a normalized velocity vector.

However, we have a slight complication. Recall that rays of light are bent in curved spacetime. Therefore, the simple ray equation does **not** apply. We instead need to numerically integrate to find the position of a ray at time $t$. To do this, we will use two derived equations from Schwarzschild spacetime:

$$
\frac{d^2 r}{ds^2} = -\frac{M}{r^2} + \frac{l^2}{r^3} - \frac{3Ml^2}{r^4}
$$

$$
\frac{d^2 \phi}{ds^2} = -\frac{2l}{r^3} \frac{dr}{ds}
$$
(there is a third equation, $\theta'' = 0$, but it's unnecessary to really mention it, as $\theta$ is constant)

We derived the two above equations from the radial motion equation and the $p_\phi$ constant of motion equation, and taking the derivative to get a second-order differential equation (which also fortunately cancels out the square roots and all the annoyances of squares appearing in derivative terms).

Here, we use the affine parameter $s$ as a substitute for time $t$, because light in General Relativity doesn't really experience time as it moves. So $s$ is like a "substitute" for time that we can effectively consider time, just for a light ray instead of an ordinary massive particle.

We will integrate from time $t_0$, when the rays leave the camera, to time $t_f$, where we can arbitrarily set $t_f$. We want to then find the position vector $\vec R(t)$ of the ray at $t_f$, or $\vec R(t_f)$. If $\|\vec R(t_f)\| < 2M$, that means the ray vector is inside the event horizon, so we want to return a color of black. Otherwise, we sample the color of the patch of night sky the ray position vector hits. 

Alternatively, we can simply find the vector $\vec W = \vec R(t_r) - \vec O$, where $\vec W$ is the vector pointing from the origin (where the camera is) to the ray vector position. By converting it to Cartesian coordinates and then normalizing this vector, we have the $(x, y, z)$ coordinates of the ray vector position in terms of the camera's world unit sphere. Now this is where equirectangular textures come in handy, because we can then take those $(x, y, z)$ coordinates and find the $(u, v)$ coordinates of the world texture, from which we can sample the color. The formula is:

$$
u = \frac{1}{2} + \frac{\operatorname{atan2}(W_z, W_x)}{2\pi}
$$

$$
v = \frac{1}{2} + \frac{\operatorname{arcsin}(W_y)}{\pi}
$$

For each pixel, we will repeat these steps - creating a ray, tracing the ray, and then finding the final color it outputs, and assigning that color to that pixel. Then, we will have our complete image of the black hole.

---

$$
\left(\frac{dr}{d\phi}\right)^2 = \frac{r^4}{b^2} - \left(1 - \frac{r_s}{r}\right) \left(\frac{r^4}{a^2} + r^2\right)
$$

Now we take the limit as $m \to 0$, therefore $a \to \infty$ and $b \to \infty$, so:

$$
\left(\frac{dr}{d\phi}\right)^2 =  - \left(1 - \frac{r_s}{r}\right)r^2
$$

We take the derivative again to get a second-order differential equation that doesn't have a squared derivative:

$$
\frac{d^2 r}{d\phi^2} = \frac{1}{2} r_s - r
$$

---

For debugging, we want to use the standard Newtonian $a = -\frac{GM}{r^2}$ equation to test.

<https://github.com/tyler-a-cox/black_hole_raytracer/tree/master>
