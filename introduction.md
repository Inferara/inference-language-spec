# 1 Introduction

Web3 space is the area where every application correctness and hackers attack resistance is crucial. It is crucial for both the application itself, because the security violation immediately breaks the trust of the users, and for the whole ecosystem, because the insecure set of applications running on the platform can shed a bad light on the whole platform.

> [!IMPORTANT]
> Web3 native applications work with users' assets and data. The correctness of the application is crucial for the users' trust.

Since the security is the gold for the Web3 application the exhausitve security assessing is a must. In Inferara we see that during the evolution of the blockchain technology the logical attacks became the most dangerous and devastating. Modern tools and techniques can help detect technical errors (such as zero division or similar) but they cannot deal with algorithmic weaknesses and cornes cases.

Technical bugs and the overall work correctness can be tested by:
- UnitTests
- Fuzzing and Invariant
- Intergration tests

UnitTests are the most common and widely used. They help developers check if a given input to the program produces the expected output.

Fuzzing and Invariant are used to check the program's behavior under the set of input in order to either check that the fuzzer is not able to find an input that makes the program produce unexpected outputr or, in the case of Invariant, to check that the fuzzer is not able to find a set of inputs that turns the program into an invalid state. Both of these techniques uses brute force to produce the input.

More complicated techniques that are named under the umbrella term "Formal methods" uses mathematical logic (the first-order logic, oftern) to interpret a program flow in logical terms and deduce the correctness of these formulas according to the provided properties, claimed for the program from the developer.

SMT solvers are the most common tools used in formal methods. They are used to check the satisfiability of the logical formulas. Usually, the solvers (`z3`, `cvc4`, `yices`, etc.) are used in pair with the symbolic execution engines that help to generate the logical formulas from the program code.

Model checking, especially the $TLA^+$ model checker, is another formal method that is used to check the correctness of the program. It is based on the temporal logic and is used to check the correctness of the alrogithm for distributed systems.

Automated theorem proving is a technique that is used to prove the correctness of the program by using the logical formulas and the set of theories, axioms, and rules of inference. The most common theorem provers are `Isabelle`, `Coq`, `Agda`, etc.

Inference uses the automated theorem proving technique to check the correctness of the program. It uses internally developed theories and internal techniques allowing automatically generate axioms and theorems along with the propetries (defined in the spec) proofs. Inference theory system is based on the first-order logic, intuitionistic logic, hoare logics and extent it with non-determenistic computational theory that describes is used to desribe an abstract virstual machine.

For more information, read Inferara [papers](https://inferara.com/papers).

Speaking about Inference as a special purpose programming language, it is important to list important considerations that were taken into account during the language design:

- Inference is intended to be a simple, concise, unambiguous, and easy-to-understand formal speficiation language.
- The language should have a taste of an imperative language and hide all the complexity of formal verification details.
- The language is intended for use in developing Web3 applications as a correctness specification language that ensures the application's properties and certifies the application's correctness.
- The language evolves in the directions of improving the power of automated deduction and beign applicaable to a wide range of blockchain arhitectures.
- Support for modern IDEs and code editors is very important.
