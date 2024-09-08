# 12 Constants

## Description

Inference supports the following types of constants:
- [boolean](./types.md#611-boolean)
- [i32](./types.md#613-i32)
- [i64](./types.md#614-i64)
- [u32](./types.md#615-u32)
- [u64](./types.md#616-u64)
- [array](./types.md#62-array)
- [structs](./definitions.md#102-structs)

Constants can be define in the following structures:
- [context](./definitions.md#101-context)
- [function](./functions.md)

## Examples

```inference
total fn foo() {
    const x: i32 = 10;
    const y: bool = true;
    const z: u32 = 0;
}
```
