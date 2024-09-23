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
