# 13 Directives

## 13.1 Use directive

### Description

The `use` directive is used to link enternal modules to the current module. Linked objects might be:
- [contexts](./definitions.md#101-context) from other `.inf` files
- [functions](./functions.md) from other `.inf` files
- external modules compatible with the target execution platform. It means that it is possible to import WASM modules, but not a mix of WASM and EVM modules.

### Examples

```inference
use inference::std::algorithms::sort;
use { sort, hash } from "./sort.0.wasm";
use inference::std::algorithms::{sort,hash};
```
