+++
title = "All about partial differential equations"
+++

A **partial differential equation** or PDE is any equation that contains a function and its partial derivatives. The general form of a PDE is:

$$
F \left(x, y, \dots, f(x, y), \dots\frac{\partial f}{\partial x}, \frac{\partial f}{\partial y}, \dots \frac{\partial^n f}{\partial x^n}, \frac{\partial^n f}{\partial y^n} \dots \right) = G(x, y, \dots)
$$
To shorten derivatives, we often use the $\nabla$ symbol, where:

$$
\nabla^2 u = \frac{\partial^2 u}{\partial x^2} + \frac{\partial^2 u}{\partial y^2} + \frac{\partial^2 u}{\partial z^2}
$$

Some specific examples of PDEs include the heat equation:

$$
\frac{\partial u}{\partial t} = \alpha^2 \nabla^2 u
$$
The wave equation:

$$
\frac{\partial^2 u}{\partial t^2} = c^2 \nabla^2 u
$$
And Poisson's equation:

$$
\nabla^2 u = f
$$
PDEs can have many, many solutions. For instance, take the PDE:

$$
\frac{\partial^2 u}{\partial x^2} = 0
$$

The general solution of this PDE is $u(x, y) = xf(y) + g(y)$. And yes, those can be any functions of $y$, whether they be $e^y$ or $y \sin(y)$ or $y + 3y - \sqrt{y}$. Thus, just like ODEs need initial conditions to give unique solutions, PDEs need **boundary conditions** to give unique solutions. The typical set of boundary conditions for a PDE are values at the edges of the domain of the PDE (e.g. $u(x, t) \to 0$ as $x \to \pm \infty$). When the PDE is dependent on time, then the boundary condition for $t \to 0$ is often called an initial condition - remember, for PDEs, initial conditions are considered a type of boundary condition.


## Numerically solving PDEs

The most common methods of solving PDEs numerically are the finite difference, finite element, and boundary element methods.

Encode derivatives as matrices - take wave equation as example. We have:

$$
\frac{\partial^2 u}{\partial t^2} = c^2\frac{\partial^2 u}{\partial x^2}
$$

We want to discretize the second partial derivative. Recall that:

$$
\frac{\partial^2 u}{\partial x^2} \approx \frac{u(x - h, t) + 2u(x, t) + u(x + h)}{h^2}
$$

Or, alternatively written:

$$
\frac{\partial^2 u}{\partial x^2} \approx \frac{u_{i - 1, j} + 2u_{i, j} + u_{i + 1, j}}{h^2}
$$

This works in all cases except at the left and right boundaries of the domain, as there is not another point to the left of the boundary to take the central difference. Instead, we use the single-sided difference - for the left, we have:

$$
\frac{2u_{i, j} - 5u_{i+ 1, j} + 4u_{i + 2, j} - u_{i+3, j}}{h^2}
$$
And for the right, we have:

$$
\frac{-u_{i-3, j} + 4u_{i - 2, j} - 5u_{i - 1, j} + 2u_{i, j}}{h^2}
$$

Note: see <https://web.media.mit.edu/~crtaylor/calculator.html> for a calculator for these values (enter `0, 1, 2, 3` for the left-handed 2nd derivative and `-3, -2, -1, 0` for the right-handed 2nd derivative).

Now, partial derivatives are linear operators, just like matrices. So we can spatially discretize the equation by turning the 2nd spatial partial derivative into a matrix $A$ acting on the solution vector $U$:

$$
\frac{\partial^2 u}{\partial x^2} = AU
$$
Where:

$$
AU=\begin{bmatrix}
2 & -5 & 4 & -1 & & & \\\\
1 & -2 & 1 & & & \\\\
& 1 & -2 & 1 & & & \\\\
& & \ddots & \ddots & \ddots & \\\\
& & & 1 & -2 & 1 & \\\\
& & -1 & 4 & -5 & 2
\end{bmatrix}
\begin{bmatrix}
u_{1, 1} \\\\
u_{1, 2} \\\\
\vdots \\\\
u_{1, n - 1} \\\\
u_{1, n} \\\\
u_{2, 1} \\\\
\vdots \\\\
u_{m, n}
\end{bmatrix}
= \begin{bmatrix}
2u_{1, 1} -5 u_{1, 2} + 4 u_{1, 3} - u_{1, 4} \\\\
u_{1, 3} - 2u_{1, 2} + u_{1, 1} \\\\
u_{1, 4} - 2u_{1, 3} + u_{1, 2} \\\\
\vdots \\
u_{i, 1} - 5u_{i, 2} + 4u_{i, 3} - u_{i, 4} \\\\
u_{i, 3} - 2u_{i, 2} + u_{i, 1} \\\\
\vdots \\
u_{i, n} - 2u_{i, n - 1} + u_{i, n - 1} \\\\
-u_{i, n - 3} + 4u_{i, n - 2} -5u_{i, n - 1} + 2u_{i, n}
\end{bmatrix}
$$

By discretizing the spatial derivative, the partial time derivative simply becomes an ordinary time derivative. Thus we can rewrite as:

$$
U'' = c^2 AU
$$
Which is an ordinary differential equation that can be solved with conventional ODE solvers. The only thing left is to compute an initial condition $U_0(x)$ and $U_0'(x)$ (if you know the initial condition you can differentiate it to get the initial time derivative).

This method works well even in the higher-dimensional case. For instance, consider the generalized wave equation:

$$
\frac{\partial^2 u}{\partial t^2} = c^2 \nabla^2 u
$$
By encoding the Laplacian $\nabla^2 u$ as a matrix $A$ acting on $U$, the same approach can be used as with the other approaches previously mentioned. In fact, this approach works for all linear partial differential equations that have one time and one or several space derivatives.

For other cases of linear PDEs that are not time-based, such as Poisson's or Laplace's equation, they can be generally written in the form:

$$
AU = B
$$
And solved using standard linear algebra techniques.

Finally, for nonlinear PDEs, they can be generally written in the form:

$$
AU - f(U) = 0
$$

Which is a rootfinding problem that can be solved with Newton's method:

$$
U_{i + 1} = U_{i} - J^{-1}U(x_0)
$$
## Sources

- <https://www.diva-portal.org/smash/get/diva2:1438836/FULLTEXT01.pdf>
- <https://youtu.be/hxGA1Je1P-s>
- <https://github.com/lukepolson/youtube_channel/blob/main/Python%20GPU/schrodinger.ipynb>
- <https://gist.github.com/c0rychu/6fc08cf5cdec2cc78a1f9334d103a869#file-schrodingereq_1d_tutorial-ipynb>
- <https://mathematica.stackexchange.com/questions/221207/finite-difference-method-for-1d-wave-equation>
- <https://nextjournal.com/sosiris-de/pde-2018>
- <https://empossible.net/wp-content/uploads/2019/08/Lecture-4a-Finite-Difference-Method.pdf>
- <https://aquaulb.github.io/book_solving_pde_mooc/solving_pde_mooc/notebooks/01_Introduction/01_00_Preface.html>

## Elara-math integration

In the future, the differential equation solver RK4 will be ported from `elara-array` to `elara-math`. `elara-math` will then have two differential equations solver classes - `odesolve`, which solves (singular or systems of) ordinary differential equations, and `pdesolve` for (singular or systems of) partial differential equations. Its API is to be inspired by Mathematica's `ndsolve` - see https://reference.wolfram.com/language/ref/NDSolve.html

The proposed numerical API:

```python
h = 0.1
n = 100
c = 1

# Auto-compute sparse matrix
# for ∂2u/∂x^2
# with 2nd-order approximation to second-order derivative
diff_x = CentralDifference(2, order=2, step_size=h, grid_size=n)

# Solve as ODE, etc. etc.
# ... 
```

The proposed symbolic API:

```python
def pde(c=1):
	return D(D(u, t), t) - c ** 2 * D(D(u, x), x)

solver = PDEsolve(pde, bc="Dirichlet")
u = solver.compute(grid="disk")

plt.title("Wave equation results")
plt.imshow(u)
plt.show()
```


## Solving vector differential equations

To solve vector differential equations, it is necessary to break up the vector differential equation into its components, and then solve for each of the components. For example, the first two of Maxwell's equations result in 2 trivial equations. The second two are more complex, as the curl of a vector field produces another vector field. Thus, we must instead rewrite the curl component-by-component. The complete equations are:

$$
\frac{\partial E_x}{\partial x} + \frac{\partial E_y}{\partial y} + \frac{\partial E_z}{\partial z} = \frac{\rho}{\epsilon_0}
$$
$$
\frac{\partial B_x}{\partial x} + \frac{\partial B_y}{\partial y} + \frac{\partial B_z}{\partial z} = 0
$$
$$
\frac{\partial E_z}{\partial y} - \frac{\partial E_y}{\partial z} = -\frac{\partial B_x}{\partial t}
$$
$$
\frac{\partial E_x}{\partial z} - \frac{\partial E_z}{\partial x} = -\frac{\partial B_y}{\partial t}
$$
$$
\frac{\partial E_y}{\partial x} - \frac{\partial E_x}{\partial y} = -\frac{\partial B_z}{\partial t}
$$
$$
\frac{\partial B_z}{\partial y} - \frac{\partial B_y}{\partial z} = \mu_0 \left(J_x + \epsilon_0 \frac{\partial E_x}{\partial t}\right)
$$
$$
\frac{\partial B_x}{\partial z} - \frac{\partial B_z}{\partial x} = \mu_0 \left(J_y + \epsilon_0 \frac{\partial E_y}{\partial t}\right)
$$
$$
\frac{\partial B_y}{\partial x} - \frac{\partial B_x}{\partial y} =\mu_0 \left(J_z + \epsilon_0 \frac{\partial E_z}{\partial t}\right)
$$

So Maxwell's equations result in 8 coupled PDEs. However, only 6 of them are independent - any system that satisfies the last 6 PDEs also must satisfy the first 2, so long as the boundary conditions satisfy the conservation of charge.

Here, note that for a solution, the current density must be set, which consists of one function each for $J_x, J_y, J_z$.

## Common PDEs

Wave equation:

$$
\frac{\partial^2 u}{\partial t^2} = c^2 \nabla^2 u
$$

Heat/diffusion equation:

$$
\frac{\partial T}{\partial t} = \alpha^2 \nabla^2 T
$$

Laplace's equation:

$$
\nabla^2 u = 0
$$
Poisson's equation:

$$
\nabla^2 u = f(u)
$$

Burger's equation:

$$
\frac{\partial u}{\partial t} + c \frac{\partial u}{\partial x} = \nu \frac{\partial^2 u}{\partial x^2}
$$

Continuity equation:

$$
\frac{\partial \rho}{\partial t} + \nabla \cdot u = 0
$$

Navier-Stokes:

$$
\nabla \cdot u = 0
$$
$$
\rho \left(\frac{\partial u}{\partial t} + (u \cdot \nabla) u\right) = -\nabla p + \mu \nabla^2 u + \rho g
$$
Maxwell's equations:

$$
\nabla \cdot E = \frac{\rho}{\epsilon_0}
$$
$$
\nabla \cdot B = 0
$$
$$
\nabla \times E = -\frac{\partial B}{\partial t}
$$
$$
\nabla \times B = \mu_0 \left(J + \epsilon_0 \frac{\partial E}{\partial t}\right)
$$
Einstein's field equations:

$$
G_{\mu \nu} = \frac{8\pi G}{c^4} T_{\mu \nu}
$$

Equations of quantum mechanics:

- Schrödinger equation
- Pauli equation
- Klein-Gordon equation
- Dirac equation

Euler-Lagrange equations, Hamilton's equations

Black-Scholes equation
