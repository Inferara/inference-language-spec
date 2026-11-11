# 9 Statements

## 9.1 Assign

### 9.1.1 Description

The assign statement is used to assign a value, or generally speaking, the result of an expression evaluation to a variable or the `lval` expression. The syntax for the assign statement is the variable name followed by the assignment operator (`=`) and the expression value.

### 9.1.2 Examples

```inference
fn example() -> u32 {
  let mut a: u32 = 42;
  a = 10;
  return a;
}
```

## 9.2 Block

### 9.2.1 Description

A block is a sequence of statements enclosed in curly braces `{}`. Blocks are used to group statements together and can be nested. Usually, blocks are used as function bodies, loop bodies, or conditional branches.

### 9.2.2 Examples

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

    let mut i: i32 = 0;
    loop i < 10 {
        /// This is the loop block
        i = i + 1;
    }
}
```

## 9.3 Return

### 9.3.1 Description

The `return` statement returns an expression from a function. The expression must have the same type as the function's return type.

### 9.3.2 Examples

```inference
fn foo() -> i32 {
    let res: i32 = 0;
    return res;
}
```

## 9.4 Forall

The `forall` keyword is followed by a [block](#92-block). Semantically, `forall` is a logical analog of for all $\forall$ quantifier.

## 9.5 Exists

The `exists` keyword is followed by a [block](#92-block). Semantically, `exists` is a logical analog of exists $\exists$ quantifier.

## 9.6 Assume

### 9.6.1 Description

The `assume` keyword is followed by a [block](#92-block). Semantically, `assume` converts [traps](https://webassembly.github.io/spec/core/intro/overview.html) and proven infinite loops into the successful completion of the closest `forall` block (understood in terms of dynamic, not lexical scope).

Regarding unwinding the call stack, handling traps inside `assume` is similar to exception handling in other languages (termination propagates up through activation frames until the point of handling) but differs in that `assume` doesn't retain any information about the cause and spec of failure.

`assume` is not a representation of any logical quantifier and makes sense only when it is embedded into the `forall` block as a mechanism of filtering execution paths (that are not satisfied with the pre-conditions).

### 9.6.2 Examples

```inference
fn foo(i: i32) -> () forall {

    assume {
        assert i > 0;
    }

    // This is equivalent to:
    if !(i > 0) {
        // Terminate the function or handle the case accordingly
        return ();
    }
}
```

## 9.7 Unique

### 9.7.1 Description

The `unique` keyword is followed by a [block](#92-block). The block succeeds if and only if exactly one assignment of values to the `@` variables it declares completes it: at least one assignment does, and any two assignments that do are equal.

The variables compared are those introduced by `let x: T = @;` statements written directly in the block. They must be scalar, and the block must declare at least one. Such a declaration inside an `if` arm, an `assume` block or a plain nested block of the `unique` block is an error. A `@` variable declared under a nested `forall` or `exists` block belongs to that quantifier and is not compared.

`unique` may be used as a block statement anywhere inside a specification function, as the kind of an `if` arm (`if c unique { ... }`), and as the body kind of a function (`fn f() unique { ... }`). It cannot be used in deterministic code.

### 9.7.2 Examples

```inference
spec Factors {
  // 6 has exactly one factor pair with 1 < p < q < 6.
  fn factor_pair() {
    unique {
      let p: u32 = @;
      let q: u32 = @;
      assert(p > 1);
      assert(p < q);
      assert(q < 6);
      assert(p * q == 6);
    }
  }

  // `z` belongs to the nested `exists` and is not compared:
  // the claim is about `x` alone.
  fn dismissed_draw(n: i32) forall {
    unique {
      let x: i32 = @;
      assert(x == n);
      exists {
        let z: i32 = @;
        assert(z == x);
      }
    }
  }
}
```

`factor_pair` holds because `(2, 3)` is the only pair that satisfies all four conditions. Without the bound `q < 6` it would not: the multiplication is 32-bit machine arithmetic, and pairs whose product wraps around to 6 would count as further solutions.

## 9.8 Loop

### 9.8.1 Description

The `loop` statement comes in two forms:

1. **Conditional loop**: `loop CONDITION { ... }` — evaluates the boolean condition before each iteration and continues as long as the condition is `true`. The condition is an expression that must evaluate to a boolean value.

2. **Infinite loop**: `loop { ... }` — repeats the loop body indefinitely. An infinite loop **must** contain a `break` statement to exit.

The `break` statement exits the innermost enclosing loop.

### 9.8.2 Examples

```inference
fn loop_example() {
    let mut i: i32 = 0;
    loop i < 10 {
        // This block will be executed 10 times
        i = i + 1;
    }

    loop false {
        // This block will not be executed
    }

    let mut n: i32 = 10;
    loop n > 0 {
        // This block will be executed 10 times
        n = n - 1;
    }
}
```

The infinite loop form uses `loop` without a condition. It **must** contain a `break` statement to exit.

```inference
fn infinite_loop_example() {
    let mut i: i32 = 0;
    loop {
        if i >= 100 {
            break;
        }
        i = i + 7;
    }
}
```

## 9.9 If

### 9.9.1 Description

The `if` statement is used to execute a block of code if a condition is true. If the condition is false, an optional `else` block can be executed. The condition is an expression that must explicitly evaluate to a Boolean value.

### 9.9.2 Examples

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

## 9.10 Variable Definition

### 9.10.1 Description

Variable definition is a statement that declares a variable and optionally initializes it with a value. The type of the variable must be explicitly specified.

### 9.10.2 Value Modifiers

#### 9.10.2.1 Uzumaki

When a variable is declared with `@` on its right-hand side, it carries no concrete initial value — instead, the expression splits the surrounding computation into a sub-path per value of the variable's type. The `@` expression may appear only inside a non-deterministic block — `forall`, `exists`, `assume`, or `unique` — or inside the body of a function whose body is itself such a block (e.g. a function marked with the [`forall`](./functions.md#11131-forall) modifier).

```inference
let x: i32 = @;
```

Here, `@` for `x` splits the execution path of the non-deterministic computation into sub-paths. Each sub-path considers one of every possible `i32` value.

`@` is an expression, not exclusively a `let`-initializer: it may appear anywhere a value of its inferred type is expected — for example as a function argument or a `return` operand. See also [§8.3 Function Call](./expressions.md#83-function-call).

```inference
fn sum_is_zero_or_not() forall {
    let y: i32 = sum(@, @);
    assert(y >= 0 || y < 0);
}
```

### 9.10.3 Examples

```inference
fn foo() {
    let x: i32 = 10;
    let y: i32 = @;
    // 'y' can be any possible i32 value
}
```

## 9.11 Type Definition

### 9.11.1 Description

The type definition statement is a way to create a type alias or reference an existing type.

### 9.11.2 Examples

```inference
type Address = u32;
```

## 9.12 Assert

### 9.12.1 Description

The `assert` statement is used to check a condition and generate properties for the verifier. If the condition is false, the verifier will find a contradiction.

In *compile* mode `assert(cond)` evaluates `cond` at runtime and traps via `unreachable` if `cond` is `false`. In *proof* mode the `assert` becomes a proof obligation: the verifier must show that no execution path can reach the `assert` with `cond == false`. See [§3.2 Compiler Design](./general-description.md#32-compiler-design) for the difference between the two modes.

### 9.12.2 Examples

```inference
fn foo() {
    let flag: i32 = 0;
    assert(flag <= 0);
}
```

---

[<kbd><br>⏮️ Expressions<br><br></kbd>](./expressions.md)
[<kbd><br>⏭️ Definitions<br><br></kbd>](./definitions.md)
