# 8 Expressions

## 8.1 Assign

### 8.1.1 Description

The assign expression is used to assign a value, or generally speaking, the result of an expression evaluation to a variable or the `lval` expression. The syntax for the assign expression is the variable name followed by the assignment operator (`=`) and the expression.

### 8.1.2 Examples

```inference
fn example() -> u32 {
  let a: u32 = 42;
  let b: u32 = a;
  return b;
}
```

## 8.2 Member Access

### 8.2.1 Description

Member access is an expression allowing you to access a struct's fields. The syntax for member access is the dot operator (`.`) followed by the name of the field or element.

### 8.2.2 Examples

```inference
struct Account {
  address: Address;
  balance: u64;
}

fn get_balance(account: Account) -> u64 {
  return account.balance;
}
```

## 8.3 Array Index Access

### 8.3.1 Description

Array index access is an expression that allows you to access the elements of an array. The syntax for array index access is square brackets (`[]`) enclosing the element's index.

### 8.3.2 Examples

```inference
fn get_element(arr: [u32; 10], index: u32) -> u32 {
  return arr[index];
}
```

## 8.4 Function Call

### 8.4.1 Description

A function call is an expression that allows you to call a function with the specified arguments. The syntax for a function call is the function name followed by the arguments enclosed in parentheses.

### 8.4.2 Examples

```inference
fn sum(a: u32, b: u32) -> u32 {
  return a + b;
}

fn example() -> u32 {
  return sum(1, 2);
}
```

## 8.5 Parenthesized

### 8.5.1 Description

The parenthesized expression is used to group `lval` expressions and control the order of evaluation. The syntax for the parenthesized expression is the opening and closing parentheses enclosing the expression.

### 8.5.2 Examples

```inference
fn example() -> u32 {
  return (1 + 2) * 3;
}
```

## 8.6 Unary Operators

### 8.6.1 Description

Unary operators are operators that operate on a single operand. In Inference, only the unary minus operator (`-`) is supported. The operand of a unary expression is an expression.

### 8.6.2 Examples

```inference
fn example() -> i32 {
  let a: i32 = -42;
  return a;
}
```

## 8.7 Binary Operators

### 8.7.1 Description

Binary operators are operators that operate on two operands. In Inference, the following binary operators are supported (in order of precedence):

- `**` (power)
- `*` (multiplication)
- `/` (division)
- `%` (modulo)
- `+` (addition)
- `-` (subtraction)
- `<` (less than)
- `<=` (less than or equal to)
- `==` (equal to)
- `!=` (not equal to)
- `>` (greater than)
- `>=` (greater than or equal to)
- `&&` (logical AND)
- `||` (logical OR)

Bitwise operators are available, but they are added to support imported code. For instance, if an external function that is part of a specification returns a bit-packed value, we need a way to unpack such a union. Other possible reasons for using bitwise operators in a specification, like memory or computation optimization, are not relevant because a specification is not an execution unit.

- `<<` (shift one bit left)
- `>>` (shift one bit right)
- `^` (logical xor)
- `|` (logical disjunction aka set bit)
- `&` (logical conjunction aka check bit)

Left and right operands of a binary expression are expressions.

### 8.7.2 Examples

```inference
fn example() -> u32 {
  return 1 + 2 * 3;
}
```

---

[<kbd><br>⏮️ Variables<br><br></kbd>](./variables.md)
[<kbd><br>⏭️ Statements<br><br></kbd>](./statements.md)
