+++
title = "Elara math optimization strategies"
+++

(All times given on macos)

Original time: 309s

After integration with `matrixmultiply` crate:

Time: 309s

Without `println!()` for loss:

Time: 317s

With reduced clones

Time: 277s

-----

Use libblas to implement basic operations

Faster transpose implementation

Use release mode

----- 

Functions to optimize:

- Matmul
- Relu 
- Sum

-----

Implement model saving to JSON, TOML and binary with nanoserde
Unit tests for the library
