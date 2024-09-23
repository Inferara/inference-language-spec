# 5 Basic Concepts

## 5.1 Specification Definition

Formally speaking, a specification is a set of properties that define the specified system's behavior. A property is a statement that must be true for all possible executions of the system. Therefore, we can say that a specification is a set of statements that must be true for all possible executions of the system. In formal logic terms, properties can be thought of as first-order logic formulas with a number of variables, and the specification is a super-formula that is the conjunction of all properties.

$$
\text{Specification} = \text{Property}_1 \land \text{Property}_2 \land \ldots \land \text{Property}_n
$$

In Inference, the specification is defined as a [context](./definitions.md#101-context). Every context is then compiled into a [module](./terms-and-definitions.md#module) and into a [proof-unit](./terms-and-definitions.md#proof-unit). This modular structure enables Inference to support super-specifications, where a super-specification is a set of specifications combined as a conjunction of all specifications.

$$
\text{Super-specification} = \text{Specification}_1 \land \text{Specification}_2 \land \ldots \land \text{Specification}_n
$$

> [!IMPORTANT]
> **Super-specification (SS)** allows the description of the whole blockchain behavior and the behavior of a DApp in the context of its direct or indirect interaction with other DApps in the system.

## 5.2 Specification Is Not a Program

Despite using the Inference programming language, a specification is a formula, not a program. Inference is designed to be very similar to usual imperative programming languages, providing a convenient way to write specifications.

However, the imperative flavor of Inference unlocks another powerful feature: **writing formal specifications in Inference is as easy as writing unit tests** in any other common programming language. This is a huge advantage because it allows developers to write specifications in a way that is very close to how they are used to writing code.

> [!IMPORTANT]
> The key difference between unit tests and properties defined by Inference is that unit tests check correctness at certain points (a set of particular inputs), while the properties defined by Inference check correctness for **all** possible inputs.

For example, consider specifying that a `sum` function correctly adds two numbers for all possible inputs. In unit tests, you might write tests for specific inputs:

```rust
fn test_sum() {
    assert_eq!(sum(2, 3), 5);
    assert_eq!(sum(-1, 1), 0);
}
```

In Inference, you can write a specification that asserts the correctness of `sum` for all inputs:

```inference
extern fn sum(a: i32, b: i32) -> i32;

total fn sum_spec() {
    let undef a: i32;
    let undef b: i32;
    assert(sum(a, b) == a + b);
}

fn proof() {
    verify sum_spec();
}
```

In this example, `sum_spec` is a total function that, using `undef`, considers all possible values of `a` and `b`, and asserts that `sum(a, b)` equals `a + b` for all of them.

## 5.3 Execution Model

Now, as we see, the spec is not an execution unit because it is not compiled to bytecode for any platform. The target platform of `infc` is a theorem prover. The [Inferara execution theory](TODO) enables reasoning about the program as if it has been executed on a certain platform. As of now, the execution platform covered by the Inferara theory is [WASM](https://webassembly.org/).

Essentially, the theory describes how a virtual machine (VM) works, its memory state, and operations. It contains a description of how operations are executed, how memory is managed, how the stack is used, and so on. It is worth highlighting that the proof theory is not similar to symbolic execution. The symbolic execution technique actually executes commands and works with the memory state, while the proof theory is a way to describe what happens with memory after a command is executed without having this memory in the real world.

For illustration, consider the well-known formula of energy $E=mc^2$. This formula is a property of one aspect of how the universe works. In order to prove this formula, other formulas and theories are used. But the formula itself is not proven by experiment simply because an experiment cannot cover all possible combinations of $m$ and $c$ to ensure the formula is correct for all possible values.

In this analogy, the Inference proof is akin to a mathematical proof, whereas symbolic execution is akin to an experimental approach.

## 5.4 Non-Deterministic Execution

Inference leverages non-deterministic execution to model and reason about all possible execution paths of a program. Non-determinism allows the specification to consider every possible value a variable might take, enabling comprehensive verification.

In Inference, non-determinism is introduced using the `undef` keyword for variables and the `total` keyword for functions. An `undef` variable represents all possible values of its type, and a `total` function is defined for all possible inputs and must terminate for all of them.

For example:

```inference
total fn example_spec() {
    let undef x: i32;
    assert(x + 1 > x);
}
```

Here, `x` can be any integer, and the assertion must hold for all possible values of `x`.

## 5.5 Platform-Specific Execution

Blockchains are complex systems that have massive codebases and various features and capabilities like different consensus algorithms, finality algorithms, and so on. Keeping this fact in mind, we can see that it is barely possible to reproduce the whole blockchain in terms of the proof theory as it is done for platforms like [CompCert](https://github.com/AbsInt/CompCert).

The approach used in Inference is as follows: the proof theory consists of two parts—the execution platform theory and a set of axioms that describe the blockchain behavior. The axioms are the set of properties that are stated to be true for the blockchain by its design and implementation.

So when Inference reasons about the DApp, it reasons about the DApp itself, and the blockchain API that is used by the DApp is covered by the axiomatic theory. We can think about these API axioms in a way similar to an SDK for a platform that we are using, as provided by the platform developers.

## 5.6 Poly-Blockchain Design

Inference is a low-level language by nature because property definitions must be straightforward and unambiguous. From the proof-unit viewpoint, the execution model follows the command set of a certain platform—for example, WASM. This means that if a blockchain uses WASM as a compilation target, then compiled DApp modules can be linked to the Inference modules, and proofs can be constructed.

For reference, the following blockchains use WASM as a compilation target:

- [CosmWasm](https://book.cosmwasm.com/index.html)
- [Polkadot Ink!](https://use.ink/smart-contracts-polkadot/)
- [Arbitrum Stylus](https://arbitrum.io/stylus)
- [Near](https://docs.near.org/build/smart-contracts/what-is)
- [Fluent](https://docs.fluentlabs.xyz/learn)

`infc` is [designed](./general-description.md#compiler-design) to be modular, and it is possible to add support for more platforms in the future. The theory can be written (or extended) to use a platform variation, or new platforms (EVM, RISC-V, etc.) can be formulated. The front end—the Inference programming language—remains the same.

## 5.7 Automated Reasoning

The proof artifact generated by `infc` is a Coq module that contains axioms and theorems for the properties stated in the `.inf` files. However, it would be non-trivial to manually prove all the theorems generated by the compiler.

> [!IMPORTANT]
> The proof automation is another powerful feature of Inference.

Inference employs automated proof strategies to handle the generated proofs. The compiler leverages Coq's automation facilities, such as tactics and decision procedures, to automatically discharge many proof obligations without human intervention.

For example, arithmetic properties and simple logical assertions can often be proven automatically. This significantly reduces the effort required to verify complex specifications.

> [!WARNING]
> The proof automation is not a silver bullet. It is not always possible to prove all the properties automatically. In such cases, meaningful diagnostics are provided to help developers understand and address the issues.

## 5.8 Correctness Certificate

Given all described above, a reasonable question is whether the output of Inference and its proof is reliable and proves the defined properties for the given code and nothing else. Simply put, does it make sense to trust the proof?

The answer is yes, because the proof is a Coq module that is a formal proof and can be checked by the Coq theorem prover. The Coq theorem prover is a well-known and widely used tool in the formal verification community, especially in academia.

Since the final artifact of `infc` is a Coq file `.v`, it can be checked by the Coq theorem prover either automatically (ensuring there are no errors, unsoundness, or open goals) or manually by anyone who is interested in the proof's correctness; they can open the file and read it.

Furthermore, correctness tracking is also available, so we can check if the proof for the code at state $S$ is equivalent to the proof for the code at state $S'$.

There is a [database](TODO) of correctness certificates that Inferara holds, and it is possible to ensure if a given code has been proven before and if the proof is still valid.

---

[<kbd><br>⏭️ Types<br><br></kbd>](./types.md)
[<kbd><br>⏮️ Lexical structure<br><br></kbd>](./lexical-structure.md)
