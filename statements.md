# 9 Statements

## 9.1 Block

### 9.1.1 Description

A block is a sequence of statements enclosed in curly braces `{}`. Blocks are used to group statements together and can be nested. Usually, blocks are used as function bodies, loop bodies, or conditional branches.

### 9.1.2 Examples

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

### 9.2.1 Description

The `return` statement returns an expression from a function. The expression must have the same type as the function's return type.

### 9.2.2 Examples

```inference
fn foo() -> i32 {
    let res: i32 = 0;
    return res;
}
```

## 9.3 Filter

### 9.3.1 Description

The `filter` keyword is followed by a [block](#91-block). Semantically, `filter` converts [traps](https://webassembly.github.io/spec/core/intro/overview.html) and proven infinite loops into the successful completion of the closest `total` context (understood in terms of dynamic, not lexical scope). Regarding unwinding the call stack, handling traps inside `filter` is similar to exception handling in other languages (termination propagates up through activation frames until the point of handling) but differs in that `filter` doesn't retain any information about the cause and context of failure.

### 9.3.2 Examples

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

### 9.4.1 Description

The `loop` statement is used to perform a certain number of iterations repetitively. It is configured by a numeric literal or a variable. The loop body is a block of code executed in each iteration. The loop cannot be parameterized by a negative number. If a loop is parameterized by a variable, the variable must be initialized before the loop.

### 9.4.2 Examples

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

### 9.5.1 Description

The `if` statement is used to execute a block of code if a condition is true. If the condition is false, an optional `else` block can be executed. The condition is an expression that must explicitly evaluate to a Boolean value.

### 9.5.2 Examples

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

### 9.6.1 Description

Variable definition is a statement that declares a variable and optionally initializes it with a value. The type of the variable must be explicitly specified.

### 9.6.2 Modifiers

#### 9.6.2.1 `undef`

When a variable is declared with the `undef` modifier, it has a type but omits initialization. Declaration of an undefined variable may appear only inside blocks or functions with non-deterministic semantics (with [total](./functions.md#111-total) or `filter` modifiers).

```inference
let undef x: i32;
```

Here, `undef` for `x` splits the execution path of the non-deterministic computation into sub-paths. Each sub-path considers one of every possible `i32` value.

### 9.6.3 Examples

```inference
total fn foo() {
    let x: i32 = 10;
    let undef y: i32;
    // 'y' can be any possible i32 value
}
```

## 9.7 Type Definition

### 9.7.1 Description

The type definition statement is a way to create a type alias or reference an existing type.

### 9.7.2 Examples

```inference
type Address = u32;
```

## 9.8 Assert

### 9.8.1 Description

The `assert` statement is used to check a condition and generate properties for the verifier. If the condition is false, the verifier will find a contradiction.

### 9.82 Examples

```inference
fn foo() {
    let flag: u32 = 0;
    assert(flag <= 0);
}
```

## 9.9 Verify

### 9.9.1 Description

The `verify` keyword can be used in the body of a proof function. When you put `verify` in front of a non-deterministic block or expression marked with `total`, it means this block must be checked to ensure it always finishes correctly in the current deterministic context. If it can be confirmed that the block finishes correctly in every possible scenario, the `verify` process ignores any non-deterministic effects and continues normally. If it can't be confirmed, the verification process will not finish.

### 9.9.2 Examples

**Verification of a total block:**

```inference
extern fn predicate(x: SomeType) -> bool;

fn proof() {
    verify total {
        let undef x: SomeType;
        filter {
            assert predicate(x);
        }
        assert predicate(x);
    };
}
```

In this example the verifier will check if the block finishes correctly in every possible scenario.

**Verification of a total function:**

```inference
extern fn predicate(x: SomeType) -> bool;

total fn implies() {
    let undef x: SomeType;
    filter {
        assert predicate(x);
    }
    assert predicate(x);
}

fn proof() {
    verify implies();
}
```

---

[<kbd><br>⏮️ Expressions<br><br></kbd>](./expressions.md)
[<kbd><br>⏭️ Definitions<br><br></kbd>](./definitions.md)
