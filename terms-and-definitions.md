# 2 Terms and definitions

## DApp

Decentralized Applications are applications that run on a decentralized network of computers, such as a blockchain. Since the terminilogy can differ (Smart contract, most likely) from platform to platform, in this document DApp is used in a meaning of an application that is a part of a blockchain network.

## infc

Inference Compiler is a tool that compiles Inference programs into proof code. The compiler is responsible for parsing the Inference code, type-checking it, and generating the output code. The source code is located in the [Inference](https://github.com/Inferara/inference) repository.

## module

A module is an assembled distributable unit of code that can be used itself or by other modules and platforms. In this document, a modules can be used to refer to a single `.inf` file or a set of `.inf` files that are compiled together, WASM modules or other platform-specific modules.

## Theory

Theory is a set of definitions, inductive types, axioms, theorems, and proofs that describe the behavior of a system. A theory is written in `.v` files and can consist of multiple files.

## proof-unit

A proof-unit is a [module](./#module) of proof code that is generated from the Inference spec and contains attached required theories to build the proof.

## lval and rval expressions

In Inference, an expression can be either an lval or an rval expression. An lval expression is an expression that can appear on the left-hand side of an assignment, while an rval expression is an expression that can exclusively appear on the right-hand side of an assignment.
