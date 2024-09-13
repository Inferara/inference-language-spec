# 6 Types

Inference provide a minimal set of types that are necessary for the language to be useful. The types are divided into elementary types and user-defined types.

## 6 Unit

### Description

Unit is a type that has only one value, which is `()`. It is used to represent the absence of a value.

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

### 6.1.2 Integers

#### Description

Integer numbers are represented by the following types: `i8`, `i16`, `i32`, `i64`. The `i` prefix stands for signed integer. The number after the `i` prefix stands for the number of bits that the integer occupies in memory.

> [!NOTE]
> In case of using `i8` or `i16` types, 4 byes of memory will be allocated for the variable.

#### Examples

```inference
let a: i8 = (2 ** 8) - 1;
let b: i16 = (2 ** 16) - 1;
let c: i32 = (2 ** 32) - 1;
let d: i64 = (2 ** 64) - 1;
```

### 6.1.3 Unsigned integers

#### Description

Unsigned integer numbers are represented by the following types: `u8`, `u16`, `u32`, `u64`. The `u` prefix stands for unsigned integer. The number after the `u` prefix stands for the number of bits that the integer occupies in memory.

> [!NOTE]
> In case of using `u8` or `u16` types, 4 byes of memory will be allocated for the variable.

#### Examples

```inference
let a: u8 = (2 ** 8);
let b: u16 = (2 ** 16);
let c: u32 = (2 ** 32);
let d: u64 = (2 ** 64);
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
