# 2 Terms and Definitions

## 2.1 DApp

Decentralized Applications (DApps) are applications that run on a decentralized network of computers, such as a blockchain. Since terminology can differ (e.g., "smart contract"), in this document, "DApp" refers to an application that is a part of a blockchain network.

## 2.2 `infc`

Inference Compiler (`infc`) is a tool that compiles Inference programs into proof code. The compiler is responsible for parsing the Inference code, type-checking it, and generating the output code. The source code is located in the [Inference](https://github.com/Inferara/inference) repository.

## 2.3 Module

A module is an assembled distributable unit of code that can be used independently or by other modules and platforms. In this document, a module can refer to a single `.inf` file or a set of `.inf` files that are compiled together, WASM modules, or other platform-specific modules.

## 2.4 Theory

A theory is a set of definitions, inductive types, axioms, theorems, and proofs that describe the behavior of a system. A theory is written in theorem-prover language files and can consist of multiple files.

## 2.5 Proof-Unit

A proof-unit is a [module](./terms-and-definitions.md#module) of proof code generated from the Inference spec and contains attached required theories to build the proof.

---

[<kbd><br>⏮️ Introduction<br><br></kbd>](./introduction.md)
[<kbd><br>⏭️ General description<br><br></kbd>](./general-description.md)
