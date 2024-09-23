# 12 Constants

## Description

Inference supports the following types of constants:

- [boolean](./types.md#611-boolean)
- [i32](./types.md#613-integers)
- [i64](./types.md#613-integers)
- [u32](./types.md#613-unsigned-integers)
- [u64](./types.md#613-unsigned-integers)
- [array](./types.md#62-array)
- [structs](./definitions.md#103-struct)

Constants can be defined in the following structures:

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

---

[<kbd><br>⏭️ Directives<br><br></kbd>](./directives.md)
[<kbd><br>⏮️ Functions<br><br></kbd>](./functions.md)
