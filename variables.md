# 7 Variables

## 7.1 General description

Variable structure in programming languages are usually reflects the memory model of a language. So, this section describes the Inference memory model.

> [!IMPORTANT]
>And to begin with, we say that there is no concept of a memory model in Inference.

Inference is a language used to define formal specifications. So despite the spec defines variables and operations on them, these are definitions but not the matter of the interaction with the actual physical (or virtual) memory. It means there is no stack, heap or any other specific memory location where the variables are stored. The same situation applies to the method of how variables are passed to functions. It means that it is possible to create an array of infinite lengths and pass it to a function.

The question is how come Inference can reason about the platforms used in real life in a theoretical way that reflects the real machine behavior **indeed**. The answer @Keyholder

## 7.2 Variables categories

Inference defines the following categories of variables:
- local variables
- array elements

### 7.2.1 Local variables

#### Description

Local variables are variables that are defined within the scope of a function. They are not accessible from outside of the function. If a function has parameters, local variables cannot shadow those regardless of variable types.

#### Examples

```inference
total fn foo() {
    let a: i32 = 42;
    let b: bool = true;
    bar(a);
}

total fn bar(a: i32) {
    ///let a: u32 = 0; impossible
    let b : i32 = a + 10; ///OK
}
```

### 7.2.2 Array elements

#### Description

Array elements are variables that are defined when an array is defined. All array variable values must be defined at the time of array definition.

#### Examples

```inference
total fn foo() {
    let a: [i32; 3] = [1, 2, 3];
    let b: [bool; 2] = [true, false];
}
```

## 7.3 Default values

Inference, as a language for formal specification is assumed to be used for critical system components.

> [!WARNING]
>So no default values are allowed for variables.

Each variable must be assigned before it is used.

The only exception from this rule if a variable has `undef` modifier. In this case, it is assumed that all possible values for the variable are considered.

### Examples

```inference
total fn foo() {
    let undef len: u32;
    let undef arr: [i32; len];
}
```

Number literals actual type is `i64`. If other size is required, the type must be explicitly defined.

## 7.4 Define assignment

### 7.1 Block statement

#### Description

A block of code `{}` is a usual syntax location where variables are defined and assigned. Blocks are form function bodies, `if` statements arms, `for` loop body.

#### Examples

```inference
total fn foo() {
    let a: i32 = 42;
    let b: bool = true;

    for (; a < 10; a++) {
        let c: i32 = a * 2;
    }
}
```

### 7.2 For statement

#### Description

A `for` statement is a loop that iterates over a range of values. It is used to iterate over a sequence of values. The first part of the `for` statement is an initialization part where the variable can be defined and assigned.

#### Examples

```inference
total fn foo() {
    for (let a: i32 = 0; a < 10; a++) {
        let b: i32 = a * 2;
    }
}
```
