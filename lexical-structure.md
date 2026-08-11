# 4 Lexical structure

## 4.1 Comments

### 4.1.1 Description

Comments are used to document the code and are ignored by the compiler. Inference supports double-slash comments `//` and tripple-slash `///` docstrings interpreted also as comments.

### 4.1.2 Examples

```inference
/// This is a docstring
//  This is a line comment
```

## 4.2 Keywords

- `fn`
- `external`
- `let`
- `mut`
- `forall`
- `exists`
- `@`
- `assume`
- `unique`
- `loop`
- `return`
- `from`
- `use`
- `pub`
- `mod`
- `if`
- `break`
- `else`
- `struct`
- `enum`
- `type`
- `spec`
- `const`
- `assert`
- `self`

## 4.3 Identifiers

### 4.3.1 Description

An identifier is a sequence of characters that is used to name variables, functions, and other entities in the program. Identifiers must start with a letter or an underscore and can contain letters, digits, and underscores.

### 4.3.2 Examples

```inference
let a: u32 = 42;
let _ident : i64 = 42;
```

## 4.4 Reserved identifiers

- `constructor`
- `proof`
- `typeof`
- `uzumaki`

## 4.5 Qualified identifiers

### 4.5.1 Description

A qualified identifier is a sequence of identifiers separated by `::`. It is used to refer to functions in structs and contexts.

### 4.5.2 Examples

```inference
let a: spec::AuctionSpec = spec::AuctionSpec::new();
```

## 4.6 Member access

### 4.6.1 Description

The member access operator `.` is used to access fields and methods of a struct.

### 4.6.2 Examples

```inference
struct Account {
    address: i32;

    fn new(addr: i32) -> Account {
        return Account { address: addr };
    }
}

fn main() {
    let a: Account = Account::new(42);
    let b: i32 = a.address;
}
```

## 4.7 Literals

### 4.7.1 Bool

#### 4.7.1.1 Description

`bool` is a boolean type that can have one of two values: `true` or `false`.

#### 4.7.1.2 Examples

```inference
let a: bool = true;
let b: bool = false;
```

### 4.7.2 Unit

#### 4.7.2.1 Description

`unit` is a [type](./types.md#61-unit) that has only one value: `()`. It is used to represent the absence of a value.

#### 4.7.2.2 Examples

```inference
let a: unit = ();
```

### 4.7.3 Numeric

#### 4.7.3.1 Description

A numeric literal is a sequence of decimal digits, optionally preceded by the unary `-` sign for negation. The literal's type is determined by the context — typically the explicit type annotation on the surrounding declaration or the expected type of the surrounding expression.

The sign, when present, belongs to the literal and must be written immediately against the first digit: no whitespace or comment may come between them. `-7` is one numeric literal, whereas `- 7` is not a literal at all but the unary negation operator (see [8.5 Unary Operators](./expressions.md#85-unary-operators)) applied to the literal `7`, and is ill-formed. A negative number therefore has exactly one spelling, and what a program denotes never depends on whitespace. The distinction is not cosmetic: a signed type's minimum has no positive counterpart in that type, so it is expressible only as a literal carrying its own sign — `-128` is an `i8`, while `128` is not.

A `-` written immediately before a digit is part of the literal only where a literal may begin. Where it directly follows an expression it is the binary subtraction operator (see [8.6 Binary Operators](./expressions.md#86-binary-operators)), and spacing around it is free: `value-1`, `value -1`, and `value - 1` all subtract `1` from `value`. Where no expression has just ended — after `=`, `(`, `[`, `,`, `return`, or another operator — `-1` is the negative literal.

#### 4.7.3.2 Examples

```inference
let a: i32 = 42;
let b: u64 = 1000;
let c: i32 = -7;    // a negative literal: the sign is attached
let d: i8 = -128;   // the minimum of `i8`, expressible only this way
let e: i32 = c-1;   // subtraction: the `-` follows an expression
let f: i32 = -c;    // negation of a variable, which is not a literal
```

The following is ill-formed, because the sign is separated from the digits:

```inference
let g: i32 = - 7;
```

## 4.8 Right arrow

### 4.8.1 Description

The right arrow `->` is used to specify the return type of a function.

### 4.8.2 Examples

```inference
fn add(a: u32, b: u32) -> u32 {
    return a + b;
}
```

## 4.9 Terminator

### 4.9.1 Description

The terminator `;` is used to separate statements in Inference.

### 4.9.2 Examples

```inference
let a: u32 = 42;
```

## 4.10 Braces

### 4.10.1 Curly braces

#### 4.10.1.1 Description

Curly braces `{}` are used to define blocks of code.

See also: [Statements](./statements.md#92-block)

#### 4.10.1.2 Examples

```inference
fn foo() {
    /// code block
}
```

### 4.10.2 Parentheses

#### 4.10.2.1 Description

Parentheses `()` are used to group expressions and arguments in function calls. Also a `()` outside of a function definition spec is interpreted as a single token and is used to represent the unit type.

See also: [Functions](./functions.md#111-function-definition)
See also: [Types](./types.md#61-unit)

#### 4.10.2.2 Examples

```inference
fn foo(a: u32, b: u32) -> u32 {
    return a + b;
}
```

```inference
let a: () = ();
```

### 4.10.3 Square brackets

#### 4.10.3.1 Description

Square brackets `[]` are used to define arrays and address individual elements of an array.

See also: [Types](./types.md#63-array)

#### 4.10.3.2 Examples

```inference
let a: [u32; 3] = [1, 2, 3];
let b: u32 = a[0];
```

### 4.10.4 Prime symbol

#### 4.10.4.1 Description

Prime symbol `'` is used to define type parameters.

See also: [Types](./types.md#65-user-defined-types)

#### 4.10.4.2 Examples

```inference
fn foo T' (a: T') {
    // code block
}
```

## 4.11 Visibility

### 4.11.1 Description

The `pub` keyword marks a top-level definition as exported from the compiled module. Items without `pub` are private to the compilation unit and may be referenced only from inside the same source file. Spec functions are never exported even when declared `pub` — see [§10.5 Spec](./definitions.md#105-spec).

`pub` is currently accepted on `fn`, `struct`, and `enum` definitions.

### 4.11.2 Examples

```inference
pub fn add(a: i32, b: i32) -> i32 {
    return a + b;
}

pub struct Point {
    x: i32;
    y: i32;
}

pub enum Color {
    Red,
    Green,
    Blue,
}
```

---

[<kbd><br>⏮️ General description<br><br></kbd>](./general-description.md)
[<kbd><br>⏭️ Basic concepts<br><br></kbd>](./basic-concepts.md)
