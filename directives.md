# 13 Directives

## 13.1 Use Directive

### Description

The `use` directive is used to link external modules to the current module. Linked objects might be:

- [contexts](./definitions.md#101-context) from other `.inf` files
- [functions](./functions.md) from other `.inf` files
- external modules compatible with the target execution platform. It means that it is possible to import WASM modules, but not a mix of WASM and EVM modules.

### Examples

```inference
use inference::std::algorithms::sort;
use { sort, hash } from "./sort.0.wasm";
use inference::std::algorithms::{sort, hash};
```

---

[<kbd><br>⏭️ Grammar<br><br></kbd>](./grammar.md)
[<kbd><br>⏮️ Constants<br><br></kbd>](./constants.md)
