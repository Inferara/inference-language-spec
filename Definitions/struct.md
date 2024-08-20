# Struct

## Description

Struct in Inference is a user-defined algebraic data type that allows you to define a group of variables under a single name. The underlying data structure of a struct is a record, which is a collection of fields or members. Each field can have a different type, and the fields are accessed using the dot operator.

Along with the fields, a struct can also have methods that define the behavior of the struct. These methods can be used to manipulate the fields of the struct or perform some operations on them. The methods are defined inside the struct block and can access the fields of the struct using the `ctx` keyword.

## Examples

```inference

struct Account {
  address: Address;
  balance: u256;

  fn can_withdraw(amount: u256) -> bool {
    ctx.balance >= amount;
  }
}
