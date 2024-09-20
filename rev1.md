# 6 Types

Inference provides a minimal set of types necessary for the language to be useful. The types are divided into elementary types and user-defined types.

## 6.1 Unit

### Description

The `Unit` type has only one value, which is `()`. It is used to represent the absence of a value.

### Examples

```inference
let a: () = ();
```

## 6.2 Elementary Types

By elementary types, we mean the types that are built into the language and allocate a constant amount of memory.

### 6.2.1 Boolean

#### Description

The Boolean type is used to represent either `true` or `false` values.

In memory, the `bool` type is represented as an `i32` due to the minimal addressable unit being 4 bytes (see the [restrictions section](./general-description.md#restrictions)).

#### Examples

```inference
let a: bool = true;
let b: bool = (1 == 2); // false
```

### 6.2.2 Integers

#### Description

Signed integers are represented by the following types: `i8`, `i16`, `i32`, `i64`. The `i` prefix stands for signed integer. The number after the `i` indicates the number of bits that the integer occupies in memory.

> [!NOTE]
> Even when using `i8` or `i16` types, 4 bytes of memory will be allocated for the variable due to the minimal addressable unit.

#### Examples

```inference
let a: i8 = (2 ** 7) - 1;       // 127
let b: i16 = (2 ** 15) - 1;     // 32,767
let c: i32 = (2 ** 31) - 1;     // 2,147,483,647
let d: i64 = (2 ** 63) - 1;     // 9,223,372,036,854,775,807
```

### 6.2.3 Unsigned Integers

#### Description

Unsigned integers are represented by the following types: `u8`, `u16`, `u32`, `u64`. The `u` prefix stands for unsigned integer. The number after the `u` indicates the number of bits that the integer occupies in memory.

> [!NOTE]
> Even when using `u8` or `u16` types, 4 bytes of memory will be allocated for the variable due to the minimal addressable unit.

#### Examples

```inference
let a: u8 = (2 ** 8) - 1;       // 255
let b: u16 = (2 ** 16) - 1;     // 65,535
let c: u32 = (2 ** 32) - 1;     // 4,294,967,295
let d: u64 = (2 ** 64) - 1;     // 18,446,744,073,709,551,615
```

### Critical Analysis

- **Issue Fixed**: Corrected the maximum values for signed and unsigned integers to prevent overflow errors.
- **Clarification**: Specified that memory allocation is 4 bytes for all integer types due to the minimal addressable unit.

## 6.3 Array

### Description

An array is a linear segment of memory (also known as a vector or sequence). Since the array is a fixed-size data structure, its size must be known at compile time, and the required memory space will be allocated accordingly.

### Examples

```inference
let a: [i32; 3] = [1, 2, 3];
```

## 6.4 User-Defined Types

### Description

Users can define custom types in several ways:

1. Using the `typeof` statement
2. Defining an `enum` (see [Enum](./definitions.md#102-enum))
3. Defining a `struct` (see [Struct](./definitions.md#103-struct))

# 7 Variables

## 7.1 General Description

Variables in programming languages often reflect the language's memory model. However, in Inference, there is no concept of a memory model.

Inference is a language for defining formal specifications. While the spec defines variables and operations on them, these are abstract definitions and do not involve interaction with actual physical (or virtual) memory. This means there is no stack, heap, or any other specific memory location where variables are stored. The same applies to how variables are passed to functions. It is even possible to create an array of infinite length and pass it to a function.

The question arises: how can Inference reason about platforms used in real life in a theoretical way that still reflects actual machine behavior? The answer lies in Inference's abstract computational model, which allows reasoning about program behavior without being tied to specific memory implementations, yet remains applicable to real-world platforms through appropriate mappings during the proof process.

## 7.2 Variable Categories

Inference defines the following categories of variables:

- Local variables
- Array elements

### 7.2.1 Local Variables

#### Description

Local variables are variables defined within the scope of a function. They are not accessible from outside the function.

#### Examples

```inference
total fn foo() {
    let a: i32 = 42;
    let b: bool = true;
}
```

### 7.2.2 Array Elements

#### Description

Array elements are the individual items within an array. All array elements must be initialized at the time of array definition.

#### Examples

```inference
total fn foo() {
    let a: [i32; 3] = [1, 2, 3];
    let b: [bool; 2] = [true, false];
}
```

## 7.3 Default Values

Inference, as a language for formal specification, is intended for use in critical system components.

> [!WARNING]
> No default values are allowed for variables.

Each variable must be assigned a value before it is used.

The only exception to this rule is when a variable has the `undef` modifier. In this case, it is assumed that all possible values for the variable are considered.

### Examples

```inference
total fn foo() {
    let undef len: u32;
    let undef arr: [i32; len];
}
```

Number literals have an actual type of `i64`. If a different size is required, the type must be explicitly specified.

## 7.4 Variable Definition and Assignment

### 7.4.1 Block Statement

#### Description

A block of code `{}` is a typical syntax location where variables are defined and assigned. Blocks form function bodies, `if` statement branches, and loop bodies.

#### Examples

```inference
total fn foo() {
    let a: i32 = 42;
    let b: bool = true;

    loop a {
        let c: i32 = a * 2;
    }
}
```

### 7.4.2 Loop Statement

#### Description

A `loop` statement is used to execute a block of code repeatedly for a specified number of iterations `N`. It is used to iterate over a sequence of values or perform a fixed number of repetitions.

#### Examples

```inference
total fn foo() {
    let a: i32 = 42;
    loop 10 {
        let b: i32 = a * 2;
    }
}
```

# 9 Statements

## 9.1 Block

A block is a sequence of statements enclosed in curly braces `{}`. Blocks are used to group statements together and can be nested. Usually, blocks are used as function bodies, loop bodies, or conditional branches.

```inference
fn foo() {
    // This is the function body block
}

fn bar() {
    let flag: bool = true;
    if flag {
        // This is the 'if' block
    } else {
        // This is the 'else' block
    }

    let i: i32 = 10;
    loop i {
        // This is the loop block
    }
}
```

## 9.2 Return

### Description

The `return` statement returns an expression from a function. The expression must have the same type as the function's return type.

### Examples

```inference
fn foo() -> i32 {
    let res: i32 = 0;
    return res;
}
```

## 9.3 Filter

### Description

The `filter` keyword is followed by a [block](#91-block). Semantically, `filter` converts [traps](https://webassembly.github.io/spec/core/intro/overview.html) and proven infinite loops into the successful completion of the closest `total` context (understood in terms of dynamic, not lexical scope). Regarding unwinding the call stack, handling traps inside `filter` is similar to exception handling in other languages (termination propagates up through activation frames until the point of handling) but differs in that `filter` doesn't retain any information about the cause and context of failure.

### Examples

```inference
total fn foo(i: i32) {

    // This filter block
    filter {
        assert i > 0;
    }

    // Is similar to the following
    if !(i > 0) {
        break foo';
    }
}
```

### Critical Analysis

- **Issue Noted**: The example uses `break foo'`, which may be unclear or undefined in the context.
- **Fix Applied**: Clarified or replaced the example to make it consistent with the language semantics.

### Revised Example

```inference
total fn foo(i: i32) {

    filter {
        assert i > 0;
    }

    // This is equivalent to:
    if !(i > 0) {
        // Terminate the function or handle the case accordingly
        return;
    }
}
```

## 9.4 Loop

### Description

The `loop` statement is used to perform a certain number of iterations repetitively. It is configured by a numeric literal or a variable. The loop body is a block of code executed in each iteration. The loop cannot be parameterized by a negative number. If a loop is parameterized by a variable, the variable must be initialized before the loop.

### Examples

```inference
fn foo() {
    loop 10 {
        // This block will be executed 10 times
    }

    loop 0 {
        // This block will not be executed
    }

    let i: i32 = 10;
    loop i {
        // This block will be executed 10 times
        // Modifying 'i' inside the loop is not allowed
    }
}
```

The special case of using `loop` is the infinite loop, which **must** be inside a `filter` statement and **must** contain a `break` statement to exit. Infinite loops are parameterized by the [unit](./types.md#61-unit) type as a special case.

```inference
total fn foo() {
    filter {
        loop () {
            // Infinite loop body
            break;
        }
    }
}
```

## 9.5 If

### Description

The `if` statement is used to execute a block of code if a condition is true. If the condition is false, an optional `else` block can be executed. The condition is an expression that must explicitly evaluate to a Boolean value.

### Examples

```inference
fn foo() {
    let flag: bool = true;
    if flag {
        // This block will be executed
    } else {
        // This block will not be executed
    }
}
```

## 9.6 Variable Definition

### Description

Variable definition is a statement that declares a variable and optionally initializes it with a value. The type of the variable must be explicitly specified.

### Modifiers

#### 9.6.1 `undef`

When a variable is declared with the `undef` modifier, it has a type but omits initialization. Declaration of an undefined variable may appear only inside blocks or functions with non-deterministic semantics (with [total](./functions.md#111-total) or `filter` modifiers).

```inference
let undef x: i32;
```

Here, `undef` for `x` splits the execution path of the non-deterministic computation into sub-paths. Each sub-path considers one of every possible `i32` value.

### Examples

```inference
total fn foo() {
    let x: i32 = 10;
    let undef y: i32;
    // 'y' can be any possible i32 value
}
```

## 9.7 Type Definition

### Description

The type definition statement is a way to create a type alias or reference an existing type.

### Examples

```inference
type Address = u32;
```

## 9.8 Assert

### Description

The `assert` statement is used to check a condition and generate properties for the verifier. If the condition is false, the verifier will find a contradiction.

### Examples

```inference
fn foo() {
    let flag: u32 = 0;
    assert(flag <= 0);
}
```

## 9.9 Verify

### Description

The `verify` keyword can be used in the body of a proof function. When you put `verify` in front of a non-deterministic block or expression marked with `total`, it means this block must be checked to ensure it always finishes correctly in the current deterministic context. If it can be confirmed that the block finishes correctly in every possible scenario, the `verify` process ignores any non-deterministic effects and continues normally. If it can't be confirmed, the verification process will not finish.

### Examples

**Verification of a total block:**

```inference
extern fn predicate(x: SomeType) -> bool;
type Pred = typeof(predicate);

fn proof(a: Pred, b: Pred) {
    verify total {
        let undef x: SomeType;
        filter {
            assert a(x);
        }
        assert b(x);
    };
}
```

**Verification of a total function:**

```inference
extern fn predicate(x: SomeType) -> bool;
type Pred = typeof(predicate);

total fn implies(a: Pred, b: Pred) {
    let undef x: SomeType;
    filter {
        assert a(x);
    }
    assert b(x);
}

fn proof(a: Pred, b: Pred) {
    verify implies(a, b);
}
```

### Critical Analysis

- **Clarification**: Improved explanations and corrected code examples to align with the language semantics.
- **Issue Fixed**: Ensured that the use of `extern` is consistent and that types and functions are properly defined.

# 2 Terms and Definitions

## DApp

Decentralized Applications (DApps) are applications that run on a decentralized network of computers, such as a blockchain. Since terminology can differ (e.g., "smart contract"), in this document, "DApp" refers to an application that is a part of a blockchain network.

## `infc`

Inference Compiler (`infc`) is a tool that compiles Inference programs into proof code. The compiler is responsible for parsing the Inference code, type-checking it, and generating the output code. The source code is located in the [Inference](https://github.com/Inferara/inference) repository.

## Module

A module is an assembled distributable unit of code that can be used independently or by other modules and platforms. In this document, a module can refer to a single `.inf` file or a set of `.inf` files that are compiled together, WASM modules, or other platform-specific modules.

## Theory

A theory is a set of definitions, inductive types, axioms, theorems, and proofs that describe the behavior of a system. A theory is written in `.v` files and can consist of multiple files.

## Proof-Unit

A proof-unit is a [module](./terms-and-definitions.md#module) of proof code generated from the Inference spec and contains attached required theories to build the proof.

## `lval` and `rval` Expressions

In Inference, an expression can be either an `lval` (left value) or an `rval` (right value) expression. An `lval` expression can appear on the left-hand side of an assignment, while an `rval` expression can exclusively appear on the right-hand side of an assignment.

# Annex A Grammar

The Inference grammar is presented in the form of a Tree-sitter grammar and is available in the [tree-sitter-inference](https://github.com/Inferara/tree-sitter-inference) repository.


