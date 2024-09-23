# 3 General Description

Inference is a formal specification language that aims to provide a straightforward way to specify the correctness properties of Web3 native application algorithms.

> [!IMPORTANT]
> Based on this fact, the following important consequences follow:
> 1. Inference is a Turing-complete language, **but**
> 2. It is not intended to be used for general-purpose programming.

## Non-Deterministic Computations

One of the most important concepts that allows covering all possible execution paths is non-deterministic computation. Inference provides a way to specify and manage non-deterministic computations in the program.

The following keywords are used for that:

- [verify](./statements.md#9-verify)
- [total](./functions.md#111-total)
- [filter](./statements.md#9-filter)
- [undef](./statements.md#911-undef)

### Verify

The `verify` keyword is used to create an expression that tells a prover that the function this expression is applied to must be proven by a proof assistant.

### Total

`total` is a function modifier that indicates that the function is total, i.e., it is guaranteed to terminate for all possible inputs (in contrast to [partial functions](https://en.wikipedia.org/wiki/Partial_function)).

### Filter

The `filter` keyword is used to make a statement that continues only execution paths conforming to a given precondition. For instance, if some property needs to be checked only for the even values of $\mathbb{N}$, we can prepend it with a filter to retain only needed variants as follows: `filter { assert (N % 2 == 0); }`.

### Undef

The `undef` keyword is a variable modifier that indicates that the variable can have any value (eventually includes all possible variants). So if we declare `let undef a: i32;` it means that `a` is considered as a set of all possible `i32` values.

For more information, see the following article: [Specifying Algorithms Using Non-Deterministic Computations](https://www.inferara.com/en/papers/specifying-algorithms-using-non-deterministic-computations/)

## Compiler Design

`infc` is a multi-pass compiler. But since the final target of the language is a [proof-unit](./terms-and-definitions.md#proof-unit), it produces required proof modules as intermediate representations instead of different languages.

The compilation process consists of the following stages:

1. Inference source code parsing using [tree-sitter-inference](https://github.com/Inferara/tree-sitter-inference) grammar parser.
   - Building required internal representations.
   - Semantic analysis and type checking. [Tracking issue](https://github.com/Inferara/inference/issues/8)
   - Linked external modules integrity and capability check.
2. Generating IR for the target execution platform (WASM) amended with Inference non-deterministic markers.
3. Building a compound module containing the Inference module IR and linked external code.
4. Translating the compound module to a `.v` [proof-unit](./terms-and-definitions.md#proof-unit).
5. Attaching Inference [theory](./terms-and-definitions.md#theory), platform axioms, and generating theorems.
6. Building proofs for the specification.

![Inference IR Building Sequence](./assets/inference-ir-building-sequence.png)

## Automated Theorem Proving

As an automated theorem proving backend, Inference uses the Coq theorem prover. The Coq type system is based on the Calculus of Inductive Constructions (CIC), which is a dependently typed lambda calculus.

Inference uses CIC along with first-order logic and Hoare logic to build theories for the given properties and the program code, as well as automatically prove the correctness of the properties if possible.

Otherwise, meaningful diagnostics are provided.

## Restrictions

- The minimal addressable unit of memory in Inference is a 4-byte word (32 bits).
- The language does not support floating-point numbers.
- The language tends to be as explicit as possible, so no implicit type conversions are allowed, as well as no type inference.
- The language does not support dynamically sized arrays.
- The language does not support pointers.
- The language does not support strings.

---

[<kbd><br>⏮️ Terms and definitions<br><br></kbd>](./terms-and-definitions.md)
[<kbd><br>⏭️ Lexical structure<br><br></kbd>](./lexical-structure.md)
