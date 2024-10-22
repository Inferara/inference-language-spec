# 11 Functions

## 11.1 Function Definition

### 11.1.1 Description

Functions in Inference are the basic blocks used to build the specification. They are used to describe the behavior of the system in functional terms. A function is a matter of verification; hence, it is the only possible argument of the [verify](./statements.md#9-verify) expression statement.

Functions can be defined at the top level of the program, inside a [context](./definitions.md#101-context), or inside a [struct](./definitions.md#103-struct) definition. In the latter case, the function is considered a method of the struct and acquires access to struct fields.

### 11.1.2 Modifiers

#### 11.1.2.1 `total`

The `total` keyword is used to specify that the function is total. A total function is a function that is defined for all possible inputs. In other words, a total function is a function that is guaranteed to terminate for all possible inputs.

### 11.1.3 Examples

```inference
total fn sum(a: u32, b: u32) -> u32 {
  return a + b;
}

context Bridge {
  // Context-specific functions
}
```

## 11.2 External Function

### 11.2.1 Description

External functions are functions that are defined outside of the current specification or context. They are used to interact with the external environment, such as calling functions from other modules or libraries. External functions are declared using the `extern` keyword followed by the function signature.

External functions are an entry point to the actual implementation of the system. This means the specification is a description of the system behavior, and the external functions are the actual implementation of the system. A specification can exist without the linked external implementation, but if it is desired to verify the correctness of the implementation, the external functions must be provided.

### 11.2.2 Examples

```inference
extern total fn hash(b: u8[100]) -> u8[32];

context Hasher {
    total fn hash_verifier() {
        let undef data1: u8[100];
        let result_1: u8[32] = hash(data1);
        let undef data2: u8[100];
        let result_2: u8[32] = hash(data2);
        if data1 == data2 {
            assert(result_1 == result_2);
        } else {
            assert(result_1 != result_2);
        }
    }

    fn proof() {
        verify hash_verifier();
    }
}
```

## 11.3 High-Order Functions

### 11.3.1 Description

In Inference, functions are the first-class citizens. This means that functions can be passed as arguments to other functions, returned as values from other functions, and assigned to variables. This allows for the creation of high-order functions, which are functions that take other functions as arguments or return functions as results.

### 11.3.2 Examples

```inference
fn bubble_sort(arr: i32[10], compare_function: fn(i32, i32) -> i32) -> () {
    let n: i32 = 10;
    let i: i32 = 0;
    loop n {
        i = i + 1;
        let j: i32 = 0;
        loop n - i - 1 {
            j = j + 1;
            if compare_function(arr[j], arr[j + 1]) > 0 {
                let temp: i32 = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}
```

This example demonstrates a high-order function `bubble_sort` that takes an array of integers and a comparison function as arguments. The comparison function is used to determine the order of elements in the array. The `bubble_sort` function sorts the array using the comparison function provided.

```inference

use { hash } from "./cryptography.0.wasm";

context HashContext {

    const hash_function: predicate = #hash;

    total fn verify(hash_f: hash_function) -> () {
        let undef data1: u8[100];
        let result_1: u8[32] = hash_f(data1);
        let undef data2: u8[100];
        let result_2: u8[32] = hash_f(data2);
        if data1 == data2 {
            assert(result_1 == result_2);
        } else {
            assert(result_1 != result_2);
        }
    }

    total fn verify_hash() -> () {
        verify(hash);
    }

}
```

Operator [`#`](./expressions.md#862-get-function-type) (a pictogram for a table or a memory cell) is used to get the type of the function. Fucntinon type is a function signature (attributes and return value). [`predicate`](./types.md#64-predicate) is an embedded type that represents a general function type. It can be used to automaticaly deduce function types. Alternatively, the qualified function type can be used as a type of a `predicate`.

```inference
const hash_function: predicate(fn(u8[100]) -> u8[32]) = #hash;
```

> [!IMPORTANT]
> `hash_function` and `predicate` type are not variables, they do not appear on the stack and in the memory. They are used to address functions types, functinos and type-checking. You can think about them as a type-level variables.

```inference

total fn add(a: i32, b: i32) -> i32 {
    return a + b;
}

total fn subtract(a: i32, b: i32) -> i32 {
    return a - b;
}

assert (#add == #subtract);
```

Consequently (see example 1), the `#add` and `#subtract` are the same type, and the assertion is correct.


---

[<kbd><br>⏮️ Definitions<br><br></kbd>](./definitions.md)
[<kbd><br>⏭️ Constants<br><br></kbd>](./constants.md)
