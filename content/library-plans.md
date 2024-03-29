+++
title = "Project Elara library plans"
+++

## All libraries

- Implement [custom error types](https://learning-rust.github.io/docs/custom-error-types/) (this is already being implemented in `elara-gfx` pretty well) following <https://mmapped.blog/posts/12-rust-error-handling.html> - the correct approach is **small enums** of error categories rather than one humongous global error enum
- Move to [Codeberg](https://codeberg.org/) for hosting in the future and keep github as mirrors
- All libraries that currently depend on `elara-log` should be able to make `elara-log` an optional dependency behind the `logging` feature flag. If disabled, they will simply use `println!` instead.

### Standard library architecture

Above all else, project elara libraries (not apps!) should aim for minimalism and simplicity, in the style of [tinygrad](https://github.com/tinygrad/tinygrad). Where more features and more code isn't necessary, it's not written, or it's an optional feature. This does mean users need to write more code to compensate for things libraries don't themselves do, but it also gives the user a lot more control. The idea is the project elara libraries should never be giant megaframeworks like React.js or TensorFlow.

To do this, project elara libraries rely on the idea of implementation-agnostic code and backends. E.g. `elara-ui` does not come with a backend. There is, however, a few simple lines of boilerplate code that users can easily add to use `elara-gfx` as the backend. This allows `elara-ui` to have no dependencies (other than the optional logger). Usage would be like this:

```rust
use elara_ui::prelude::*;
use elara_gfx::prelude::*;

fn main() {
	// elara-gfx boilerplate...
	let rect_renderer = RectRenderer::new();
	let line_renderer = LineRenderer::new();
	let text_renderer = TextRenderer::new();

	let ui = UI::new();
	ui.set_layout(5, 5, 5, 8);
	label = Label::new("Hi!");
	ui.add_element(label)
	// ui stuff...

	let frame = ui.render();
	for el in frame.iter() {
		match el {
			Component::Button => {
				text_renderer.draw([10, 5], 50, 0, 5);
				line_renderer.draw(50, 60, 1, false);
				// etc.
			}
		}
	}
}
```

Similarly, `elara-ml` doesn't have a backend either (though it does have an optional feature flag that gives it a CPU backend implementation that uses both `elara-math` and `elara-array`). The library docs give more examples of possible implementations, and custom backends can be written for `elara-ml` as well, which would probably be useful for custom machine learning hardware in the future.

`elara-math` also doesn't have a backend - this allows its autodiff, integration, diff eq solvers, and special functions to work regardless of what backend provides the backing array.

The only library for which this mostly doesn't apply is `elara-array`, which comes with a built-in CPU and GPU backend by default (though the GPU backend is behind an optional feature flag). This is because `elara-array` itself is a very minimal library anyways, and all it implements is a n-dimensional array that supports elementwise operations. In addition, its CPU and GPU backends together only have 3 dependencies.

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

Elara UI offers three different APIs:

- The widget API provides ready-to-use, styled widgets following Elara design conventions. However, widgets are not customizable
- The component API provides basic building blocks of UIs that can then be joined together. They are flexible while not being overly verbose.
- The draw API allows painting custom widgets. It is the lowest-level API.

Elara UI requirements:

- Implementation of the entire Elara UI design system
- Good performance, low footprint (but it does not have to be aggressively optimized)
- Simple to use and easy to maintain
- Battle-tested: to prove this, several apps meant for production use will be written in Elara UI to test its functionality (see the demo apps below)

The single biggest inspiration for Elara UI's API is Gtk.

For Elara UI apps, the primary architecture will be a core-frontend approach. The functionality of the app will reside in the core, essentially library that contains all app functions. This allows the app to be run from the terminal, as well as controlled by scripting. The core should be able to do everything the app needs to do. Meanwhile, the frontend will communicate with the core and present the interface that the user will use to control the app. However, the frontend has no functionality of its own; all the functionality is in the core, which the frontend merely provides an interface to.

To integrate with this architecture, Elara UI is a retained-mode UI library, and uses the simplest API imaginable:

```rust
// 1. Create UI
let mut ui = UI::new(1600, 1200);

// 2. Create layouts
let mut left_sidebar = Layout::default();

// 3. Add elements with widgets API (easier) or 
// components API (more control) to layout
let mut sidebar_label = Label::new("Sidebar");
let mut sidebar_list = List::from("Item {}", 0..5);

// 4. Add callbacks to widgets to make them interactive
// widgets can be bound to a state so that their appearance
// is linked to that state
sidebar_list.on(move |event| {
    match event {
        Event::Click => {
            ui.close();
        },
        _ => (),
    }
})

// 5. Add components to layout, and add layout to UI
left_sidebar.add_element(sidebar_label);
left_sidebar.add_element(sidebar_list);
ui.add_element(left_sidebar);
```

Testing demo apps:

- Make an analogue of <https://sindresorhus.com/plain-text-editor>
- Make a gui version of <https://pypi.org/project/share-file-qr/>
- Make a previewer/slide shower based on <https://en.wikipedia.org/wiki/Takahashi_method> like `sent` from the suckless project
- Make a basic demo terminal based on <https://github.com/dhanoosu/TkTerm>

Follow tips from <https://2d.graphics/>

Tips for fast GPU rendering:

- Only render on user input/interaction, otherwise don't re-render frames (no immediate-mode UI that re-renders every frame)
- Only re-render changed areas ("damage tracking")
	- This requires caching the last render to a texture in memory and loading that texture on next frame, cropping out the region that was changed
- Must render on GPU
- For repeated elements (e.g. long lists or tree views) caching is necessary

design stuff:

For elaraui components - implement all the components that egui has in its demo

Elara UI should support a unified theme framework like https://github.com/hundredrabbits/Themes. Config specifies whether a theme is light or dark, and then specifies the colors of the theme. The Elara Design demo app can be used to preview the colors, and there is also an editor to visually edit themes. The Rust version of Elara UI (from which all the other versions are built from) will ship with a number of themes by default.

Cool apps to maybe port one day to `elara-ui` as demo applications:

- <https://github.com/FPurchess/blank>
- <https://github.com/adileo/squirreldisk>
- <https://github.com/spacedriveapp/spacedrive>
- <https://github.com/kkoomen/pointless>

## elara-math

Implement the following:

- Adam optimizer

Current progress: see [[Elara math optimization plan]](@/elara-math-optimization.md)

Elara-math will use elara-array for implementation of n-dimensional arrays, however everything else (e.g. diff eq solvers, FFT, special functions, quadrature, etc.) is custom-implemented.

In the future, do a "blank rewrite" - rewriting the entire library without looking at any reference github source code. Doing this often will help improve the code quality.

Autodiff implementations to look at:

- <https://github.com/ziap/autodiff/>
- <https://github.com/matiasbattocchia/simple-grad>
- <https://github.com/conradludgate/autograd-rs/tree/main>
- (Nearly 1-1 reimplementation of Jax in Rust) <https://getcode.substack.com/p/beyond-backpropagation-higher-order>

In the future elara-math should have a second functional API like Jax's that will be the primary API. This functional API has a `elara_math::grad()`function that transforms one function into a corresponding function for its derivative. Base this on how https://github.com/HIPS/autograd implements it. This is bc the current PyTorch-inspired API is basically only good for neural networks and wastes computational time creating graphs. One similar rust implementation to reference is <https://github.com/ibab/rust-ad/blob/master/src/lib.rs>.

Implement sparse matrices: <https://crates.io/crates/sprs>

- [x] Also add a numerical quadrature feature to `elara-math` to compute integrals:
- [ ] Implement simpson's rule and Gauss-Konrod rule based on https://github.com/esa/torchquad for 1D case, Monte-Carlo integration for N-D case, as well as Trapezoidal and Romberg quadrature for integrating over discrete arrays
- [ ] Add automatic integration - this is like automatic differentiation, only it takes a function and integrates its nth Taylor polynomial (which can be found exactly) - since unlike typical numerical integration it can be done to machine precision and thus doesn't suffer from numerical precision errors, it could be far more precise
    - Note: for this approach, automatic differentiation is necessary to compute the values of the derivative to construct the Taylor series
    - Techniques similar to Gauss-Konrod quadrature will probably be necessary to do range reduction for this approach to work well

<https://crates.io/crates/quad-rs/0.1.2>

Also learn matrix calculus for `elara-math` from https://www.lesswrong.com/s/nMGrhBYXWjPhZoyNL/p/9L9XuXhLYBm47yYkf

Also eventually create a tensor algebra system like cadabra: <https://cadabra.science/>

Also add a monte-carlo solver for ODEs: <https://jotterbach.github.io/content/posts/mc_ode/2018-08-08-MonteCarloODE/>

## elara-ml

Note: Elara ML is responsible for implementing the higher-level constructs for efficient machine learning (e.g. dense layers, convolution layers, model architectures, pretrained models, etc.) while Elara Math handles the underlying computations.

See [[Elara ML API proposal]](@/elara-ml-api.md)

`elara-ml` goals:

- Correct: a model written in PyTorch or TensorFlow should make equivalent predictions when ported to `elara-ml`
- Comprehensible: source code is in idiomatic Rust with lots of comments
- Minimal: only the features you really need
- Efficient: works fast, handles errors gracefully, and has an API with good developer experience, such as `model.summary()` visualizations (and isn't error-prone or overly verbose). It should also have good UI - e.g. progress bars on training.

For `elara-ml` instead of a vec of two tensors (the weights and the biases) which is wasteful in terms of memory, it would be more efficient to just iterate separately on the weights and biases. So instead of:

```rust
for t in self.parameters().iter() {
	t.update(lr);
}
```

instead do:

```rust
for t in self.weights().iter() {
	t.update(lr);
}

for t in self.biases().iter() {
	t.update(lr);
}
```

Also `elara-ml` should have dedicated `Input` and `Output` layers - they are basically normal Linear layers with a type alias, but they greatly improve in preventing errors in models.

`elara-ml` should have both tests verifying correctness in code and in functionality. That is, part of its tests should be various neural networks that take in a random number of input features and labels (of different shapes, e.g. 500x3, 1x8, etc.) and should be able to predict the same features and labels with minimal configuration correctly at **every point**.

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

Look at <https://lodev.org/lodepng/> and port the 500-line `picoPNG` into a Rust version for `elara-gfx`.

Implement several libraries on top of elara-gfx:

- `elara-vg`: Fabric.js/Paper.js-like vector graphics library on top of elara-gfx, see <http://fabricjs.com/fabric-intro-part-1>, basically it implements the vector graphics parsing and processing and vector graphics operations, but it leaves all the rendering to `elara-gfx`, which allows it to render to any platform `elara-gfx` supports (basically all the platforms)
- `elara-ui`: minimal UI library on top of elara-gfx used for all the Elara apps, again thanks to the library it supports all the platforms `elara-gfx` supports (further details below)

<https://zed.dev/blog/videogame>

`elara-gfx` is meant to not include mathematical operations. However, for certain operations that do require handling arrays, elara-gfx will have a micro-implementation of basic math using purely functional programming. That means function signatures will look like this:

```rust
// Takes in matrix a of dims (M x N) and matrix b of dims (N x K)
// and write the result to matrix c
fn matmul(a: &[f64], b: &[f64], m: usize, n: usize, k: usize, c: &mut [f64]);
```

`elara-gfx` should have three main APIs:

- `GfxRenderer`, which is common graphics rendering layer like SDL (just with the ability to render both on the CPU and the GPU)
- `GPUCompute`, which is an OpenGL wrapper for GPU computations (like CUDA), inspired by [this](https://github.com/DanRuta/GPGPU)
- `Platform`, a very low-level API to do things like window creation, event listening, etc.

Again, it aims to eventually be zero-dependency, which means that you do need to write OpenGL boilerplate initialization code yourself.

Crucially `elara-gfx` **should not** implement any math functions. That is what `elara-math` and `elara-array` does.

The `GfxRenderer` layer is able to draw basically anything graphics-related: 

- Lines
- Primitives
	- Rectangles
	- Circles
	- Triangles
- Text
- Images
    - Typical textures
    - Optionally images created procedurally using elara-gfx shaders
        - The shaders are Rust closures that take in 32-bit vertex data and fragment data as input and output a 32-bit raster image
        - This means theoretically the shaders can render anything (even 3D objects or scenes)
        - However since the shaders are CPU-based they are slow, the OpenGL canvas is faster and should be used for intensive rendering tasks
- Any 2D curve of arbitrary stroke and line color given a set of vertices
- Any 2D shape of arbitrary stroke and fill given a set of vertices (which means e.g. if you want to draw a bezier curve, you need to write your algorithm to convert it to an array of vertices)
- OpenGL canvas (GPU only, can run arbitrary OpenGL drawing commands)

This means that to use `elara-gfx` is as simple as declaring:

```rust
use elara_gfx::{GfxRenderer, RenderBackend};

fn main() {
    // use the cross-platform CPU backend
    // the backend is kind of like https://zserge.com/posts/fenster/
    let ctx = GfxRenderer::create_ctx(RenderBackend::CPU);
    ctx.draw(...);

    // use the cross-platform OpenGL backend
    let ctx = GfxRenderer::create_ctx(RenderBackend::OpenGL);
    ctx.draw(...);

    // use the Apple-only Metal backend
    let ctx = GfxRenderer::create_ctx(RenderBackend::Metal);
    ctx.draw(...);

    // use the cross-platform Image backend
    // this is most suitable to rendering to an image like a PNG
    let ctx = GfxRenderer::create_ctx(RenderBackend::Image);
    ctx.draw(...);
}
```

Elara GFX should eventually be zero-dependency (other than `elara-log`, but even that should be an optional feature flag), like `space-shooter.c`/`tigr`. This is a _very_ long-term thing.

To implement this, I can make a simplified port of `tigr` to C with just the OpenGL parts, and then port it to Rust for a zero-dependency GL/platforming library. This is in line with the long-term goal of making the entire Elara Project zero-dependency.

## elara-array

More details in [[Elara-array API plan]](@/elara-array-api-plan.md)

`elara-array` should include its GPU backend as an optional feature. This speeds up compile times - because if it doesn't use the GPU backend, it has basically zero dependencies.

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
