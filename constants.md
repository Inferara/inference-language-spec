# 12 Constants

## 12.1 Description

Inference supports the following types of [constants](./definitions.md#101-constant):

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

## 12.2 Examples

```inference
context Constants {
    const a: i32 = 10;
    const b: bool = true;
    const c: u32 = 0;

    fn define_local_constants() {
        const d: i32 = 10;
        const e: bool = true;
        const f: u32 = 0;
    }
}
```

---

[<kbd><br>⏮️ Functions<br><br></kbd>](./functions.md)
[<kbd><br>⏭️ Directives<br><br></kbd>](./directives.md)
