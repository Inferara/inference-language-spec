# Inference programming language specification

Inference is a domain-specific programming language designed to enable Web3 developers to formulate properties of the native application algorithms in a familiar format similar to how unit tests are written.

Inference allows formal proof of the correctness of the specified properties to be received in an automated way.

> [!IMPORTANT]
> Inference is a Web3 native applications-oriented, formal specification language.

This repository contains the specification of the Inference programming language. The specification is divided into several sections, each describing a specific aspect of the language. The language is designed to be simple and easy to learn, and its syntax is concise and similar to Rust.

## Table of contents

- [Foreword](./foreword.md)
- [§1](./introduction.md) Introduction
- [§2](./terms-and-definitions.md) Terms and definitions
- [§3](./general-description.md) General description
- [§4](./lexical-structure.md) Lexical structure
- [§5](./basic-concepts.md) Basic concepts
- [§6](./types.md) Types
  - [§6.1](./types.md#61-elementary-types) Elementary types
    - [§6.1.1](./types.md#611-boolean) Boolean
    - [§6.1.2](./types.md#612-i32) i32
    - [§6.1.3](./types.md#613-i64) i64
    - [§6.1.4](./types.md#614-u32) u32
    - [§6.1.5](./types.md#615-u64) u64
  - [§6.2](./types.md#62-array) Array
  - [§6.3](./types.md#63-user-defined-types) User-defined types
- [§7](./variables.md) Variables
- [§8](./expressions.md) Expressions
  - [§8.1](./expressions.md#81-verify) Verify
- [§9](./statements.md) Statements
  - [§9.1](./statements.md#91-variable-definition-statement) Variable Definition Statement
- [§10](./definitions.md) Definitions
  - [§10.1](./definitions.md#101-enum) Enum
  - [§10.2](./definitions.md#102-struct) Struct
- [§11](./functions.md) Functions
- [§12](./constants.md) Constants
- [§A](./grammar.md) Grammar
- [§B](./standard-library.md) Standard Library
- [§C](./comments.md) Comments
- [§D](./examples.md) Examples
- [§E](./bibliography.md) Bibliography
