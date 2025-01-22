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

A spec is an abstract _module_ representation. By module, we mean a scoped set of imports, definitions, and functions. Contexts are used to group related definitions and functions together and provide a way to organize the specification.

Contexts may contain definitions of constants and spec-level variables, structs, enums, and functions. Nested contexts are not allowed.

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

Along with the fields, a struct can also have methods that define the behavior of the struct. These methods can be used to manipulate the fields of the struct or perform some operations on them. The methods are defined inside the struct block and can access the fields of the struct using the `self` keyword.

### 10.7.2 Examples

```inference
struct Account {
  address: Address;
  balance: u64;

  fn can_withdraw(amount: u64) -> bool {
    return self.balance >= amount;
  }
}
```

---

[<kbd><br>⏮️ Statements<br><br></kbd>](./statements.md)
[<kbd><br>⏭️ Functions<br><br></kbd>](./functions.md)
