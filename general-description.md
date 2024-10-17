# 3 General Description

Inference is a formal specification language that aims to provide a straightforward way to specify the correctness properties of Web3 native application algorithms.

> [!IMPORTANT]
> Based on this fact, the following important consequences follow:
> 1. Inference is a Turing-complete language, **but**
> 2. It is not intended to be used for general-purpose programming.

## 3.1 Non-Deterministic Computations

One of the most important concepts that allows covering all possible execution paths is non-deterministic computation. Inference provides a way to specify and manage non-deterministic computations in the program.

The following keywords are used for that:

- [verify](./statements.md#9-verify)
- [total](./functions.md#111-total)
- [filter](./statements.md#9-filter)
- [undef](./statements.md#911-undef)

### 3.1.1 Verify

The `verify` keyword is used to create an expression that tells a prover that the function this expression is applied to must be proven by a proof assistant.

### 3.1.2 Total

`total` is a function modifier that indicates that the function is total, i.e., it is guaranteed to terminate for all possible inputs (in contrast to [partial functions](https://en.wikipedia.org/wiki/Partial_function)).

### 3.1.3 Filter

The `filter` keyword is used to make a statement that continues only execution paths conforming to a given precondition. For instance, if some property needs to be checked only for the even values of $\mathbb{N}$, we can prepend it with a filter to retain only needed variants as follows: `filter { assert (N % 2 == 0); }`.

### 3.1.4 Undef

The `undef` keyword is a variable modifier used in Inference to indicate that a variable can assume any possible value within its type's domain. It represents non-deterministic or unspecified values, effectively modelling all potential values that a variable of a given type can hold. This concept is particularly useful in formal specification and modelling non-deterministic behavior in programs.

The typical syntax for declaring an undef variable is:

```inference
let undef a: i32;
```

In this example, `a` is declared as an `i32` (32-bit integer) variable with the `undef` modifier. This means that a is not assigned a specific value but is considered to represent any possible `i32` value.

#### 3.1.4.1 Semantics

- Non-Determinism: The `undef` keyword introduces non-determinism into the program by allowing a variable to take on any value within its type. This is useful for modelling scenarios where inputs or states are unpredictable or for simulating all possible execution paths.
- Universal Quantification: Conceptually, `undef` is similar to the universal quantifier $\forall$ (for all) in formal logic. It signifies that the variable should be considered as potentially holding any value from its type's domain during analysis or execution.
- Defined Behavior: Unlike undefined behavior in languages like C++, where the result of an operation is unpredictable and potentially hazardous, `undef` variables have well-defined semantics. They are undefined in terms of which specific value they hold, but the behavior of operations involving them is specified according to the Inference language rules.

**Differences from Uninitialized Variables and Undefined Behavior**

Uninitialized Variables: In many languages, declaring a variable without initializing it (e.g., `int x;`) leaves it with an indeterminate value, which can lead to undefined behavior if used before the assignment. However, an `undef` variable is explicitly declared to represent any value within its type, and operations on it are well-defined.

Undefined Behavior: Undefined behavior refers to program operations that the language specification does not define, leading to unpredictable results. The use of `undef` does not cause undefined behavior; instead, it allows for the intentional representation of any possible value.

For more information, see the following article: [Specifying Algorithms Using Non-Deterministic Computations](https://www.inferara.com/en/papers/specifying-algorithms-using-non-deterministic-computations/)

#### 3.1.4.2 Examples

```inference
let undef x: i32;
assert(x + 1 > x);
```

Here, `x` can be any `i32` value. The assertion checks that adding `1` to any integer `x` will result in a value greater than `x`, which is a property that can be universally verified.

> [!IMPORTANT]
> This example requires additional attention to pay. Since we know that `x` holds all possible `i32` values, it follows that it holds `0x7FFF` also. **Inference does not consider numeric overflows**, so let's take a look at two cases for `x == 0x7FFF`. If the expression `assert(x + 1 > x)` appears in the `total` block, it will make it impossible to prove such block's totality[^1]. If the expression appears inside `filter`, it simply forbids `x` holding the maximum value of its type and after the asserting, only execution paths with `x != 0x7FFF` will be considered.

![`undef` for `i32` assertion](./assets/undef-i32-assert-diagram.png)

```inference
let undef user_input: bool;
if (user_input) {
    // Handle true case
} else {
    // Handle false case
}
```
In programs where input values are not known ahead of time or can vary widely, `undef` variables can model these uncertainties. In this code, it is assumed that `user_input` can be either `true` or `false`, and both branches need to be considered during testing or analysis.

```inference
let undef choice: i32;
choice = choice % 3;
if (choice == 0) {
    // Handle case 0
} else if (choice == 1) {
    // Handle case 1
} else {
    // Handle case 2
}
```

Algorithms that rely on randomization or non-deterministic choices can use `undef` to represent the variability in their behavior. This models all possible cases among three options.

## 3.2 Compiler Design

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

## 3.3 Automated Theorem Proving

As an automated theorem proving backend, Inference uses the Coq theorem prover. The Coq type system is based on the Calculus of Inductive Constructions (CIC), which is a dependently typed lambda calculus.

Inference uses CIC along with first-order logic and Hoare logic to build theories for the given properties and the program code, as well as automatically prove the correctness of the properties if possible.

Otherwise, meaningful diagnostics are provided.

## 3.4 Restrictions

- The minimal addressable unit of memory in Inference is a 4-byte word (32 bits).
- The language does not support floating-point numbers.
- The language tends to be as explicit as possible, so no implicit type conversions are allowed, as well as no type inference.
- The language does not support dynamically sized arrays.
- The language does not support pointers.
- The language does not support strings.

---

[<kbd><br>⏮️ Terms and definitions<br><br></kbd>](./terms-and-definitions.md)
[<kbd><br>⏭️ Lexical structure<br><br></kbd>](./lexical-structure.md)

[^1]: The reason of that is the fact that the totality of the block is proven by checking all possible execution paths. Since we know that the particular path with `x == 0x7FFF` is impossible, the block is not total.
