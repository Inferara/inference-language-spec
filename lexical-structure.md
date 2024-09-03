# 4 Lexical structure

## 4 Keywords

- `fn`
- `external`
- `let`
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

## 4 Reserved identifiers

- `constructor`
- `proof`
- `filter`
- `type`
- `ctx`

## 4 Qualified identifiers and names

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
