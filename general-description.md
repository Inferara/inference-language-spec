# 3 General Description

Inference is a formal specification language that aims to provide a straightforward way to specify the correctness properties of Web3 native application algorithms.

> [!IMPORTANT]
> Based on this fact, the following important consequences follow:
> 1. Inference is a Turing-complete language, **but**
> 2. It is not intended to be used for general-purpose programming.

## 3.1 Non-Deterministic Computations

One of the most important concepts that allows covering all possible execution paths is non-deterministic computation. Inference provides a way to specify and manage non-deterministic computations in the program.

The following keywords are used for that:

- [uzumaki](./statements.md#9821-uzumaki)
- [forall](./statements.md#93-forall)
- [exists](./statements.md#94-exists)
- [assume](./statements.md#95-assume)

### 3.1.1 Uzumaki

The `@` keyword (pronounced as u-zu-ma-ki うずまき) is a primitive expression that can occupy `rval` slot of any primitive type serving as a point of non-deterministic branching of passing computation. Its evaluation split execution path into finite set of subpaths, each of which differs from the others only by `@`'s result, collectively covering all possible values that given type can hold. Representing undetermined values, this construct lays foundation for every formal specification, idiomatically expressed in Inferara through modelling non-deterministic behavior of covered functions and modules. As classical imperative programming paradigm doesn't have operational semantics for non-deterministic computations, `@` can be evaluated only inside specilized quantified contexts explained below.

### 3.1.2 Forall

`forall` (execution paths) is a quantifying block construct that passes control down the flow without any side effects, iff the execution of its body sucessfully terminates for all possible non-deterministic paths emanating from evaluation of `@`s inside. For example:

```inference
forall {
    /// Here computation splits into 2^32 subpaths,
    /// with `x` holding distinct value on each
    let x: u32 = @;

    /// Here each computation splits further, independently
    /// checking both required properties for every possible
    /// value of `x` on separate execution paths.
    if @ { check_foo(x); }
    else { check_bar(x); }
}

/// This point is reached iff both functions terminate
/// successfully on every possible input value.
print('Success!');
```

Take note that each execution path of non-deterministic computation is completely isolated from its neighbors. Nothing that happens through computation of `check_foo` function can affect any aspect of `check_bar` computation and vice versa. Moreover, this holds here for the computations of calls to the same function with different arguments too. Practically, you can percieve non-deterministic branching as act of passing duplicated state of whole execution context in its entirety to every spawned subpath. Also, it's worth underlining that quantifying blocks like `forall` are dismissing all changes made to such duplicated contexts on the way of its body execution, taking into account only totality of their successfull termination - for each execution path that enters `forall` block, no more then one continuation may exit it, and if that happens, it has same effect as if `nop` statement was in its place.

### 3.1.3 Exists

`exists` (an execution path) is a quantifying block construct that passes control down the flow without any side effects, iff the execution of its body has at least one sucsessfully terminating non-deterministic path. Similarly:

```inference
exists {
    /// Here computation splits into 2^32 subpaths,
    /// with `x` holding distinct value on each
    let x: u32 = @;

    /// Here each computation splits further, independently
    /// checking both required properties for every possible
    /// value of `x` on separate execution paths.
    if @ { check_foo(x); }
    else { check_bar(x); }
}

/// This point is reached iff one of the functions terminate
/// successfully on at least one possible input value.
print('Success!');
```

Rules of execution path isolation and dismissal of side effects for `exists` are the same as for `forall`. One computation enters, no more then one exits, and if so, continuation recieves untouched execution context.

### 3.1.4 Assume

The `assume` block construct propagates down the control flow only execution paths which successfully complete its body, while absorbing all internal failuers (be it outright traps, or neverending cycles) that would otherwize preemptively fail enclosing quantifying block. Practically it can be used to limit property checking to situations conforming to a given precondition. For instance, if some property needs to be checked only for the even values of $\mathbb{N}$, we can prepend it with a assume to retain only needed variants as follows: `assume { assert (N % 2 == 0); }`. Take note that it makes sense only inside `forall` quantification context, as transforming failures into preemptive successfull termination of `exists`' body is not very useful. For example:

```inference
forall {
    /// Here computation splits into 2^32 subpaths,
    /// with `x` holding distinct value on each
    let x: u32 = @;

    /// Here we filter only execution paths, where given
    /// precondition is met, indicated by successfull termination
    /// of function.
    assume { check_foo(x); }

    /// Here we check given implication of precondition above
    check_bar(x);
}

/// This point is reached iff every value of x that successfully
/// pass through `check_foo` also successfully pass through
/// `check_bar`.
print('Success!');
```

It's important to remember that `assume` is not a quantifier by itself, so it has no meaningful use without enclosing `forall` block, for which it merely denotes local change of failure interpretetion rules. For same reason `assume` blocks (unlike quantifers `forall` and `exists`) retain all changes to machine state along execution paths going through its body.

#### 3.1.4.1 Semantics

- Non-Determinism: The `@` keyword introduces non-determinism into the program by declaring that a variable all possible values within its type. This is useful for modelling scenarios where inputs or states are unpredictable or for simulating all possible execution paths.
- Universal Quantification: Conceptually, `@` is similar to the universal quantifier $\forall$ (for all) in formal logic. It signifies that the variable should be considered as potentially holding any value from its type's domain during analysis or execution. However, this quantification is only applied to the variable values. The operational $\forall$ semantics is represented by the `forall` block modifier in Inference.
- Defined Behavior: Unlike undefined behavior in languages like C++, where the result of an operation is unpredictable and potentially can be hazardous, `@` variables have well-defined semantics. They are undefined in terms of which specific value they hold, but the behavior of operations involving them is specified according to the Inference language rules.

**Differences from Uninitialized Variables and Undefined Behavior**

Uninitialized Variables: In many languages, declaring a variable without initializing it (e.g., `int x;`) leaves it with an indeterminate value, which can lead to undefined behavior if used before the assignment. However, an `@` variable is explicitly declared to represent any all possible values within its type, and operations on them are well-defined.

Undefined Behavior: Undefined behavior refers to program operations that the language specification does not define, leading to unpredictable results[^2]. The use of `@` does not cause undefined behavior; instead, it allows for the intentional representation of any possible value.

For more information, see the following article: [Specifying Algorithms Using Non-Deterministic Computations](https://www.inferara.com/en/papers/specifying-algorithms-using-non-deterministic-computations/)

#### 3.1.4.2 Examples

```inference
let x: i32 = @;
assert(x + 1 > x);
```

Here, `x` can hold any `i32` value. The assertion checks that adding `1` to any integer `x` will result in a value greater than `x`, which is a property that can be universally verified.

> [!IMPORTANT]
> This example requires additional attention to pay. Since we know that `x` holds all possible `i32` values, it follows that it holds `0x7FFF` also. **Inference does not consider numeric overflows**, so let's take a look at two cases for `x == 0x7FFF`. If the expression `assert(x + 1 > x)` appears in the `forall` block, it will make it impossible to prove such block's sucessful termination[^1]. If the expression appears inside an `assume` block, it simply forbids `x` holding the maximum value of its type and after the asserting, only execution paths with `x != 0x7FFF` will be considered.

![`@` for `i32` assertion](./assets/uzumaki-i32-assert-diagram.png)

```inference
let user_input: bool = @;

if user_input {
    /// Handle true case
} else {
    /// Handle false case
}
```
In programs where input values are not known ahead of time or can vary widely, `@` variables can model these uncertainties. In this code, it is assumed that `user_input` can be either `true` or `false`, and both branches need to be considered during testing or analysis.

```inference
let mut choice: i32 = @;
choice = choice % 3;
if (choice == 0) {
    /// Handle case 0
} else if (choice == 1) {
    /// Handle case 1
} else {
    /// Handle case 2
}
```

Algorithms that rely on randomization or non-deterministic choices can use `@` to represent the variability in their behavior. This approach models all possible cases among three options.

## 3.2 Compiler Design

`infc` is a multi-pass compiler. But since the final target of the language is a [proof-unit](./terms-and-definitions.md#proof-unit), it produces required proof modules as intermediate representations.

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

[^1]: The reason of that is the fact that the totality of the block is proven by checking all possible execution paths. Since we know that the particular path with `x == 0x7FFF` is impossible, the block is not sealed.
[^2]: Raph Levien. [With Undefined Behavior, Anything is Possible](https://raphlinus.github.io/programming/rust/2018/08/17/undefined-behavior.html)
