# 9 Statements

## 9.1 Variable Definition Statement

### Description

TODO

### Modifiers

### 9.1.1 `undef`

When a variable is declared with the `undef` modifier, such definition has a type but omits initialization. Declaration of an undefined variable may appear only inside blocks or functions with non-deterministic semantics (with [total](./functions.md#1111-total-keyword) or `filter` modifiers).

```
let undef x: Type;
```
where `undef` for `x` splits the execution path of nondeterministic computation into sub-paths. Each sub-path returns one of every possible `Type` instances.

### Examples

TODO

## 9.2 `typeof`

### Description

TODO

### Examples

TODO
