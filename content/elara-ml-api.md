+++
title = "Elara ML API proposal"
+++

`elara-ml` API - so there are actually 3 APIs currently being compared. The first one is PyTorch-style:

`elara-ml` API - so there are actually 3 APIs currently being compared. The first one is PyTorch-style:

```rust
pub struct MyModel {
    input_layer: Input,
    hidden_1: Dense,
    hidden_2: Dense,
    output_layer: Output
}

impl MyModel {
    // x and y are here only for shape determination
    fn new(x: Tensor, y: Tensor) -> MyModel {
        // automatic shape determination by passing
        // another layer as first argument
        let input_layer = Input::new(x);
        let hidden_1 = Dense::new(input_layer, 16);
        let hidden_2 = Dense::new(hidden_1, 16);
        let output_layer = Output::new(hidden_2, y);
        
        MyModel {
            input_layer,
            hidden_1,
            hidden_2,
            output_layer
        }
    }
}

impl Model for MyModel {
    // Models can only have one output, for
    // multi-input-output neural networks you
    // need to chain together multiple Models
    fn forward(x: Tensor) -> Tensor {
        // These absolutely don't need to
        // be in the same order as you declared
        // in new() (but probably should be so
        // that the auto shape determination works)
        let a = self.input_layer.forward(x);
        let b = self.hidden_1.forward(a);
        let c = self.hidden_2.forward(b);
        let d = self.output_layer.forward(c);
        d
    }
}

fn main() {
    let model = MyModel::new();
    model.compile(Optimizers::SGD);
    model.fit(&x, &y, 500, 0.00001, true);
}
```

This API makes it easiest to use pre-made models, because you can simply import the model and compile it. However, it might be too much abstraction - it can be a little hard to see what the model is actually doing, especially with methods like `compile()` and `fit()` that no longer have a 1-1 correspondence with performing operations on tensors.

The second uses a macro `Sequential!` to imitate Keras's sequential API. This makes it easiest to learn, but again, abstracts away too much, which is not ideal, especially given how much debugging is done when making NNs.

The third is most barebones, and is the Jax-inspired API. It looks like this:

```rust
// This is just a convenient way of
// holding layers, there is nothing
// special about this struct
struct Layers {
    pub input_layer: Input,
    pub hidden_1: Dense,
    pub hidden_2: Dense,
    pub output_layer: Output
}

impl Layers {
    // x and y are here only for shape determination
    fn new(x: Tensor, y: Tensor) -> Layers {
        // automatic shape determination by passing
        // another layer as first argument
        let input_layer = Input::new(x);
        let hidden_1 = Dense::new(input_layer, 16);
        let hidden_2 = Dense::new(hidden_1, 16);
        let output_layer = Output::new(hidden_2, y);
        
        MyModel {
            input_layer,
            hidden_1,
            hidden_2,
            output_layer
        }
    }

    // Note: for zero_grad()
    // and update(), these can be
    // made less verbose by creating an
    // iter() method - see
    // https://stackoverflow.com/questions/30218886/how-to-implement-iterator-and-intoiterator-for-a-simple-struct
    fn zero_grad(&self) {
        self.input_layer.zero_grad();
        self.hidden_1.zero_grad();
        self.hidden_2.zero_grad();
        self.output_layer.zero_grad();
    }

    fn update(&self, lr: f64) {
        self.input_layer.update(lr);
        self.hidden_1.update(lr);
        self.hidden_2.update(lr);
        self.output_layer.update(lr);
    }

    fn save(&self) {
        let weights = NNSerializer::new("weights.bin");
        // Add labels to weights; they will be referred
        // to by their labels when the weights are loaded
        weights.add(self.input_layer, "input_layer");
        weights.add(self.hidden_1, "hidden_1");
        weights.add(self.hidden_2, "hidden_2");
        weights.add(self.output_layer, "output_layer");
        weights.write();
    }
}

fn forward(layers: Layers, x: Tensor) -> Tensor {
    let a = layers.input_layer.forward(x);
    let b = layers.hidden_1.forward(a);
    let c = layers.hidden_2.forward(b);
    let d = layers.output_layer.forward(c);
    d
}

fn mean_squared_error(y: Tensor, y_pred: Tensor) -> Tensor {
    (&y_pred - &y).pow(2)
}

fn main() {
    // load x and y...
    let layers = Layers::new();
    let pbar = TrainingProgress::new(); // used to display progress bars

    // here we write our custom optimizer
    for i in 0..1000 {
        let preds = forward(layers, x);
        // preds and loss are both tensors, so they
        // can work with all the standard tensor methods,
        // including output to graphviz files!
        let loss = mean_squared_error(y, preds);
        pbar.update(i, &loss); // shows latest progress
        let lr = 1.0 - 0.9*i/100.0
        loss.backward();
        layers.update(lr);
        layers.zero_grad();
    }
}
```

This approach has just the right amount of abstraction, and is very flexible, because it allows defining custom forward passes (with the ability to do multiple inputs or multiple outputs), custom loss functions, and custom optimizers. Furthermore, this API can easily interoperate with the PyTorch-style API. So this will be the API that is primarily focused on.
