# Floating-Point Numbers

Floating-point numbers in Cloth are used to store real numbers, including values with fractional components. 
They are designed to represent a wide range of magnitudes, from very small decimals to extremely large numbers, with a 
tradeoff between precision and range.

All floating-point types in Cloth follow the _**IEEE-754**_ standard.

| Type  | Size    | Precision |
|-------|---------|-----------|
| `f16` | 2 bytes | Half      |
| `f32` | 4 bytes | Single    |
| `f64` | 8 bits  | Double    |
<note>Support for f16 may be platform-dependent and is primarily intended for graphics, scientific workloads, and
memory-constrained environments.</note>

## IEEE-754 Representation
A floating-point number is composed of three parts:
```
[ Sign ] [ Exponent ] [ Mantissa (Fraction) ]
```

- Sign bit
    - `0` for positive numbers
    - `1` for negative numbers
- Exponent
    - Encodes the scale (power of two) of the number.
- Mantissa
    - Stores the significant digits of the number.

The value of a floating-point number is calculated as:
```tex
(-1)^{sign} \times 2^{(exponent - bias)} \times (1.fraction)
```
This structure allows floating-point numbers to represent extremely large and extremely small values efficiently.

## Floating-Point Ranges
Approximate ranges for Cloth floating-point types:

| Type  | Minimum (≈)              | Maximum (≈)            |
|-------|--------------------------|------------------------|
| `f16` | 6.10 * 10<sup>-5</sup>   | 6.55 * 10<sup>4</sup>  |
| `f32` | 1.18 * 10<sup>-38</sup>  | 3.40 * 10<sup>38</sup> |
| `f64` | 2.23 * 10<sup>-308</sup> | 1.80 * <sup>308</sup>  |

And their precision:

| Type  | Decimal Digits of Precision |
|-------|-----------------------------|
| `f16` | ~3-4 digits                 |
| `f32` | ~6-7 digits                 |
| `f64` | ~15-16 digits               |

## Special Floating-Point Values
IEEE-754 defines several special values that Cloth supports:

| Value       | Description                                                     |
|-------------|-----------------------------------------------------------------|
| `+Infinity` | Result of overflow or division by zero.                         |
| `-Infinity` | Negative overflow or division by zero.                          |
| `NaN`       | "Not a Number", result of invalid operations (e.g., 0.0 / 0.0). |
| `-0.0`      | Negative zero, distinct from positive zero in representation.   |

The values propagate through calculations according to the IEEE-754 standard.

## Precision and Rounding
Floating-point arithmetic is not exact. Many decimal values cannot be represented perfectly in binary form.

```
let x = 0.1 + 0.2
# x is not exactly 0.3
```
This is expected behavior for all IEEE-754 compliant systems.

Cloth uses the default IEEE-754 rounding mode:
- Round to nearest, ties to even.

This provides the best statistical accuracy for most numerical workloads.

## Choosing the Right Floating-Point Type

| When to use                                         | Recommended type |
|-----------------------------------------------------|------------------|
| Graphics, ML, memory-sensitive workloads            | `f16`            |
| General computation, games, physics                 | `f32`            |
| Financial calculations, simulations, high precision | `f64`            |

## Floating-Point vs Integers

| Feature           | Integers(`i*`) | Floating Point (`f*`) |
|-------------------|----------------|-----------------------|
| Exact arithmetic  | Yes            | No                    |
| Fractional values | No             | Yes                   |
| Range             | Fixed, limited | Extremely large       |
| Precision         | Absolute       | Approximate           |

