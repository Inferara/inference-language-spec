# 10 Definitions

## 10 Constant

### Description

Constants are immutable values that are assigned to a variable at compile time. Constants are declared using the `const` keyword and must be initialized with a value or with an expression that can be evaluated on the compile time. The value of a constant cannot be changed once it is assigned.

### Examples

```inference
const MAX_SIZE: u32 = 100;
```

## 10 Function

### Description

Functions in Inference are used to define a block of code that can be executed multiple times with different inputs. Functions are defined using the `fn` keyword followed by the function name, parameters, return type, and body. Functions can have multiple parameters and can return a single value or a tuple of values.

For the detailed information about functions, see the [Functions](./functions.md) section.

### Examples

```inference
total fn sum(a: u32, b: u32) -> u32 {
  return a + b;
}
```

## 10 External Function

### Description

External functions are functions that are defined outside of the current module or context. They are used to interact with the external environment, such as calling functions from other modules. External functions are declared using the `extern` keyword followed by the function signature.

For the detailed information about functions, see the [Functions](./functions.md#11-external-function) section.

### Examples

```inference
extern fn external_function(a: u32, b: u32) -> u32;
```

## 10 Type

### Description

Types in Inference are used to define the structure of data. Inference provides a set of built-in types, such as integers, booleans, and arrays, as well as the ability to define custom types using structs and enums.

For the detailed information about embedded types, see the [Types](./types.md) section.

### Examples

```inference
type Address = u32;
```

## 10.1 Context

### Description

Context is an abstract _module_ representation. By module, we mean a scoped set of imports, definitions, and functions. Contexts are used to group related definitions and functions together and provide a way to organize the specification.

Contexts may contain definitions of constants and context-level variables, structs, enums and functions. Nested contexts are not allowed.

### Examples

```inference
context AuctionSpec {  
  const MAX_BID: u64 = 1000;
  const MIN_BID: u64 = 100;
  
  struct Bid {
    bidder: Address;
    amount: u64;
  }
  
  enum AuctionState {
    Open,
    Closed,
  }
  
  total fn is_valid_bid(bid: Bid) -> bool {
    return bid.amount >= MIN_BID && bid.amount <= MAX_BID;
  }

  total fn is_auction_open(state: AuctionState) -> bool {
    return state == AuctionState::Open;
  }
}
```

## 10.2 Enum

### Description

Definition

Enums, short for enumerations, are a user-defined data type in Inference that consists of a set of named integral constants. They provide a way to assign symbolic names to integral values, enhancing code readability and maintainability.

During compilation, the compiler replaces the enum constants with their corresponding integral values, making enums a compile-time construct.

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

## 10.3 Struct

### Description

Struct in Inference is a user-defined algebraic data type that allows you to define a group of variables under a single name. The underlying data structure of a struct is a record, which is a collection of fields or members. Each field can have a different type, and the fields are accessed using the dot operator.

Along with the fields, a struct can also have methods that define the behavior of the struct. These methods can be used to manipulate the fields of the struct or perform some operations on them. The methods are defined inside the struct block and can access the fields of the struct using the `ctx` keyword.

### Examples

```inference
struct Account {
  address: Address;
  balance: u64;

  total fn can_withdraw(amount: u64) -> bool {
    ctx.balance >= amount;
  }
}
```
