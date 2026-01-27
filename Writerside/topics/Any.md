# Any

`any` is Cloth’s most general type. A value of type `any` can hold a value of **any other type**, including primitives, complex objects, and user-defined types.

It represents the top type in Cloth’s type system and is primarily used when the exact type of a value is not known at compile time or must be flexible by design.

---

## Summary

* **category:** dynamic / top type
* **can hold:** any value
* **nullable:** yes
* **primary use cases:** dynamic data, generic containers, interoperability
* **tradeoff:** reduced compile-time type safety

---

## Basic Usage

```
let a: any = 42
let b: any = "hello"
let c: any = true
let d: any = null
```

All of these values are valid assignments to `any`.

---

## Type Narrowing

Because `any` removes compile-time type guarantees, values should be narrowed back into concrete types before performing type-specific operations.

```
let x: any = 123

# example cast / narrow
let n: i32 = x as i32
```

If Cloth supports runtime type checks:

```
if x is i32 {
    let n = x as i32
}
```

> use `any` at system boundaries, then convert into concrete types as early as possible.

---

## Common Use Cases

### Heterogeneous Collections

```
let values: List<any> = [1, "two", 3.14, false]
```

### Dynamic or Json-like Data

```
let payload: any = {
    "name": "blair",
    "level": 7,
    "active": true
}
```

### Plugin or Scripting Interfaces

`any` is ideal for APIs that must accept user-defined or external data where the type is unknown ahead of time.

---

## Performance and Safety

* operations on `any` may require runtime checks
* misuse of `any` can hide programming errors
* prefer concrete types wherever possible
* treat `any` as an *escape hatch*, not a default

---

## Relation to Other Types

| type   | difference                                           |
|--------|------------------------------------------------------|
| `void` | represents absence of a value, not a value container |
| `null` | represents an explicit empty value                   |
| `any`  | can hold all values, including `null`                |

---

## Design Philosophy

`any` exists to provide flexibility without compromising Cloth’s strong typing model.
It is powerful, but intentionally unsafe compared to concrete types.

Use it sparingly and deliberately.
