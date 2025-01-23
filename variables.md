# 7 Variables

## 7.1 General Description

Variables in programming languages often reflect the language's memory model. However, in Inference, there is no concept of a memory model.

Inference is a language for defining formal specifications. While the spec defines variables and operations on them, these are abstract definitions and do not involve interaction with actual physical (or virtual) memory. This means there is no stack, heap, or any other specific memory location where variables are stored. The same applies to how variables are passed to functions. It is even possible to create an array of infinite length and pass it to a function.

The question arises: how can Inference reason about platforms used in real life in a theoretical way that still reflects actual machine behavior? The answer lies in Inference's abstract computational model, which allows reasoning about program behavior without being tied to specific memory implementations, yet remains applicable to real-world platforms through appropriate mappings during the proof process.

## 7.2 Variable Categories

Inference defines the following categories of variables:

- Local variables
- Array elements

### 7.2.1 Local Variables

#### 7.2.1.1 Description

Local variables are variables defined within the scope of a function. They are not accessible from outside the function.

#### 7.2.1.2 Examples

```inference
fn foo() {
    let a: i32 = 42;
    let b: bool = true;
}
```

### 7.2.2 Array Elements

#### 7.2.2.1 Description

Array elements are the individual items within an array. All array elements must be initialized at the time of array definition.

#### 7.2.2.2 Examples

```inference
fn foo() {
    let a: [i32; 3] = [1, 2, 3];
    let b: [bool; 2] = [true, false];
}
```

## 7.3 Default Values

### 7.3.1 Description

Inference, as a language for formal specification, is intended for use in critical system components. Hence,

> [!WARNING]
> No default values are allowed.

Each variable must be assigned a value before it is used. Inside quantified contexts it can be assigned the `@` value. In this case, it is assumed that all possible values for the variable are considered.

### 7.3.2 Examples

```inference
fn foo() {
    let len: u32 = @;
    let arr: [i32; len] = @;
}
```

Number literals have an actual type of `i64`. If a different size is required, the type must be explicitly specified.

## 7.4 Variable Definition and Assignment

### 7.4.1 Description

All variables are **immutable** by default. This means that once a variable is assigned a value, it cannot be changed. To create a mutable variable, the `mut` keyword must be used.

Variable names cannot shadow each other. For instance, if a function has a parameter `a`, a local variable `a` cannot be defined in the function. The same rule applies to the variable re-definition. If a variable is defined in a function, it cannot be redefined in the same function.

### 7.4.2 Block Statement

#### 7.4.2.1 Description

A block of code `{}` is a typical syntax location where variables are defined and assigned. Blocks form function bodies, `if` statement branches, and loop bodies.

#### 7.4.2.2 Examples

```inference
fn foo() {
    let a: i32 = 42;

    let mut b: bool = true;
    let mut i: i32 = 10;
    let mut acc: i32 = 0; 

    loop i {
        i = i + 1;
        b = i % 2 == 0;
        if b {        
            let c: i32 = a * 2;
            acc = acc + c;
        }
    }
}
```

### 7.4.3 Loop Statement

#### 7.4.3.1 Description

A `loop` statement is used to execute a block of code repeatedly for a specified number of iterations `N`. It is used to iterate over a sequence of values or perform a fixed number of repetitions.

#### 7.4.3.2 Examples

```inference
fn inverse_bool_array(mut bool_arr: [bool; 10]) {
    let mut i: i32 = 0;
    loop 10 {
        bool_arr[i] = !bool_arr[i];
        i = i + 1;
    }
}
```

---

[<kbd><br>⏮️ Types<br><br></kbd>](./types.md)
[<kbd><br>⏭️ Expressions<br><br></kbd>](./expressions.md)
