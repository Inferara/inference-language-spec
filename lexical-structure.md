# 4 Lexical structure

## 4.1 Comments

### 4.1.1 Description

Comments are used to document the code and are ignored by the compiler. Inference supports triple-slash comments `///` only.

### 4.1.2 Examples

```inference
/// This is a comment
```

## 4.2 Keywords

- `fn`
- `external`
- `let`
- `mut`
- `total`
- `undef`
- `filter`
- `return`
- `from`
- `use`
- `for`
- `if`
- `break`
- `else`
- `struct`
- `enum`
- `type`
- `context`
- `const`
- `assert`
- `verify`


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
- `filter`
- `type`
- `ctx`
- `typeof`

## 4.5 Qualified identifiers

### 4.5.1 Description

A qualified identifier is a sequence of identifiers separated by `::`. It is used to refer to functions in structs and contexts.

### 4.5.2 Examples

```inference
let a: context::AuctionSpec = context::AuctionSpec::new();
```

## 4.6 Member access

### 4.6.1 Description

The member access operator `.` is used to access fields and methods of a struct.

### 4.6.2 Examples

```inference
struct Account {
    address: u32;

    fn new(addr: u32) -> Account {
        ctx.address = addr;
    }
}

total fn main() {
    let a: Account = Account::new(42);
    let b: u32 = a.address;
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

`unit` is a [type](./types.md#6-unit) that has only one value: `()`. It is used to represent the absence of a value.

#### 4.7.2.2 Examples

```inference
let a: unit = ();
```

## 4.8 Right arrow

### 4.8.1 Description

The right arrow `->` is used to specify the return type of a function.

### 4.8.2 Examples

```inference
total fn add(a: u32, b: u32) -> u32 {
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

See also: [Statements](./statements.md#9-block)

#### 4.10.1.2 Examples

```inference
fn foo() {
    /// code block
}
```

### 4.10.2 Parentheses

#### 4.10.2.1 Description

Parentheses `()` are used to group expressions and arguments in function calls. Also a `()` outside of a function definition context is interpreted as a single token and is used to represent the unit type.

See also: [Functions](./functions.md#11-function-definition)
See also; [Types](./types.md#6-unit)

#### 4.10.2.2 Examples

```inference
total fn foo(a: u32, b: u32) -> u32 {
    return a + b;
}
```

```inference
let a: () = ();
```

### 4.10.3 Square brackets

#### 4.10.3.1 Description

Square brackets `[]` are used to define arrays and address individual elements of an array.

See also: [Types](./types.md#62-array)

#### 4.10.3.2 Examples

```inference
let a: [u32; 3] = [1, 2, 3];
let b: u32 = a[0];
```

### 4.10.4 Angle brackets

#### 4.10.4.1 Description

Angle brackets `<>` are used to define type parameters.

See also: [Types](./types.md#63-user-defined-types)

#### 4.10.4.2 Examples

```inference
total fn foo<T>(a: T) {
    /// code block
}
```

---

[<kbd><br>⏮️ General description<br><br></kbd>](./general-description.md)
[<kbd><br>⏭️ Basic concepts<br><br></kbd>](./basic-concepts.md)
