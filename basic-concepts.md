# 5 Basic concepts

## 5.1 Specification definition

Formally saying, a specification is a set of properties that defines the specified system behavior. A property is a statement that must be true for all possible executions of the system. So, we can say that a specification is a set of statements that must be true for all possible executions of the system. The formal way of thinking about properties in terms of logic like they are the first-orger logic formulas with a number of variables, and the specification is a super-formula that is a conjunction of all properties.

$$
\text{Specification} = \text{Property}_1 \land \text{Property}_2 \land \ldots \land \text{Property}_n
$$

In Inference the specification is defined as a [context](./definitions.md#101-context). Every context is then compiled into a [module](./terms-and-definitions.md#module) and to a [proof-unit](./terms-and-definitions.md#proof-unit). This modular structure enables Inference super-power of super-specifications. Where the super-specification is a set of specifications that are combined as a conjunction of all specifications.

$$
\text{Super-specification} = \text{Specification}_1 \land \text{Specification}_2 \land \ldots \land \text{Specification}_n
$$

Super specification (SS) allows to describe the whole blockchain behavior and the behaviour of a DApp in a context of its direct or indirect interaction with other DApp in the system. 

## 5.2 Specification is not a program

Despite of using Inference programming language, a specification is a formula, not a program. Inference is designed to be very similar to usual imperative programming languages, although it is way to conviniently write a specification.

However; the imperative flavor of Inference unlocks another Inference super-power: writing formal specification in Inference is as easy as write unit tests in any other common programming language. This is a huge advantage because it allows to write a specification in a way that is very close to the way the developers are used to write the code.

The key difference between unit tests and properties defined by Inference is that unit tests check the correctness at the certain point (a set of particular inputs) while the properties defined by Inference check the correctness for **all** possible inputs.

For example, if you want to TODO @Keyholder

```inference
```

## 5.3 Execution model

Now, as we see, the spec is not an execution unit, because it is not compiled to the bytecode for any platform. The target platform of `infc` is a theorem prover. The [Inferara execution theory](TODO) enables reasoning about the program in a way it has been executed on a certain platform. As of now, the execution platform Inferara theory covers is [WASM](https://webassembly.org/).

Essentially, the theory describes how a VM works, its memory state and operations. So it contains a description of how operations are executed, how the memory is managed, how the stack is used, and so on. It worth higlighting that the proof theory is not similar to the symbolic execution. The symbolic execution technique actually executes commands and works with the memory state, while the proof theory is a way to describe what happens with memory after the command is executed without having this memory in the real world.

For the illustration we can draw all known formula of energy $E=mc^2$. So this formula is a property of one aspect of how the universe works. In order to prove this formula, other formulas and theories are used. But the formula itself is not proven by the experiment simply because an experiment cannot cover all possible combinations of $m$ and $c$ to ensure the formula is correct on all possible values.

So here the Inference proof is a proof by mathematics and an experimental approach is a symbolic execution.

## 5.4 Non-deterministic execution

TBD @Keyholder

## 5.5 Platform-specific execution

Blockchains are complex system that have massive codebases and a various features and capabilities like different consensus algorithms, finality algorthims and so on. Keeping this fact in mind, we can see that it is barely possible to reproduce the whole blockchain in terms of the proof theory as it is done for such platforms like [Compcert](https://github.com/AbsInt/CompCert).

The approach that is used in Inference is as follows: the proof theory consists of two parts, the execution platform theory and a set of axioms that describe the blockchain behavior. The axioms are the set of properties that are stated to be true for the blockchain by the blockchain design and implementation.

So when Inference reasons about the DApp, it reasons about the DApp itself and the blockchain API that is used by the DApp is covered by the axiomatic theory. We can think about this API-axioms in a way of a sdk for a platform that we are using as it is provided by the platform developers.

## 5.6 Poly-blockchain design

Inference is a low-level language in by nature, because properties definitions must be straigtforward and inumabiguous. From the proof-unit viewpoint, the execution model follows the command set of a certain platform, for example WASM. So it means that if a blockchain uses WASM as a compilation target, then compiled DApp modules can be linked to the Inference modules and proofs can be constructed.

For the reference, the following blockchains uses WASM as a compilation target:

- Solana [CosmWasm](https://book.cosmwasm.com/index.html)
- Polkadot [Ink!](https://use.ink/smart-contracts-polkadot/)
- Arbitrum [Stylus](https://arbitrum.io/stylus)
- [Near](https://docs.near.org/build/smart-contracts/what-is)
- [Fluent](https://docs.fluentlabs.xyz/learn)

Infc is [designed](./general-description.md#compiler-design) to be modular and it is possible to add more platforms support in the future. So the theory can be written (or extended) to use a platform variation or a new platforms (EVM, RISK-V, etc.) can be formulated. So the front-end - Inference programming language remain the same.

## 5.7 Automated reasoning

The proof artifact `infc` generates is a Coq module that contains axioms and theorems for the properties that are stated in the `.inf` files. But it would be non-trivial to manually prove all set of theorems that are generated by the compiler. So the proof automation is another super power of Inference.

TODO @Keyholder

> [!IMPORTANT]
> The proof automation is not a silver bullet. It is not always possible to prove all the properties automatically. In this case, the meaningful diagnostics is provided.

## 5.8 Correctness certificate

Having all described above the reasonable question is if the output of Inference and its proof is reliable and proofs defined properties for the given code and nothing else. Simply if it makes sense to trust the proof.

The answer is yes, because the proof is a Coq module that is a formal proof that can be checked by the Coq theorem prover. The Coq theorem prover is a well-known and widely used tool in the formal verification community especially in academia.

Since the final artifact of `infc` is a Coq file `.v` it can be checked by the Coq theorem prover either automatically (no erros, unsoundness, or open goals) or manually for everyone who is interested in the proof correctness, so they can open the file and read it.

Henceforth, correctness tracking is also available, so we can check if the proof for the code at the state $S$ is equivalent to the proof for the code at the state $S'$.

There is a [database](TODO) of correctness certificates that Inferara holds and it is possible to ensure if a given code has been proven before and if the proof is still valid. 
