# Signed Integers

Integers in Cloth are used to store any whole number, negative or positive. Each signed integer type has a fixed size and 
a fixed range of values determined by its bit width.

The most significant bit (MSB) is used as the sign bit:
- `0` indicates a positive value
- `1` indicates a negative value

| Type  | Size    | Bits |
|-------|---------|------|
| `i8`  | 1-byte  | 8    |
| `i16` | 2-bytes | 16   |
| `i32` | 4-bytes | 32   |
| `i64` | 8-bytes | 64   |

All signed integers in Cloth use _**two's complement**_ representation.

In two's complement:
- Positive numbers are stored normally in binary.
- Negative numbers are stored by:
    1. Inverting all bits of the absolute value.
    2. Adding `1` to the result.
  
This allows arithmetic operations to work uniformly on both positive and negative values at the hardware level.
  
## Understanding the Size of Signed Integers
The size of a signed integer determines how many unique values it can represent.
For an integer with `N` bits:

|         |                         |
|---------|-------------------------|
| Minimum | -2<sup>(N - 1)</sup>    |
| Maximum | 2<sup>(N - 1) - 1</sup> |

Which gives Cloth's integer ranges:

| Type  | Minimum Value                | Maximum Value               |
|-------|------------------------------|-----------------------------|
| `i8`  | `-128`                       | `127`                       |
| `i16` | `-32,768`                    | `32,767`                    |
| `i32` | `-2,147,483,648`             | `2,147,483,647`             |
| `i64` | `-9,223,372,036,854,775,808` | `9,223,372,036,854,775,807` |
Smaller integer types use less memory but have a smaller numeric range.
Larger integer types use more memory but allow significantly larger values.

## See Also
- [Floating-Point Numbers](Floating-Point.md)
