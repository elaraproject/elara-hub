+++
title = "Multivariable calculus guide"
+++

## Partial derivatives

Partial derivatives differentiate only to the given variable while holding all other variables constant. They can be noted with $\frac{\partial f}{\partial x}$ or $f_x$. The formal definition of the partial derivative in 2D is:

$$
\frac{\partial f}{\partial x} = \lim_{h \to 0} \frac{f(x + h, y) - f(x, y)}{h}
$$

## Mixed partial derivatives

Second partial derivatives are denoted by $\frac{\partial^2 f}{\partial x^2}$ or $f_{xx}$. When using the "del" notation (with the $\partial$ symbol) you take derivatives from right to left order; when using the subscript notation, you take derivatives from left to right order.

Second partial derivatives **commute**, which means the order you take them doesn't matter. Thus $\frac{\partial f}{\partial x \partial y} = \frac{\partial f}{\partial y \partial x}$.

## The nabla symbol

The nabla symbol $\nabla$ represents a "vector" of partial derivative operators:

$$
\nabla = \left\langle \frac{\partial}{\partial x}, \frac{\partial}{\partial y}, \frac{\partial}{\partial z} \right\rangle
$$

## Scalar-valued functions

A scalar-valued function is a function that always outputs a number for each input, not a vector, such as $f(x, y) = 2x + 3y^2$.

## Gradients

The gradient of a scalar-valued function, denoted $\nabla f$, is given by a vector containing all the partial derivatives of the function. In two dimensions, it is given by:

$$
\nabla f(x, y) = \left\langle \frac{\partial f}{\partial x}, \frac{\partial f}{\partial y} \right\rangle
$$

In three dimensions, it is similarly given by:

$$
\nabla f(x, y, z) = \left\langle \frac{\partial f}{\partial x}, \frac{\partial f}{\partial y}, \frac{\partial f}{\partial z} \right\rangle
$$

The gradient can be evaluated at a point $(x, y, z)$ by evaluating each of the partial derivatives at that point

## Directional derivative

The directional derivative is the rate of change along a specific vector $\vec v$ and is given by:

$$
\nabla_{\vec v} \, f = \nabla f \cdot \vec v
$$

## Multivariable chain rule

If we have a function $f(g(t))$, then:

$$
\frac{df}{dt} = \frac{\partial f}{\partial x} \frac{dx}{dt} + \frac{\partial f}{\partial y} \frac{dt}{dt} = \nabla f(g(t)) \cdot g'(t)
$$

## Parametric surfaces

Just as there can be surfaces formed by functions of $x$ and $y$, there can be parametric surfaces formed by functions of $s$ and $t$. They are denoted $f(s, t)$ and can be differentiated in the same way as normal surfaces, i.e. $\frac{\partial f}{\partial s}$ and $\frac{\partial f}{\partial t}$.

## Vector-valued functions

Vector-valued functions produce a vector for each input, for instance:

$$
\vec f(t) = 
\begin{bmatrix}
t^2 + 2t \\
\sin(2t) + t
\end{bmatrix}
$$
Note that vector-valued functions can have components that are functions of $x, y, z$, or functions of $t$, in which case its components are **parametric functions**.

The derivative of a vector-valued function is another vector in all cases. For example, velocity is often given by a vector-valued function where:

$$
\vec v(t) = \begin{bmatrix}
v_x(t) \\
v_y(t) \\
v_z(t)
\end{bmatrix} = \begin{bmatrix}
x'(t) \\
y'(t) \\
z'(t)
\end{bmatrix}
$$

Speed is given by the norm of the velocity function:

$$
v = \sqrt{v_x^2 + v_y^2 + v_z^2}
$$
If the vector-valued function is of one variable, e.g. $\vec v(t)$, then its derivative is a regular derivative $\frac{d \vec v}{dt}$. If the vector-valued function is of several variables, e.g. $\vec v(s, t)$, then it has one partial derivative for each variable, e.g. $\frac{\partial \vec v}{\partial s}$ and $\frac{\partial \vec v}{\partial t}$.

The same methodology for vector-valued functions applies to vector fields.

## Curvature

The curvature $\kappa$ of a vector-valued function $\vec v(t)$ is given by:

$$
\kappa = \frac{\|d \vec T\|}{\|ds\|} = \frac{\vec v'(t)}{\| v'(t)\|}
$$
where the double bars indicate normalizing the vector.
## Vector fields and scalar fields

Fields are mathematical objects that span space and have a unique value at each point in space. Familiar examples include the electromagnetic field and gravitational field. They are written with the same notation as functions, just usually with a different symbol such as $E$, $B$, $g$ or $\phi$.

Scalar fields, such as temperature, assign a number to each point in space. Vector fields assign a vector to each point in space. A gradient is only defined for a scalar field - the vector-field equivalent of the gradient for vector fields is called the Jacobian, and will be covered later.

## Divergence

Divergence, denoted by $\nabla \cdot \vec F$, represents the tendency of a vector field to flow out or in to a certain point:

- If vectors that point inwards towards a certain point are bigger than vectors that point outwards at that point, the divergence is **negative** and we have a **sink**
- If vectors that point inwards towards a certain point are smaller than vectors that point outwards at that point, the divergence is **positive** and we have a **source**
- If vectors that point inwards and vectors that point outwards at a certain point are equal in size, the divergence is **zero**

The divergence of a vector field $\vec F$ with components $\langle F_x, F_y, F_z\rangle$ is given by:

$$
\nabla \cdot \vec F = \frac{\partial F_x}{\partial x} + \frac{\partial F_y}{\partial y} + \frac{\partial F_z}{\partial z}
$$
Divergence is **only** defined on vector fields, not on scalar fields.

## Curl

Curl, denoted by $\nabla \times \vec F$, represents the tendency of a vector field to rotate around a certain point:

- Positive curl is a tendency of vectors to rotate counterclockwise around a point
- Negative curl is a tendency of vectors to rotate clockwise around a point
- Zero curl is when counterclockwise and clockwise vectors cancel out or if the vectors do not rotate around a point

In 2D, the curl of a vector field is given by:

$$
\nabla \times \vec F = \frac{\partial F_y}{\partial x} - \frac{\partial F_x}{\partial y}
$$

In 3D, the curl of a vector field is the determinant of a 3 x 3 matrix:

$$
\nabla \times \vec F = 
\begin{vmatrix}
\hat i & \hat j & \hat k \\
\frac{\partial}{\partial x} & \frac{\partial}{\partial y} & \frac{\partial}{\partial z} \\
F_x & F_y & F_z
\end{vmatrix}
$$

## Laplacian

The Laplacian, denoted by $\nabla^2 f$, is a vector field formed from the second derivatives of a scalar field. It is given by:

$$
\nabla^2 f = \frac{\partial^2 f}{\partial x^2} + \frac{\partial^2 f}{\partial y^2} + \frac{\partial^2 f}{\partial z^2}
$$

A function is called **harmonic** if it satisfies $\nabla^2 f = 0$. This is a partial differential equation called **Laplace's equation**.

## Jacobian

The Jacobian is the analogue of the gradient for vector-valued functions. For a vector-valued function $\vec F$ with components $\langle F_x, F_y \rangle$, the Jacobian is given by:

$$
J = 
\begin{bmatrix}
\nabla F_x \\
\nabla F_y
\end{bmatrix} =
\begin{bmatrix}
\frac{\partial F_x}{\partial x} & \frac{\partial F_x}{\partial y} \\
\frac{\partial F_y}{\partial x} & \frac{\partial F_y}{\partial y}
\end{bmatrix}
$$
For a vector valued function $\vec F$ of three dimensions, the Jacobian is given by:

$$
J =
\begin{bmatrix}
\frac{\partial F_x}{\partial x} & \frac{\partial F_x}{\partial y} & \frac{\partial F_x}{\partial z} \\
\frac{\partial F_y}{\partial x} & \frac{\partial F_y}{\partial y} & \frac{\partial F_y}{\partial z} \\
\frac{\partial F_z}{\partial x} & \frac{\partial F_z}{\partial y} & \frac{\partial F_z}{\partial z}
\end{bmatrix}
$$
The determinant of the Jacobian is calculated just like the determinant of any other matrix. The absolute value of the determinant of the Jacobian tells us about how areas change area around a point:

- If the determinant is 1, the area remains the same area
- If the determinant is less than 1, the area contracts
- If the determinant is greater than 1, the area expands
- If the determinant is 0, the area contracts infinitely

## Tangent plane

The tangent plane is the local **linear approximation** to a function at a certain point. It is often used to simplify a complicated function when the area being studied is around a certain point.

## Tangent plane (explicit functions)

The tangent plane to a function $f(x, y)$ at a point $(x_0, y_0)$ is given by:

$$
T(x, y) = f(x_0, y_0) + f_x (x_0, y_0) (x - x_0) + f_y (x_0, y_0) (y - y_0)
$$
## Tangent plane (implicit functions)

The implicit tangent plane to a function $f(x, y, z) = 0$ at a point $(x_0, y_0, z_0)$ is given by:

$$
f_x (x - x_0) + f_y(y - y_0) + f_z (z - z_0) = 0
$$
