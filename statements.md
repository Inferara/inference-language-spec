# 9 Statements

## 9 Block

Block is a sequence of statements enclosed in curly braces `{}`. Blocks are used to group statements together. Blocks can be nested. Usually, blocks are used as function bodies, loop bodies, or conditional arms.

```inference
fn foo() {
    /// this is the body block
}

fn bar() {
    let flag: bool = true;
    if flag {
        /// this is the if block
    } else {
        /// this is the else block
    }

    let i: i32 = 10;
    let acc: i32 = 0;
    for (; i > 0; i--) {
        /// this is the for block
    }
}
```

## 9 Return

### Description

Return statement returns an expression from a function. The expression must have the same type as the function return type.

### Examples

```inference
fn foo() -> i32 {
    let res: i32 = 0;
    return res;
}
```

## 9 Filter

### Description

TBD @Keyholder

The filter is defined as a `filter` keyword followed by a [block](#9-block).

### Examples

```inference
TODO @Keyholder
```

## 9 For

### Description

The `for` statement is used to perform a certain number of iterations in a loop. It is configured by an inlined range syntax. As boundaries can be used as either constants or identifiers.

### Examples

```inference
fn foo() {
    for 0..10 {
        ///
    }

    let a: u32 = 0;
    let b: u32 = 10;

    for a..b {
        ///
    }
}
```

## 9 If

### Description

If statement is used to execute a block of code if a condition is true. If the condition is false, an optional else block can be executed. The condition is an expression that must to be explicitly evaluated to a boolean value.

### Examples

```inference
fn foo() {
    let flag: bool = true;
    if flag {
        /// this block will be executed
    } else {
        /// this block will not be executed
    }
}
```

## 9 Variable Definition

### Description

Variable definition is a statement that declares a variable and optionally initializes it with a value. The type of the variable must be explicitly specified.

### Modifiers

### 9.1.1 `undef`

When a variable is declared with the `undef` modifier, such definition has a type but omits initialization. Declaration of an undefined variable may appear only inside blocks or functions with non-deterministic semantics (with [total](./functions.md#1111-total-keyword) or `filter` modifiers).

```
let undef x: i32;
```
where `undef` for `x` splits the execution path of nondeterministic computation into sub-paths. Each sub-path returns one of every possible `Type` instances.

### Examples

```inference
fn total foo() {
    let x: i32 = 10;
    let undef y: i32;
}
```

## 9 Type Definition

### Description

Type definition statement is a way to get a reference to a type. It can be used to define type aliases or get a reference to a type.

### Examples

```inference
type Address = typeof(u32);
```

## 9 Assert

### Description

Assert statement is used to check a condition and generate properties for the verifier. If the condition is false, the verifier will find a contradiction.

### Examples

```inference
fn foo() {
    let flag: u32 = 0;
    assert(flag <= 0);
}
```

## 9 Verify

TODO @Keyholder

### Description

The keyword `verify` can be used in the body of a proof function. When you put `verify` in front of a non-deterministic block or expression marked with `total`, it means this block must be checked to ensure it always finishes correctly in the current deterministic context. If it can be confirmed that the block finishes correctly in every possible scenario, the `verify` process ignores any non-deterministic effects and continues normally. If it can't be confirmed, the verification process will not finish.

### Example

Verification of total block:

```
external fn predicate (SomeType) -> bool;
type Pred = typeof(predicate);

fn proof(a: Pred, b: Pred) {
  verify total {
    let undef x: SomeType;
    filter assert a(x);
    assert b(x);
  };
}
```

Verification of total function:

```
external fn predicate (SomeType) -> bool;
type Pred = typeof(predicate);

total fn implies(a: Pred, b: Pred) {
  let undef x: SomeType;
  filter assert a(x);
  assert b(x);
}

fn proof(a: Pred, b: Pred) {
  verify implies(a, b);
}
```
