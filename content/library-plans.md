+++
title = "Project Elara library plans"
+++

## All libraries

- Implement [custom error types](https://learning-rust.github.io/docs/custom-error-types/) (this is already being implemented in `elara-gfx` pretty well) following https://mmapped.blog/posts/12-rust-error-handling.html - the correct approach is **small enums** of error categories rather than one humongous global error enum
- Move to https://codeberg.org/ for hosting in the future and keep github as mirrors

## elara-astro

<https://crates.io/crates/nyx-space>

## elara-md

`elaramd` is a self-contained markdown to PDF converter for project elara. This converter skips the HTML generation step and directly takes markdown (with embedded LaTeX equations) and converts it to a styled PDF in the Elara Project style. This converter should include some common themes - academic (research paper), article, technical, etc. so that future project elara papers can be written in pure markdown instead of needing to be written in LaTeX.

## elara-presenter

Ideal presentation tool for project elara - takahashi method, write slides in markdown, the tool allows presenting the slides, like sent from suckless or <https://ia.net/presenter>. The tool should include markup for:

- Text slides
	- Heading-only slides
	- Text-only slides
	- Label and text slide (image is 100% height and text takes up remaining room)
- Whole page images
- Code slides
- KaTeX math slides
- Good design for the slide themes

## elara-ui

- Make an analogue of <https://sindresorhus.com/plain-text-editor>
- Make a gui version of <https://pypi.org/project/share-file-qr/>
- Make a previewer/slide shower based on <https://en.wikipedia.org/wiki/Takahashi_method> like `sent` from the suckless project
- Make a basic demo terminal based on <https://github.com/dhanoosu/TkTerm>

Follow tips from <https://2d.graphics/book/>

Tips for fast GPU rendering:

- Only render on user input/interaction, otherwise don't re-render frames (no immediate-mode UI that re-renders every frame)
- Only re-render changed areas ("damage tracking")
	- Save last render to a texture in memory and load that texture on next frame, cropping out the region that was changed
- Must render on GPU
- For repeated elements (e.g. long lists or tree views) caching is necessary

design stuff:

For elaraui components - implement all the components that egui has in its demo

Cool apps to maybe port one day to `elara-ui` as demo applications:

- <https://github.com/FPurchess/blank>
- <https://github.com/adileo/squirreldisk>
- <https://github.com/spacedriveapp/spacedrive>
- <https://github.com/kkoomen/pointless>

## elara-math

Implement the following:

- Adam optimizer

Current progress: see [[Elara math optimization plan]](@/elara-math-optimization.md)

In the future, do a "blank rewrite" - rewriting the entire library without looking at any reference github source code. Doing this often will help improve the code quality.

Autodiff implementations to look at:

- <https://github.com/ziap/autodiff/>
- <https://github.com/matiasbattocchia/simple-grad>
- <https://github.com/conradludgate/autograd-rs/tree/main>
- (Nearly 1-1 reimplementation of Jax in Rust) <https://getcode.substack.com/p/beyond-backpropagation-higher-order>

In the future elara-math should have a second functional API like Jax's that will be the primary API. This functional API has a `elara_math::grad()`function that transforms one function into a corresponding function for its derivative. Base this on how https://github.com/HIPS/autograd implements it. This is bc the current PyTorch-inspired API is basically only good for neural networks and wastes computational time creating graphs. One similar rust implementation to reference is https://github.com/ibab/rust-ad/blob/master/src/lib.rs.

Implement sparse matrices: <https://crates.io/crates/sprs>

- [x] Also add a numerical quadrature feature to `elara-math` to compute integrals:
- [ ] Implement simpson's rule and Gauss-Konrod rule based on https://github.com/esa/torchquad for 1D case, Monte-Carlo integration for N-D case, as well as Trapezoidal and Romberg quadrature for integrating over discrete arrays
- [ ] Add automatic integration - this is like automatic differentiation, only it takes a function and integrates its nth Taylor polynomial (which can be found exactly) - since unlike typical numerical integration it can be done to machine precision and thus doesn't suffer from numerical precision errors, it could be far more precise

<https://crates.io/crates/quad-rs/0.1.2>

Also learn matrix calculus for `elara-math` from https://www.lesswrong.com/s/nMGrhBYXWjPhZoyNL/p/9L9XuXhLYBm47yYkf

Also eventually create a tensor algebra system like cadabra: <https://cadabra.science/>

Also add a monte-carlo solver for ODEs: <https://jotterbach.github.io/content/posts/mc_ode/2018-08-08-MonteCarloODE/>

## elara-geo

Rust reimplementation of <https://github.com/tauzero7/GeodesicViewer>

## elara-DE

(note: awaiting better name)

Make a specialized elara tool to plot slope fields for ODEs, and numerically solve (systems of or singular) ODEs/PDEs without needing any code (like a desmos for differential equations). So the user can just enter a differential equation symbolically and the initial/boundary conditions, and then it'll be solved automatically. Has both a PINN (neural-network based) solver and a conventional grid-based solver.

## elara-cas

(note: awaiting better name)

Elara CAS will use integral/diff eq solver from <https://arxiv.org/abs/1912.01412>

UI inspired by <https://github.com/bornova/numara-calculator>

## elara-ml

Note: Elara ML is responsible for implementing the higher-level constructs for efficient machine learning (e.g. dense layers, convolution layers, model architectures, pretrained models, etc.) while Elara Math handles the underlying computations.

<https://github.com/utility-code/tinyDL>

## elara-gfx

See [[Elara GFX continuing work]](@/elara-gfx-continuing-work.md)

Fixes:

- [x] Fix the issue of the text rendering coordinates
- [x] Unified coordinate system for all primitives

Features:

- [x] Preliminary UI rendering
- [x] Save OpenGL rendering to image file for headless rendering (see [this](https://lencerf.github.io/post/2019-09-21-save-the-opengl-rendering-to-image-file/))
- [ ] For a demo of both `elara-array` and `elara-gfx` - port the following pathtracers to Rust using only the project elara libraries:
	- [ ] <https://github.com/li-plus/tinypt>
	- [ ] <http://www.kevinbeason.com/smallpt/>
	- [ ] <https://github.com/vkoskiv/c-ray>
	- [ ] The [raytracing series](https://raytracing.github.io/)

Look at https://lodev.org/lodepng/ and port the 500-line `picoPNG` into a Rust version for `elara-gfx`.

Implement several libraries on top of elara-gfx:

- elara-draw: Fabric.js-like library on top of elara-gfx, see <http://fabricjs.com/fabric-intro-part-1>
- elara-ui: UI library on top of elara-gfx

<https://zed.dev/blog/videogame>

## elara-array

More details in [[Elara-array API plan]](@/elara-array-api-plan.md)

Implement the following:

- [x] Working differential equations solvers
- [x] Working pretty-print for NdArrays
- [x] Working 1D pretty print
- [ ] Python bindings (referencing tinynumpy for the code)
- [ ] Nanoserde serialization to JSON for NumPy bridge
- [ ] Tests for the library
- [ ] Replace ndarray-rand with nanorand

## elara-plot

Can plot 2D real and complex-valued functions, 3D functions, and 4D functions, as well as their field equivalents.

Can also plot parametric lines and parametric surfaces, as well as vector-valued functions and vector fields

Visualizing 4D functions $f(\mathbb{R}^3) \to \mathbb{R}^4$: assign a color to each point in 3D space (density plots), such as:

- <https://www.math.brown.edu/tbanchof/multivarcalc2/multivarcalc3-1.html>
- <http://www.falstad.com/qmatom/>
