# 12 Constants

## 12.1 Description

Inference supports the following types of [constants](./definitions.md#101-constant):

- [bool](./types.md#621-boolean)
- [i32](./types.md#622-integers)
- [i64](./types.md#622-integers)
- [u32](./types.md#623-unsigned-integers)
- [u64](./types.md#623-unsigned-integers)
- [array](./types.md#63-array)
- [structs](./definitions.md#107-struct)

Constants can be defined in the following structures:

- [spec](./definitions.md#105-spec)
- [function](./functions.md)

## 12.2 Examples

```inference
spec Constants {
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
