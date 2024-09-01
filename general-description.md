# 3 General description

Inference is a formal-specification language the main goal of which is to provide a straightforward way to specify the correctness properties of the Web3 native application algorithms.

Based on this fact, the following important consequences follow:
1. Inference in not a Turin-complete language
2. It is not intended to be used for general-purpose programming.

## Non-determenistic computations

One of the most important concept that allows covering all possible execution paths is the non-deterministic computation. Inference provides a way to specify and manage non-deterministic computations in the program.

The following keywords are used for that:
- `verify`
- `total`
- `filter`

TODO write explanation and draw a diagram

## Automated theorem proving

As an automated theorem proving backend, Inference uses Coq theorem prover. The Coq type system is based on the CIC (Calculus of Inductive Constructions) which is a dependently typed lambda calculus.

Inference uses CIC along with the first-order logic and Hoare logic to build theories for the given properties and the program code as well as automatically prove the correctness of the properties if possible.

Otherwise, the meaningful diagnostics is provided.

TODO need an example

## Restrictions

- The minimal addressable unit of memory in Inference is a 2-bytes word (32 bits).
- The language does not support bit operations such as `&`, `|`, `^`, `<<`, `>>`.
- The language does not support floating point numbers.
- The language does not support dynamically sized arrays.
- The language does not support pointers.
- The language does not support strings.
