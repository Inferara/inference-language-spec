# 11 Functions

## 11 Function Definition

### Description

Functions in Inference are the basic blocks used to build the specification. They are used to describe the behavior of the system in functional terms. A function is a matter of verification; hence, it is the only possible argument of the [verify](./statements.md#9-verify) expression statement.

Functions can be defined at the top level of the program, inside a [context](./definitions.md#101-context), or inside a [struct](./definitions.md#103-struct) definition. In the latter case, the function is considered a method of the struct and acquires access to struct fields.

### Modifiers

#### `total`

The `total` keyword is used to specify that the function is total. A total function is a function that is defined for all possible inputs. In other words, a total function is a function that is guaranteed to terminate for all possible inputs.

### Examples

```inference
total fn sum(a: u32, b: u32) -> u32 {
  return a + b;
}

context Bridge {
  // Context-specific functions
}
```

## 11 External Function

### Description

External functions are functions that are defined outside of the current specification or context. They are used to interact with the external environment, such as calling functions from other modules or libraries. External functions are declared using the `extern` keyword followed by the function signature.

External functions are an entry point to the actual implementation of the system. This means the specification is a description of the system behavior, and the external functions are the actual implementation of the system. A specification can exist without the linked external implementation, but if it is desired to verify the correctness of the implementation, the external functions must be provided.

### Examples

```inference
extern total fn hash(b: u8[100]) -> u8[32];

context Hasher {
    total fn hash_verifier() {
        let undef data1: u8[100];
        let result_1: u8[32] = hash(data1);
        let undef data2: u8[100];
        let result_2: u8[32] = hash(data2);
        assert((data1 == data2) => (result_1 == result_2));
    }

    fn proof() {
        verify hash_verifier();
    }
}
```
