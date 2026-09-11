# 8 Expressions

## 8.1 Member Access

### 8.1.1 Description

Member access is an expression allowing access a struct's fields. The syntax for member access is the dot operator (`.`) followed by the name of the field or element.

### 8.1.2 Examples

```inference
struct Account {
  address: Address;
  balance: u64;
}

fn get_balance(account: Account) -> u64 {
  return account.balance;
}
```

## 8.2 Array Index Access

### 8.2.1 Description

Array index access is an expression allowing access to the elements of an array. The syntax for an array index access is a pair of the square brackets (`[]`) enclosing the element's index.

### 8.2.2 Examples

```inference
fn get_element(arr: [u32; 10], index: u32) -> u32 {
  return arr[index];
}
```

## 8.3 Function Call

### 8.3.1 Description

A function call is an expression that allows to call a function with the specified type parameters and arguments. The syntax for a function call is the function name followed by the optional type parameters and arguments enclosed in parentheses.

A function can be called with an explicit argument name, but in this case, all arguments must be named.

For functions with the body marked as `forall`, the arguments can be passed as `@` to indicate that the execution paths analysis will be conducted for all possible values of the argument.

### 8.3.2 Examples

```inference
fn sum(a: u32, b: u32) -> u32 forall {
  return a + b;
}

fn sub T' (a: T', b: T') -> T' forall {
  return a - b;
}

fn example_1() -> u32 {
  let x: u32 = sum(1, 2);
  let y: u32 = sum(a: 1, b: 2);
  let z: u32 = sum(@, @);
  return sum(a: @, b: @);
}

fn example_2() -> i32 {
  let x: i32 = sub i32 (1, 2);
  let y: i32 = sub i32 (a: 1, b: 2);
  let z: i32 = sub i32 (@, @);
  return sub i32 (a: @, b: @);
}
```

## 8.4 Parenthesized

### 8.4.1 Description

The parenthesized expression is used to group `lval` expressions and control the order of evaluation. The syntax for the parenthesized expression is the opening and closing parentheses enclosing the expression.

### 8.4.2 Examples

```inference
fn example() -> u32 {
  return (1 + 2) * 3;
}
```

## 8.5 Unary Operators

### 8.5.1 Description

Unary operators are operators that operate on a single operand. In Inference, the following unary operators are supported:

- `-` (negation) — numeric sign inversion
- `!` (logical NOT) — boolean negation; evaluates to `true` if the operand is `false`, and vice versa
- `~` (bitwise NOT) — inverts all bits of the operand

The operand of a unary expression is an expression.

The `-` operator may not be applied to a numeric literal it is written apart from. A negative number is a single literal carrying its own sign (see [4.7.3 Numeric](./lexical-structure.md#473-numeric)), so `-42` is its only well-formed spelling and `- 42` is ill-formed. The restriction covers that one shape and no other: negation of any other operand may be spaced freely, so `-x` and `- x` are equivalent, as are `-(a + b)` and `- (a + b)`, and `-(42)` negates a parenthesized expression rather than a literal.

`-` is checked: when the mathematical result of the operation lies outside the range of its operand type, the program traps. This holds at every integer width and in both compilation modes. Writing the expression inside a `wrapping(...)` makes it compute that result modulo two to the power of the operand type's width instead (see [8.7 Arithmetic Overflow](#87-arithmetic-overflow)). `!` and `~` cannot overflow.

### 8.5.2 Examples

```inference
fn example() -> i32 {
  let a: i32 = -42;
  return a;
}

fn logical_not_example(flag: bool) -> bool {
  return !flag;
}

fn bitwise_not_example(x: i32) -> i32 {
  return ~x;
}
```

## 8.6 Binary Operators

### 8.6.1 Description

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

`+`, `-` and `*` are checked: when the mathematical result of the operation lies outside the range of its operand type, the program traps. This holds at every integer width and in both compilation modes. Writing the expression inside a `wrapping(...)` makes them compute that result modulo two to the power of the operand type's width instead (see [8.7 Arithmetic Overflow](#87-arithmetic-overflow)). The remaining arithmetic operators are not checked in this sense, and for different reasons: signed division traps on its own at the one quotient a width cannot hold and on a zero divisor, `%` cannot overflow at any width, and a shift discards the bits it moves out by definition.

The logical operators `&&` and `||` use short-circuit evaluation: the right operand is evaluated only when the left operand does not determine the result — `&&` evaluates its right operand only when the left operand is `true`, and `||` evaluates its right operand only when the left operand is `false`. A trap in the right operand (such as a division by zero, an out-of-bounds array index, or an arithmetic result outside its operand type) therefore cannot occur when the left operand decides the expression, which makes guard expressions such as `x != 0 && y / x > 1` well-defined.

Bitwise operators are available, but they are added to support imported code. For instance, if an external function that is part of a specification returns a bit-packed value, we need a way to unpack such a union. Other possible reasons for using bitwise operators in a specification, like memory or computation optimization, are not relevant because a specification is not an execution unit.

- `<<` (bitwise left shift)
- `>>` (bitwise right shift)
- `^` (bitwise XOR)
- `|` (bitwise OR)
- `&` (bitwise AND)

Unlike `&&` and `||`, the bitwise operators evaluate both of their operands unconditionally.

Left and right operands of a binary expression are expressions.

### 8.6.2 Examples

```inference
fn example() -> u32 {
  return 1 + 2 * 3;
}
```

## 8.7 Arithmetic Overflow

### 8.7.1 Description

`wrapping(e)` and `checked(e)` are prefix expression forms that fix how the arithmetic written inside `e` behaves on overflow. `wrapping(e)` makes every `+`, `-`, `*` and unary `-` written between its parentheses compute its result modulo two to the power of the operand type's width; `checked(e)` makes them trap, which is what they do anywhere no `wrapping(...)` encloses them. The expression has the type of `e`.

`checked` and `wrapping` are reserved words. Neither may be used as the name of anything.

The forms are lexical and they nest, innermost first: in `wrapping(a * checked(b + c))` the multiplication wraps and the addition traps. They govern the operators written between their own parentheses and no others, so the body of a function called inside `e` keeps whatever its own source says, and `wrapping(f(x))` changes nothing about `f`.

`e` is a scalar expression — an integer or a `bool`. A `bool` is admitted because the arithmetic being governed may sit inside a comparison, as in `wrapping(a + b > c)`. A compound, unit or function-typed `e` is ill-formed.

An annotation that contains no `+`, `-`, `*` or unary `-` at a type that can overflow is ill-formed. A negative number is a single literal carrying its own sign rather than a negation applied to a value (see [4.7.3 Numeric](./lexical-structure.md#473-numeric)), so `wrapping(-2147483648)` contains no arithmetic and is ill-formed under the same rule. A `checked(e)` that no `wrapping(...)` encloses names the behaviour already in force: it is well-formed and changes nothing.

### 8.7.2 Examples

```inference
fn fixmul(a: i64, b: i64) -> i64 {
  const ONE: i64 = 1048576;
  return a * b / ONE;
}

fn mix(s: u64) -> u64 {
  const C1: u64 = 11400714819323198485;
  return wrapping(s * C1);
}
```

---

[<kbd><br>⏮️ Variables<br><br></kbd>](./variables.md)
[<kbd><br>⏭️ Statements<br><br></kbd>](./statements.md)
