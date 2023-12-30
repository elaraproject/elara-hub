+++
title = "Elara-array API"
+++

Design goals:

- A pure Rust library designed NOT for FFI, but for use from only Rust.
- Minimal set of features, leave more complex math/science functions (e.g. convolution, diff eq solvers) for `elara-math` to implement
- FAST - really really really optimized and fast!!!

```rust
// Macros for convenient array creation up to 3D
// However they are not recommended for array data
arr![1, 2, 3, 4];
arr![[1, 2], [3, 4]];

// Explicit array creation
// This is the *only* time data is allocated
NdArray::new(your_vec);
NdArray::arange(0..10);

// methods
// these either operate in-place
// or don't perform any manipulations on the underlying
// data (like the transpose function) to prevent 
// copying for efficiency
arr.dot(&arr2);
arr.matmul(&arr2);
arr.product();
arr.len();

// Operations default to in-place ops to prevent additional
// copying or moves of data
arr + arr2; // returns the result in arr
arr * arr2;
arr / arr2;

// Indexing (for both getting and setting data)
// these are implemented by two index implementations,
// one for views and one for direct indices
impl Index<[f64; N]> for NdArray<T, N>;
impl Index<NdArrayView<f64, N>> for Ndarray<T, N>;

// Construct a view
// These replace numpy-style slices
let yourview<f64, N> = NdArrayView::row_view(start, end, step);

arr[[1, 3, 5]]; // direct indexing
arr[yourview]; // view indexing

// elara-array uses a specialized
// binary file format for serializing arrays
arr.save("array.elr");
NdArray::from_file("array.elr");

// The entire library supports full error handling
// and is designed to be robust even on error
```

API reference:

```rust
// utility methods
fn new();
fn zeros();
fn ones();
fn eye();
fn random();
fn size(); // number of elements
fn ndim();
fn shape();
fn flatten();
fn reshape();
fn mapv(); // elementwise in-place map

// arithmetic methods
fn sin();
fn cos();
fn tan();
fn arcsin();
fn arccos();
fn arctan();
fn sinh();
fn cosh();
fn tanh();
fn exp();
fn sqrt();
fn cbrt();
fn log();

// statistical methods
fn sum();
fn min();
fn max();
fn mean();
fn median();
fn stddev(); // standard deviation

// linalg methods
fn dot(); // 1D only
fn matmul(); // 2D only
fn transpose(); // 2D only
```

Error API:

```rust
struct ErrorInfo {
	source: &'static str, // the function that called the error
	file: &'static str,
	line: usize,
	additional_info: String
}

pub enum {
	SomeError1(ErrorInfo),
	SomeError2(ErrorInfo)
}
```

Slice/View API:

```rs
ArraySlice slicer = ArraySlice::new([.., ])
```

- [x] Change all existing copying operations to operate in-place by default
- [ ] Implement arrayslices and views
	- [ ] ArraySlices use a numpy-like syntax for indexing
	- [ ] Views wrap a standard rust slice (`&[T]`) of an array
- [ ] GPU support (this is most important!!!)
	- [ ] Headless OpenGL context with `glfw` Rust bindings
	- [ ] GPU computation
- [ ] API completion
- [x] For select operations, implement alternative methods that create a new NdArray with the `_copy` suffix e.g. `matmul_copy()`
- [ ] Clean up examples to just 2 - one showing basic usage and indexing, and the other showing an applied example with an ODE solver
- [ ] Write tests for library
- [ ] Benchmark library to ensure good performance and add elara array benchmarks
- [ ] More helpful error messages (for good developer experience)
- [ ] Implement binary serialization

Optimizations:
- GPU computation & keeping data on the GPU for as long as possible
- Using a clone-on-write backing array (`Cow`) so that no clones will be done unless necessary
