# Verify expression

## Description

The keyword `verify` can be used in the body of a proof function. When you put `verify` in front of a non-deterministic block or expression marked with `total`, it means this block must be checked to ensure it always finishes correctly in the current deterministic context. If it can be confirmed that the block finishes correctly in every possible scenario, the `verify` process ignores any non-deterministic effects and continues normally. If it can't be confirmed, the verification process will not finish.

## Example

Verification of total block:

```
external fn predicate (SomeType) -> bool;
type Pred = typeof(predicate);

fn proof(a: Pred, b: Pred) {
  verify total {
    let undef x: SomeType;
    filter assert a(x);
    assert b(x);
  };
}
```

Verification of total function:

```
external fn predicate (SomeType) -> bool;
type Pred = typeof(predicate);

total fn implies(a: Pred, b: Pred) {
  let undef x: SomeType;
  filter assert a(x);
  assert b(x);
}

fn proof(a: Pred, b: Pred) {
  verify implies(a, b);
}
```
