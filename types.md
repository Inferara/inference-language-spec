# 6 Types

Inference provides a minimal set of types necessary for the language to be useful. The types are divided into elementary types and user-defined types.

## 6.1 Unit

### Description

The `Unit` type has only one value, which is `()`. It is used to represent the absence of a value.

### Examples

```inference
let a: () = ();
```

## 6.2 Elementary Types

By elementary types, we mean the types that are built into the language and allocate a constant amount of memory.

### 6.2.1 Boolean

#### Description

The Boolean type is used to represent either `true` or `false` values.

In memory, the `bool` type is represented as an `i32` due to the minimal addressable unit being 4 bytes (see the [restrictions section](./general-description.md#restrictions)).

#### Examples

```inference
let a: bool = true;
let b: bool = (1 == 2); // false
```

### 6.2.2 Integers

#### Description

Signed integers are represented by the following types: `i8`, `i16`, `i32`, `i64`. The `i` prefix stands for signed integer. The number after the `i` indicates the number of bits that the integer occupies in memory.

> [!NOTE]
> Even when using `i8` or `i16` types, 4 bytes of memory will be allocated for the variable due to the minimal addressable unit.

#### Examples

```inference
let a: i8 = (2 ** 7) - 1;       // 127
let b: i16 = (2 ** 15) - 1;     // 32,767
let c: i32 = (2 ** 31) - 1;     // 2,147,483,647
let d: i64 = (2 ** 63) - 1;     // 9,223,372,036,854,775,807
```

### 6.2.3 Unsigned Integers

#### Description

Unsigned integers are represented by the following types: `u8`, `u16`, `u32`, `u64`. The `u` prefix stands for unsigned integer. The number after the `u` indicates the number of bits that the integer occupies in memory.

> [!NOTE]
> Even when using `u8` or `u16` types, 4 bytes of memory will be allocated for the variable due to the minimal addressable unit.

#### Examples

```inference
let a: u8 = (2 ** 8) - 1;       // 255
let b: u16 = (2 ** 16) - 1;     // 65,535
let c: u32 = (2 ** 32) - 1;     // 4,294,967,295
let d: u64 = (2 ** 64) - 1;     // 18,446,744,073,709,551,615
```

## 6.3 Array

### Description

An array is a linear segment of memory (also known as a vector or sequence). Since the array is a fixed-size data structure, its size must be known at compile time, and the required memory space will be allocated accordingly.

### Examples

```inference
let a: [i32; 3] = [1, 2, 3];
```

## 6.4 User-Defined Types

### Description

Users can define custom types in several ways:

1. Using the `typeof` statement
2. Defining an `enum` (see [Enum](./definitions.md#102-enum))
3. Defining a `struct` (see [Struct](./definitions.md#103-struct))
