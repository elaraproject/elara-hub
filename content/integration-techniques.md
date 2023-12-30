+++
title = "Integration techniques"
+++

Many physics problems encountered in Project Elara require the use of integration to solve. These are some common techniques for integration:

- Elementary integrals (which are just the derivative rules in reverse)
- U-substitution
- Integration by parts
	- Exhaustive integration by parts (DI method)
	- Integration by parts cancellation trick
- Symmetries: if $f$ is an odd function, then:

$$
\int_{-a}^a f(x) dx = 0
$$

- Multiplying by conjugate:

$$
\int \frac{1}{1 + \cos x} dx
$$
$$
\int \frac{1}{1 + \cos x} \frac{1 - \cos x}{1 - \cos x} dx
$$
$$
\int \frac{1 - \cos x}{1 - \cos^2 x} dx
$$
$$
\int \frac{1 - \cos x}{\sin^2 x} dx
$$
$$
\int \csc^2 x dx - \int \frac{\cos x}{\sin^2 x} dx
$$
For first integral - notice that this is just equal to $-\cot x + C$.

For the second integral - let $u = \sin x$, then:

$$
\int \frac{\cos x}{\sin^2 x} dx = \int \frac{1}{u^2} du = -\frac{1}{u} + C
$$

So the solution is:

$$
-\cot x + \csc x + C
$$

- Using trig identities (particularly Pythagorean ones) - e.g.

$$
\int \sin^2 x dx = \int \frac{1 - \cos (2x)}{2}dx = \frac{1}{2} x - \frac{1}{4} \cos (2x) + C
$$

- Integration by partial fractions
- Long division of integrand
- Expanding the integrand - e.g.

$$
\int (1 + x^2)^2 dx = \int 1+ 2x^2 + x^4 dx = x + \frac{2}{3} x^3 + \frac{1}{5} x^5 + C
$$

- Cancelling out common factors
- Dividing out - e.g.

$$
\int \frac{1}{\sqrt{16 - 81x^2}} dx = \int \frac{1}{\sqrt{16(1 - (81x^2 / 16))}} = \int \frac{1}{4\sqrt{1 - (9x/4)^2}}
$$


- Completing the square
- Splitting one integral into multiple simpler integrals (fraction-splitting)

For fractions:

$$
\int \frac{1 + x}{x^2} dx = \int \frac{1}{x^2} dx + \int \frac{1}{x} dx = -\frac{1}{x} + \ln |x| + C
$$

- Trig sub
- Integration via geometry (especially for circles, rectangles, and triangles)
- Feynmann integration trick (only for definite integrals)
