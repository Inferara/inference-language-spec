# 10 Definitions

## 10.1 Enum

### Description

Definition

Enums, short for enumerations, are a user-defined data type in Inference that consists of a set of named integral constants. They provide a way to assign symbolic names to integral values, enhancing code readability and maintainability.

TODO add low-level implementation details later on.

### Examples

```inference
enum Arch {
  WASM,
  EVM,
}

enum ContextType {
  Program,
  Contract,
}
```

## 10.2 Struct

### Description

Struct in Inference is a user-defined algebraic data type that allows you to define a group of variables under a single name. The underlying data structure of a struct is a record, which is a collection of fields or members. Each field can have a different type, and the fields are accessed using the dot operator.

Along with the fields, a struct can also have methods that define the behavior of the struct. These methods can be used to manipulate the fields of the struct or perform some operations on them. The methods are defined inside the struct block and can access the fields of the struct using the `ctx` keyword.

### Examples

```inference
struct Account {
  address: Address;
  balance: u64;

  fn can_withdraw(amount: u64) -> bool {
    ctx.balance >= amount;
  }
}
```