# 1 Introduction

The Web3 space is an area where every application's correctness and resistance to hackers' attacks are crucial. It is critical for both the application itself, because a security violation immediately breaks the trust of the users, and for the whole ecosystem, because an insecure set of applications running on the platform can shed a bad light on the entire platform.

> [!IMPORTANT]
> Web3 native applications work with users' assets and data. The correctness of the application is crucial for users' trust.

Since security is paramount for Web3 applications, exhaustive security assessment is a must. At Inferara, we see that during the evolution of blockchain technology, logical attacks have become the most dangerous and devastating. Modern tools and techniques can help detect technical errors (such as division by zero or similar issues), but they cannot deal with algorithmic weaknesses and corner cases.

Technical bugs and the overall work correctness can be tested by:

- Unit tests
- Fuzzing and invariants
- Integration tests

Unit tests are the most common and widely used. They help developers check if a given input to the program produces the expected output.

Fuzzing and invariants are used to check the program's behavior under a set of inputs in order to either check that the fuzzer is not able to find an input that makes the program produce unexpected output or, in the case of invariants, to check that the fuzzer is not able to find a set of inputs that turns the program into an invalid state. Both of these techniques use brute force to produce the input.

More sophisticated techniques, which fall under the umbrella term "formal methods," use mathematical logic (often first-order logic) to interpret a program flow in logical terms and deduce the correctness of these formulas according to the provided properties claimed for the program from the developer.

SMT solvers are the most common tools used in formal methods. They are used to check the satisfiability of logical formulas. Usually, the solvers (`z3`, `cvc4`, `yices`, etc.) are used in pair with symbolic execution engines that help to generate the logical formulas from the program code.

Model checking, especially the $TLA^+$ model checker, is another formal method used to check the correctness of the program. It is based on temporal logic and is used to check the correctness of algorithms for distributed systems.

Automated theorem proving is a technique used to prove the correctness of the program by using logical formulas and a set of theories, axioms, and rules of inference. The most common theorem provers are `Isabelle`, `Coq`, `Agda`, etc.

Inference uses the automated theorem proving technique to check the correctness of the program. It uses internally developed theories and internal techniques allowing automatically generating axioms and theorems along with the properties (defined in the spec) proofs. The Inference theory system is based on first-order logic, intuitionistic logic, Hoare logic, and extends it with non-deterministic computational theory used to describe an abstract virtual machine.

> [!NOTE]
> For more information, read Inferara [papers](https://inferara.com/papers).

Speaking about Inference as a special-purpose programming language, it is important to list significant considerations that were taken into account during the language design:

- Inference is intended to be a simple, concise, unambiguous, and easy-to-understand formal specification language.
- The language should have the feel of an imperative language and hide all the complexity of formal verification details.
- The language is intended for use in developing Web3 applications as a correctness specification language that ensures the application's properties and certifies the application's correctness.
- The language evolves in the direction of improving the power of automated deduction and being applicable to a wide range of blockchain architectures.
- Support for modern IDEs and code editors is very important.
