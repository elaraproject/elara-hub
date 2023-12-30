+++
title = "Alternate differential equation solver"
+++

"New" way to solve ODEs and PDEs that DOESN'T involve complicated computations or matrices (actually not new at all, but it might have potential):

Just use Taylor series and automatic differentiation. Consider, for example, the ODE:

$$
\frac{dy}{dx} = 3y + 2x^2, y(0) = 3
$$

$$
y'' = 3y' + 4x, y' = 9
$$
$$
y''' = 3y'' + 4, y'' = 27
$$
$$
y''' = 3y''', y''=85
$$

You can construct a Taylor series around the point $x = 0$ to approximate a solution $y(x)$ of the ODE, then use automatic differentiation to compute successive derivatives to refine that approximation. This approach doesn't have numerical precision errors, and it can be made arbitrarily precise, and also doesn't require complicated grids. And it can be extended for partial differential equations - you do the same, just with the partial derivatives version of Taylor series. And this works on all analytic functions, even non-elementary functions, so it's especially well-suited to arbitrary differential equations. The solver can use the Taylor remainder theorem to automatically choose the number of terms for the approximation to be within a specified maximum error tolerance.

For the Taylor series to be maximally accurate even at points far from the source, multiple taylor series is used. A new point $x_1$ is chosen based on the most distant point from the original point $x_0$ that satisfies the maximum error as defined by the Taylor remainder theorem. Then a new taylor series is constructed from the point $x_1$. This process can be repeated multiple times to generate an effective solution for all points in the domain.

(Note: there are definitely optimizations that can be done to make the Taylor series converge faster while retaining the same algorithm for ODE/PDE solving, but that is for another time.)
