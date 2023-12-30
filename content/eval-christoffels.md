+++
title = "Quickly evaluating the Christoffel symbols"
+++

Original reference: <https://properphysics.wordpress.com/2014/08/02/christoffel-symbols-and-the-geodesic-equation-the-easy-way/comment-page-1/>

Assume we want to evaluate the Christoffel symbols for the metric:

$$
ds^2 = dr^2 + r^2 d \theta^2 + r^2 \sin^2 \theta d\phi^2
$$

To do this, recall that the Lagrangian of a particle moving along a certain geodesic is given by:

$$
\mathcal{L} = g_{\mu \nu} \frac{dx^\mu}{d\lambda} \frac{dx^\nu}{d\lambda}
$$
(Yes, it's supposed to have a square root, but since the Lagrangian is a constant of motion you can square it and get the same result)

Recall also that the line element is given by:

$$
ds^2 = g_{\mu \nu} dx^\mu dx^\nu
$$

So the Lagrangian of a free particle can simply be written:

$$
\mathcal{L} = \frac{d}{d\lambda} (ds^2)
$$

We can replace the derivative to an affine parameter to one with respect to (proper) time for practical purposes. Doing this, we find the Lagrangian for the spherical metric is:

$$
\mathcal{L} = \dot r^2 + r^2 \dot \theta^2  + r^2 \sin^2 \theta \dot \phi^2
$$

We can then plug this Lagrangian into the Euler-Lagrange equations:

$$
\frac{d}{d\lambda} \frac{\partial \mathcal{L}}{\partial \dot x} = \frac{\partial \mathcal{L}}{\partial x}
$$

to get (one equation each for $r$, $\theta$ and $\phi$). Note here that we assume each coordinate is a function of the other coordinates, that is $r = r(\theta, \phi), \theta = \theta(r, \phi), \phi = \phi(r, \theta)$, so chain rule/implicit differentiation is necessary. Doing all the steps, and without simplifying, the result is:

$$
2 \ddot r = 2r \dot \theta^2 + 2r \sin^2 \theta \dot \phi^2
$$
$$
2r^2 \ddot \theta = 2r \frac{dr}{d\theta}  \dot \theta^2 +2r^2 \sin \theta \cos \theta \dot \phi^2
$$
$$
2 r^2 \sin^2 \theta \ddot \phi = 2r \sin^2 \theta \frac{dr}{d\phi} \dot \phi^2 +2r^2 \sin \theta \cos \theta \frac{d\theta}{d\phi} \dot \phi^2
$$

We can do a simplification here - note that:

$$
\frac{dr}{d\theta} \dot \theta^2 = \frac{dr}{d\theta} \frac{d\theta}{d\tau}\frac{d\theta}{d\tau} = \dot r  \dot \theta 
$$

Similarly:

$$
\frac{d\theta}{d\phi} \dot \phi^2 = \frac{d\theta}{d\phi} \frac{d\phi}{d\tau} \frac{d\phi}{d\tau} = \dot \theta \dot \phi
$$

And the same is true for $\frac{dr}{d\phi} \dot \theta^2$. So if we use these simplifications and also cancel out common factors, the geodesic equations are:

$$
\ddot r = r \dot \theta^2 + r \sin^2 \theta \dot \phi^2
$$
$$
\ddot \theta = \frac{1}{r} \dot r \dot \theta + \sin \theta \cos \theta \dot \phi^2
$$
$$
\ddot \phi = \frac{1}{r} \dot r \dot \phi + \cot \theta \dot \theta \dot \phi
$$

If you wanted to find the geodesic equations - tada! You've already got them. If you want to find the Christoffel symbols though, that's just one additional step - just read them off the geodesic equations. For example, the first geodesic equation (if we simplify it) is:

$$
\ddot r = r \dot \theta^2 + r \sin^2 \theta \dot \phi^2
$$
Compare this to:

$$
\ddot x^i = -\Gamma^i_{jk} \dot x^j \dot x^k
$$
Note that we have an implicit sum going over the indices $j$ and $k$ here due to the Einstein summation convention. So really, this expands out to:

$$
\ddot r = \ddot x^i = -\sum_{i = 0}^n \sum_{j = 0}^n \Gamma^i_{jk} \dot x^j \dot x^k
$$

So $\Gamma^r_{\theta \theta} = -r$, $\Gamma^r_{\phi \phi} = -r\sin^2 \theta$ from the first geodesic equation,  $\Gamma^\theta_{r \theta} = \frac{1}{r}$, $\Gamma^\theta_{\phi \phi} = -\sin \theta \cos \theta$ from the second, and $\Gamma^\phi_{r \phi} = \frac{1}{r}$, $\Gamma^\phi_{\theta \phi} = \cot \theta$ from the third.
