+++
title = "Project Elara app plans"
+++

Note: all of these apps follow a core-frontend architecture. That is, their entire functionalities are exposed via a *core library* (e.g. `libde` for Elara DE and `libgeod` for Elara Geo), and the frontend is just the UI. 

Project Elara apps have UIs but many have an additional python REPL with a live preview window by default. The entire app's functions is accessible via REPL thanks to the core-frontend architecture.

## elara-geo

Rust reimplementation of <https://github.com/tauzero7/GeodesicViewer>

## elara-DE

Make automated ODE and PDE solver. It should have the following features:

- Handle either single DE or system of DEs of any order
- Support multivariable or single-variable scalar, vector, or tensor-valued function (or field) to solve for, as well as real or complex arguments
- Enter in ODE or PDE as with as boundary/initial conditions symbolically (with SymPy-like syntax)
- (Optional) compare solution to manually-inputted analytical solution
- GPU acceleration for fast solving
- Slope fields for first-order ODEs
- Built-in visualization tools for solutions (defaults to lines for single-variable functions, heatmaps for two-variable (3D) functions, and density plots for three-variable (4D) functions, with the colors automatically scaled based on max intensity and the opacity of each pixel scaled based on the inputted grid density)
- Has both a PINN (neural-network based) solver and a conventional grid-based solver.
Elara DE's planned API:

```rust
// example DE solving
x, y, z = var!(x, y, z);
k = const!(k = 5);

// ODE example
let f = DE::fn("f", x);
let dfdx = diff(f, x);
let eq = diffeq![dfdx = 3 * k * y];
eq.set_initial();
eq.dsolve();
```

## elara-cas

(note: awaiting better name)

elara-cas is a Qalculate replacement, with LaTeX printing output, a syntax-highlighted REPL, and an optional visual editor for inserting complicating expressions. 

Elara CAS will use integral/diff eq solver from <https://arxiv.org/abs/1912.01412>

UI inspired by <https://github.com/bornova/numara-calculator>

This should be the syntax:

```rust
// Basic calculations (these are done symbolically)
1 + 1

// Declaring variables
var x, y, z

// Declaring constants
const G, M := 2e30, m := 1, c := 3e8

// Declaring expressions
// note the UI includes an optional MathQuill-style visual editor
// to insert expressions (which it then outputs with the CAS
// language) for convenience
schwarzschild = (2 * G * M) / c^2

// Declaring functions
// Note the same MathQuill-style visual editor is available
u(x) // function prototype (useful for differential equations)
g(x) = 3 * x // scalar-valued single variable
f(x, y, z) = 3 * x^2 + 5 * y + z // scalar-valued multivariable
h(x, y) = (5 * x + 4, 7 * x + 8) // vector-valued

// Plot a function
// in the UI you can choose the domain and range
// with sliders and pan/zoom on the graph
plot f

// Evaluating functions
// in the UI you can choose between a numerical answer (e.g. 5.114342)
// or a symbolic answer (e.g. sqrt(2) / 2)
// for numerical answers, any constants will be evaluated with their
// assigned numerical values
f(1, 4, 3)

// Taking derivatives without explicit evaluation
dudx = diff u // here x doesn't need to be specified as it is the only variable

// Taking derivatives
dfdx = diff f, x
dfdx(2, 6, 1)

// Taking the gradient
grad_f = grad f
grad_f(2, 6, 1)

// Taking the indefinite integral symbolicallly
// This uses a neural network solver under the hood
int_f = integrate f, x
// you can then evaluate the definite integral with
int_f(6) - int_f(1)

// Taking the definite integral symbolically
integrate f, x, 1, 6

// Taking the definite integral
// When doing so, the answer is computed
// exclusively numerically
quadrature f, x, 0, 5

// Create equation
myeq = eq 5 * x^2 + 8 * x, 78 * x + 5

// Solve equation
// in UI there is the option to solve
// symbolically or numerically
solve myeq, x

// You can also use this as a lazy
// expression rearranger
sch_radius_eq = eq r_s, schwarzschild
solve sch_radius_eq, M // rearrange to get mass in terms of r_s

// Substitution
subs myeq, x, 5 * y

// Using ans
5 * x^2 + 7 * sin(x)
mynewexpr = ans

// Note substitution is always symbolic
// If you want to evaluate an expression
// (not a function) then use eval
// In eval you don't need to sub in the
// value of any constants, just the variables
eval 2 * M * x^2 + 3 * t, { x = 6, t = 0.1 }

// Solving ordinary differential equations
// (can solve 1st or 2nd order ODEs)
// This uses a neural network solver under the hood
mydiffeq = eq dudx, 3 * u * x
dsolve eq // symbolic solver
ndsolve eq // numerical solver - in UI there is option to show graph

// Note however that Elara CAS is not suited
// to complex numerical work or PDE solving,
// both of which Elara DE specialize in
```

The very minimalist syntax only works because you cannot nest built-in functions like `diff` and `integrate` within other functions, and those functions are delimitated by the end of the line.

## Elara Studio

For custom project elara test path tracer - you first export the UV unwrapped 3D models from your modeling tool, then in the path tracer GUI you can add PBR materials and lights and render. This path tracer BTW is very non essential, as the project has literally no need of a path tracer (we can just use Blender), so really it is just a proof of project technology.

However this can be an alternative workflow - if the app provided a big built-in asset library (and one the user can add to) with things like materials, textures, lighting setups, models and so forth, then the renderer might actually be a good idea.
