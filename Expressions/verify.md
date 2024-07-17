# Verify expression

## Description

Keyword `verify` may appear in a body of any function of specification module regardless of its ND-modifiers. Putting it in front of any non-deterministic block or expression with `total` semantic indicates that it must be checked for totality within particular context of current deterministic execution path. If successful termination of such block or expression can be confirmed for every possible execution path, verification discards all effects of non-deterministic computation and simply passes unchanged execution state down the normal control flow. Otherwise, verification process doesn't terminate at all.

## Example

Verification of total block:

```
type Pred = fn(x: SomeType) : bool;

fn proof(a: Pred, b: Pred) {
  verify total {
    let undef x: SomeType;
    filter assert a(x);
    assert b(x);
  };

  print 'Assertion b(x) is true for every x satisfying precondition a(x)';
}
```

Verification of total function:

```
type Pred = fn(x: SomeType) : bool;

total fn implies(a: Pred, b: Pred) {
  let undef x: SomeType;
  filter assert a(x);
  assert b(x);
}

fn proof(a: Pred, b: Pred) {
  verify implies(a, b);

  print 'Assertion b(x) is true for every x satisfying precondition a(x)';
}
```
