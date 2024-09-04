# 11 Functions

## 11 Function Definition

### Description

Functions in Inference are the basic blocks to build the specification. The are used to describe the behaviour of the system in functional terms. A functions is a matter of verification; hence, it is an only possible argument of the [verify](./statements.md#9-verify) expression statement.

Functions can be defined on the top level of the [context](./definitions.md#101-context), inside the [context](./definitions.md#101-context), or inside the [struct](./definitions.md#103-struct) definition. In case of the latter, the function is considered as a method of the struct and aquires access to struct fields.

### Modifiers

### `total`

The `total` keyword is used to specify that the function is total. A total function is a function that is defined for all possible inputs. In other words, a total function is a function that is guaranteed to terminate for all possible inputs.


### Examples

```inference
total fn sum(a: u32, b: u32) -> u32 {
  return a + b;
}

context Bridge {

}
```

## 11 External Function

### Description

External functions are functions that are defined outside of the current specification or context. They are used to interact with the external environment, such as calling functions from other modules. External functions are declared using the `extern` keyword followed by the function signature.

External functions is en entry point to the actual implementation of the system. It means the specification is a description of the system behavior, and the external functions are the actual implementation of the system. A specification can exists without the linked external implementation, but if it is desired to verify the correctness of the implementation, the external functions must be provided.

### Examples

```inference
extern total fn hash(b: u32[100]) -> u32[256];

context Hasher {

    fn hash_verifier() -> {
        let undef data1: u32[100];
        let result_1: u32[256] = hash(data);
        let undef data2: u32[100];
        let result_2: u32[256] = hash(data);
        assert(result_1 == result_2);
    }

    fn proof() {
        verify hash_verifier();
    }

}
```
