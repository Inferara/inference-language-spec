# 9 Statements

## 9.1 Block

### 9.1.1 Description

A block is a sequence of statements enclosed in curly braces `{}`. Blocks are used to group statements together and can be nested. Usually, blocks are used as function bodies, loop bodies, or conditional branches.

### 9.1.2 Examples

```inference
fn foo() {
    /// This is the function body block
}

fn bar() {
    let flag: bool = true;
    if flag {
        /// This is the 'if' block
    } else {
        /// This is the 'else' block
    }

    let i: i32 = 10;
    loop i {
        /// This is the loop block
    }
}
```

## 9.2 Return

### 9.2.1 Description

The `return` statement returns an expression from a function. The expression must have the same type as the function's return type.

### 9.2.2 Examples

```inference
fn foo() -> i32 {
    let res: i32 = 0;
    return res;
}
```

## 9.3 Forall

The `forall` keyword is followed by a [block](#91-block). Semantically, `forall` is a logical analogue of for all $\forall$ quantifier.

## 9.4 Exists

The `exists` keyword is followed by a [block](#91-block). Semantically, `exists` is a logical analogue of exists $\exists$ quantifier.

## 9.5 Assume

### 9.5.1 Description

The `assume` keyword is followed by a [block](#91-block). Semantically, `assume` converts [traps](https://webassembly.github.io/spec/core/intro/overview.html) and proven infinite loops into the successful completion of the closest `forall` block (understood in terms of dynamic, not lexical scope). Regarding unwinding the call stack, handling traps inside `assume` is similar to exception handling in other languages (termination propagates up through activation frames until the point of handling) but differs in that `assume` doesn't retain any information about the cause and spec of failure.

`assume` is not a representation of any logical quantifier and makes sense only when it is embedded into the `forall` block as a mechanism of execution paths (that are not satisfied with the pre-conditions) filtering.

### 9.5.2 Examples

```inference
fn foo(i: i32) -> () {

    assume {
        assert i > 0;
    }

    /// This is equivalent to:
    if !(i > 0) {
        /// Terminate the function or handle the case accordingly
        return ();
    }
}
```

## 9.6 Loop

### 9.6.1 Description

The `loop` statement is used to perform a certain number of iterations repetitively. It is configured by a numeric literal or a variable. The loop body is a block of code executed in each iteration. The loop cannot be parameterized by a negative number. If a loop is parameterized by a variable, the variable must be initialized before the loop.

### 9.6.2 Examples

```inference
fn loop_example() {
    loop 10 {
        /// This block will be executed 10 times
    }

    loop 0 {
        /// This block will not be executed
    }

    let i: i32 = 10;
    loop i {
        /// This block will be executed 10 times
        /// Modifying 'i' inside the loop is not allowed
    }
}
```

The special case of using `loop` is the infinite loop, which **must** be inside a `assume` statement and **must** contain a `break` statement to exit. Infinite loops are parameterized by the [unit](./types.md#61-unit) type as a special case.

```inference
fn infinite_loop_example() {
    assume {
        loop () {
            /// Infinite loop body
            break;
        }
    }
}
```

## 9.7 If

### 9.7.1 Description

The `if` statement is used to execute a block of code if a condition is true. If the condition is false, an optional `else` block can be executed. The condition is an expression that must explicitly evaluate to a Boolean value.

### 9.7.2 Examples

```inference
fn foo() {
    let flag: bool = true;
    if flag {
        /// This block will be executed
    } else {
        /// This block will not be executed
    }
}
```

## 9.8 Variable Definition

### 9.8.1 Description

Variable definition is a statement that declares a variable and optionally initializes it with a value. The type of the variable must be explicitly specified.

### 9.8.2 Value Modifiers

#### 9.8.2.1 Uzumaki

When a variable is declared with the `@` modifier, it has a type but omits initialization. Declaration of an undefined variable may appear only inside blocks or functions with non-deterministic semantics (with [forall](./functions.md#111-forall) or `assume` modifiers).

```inference
let x: i32 = @;
```

Here, `@` for `x` splits the execution path of the non-deterministic computation into sub-paths. Each sub-path considers one of every possible `i32` value.

### 9.8.3 Examples

```inference
fn foo() {
    let x: i32 = 10;
    let y: i32 = @;
    /// 'y' can be any possible i32 value
}
```

## 9.9 Type Definition

### 9.9.1 Description

The type definition statement is a way to create a type alias or reference an existing type.

### 9.9.2 Examples

```inference
type Address = u32;
```

## 9.10 Assert

### 9.10.1 Description

The `assert` statement is used to check a condition and generate properties for the verifier. If the condition is false, the verifier will find a contradiction.

### 9.10.2 Examples

```inference
fn foo() {
    let flag: u32 = 0;
    assert(flag <= 0);
}
```

---

[<kbd><br>⏮️ Expressions<br><br></kbd>](./expressions.md)
[<kbd><br>⏭️ Definitions<br><br></kbd>](./definitions.md)
