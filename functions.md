# 11 Functions

## 11.1 Function Definition

### 11.1.1 Description

Functions in Inference are the basic blocks used to build the specification. They are used to describe the behavior of the system in functional terms. A function is a matter of verification.

Functions can be defined at the top level of the program, inside a [spec](./definitions.md#101-spec), or inside a [struct](./definitions.md#103-struct) definition. In the latter case, the function is considered a method of the struct and acquires access to struct fields.

### 11.1.2 Modifiers

#### 11.1.2.1 `forall`

The `forall` keyword is used to specify that the function is forall. A forall-marked block in a function defines for all possible inputs. In other words, a forall-marked function is a function that is guaranteed to successfully terminate for all possible inputs.

### 11.1.3 Examples

```inference
fn sum(a: u32, b: u32) -> u32 forall {
  return a + b;
}

spec Bridge {
  /// Spec-specific functions
}
```

## 11.2 External Function

### 11.2.1 Description

External functions are functions that are defined outside of the current specification or spec. They are used to interact with the external environment, such as calling functions from other modules or libraries. External functions are declared using the `external` keyword followed by the function signature.

External functions are an entry point to the actual implementation of the system. This means the specification is a description of the system behavior, and the external functions are the actual implementation of the system. A specification can exist without the linked external implementation, but if it is desired to verify the correctness of the implementation, the external functions must be provided.

### 11.2.2 Examples

```inference
external fn ideal_hash(b: [u8;100]) -> [u8;32];

spec Hasher {
    fn hash_verifier() forall {
        let data1: [u8;100] = @;
        let result_1: [u8;32] = ideal_hash(data1);
        let data2: [u8;100] = @;
        let result_2: [u8;32] = ideal_hash(data2);
        if data1 == data2 {
            assert(result_1 == result_2);
        } else {
            assert(result_1 != result_2);
        }
    }

    fn proof() {
        hash_verifier();
    }
}
```

> [!NOTE]
> The `ideal_hash` function is supposed to be _ideal_ in terms of collision absence.

> [!NOTE]
> The expression `data1 == data2` with operator `==` and two arrays works similarly to C++. It comapres two arrays element-wise.

## 11.3 High-Order Functions

### 11.3.1 Description

In Inference, functions are the first-class citizens. This means that functions can be passed as arguments to other functions, returned as values from other functions, and assigned to variables. This allows for the creation of high-order functions, which are functions that take other functions as arguments or return functions as results.

### 11.3.2 Examples

```inference
fn bubble_sort(arr: [i32;10], compare_function: fn(left: i32, right: i32) -> i32) -> () {
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

spec HashContext {

    type HashFunction = fn([u8; 100]) -> [u8; 32];

    fn verify_hash_transitivity(hash_f: HashFunction) -> () {
        let data1: [u8; 100] = @;
        let result_1: [u8; 32] = hash_f(data1);
        let data2: [u8; 100] = @;
        let result_2: [u8; 32] = hash_f(data2);
        if data1 == data2 {
            assert(result_1 == result_2);
        } else {
            assert(result_1 != result_2);
        }
    }

    fn verify_hash() -> () forall {
        verify_hash_transitivity(hash);
    }
}
```

A type of a function can be defined using [`type`](./statements.md#97-type-definition) statement. `HashFunction` in the example is an alias for the `hash` function type (its signature). Hence, it can be used in the type annotations but cannot be called as a function.

```inference
fn add(a: i32, b: i32) -> i32 {
    return a + b;
}

fn subtract(a: i32, b: i32) -> i32 {
    return a - b;
}

fn example() {
  let plus: fn(i32, i32) -> i32 = add;
  let minus: fn(i32, i32) -> i32 = subtract;

  assert (plus == minus);
}
```

Function references types are inferred from the function signature. The `plus` and `minus` variables are references to the `add` and `subtract` functions, respectively. The assertion checks if the `plus` and `minus` functions are equal.

---

[<kbd><br>⏮️ Definitions<br><br></kbd>](./definitions.md)
[<kbd><br>⏭️ Constants<br><br></kbd>](./constants.md)
