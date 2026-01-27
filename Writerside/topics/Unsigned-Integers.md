# Unsigned Integers

Unsigned integers in Cloth are used to store whole numbers that are always **non-negative**. Unlike signed integers, 
unsigned integers do not reserve a bit for sign. Instead, all bits are used to represent the magnitude of the value, 
allowing for a larger positive range.

Each unsigned integer type has a fixed size and a fixed range of values determined by its bit width.

| Type  | Size    | Bits |
|-------|---------|------|
| `u8`  | 1 byte  | 8    |
| `u16` | 2 bytes | 16   |
| `u32` | 4 bytes | 32   |
| `u64` | 8 bytes | 64   |

Unsigned integers are stored in standard binary form.

---

## Understanding Unsigned Integer Representation

Because unsigned integers do not have a sign bit, every bit contributes to the numeric value. This means:

* All values are ≥ `0`
* The maximum value is higher than the equivalent signed type

For an integer with `N` bits:

```
Minimum value = 0
Maximum value = 2^N - 1
```

---

## Unsigned Integer Ranges

| Type  | Minimum Value | Maximum Value                |
|-------|---------------|------------------------------|
| `u8`  | `0`           | `255`                        |
| `u16` | `0`           | `65,535`                     |
| `u32` | `0`           | `4,294,967,295`              |
| `u64` | `0`           | `18,446,744,073,709,551,615` |

Unsigned integers can represent roughly **twice the positive range** of their signed counterparts.

---

## When to Use Unsigned Integers

Unsigned integers are ideal when:

* A value should never be negative
* You need the maximum possible positive range
* Working with memory sizes, buffer lengths, array indices, bit masks, or hardware registers

Examples:

```
let size: u32 = 4096
let mask: u8 = 0b11110000
let index: u16 = 42
```

---

## Arithmetic Behavior

Unsigned arithmetic follows standard modular arithmetic rules:

* Overflow wraps around to zero
* Underflow wraps around to the maximum value

Example:

```
let x: u8 = 255
x = x + 1  # Result: 0

let y: u8 = 0
y = y - 1  # Result: 255
```

> Note: Cloth may optionally provide overflow-checking operations or compiler diagnostics in debug builds.

---

## Unsigned vs Signed Integers

| Feature             | Signed (`i*`)      | Unsigned (`u*`)      |
| ------------------- | ------------------ | -------------------- |
| Can store negatives | Yes                | No                   |
| Minimum value       | `-2^(N-1)`         | `0`                  |
| Maximum value       | `2^(N-1) - 1`      | `2^N - 1`            |
| Use cases           | General arithmetic | Sizes, masks, counts |

---

## Choosing Between Signed and Unsigned

Use **signed integers** when:

* Values may be negative
* You are doing general-purpose arithmetic

Use **unsigned integers** when:

* Values are guaranteed to be non-negative
* You are modeling sizes, counts, offsets, or raw binary data
