# 10 Definitions

## 10.1 Constant

### 10.1.1 Description

Constants are immutable values that are assigned to a variable at compile time. Constants are declared using the `const` keyword and must be initialized with a value or with an expression that can be evaluated at compile time. The value of a constant cannot be changed once it is assigned.

### 10.1.2 Examples

```inference
const MAX_SIZE: u32 = 100;
```

## 10.2 Function

### 10.2.1 Description

Functions in Inference are used to define a block of code that can be executed multiple times with different inputs. Functions are defined using the `fn` keyword followed by the function name, parameters, return type, and body. Functions can have multiple parameters and can return a single value or a tuple of values.

For detailed information about functions, see the [Functions](./functions.md) section.

### 10.2.2 Examples

```inference
fn sum(a: u32, b: u32) -> u32 {
  return a + b;
}
```

## 10.3 External Function

### 10.3.1 Description

External functions are functions that are defined outside of the current module or spec. They are used to interact with the external environment, such as calling functions from other modules. External functions are declared using the `external` keyword followed by the function signature.

For detailed information about external functions, see the [Functions](./functions.md#11-external-function) section.

### 10.3.2 Examples

```inference
external fn external_function(a: u32, b: u32) -> u32;
```

## 10.4 Type

### 10.4.1 Description

Types in Inference are used to define the structure of data. Inference provides a set of built-in types, such as integers, booleans, and arrays, as well as the ability to define custom types using structs and enums.

For detailed information about embedded types, see the [Types](./types.md) section.

### 10.4.2 Examples

```inference
type Address = u32;
```

## 10.5 Spec

### 10.5.1 Description

A spec is an abstract _module_ representation. By module, we mean a scoped set of imports, definitions, and functions. Specs are used to group related definitions and functions together and provide a way to organize the program specification as a whole.

Specs may contain definitions of constants, structs, enums, and functions. Nested specs are not allowed.

### 10.5.2 Examples

```inference
spec AuctionSpec {  
    const MAX_BID: u64 = 1000;
    const MIN_BID: u64 = 100;

    struct Bid {
      bidder: Address;
      amount: u64;
    }

    enum AuctionState {
      Open,
      Closed
    }

    fn is_valid_bid(bid: Bid) -> bool {
      return bid.amount >= MIN_BID && bid.amount <= MAX_BID;
    }

    fn is_auction_open(state: AuctionState) -> bool {
      return state == AuctionState::Open;
    }
}
```

## 10.6 Enum

### 10.6.1 Description

Enums, short for enumerations, are a user-defined data type in Inference that consists of a set of named integral constants. They provide a way to assign symbolic names to integral values, enhancing code readability and maintainability.

During compilation, the compiler replaces the enum constants with their corresponding integral values, making enums a compile-time construct.

### 10.6.2 Examples

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

## 10.7 Struct

### 10.7.1 Description

A struct in Inference is a user-defined algebraic data type that allows you to define a group of variables under a single name. The underlying data structure of a struct is a record, which is a collection of fields or members. Each field can have a different type, and the fields are accessed using the dot operator.

Along with the fields, a struct can also have methods that define the behavior of the struct. These methods can be used to manipulate the fields of the struct or perform some operations on them. The methods are defined inside the struct block and must take either `self` or `mut self` as the first parameter. A method which takes `self` can read the value of any field of the instance it was invoked on by accessing it using the `self` keyword. However, it may neither mutate any data in the struct, nor call other methods which take `mut self`. Methods which take `mut self` can additionally change data and call other `mut self` methods, but they may only be invoked on a mutable instance of the struct.

Declared structs will automatically have a `new` function in their namespace for creating instances of the struct. The parameters of the `new` function are the values to be assigned to each field in the new instance. They are ordered the same way as fields are defined in the struct. 

There are no visibility features in Inference, therefore it is not possible to mark certain fields or methods as private.
### 10.7.2 Examples

Declaring an `Account` struct:
```inference
struct Account {
  address: Address;
  balance: u64;

  fn can_withdraw(self, amount: u64) -> bool {
    return self.balance >= amount;
  }

  fn transfer(mut self, amount: u64, mut receiver : Account) -> bool {
    if !self.can_withdraw(amount) {return false;}
    self.balance = self.balance - amount;
    receiver.balance = receiver.balance + amount;
    return true;
  }
}
```

Creating an instance of `Account`:
```inference
/// Suppose we already have some address we would like to use...
let addr : Address = ... ;
/// Create the new account with that address, and a balance of 0.
let acc : Account = Account::new(addr, 0);
```

The type signature of the constructor is 
```inference
Account::new(address : Address, balance : u64) -> Account
```
Because the `address` field was declared before the `balance` field. Alternatively, it is possible to pass named arguments rather than relying on the order they are declared in:
```inference
let acc : Account = Account::new(balance: 0, address: addr);
```

Invoking methods:
```inference
let mut account1 : Account = Account::new(addr1, 10)
let mut account2 : Account = Account::new(addr2, 0);
let account3 : Account = Account::new(addr3, 5);

/// Works because account1 and account2 are mutable, so we can change
/// both of the balances.
account1.transfer(5, account2);

/// Results in error: account2 is mutable, but account3 is not, so we cannot
/// change its balance.
account2.transfer(2, account3);

/// Results in error: account1 is mutable, but account3 is not.
account3.transfer(1, account1);

/// This is OK. can_withdraw takes a regular reference, so we can invoke
/// it on immutable account instances, as it does not change any information.
let my_bool : bool = account3.can_withdraw(10);

```

---

[<kbd><br>⏮️ Statements<br><br></kbd>](./statements.md)
[<kbd><br>⏭️ Functions<br><br></kbd>](./functions.md)
