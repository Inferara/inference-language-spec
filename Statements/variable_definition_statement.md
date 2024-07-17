# Variable Definition Statement

## Description

### `undef` modifier

When variable is declared with `undef` modifier, such definition has type but omits initialization. Declaration of undefined variable may appear only inside blocks or functions with non-deterministic semantics (with `total` or `filter` modifiers). It may be interpreted as syntactic sugar of the following nature:

```
let undef x: SomeType;
```

is functionally equivalent to

```
let x = SomeType::undef();
```

where `SomeType::undef()` is splitting current execution path of nondeterministic computation into sub-paths, on each of which it returns one of every possible `SomeType` instances.

## Example

