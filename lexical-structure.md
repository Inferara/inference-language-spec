# 4 Lexical structure

## 4 Comments

### Description

Comments are used to document the code and are ignored by the compiler. Inference supports triple-slash comments `///` only.

### Examples

```inference
/// This is a comment
```

## 4 Keywords

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
- `else`
- `struct`
- `enum`
- `type`
- `context`
- `const`
- `assert`
- `verify`
- `typeof`


## 4 Identifiers

### Description

An identifier is a sequence of characters that is used to name variables, functions, and other entities in the program. Identifiers must start with a letter or an underscore and can contain letters, digits, and underscores.

### Examples

```inference
let a: u32 = 42;
let _ident : i64 = 42;
```

## 4 Reserved identifiers

- `constructor`
- `proof`
- `filter`
- `type`
- `ctx`

## 4 Qualified identifiers

### Description

A qualified identifier is a sequence of identifiers separated by `::`. It is used to refer to functions in structs and contexts.

### Examples

```inference
let a: context::AuctionSpec = context::AuctionSpec::new();
```

## 4 Member access

### Description

The member access operator `.` is used to access fields and methods of a struct.

### Examples

```inference
let a: Account = Account { address: 42 };

let b: u32 = a.address;
```

## 4 Literals

### 4.1 Bool

#### Description

`bool` is a boolean type that can have one of two values: `true` or `false`.

#### Examples

```inference
let a: bool = true;
let b: bool = false;
```

### 4.2 String

#### Description

`string` is a string type that represents a sequence of characters. Strings are enclosed in double quotes. String is nether a primitive type nor a composite type. It is a compile time type that is enterpreted as a sequence of UTF-8 bytes.

#### Examples

```inference
let a: string = "Hello, World!";
let b: string = "Inference";
```

### 4.3 Number

#### Description

`number` is a numeric type that represents a number. Numbers can be integers numbers only.

#### Examples

```inference
let a: number = 42;
```

### 4.4 Unit

#### Description

`unit` is a [type](./types.md#6-unit) that has only one value: `()`. It is used to represent the absence of a value.

#### Examples

```inference
let a: unit = ();
```

## 4 Right arrow

### Description

The right arrow `->` is used to specify the return type of a function.

### Examples

```inference
fn add(a: u32, b: u32) -> u32 {
    return a + b;
}
```

## 4 Terminator

### Description

The terminator `;` is used to separate statements in Inference.

### Examples

```inference
let a: u32 = 42;
```

## 4 Braces

### 4.1 Curly braces

#### Description

Curly braces `{}` are used to define blocks of code.

See also: [Statements](./statements.md#9-block)

#### Examples

```inference
fn foo() {
    /// code block
}
```

### 4.2 Parentheses

#### Description

Parentheses `()` are used to group expressions and arguments in function calls. Also a `()` outside of a function definition context is interpreted as a single token and is used to represent the unit type.

See also: [Functions](./functions.md#11-function-definition)
See also; [Types](./types.md#6-unit)

#### Examples

```inference
fn foo(a: u32, b: u32) -> u32 {
    return a + b;
}
```

```inference
let a: () = ();
```

### 4.3 Square brackets

#### Description

Square brackets `[]` are used to define arrays and address individual elements of an array.

See also: [Types](./types.md#62-array)

#### Examples

```inference
let a: [u32; 3] = [1, 2, 3];
let b: u32 = a[0];
```

### 4.4 Angle brackets

#### Description

Angle brackets `<>` are used to define type parameters.

See also: [Types](./types.md#63-user-defined-types)

#### Examples

```inference
fn foo<T>(a: T) {
    /// code block
}
```

---

[<kbd><br>⏭️ Basic concepts<br><br></kbd>](./basic-concepts.md)
[<kbd><br>⏮️ General description<br><br></kbd>](./general-description.md)
