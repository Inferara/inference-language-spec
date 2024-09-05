# 6 Types

Inference provide a minimal set of types that are necessary for the language to be useful. The types are divided into elementary types and user-defined types.

## 6 Unit

### Description

Unit is a type that has only one value, which is `()`. It is used to represent the absence of a value.
TODO

### Examples

```inference
let a: () = ();
```

## 6.1 Elementary types

By elementary types, we mean the type that is build into the language and allocates the constant amount of memory.

### 6.1.1 Boolean

#### Description

Boolean type is a type that is used to represent either `true` or `false` values.

In memory `bool` type is represented as `i32` because of a minimal addressing memory unit see the general description, [restrictions section](./general-description.md#restrictions).

#### Examples

```inference
let a: bool = true;
let b: bool = 1 == 2; ///false
```

### 6.1.2 i32

#### Description

`i32` is a 32-bit signed integer type.

#### Examples

```inference
let a: i32 = 42;
```

### 6.1.3 i64

#### Description

`i64` is a 64-bit signed integer type.

#### Examples

```inference
let a: i64 = 42;
```

### 6.1.4 u32

#### Description

`u32` is a 32-bit unsigned integer type.

#### Examples

```inference
let a: u32 = 42;
```

### 6.1.5 u64

#### Description

`u64` is a 64-bit unsigned integer type.

#### Examples

```inference
let a: u64 = 42;
```

## 6.2 Array

### Description

Array is a linear segment of memory (aka as a Vector or a sequence). Since the array is a fixed sized data structure, the size of the array must be known at the compile time and the required memory space will be allocated accordingly.

### Examples

```inference
let a: [i32; 3] = [1, 2, 3];
```

## 6.3 User-defined types

### Desciption

User can define custom types in several ways

1. using `typeof` statement
2. defining en `enum` ([enum](../Definitions/enum.md))
3. defining a `struct` ([struct](../Definitions/struct.md))
