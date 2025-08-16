# 6 Types

Inference provides a minimal set of types necessary for the language to be useful. The types are divided into elementary types and user-defined types.

## 6.1 Unit

### 6.1.1 Description

The `Unit` type has only one value, which is `()`. It is used to represent the absence of a value.

### 6.1.2 Examples

```inference
let a: () = ();
```

## 6.2 Elementary Types

By elementary types, we mean the types that are built into the language and allocate a constant amount of memory.

### 6.2.1 Boolean

#### 6.2.1.1 Description

The Boolean type is used to represent either `true` or `false` values.

In memory, the `bool` type is represented as an `i32` due to the minimal addressable unit being 4 bytes (see the [restrictions section](./general-description.md#34-restrictions)).

#### 6.2.1.2 Examples

```inference
let a: bool = true;
let b: bool = (1 == 2); // false
```

### 6.2.2 Integers

#### 6.2.2.1 Description

Signed integers are represented by the following types: `i8`, `i16`, `i32`, `i64`. The `i` prefix stands for signed integer. The number after the `i` indicates the number of bits that the integer occupies in memory.

> [!NOTE]
> Even when using `i8` or `i16` types, 4 bytes of memory will be allocated for the variable due to the minimal addressable unit.

#### 6.2.2.2 Examples

```inference
let a: i8 = (2 ** 7) - 1;       // 127
let b: i16 = (2 ** 15) - 1;     // 32,767
let c: i32 = (2 ** 31) - 1;     // 2,147,483,647
let d: i64 = (2 ** 63) - 1;     // 9,223,372,036,854,775,807
```

### 6.2.3 Unsigned Integers

#### 6.2.3.1 Description

Unsigned integers are represented by the following types: `u8`, `u16`, `u32`, `u64`. The `u` prefix stands for unsigned integer. The number after the `u` indicates the number of bits that the integer occupies in memory.

> [!NOTE]
> Even when using `u8` or `u16` types, 4 bytes of memory will be allocated for the variable due to the minimal addressable unit.

#### 6.2.3.2 Examples

```inference
let a: u8 = (2 ** 8) - 1;       // 255
let b: u16 = (2 ** 16) - 1;     // 65,535
let c: u32 = (2 ** 32) - 1;     // 4,294,967,295
let d: u64 = (2 ** 64) - 1;     // 18,446,744,073,709,551,615
```

## 6.3 Array

### 6.3.1 Description

An array is a linear segment of memory (also known as a vector or sequence). Since the array is a fixed-size data structure, its size must be known at compile time, and the required memory space will be allocated accordingly.

### 6.3.2 Examples

```inference
let a: [i32; 3] = [1, 2, 3];
```

## 6.4 Function type

### 6.4.1 Description
Because functions in Inference are [first class citizens](./functions.md#113-high-order-functions), there is a type to represent them. It is crucial to remember that Inference is a platform-agnostic language which compiles to a [proof-unit](./terms-and-definitions.md#25-proof-unit), not executable machine code. Hence, unlike low-level languages such as C or WASM, values of this type are _not_ stored under the hood as pointers or function indices. Instead, they behave as abstract references that get erased or inlined at compile time. This behavior is more similar to languages like Haskell.

### 6.4.2 Examples
A higher-order sorting function, which takes a comparison function as a parameter:
```inference
fn sort(arr: [i32;10], compare_function: fn(left: i32, right: i32) -> i32) -> () {
  ...
}
```

A type alias for a hashing function:
```inference
type HashFunction = fn([u8; 100]) -> [u8; 32];
```

The names of the arguments may be specified (as they are in the first example) to aid readability, or omitted (as they are in the second example), leaving only the types of the arguments separated by commas and the return type.

## 6.5 User-Defined Types

### 6.5.1 Description

Users can define custom types in two ways:

1. Defining an `enum` (see [Enum](./definitions.md#106-enum))
2. Defining a `struct` (see [Struct](./definitions.md#107-struct))

---

[<kbd><br>⏮️ Basic concepts<br><br></kbd>](./basic-concepts.md)
[<kbd><br>⏭️ Variables<br><br></kbd>](./variables.md)
