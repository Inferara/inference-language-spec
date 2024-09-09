# 3 General description

Inference is a formal-specification language the main goal of which is to provide a straightforward way to specify the correctness properties of the Web3 native application algorithms.

> [!IMPORTANT]
> Based on this fact, the following important consequences follow:
> 1. Inference in not a Turing-complete language
> 2. It is not intended to be used for general-purpose programming.

## Non-determenistic computations

One of the most important concept that allows covering all possible execution paths is the non-deterministic computation. Inference provides a way to specify and manage non-deterministic computations in the program.

The following keywords are used for that:
- [verify](./expressions.md#verify)
- [total](./functions.md##1111-total-keyword)
- [filter](./statements.md#filter)
- [undef](./statements.md#911-undef)

TODO write explanation and draw a diagram @Keyholder

For more information, see the following article: [Specifying algorithms using non-deterministic computations](https://www.inferara.com/en/papers/specifying-algorithms-using-non-deterministic-computations/)

## Compiler design

Infc is a multi-pass compiler. But since the final target of the language is a [proof-module](./terms-and-definitions.md#proof-unit) it produces required for proof modules as intermediate representations instead of different languages. 

The compilation process consists of the following stages:

1. Inference source code parsing using [tree-sitter-inference](https://github.com/Inferara/tree-sitter-inference) grammar parser.
    1. Builing required internal representations.
    0. Semantic analysis and type checking. [Tracking issue](https://github.com/Inferara/inference/issues/8)
    0. Linked external modules intergrity and capability check.
0. Generating IR for the target execution platform (WASM) amended with Inference non-deterministic markers.
0. Building a compound module contains the inference module IR and linked external code.
0. Translating the compound module to `.v` [proof-unit](./terms-and-definitions.md#proof-unit).
0. Attaching Inference [theory](./terms-and-definitions.md#theory), platform axioms, and theorems generating.
0. Build proof for the `v2` specification.


<img src="./assets/inference-ir-building-sequence.png" alt="Inference IR building sequence" width="600" style="margin: 0 auto; display: block;">

## Automated theorem proving

As an automated theorem proving backend, Inference uses Coq theorem prover. The Coq type system is based on the CIC (Calculus of Inductive Constructions) which is a dependently typed lambda calculus.

Inference uses CIC along with the first-order logic and Hoare logic to build theories for the given properties and the program code as well as automatically prove the correctness of the properties if possible.

Otherwise, the meaningful diagnostics is provided.

TODO need an example

## Restrictions

- The minimal addressable unit of memory in Inference is a 4-bytes word (32 bits).
- The language does not support floating point numbers.
- The languge tends to the as much explicit as possible, so no implicit type conversions are allowed as well as no type inference.
- The language does not support bit operations such as `&`, `|`, `^`, `<<`, `>>`.
- The language does not support dynamically sized arrays.
- The language does not support pointers.
- The language does not support strings.
