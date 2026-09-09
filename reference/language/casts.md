# Conversions and casts

Cloth separates numeric conversion from runtime reference casts.

## Implicit numeric widening

An existing numeric value converts implicitly only through these rules:

| Source | Implicit destination |
| --- | --- |
| Signed integer | Larger signed integer |
| Unsigned integer, including `byte` | Larger unsigned integer |
| Unsigned integer | Signed integer with strictly greater width |
| `float32` | `float64` |

Equal-width signed and unsigned types do not implicitly convert. Neither do
integers and floating-point types. Unsuffixed numeric literals may instead adopt
an expected type directly when representable. A suffixed literal already has
the exact type named by its suffix and follows the ordinary widening table.

## Explicit numeric conversion

Write the destination type around the operand:

```cloth
int32 count = 120;
int8 small = int8(count);
float64 approximate = float64(count);
int32 whole = int32(approximate);
```

The operand is evaluated once. This is a numeric operation, not an object
constructor.

| Conversion | Runtime behavior |
| --- | --- |
| Integer to integer | Trap unless the mathematical value fits |
| Float to integer | Truncate toward zero, then check range; NaN and infinities trap |
| Integer to float | Round to nearest, ties to even |
| `float64` to `float32` | Round once; finite overflow traps, NaN and infinities are preserved |

Explicit floating conversions accept underflow and ordinary precision loss.
The checked form does not wrap, saturate, reinterpret bits, or convert to
`bool`.

A literal conversion is checked at compile time and adopts its destination type
directly. For example, `int8(127)` is valid and `int8(128)` is a compile-time
error.

A suffix fixes the conversion's source type. `int64(1i8)` converts an `int8`
value; `int8(128i16)` is rejected because the checked conversion cannot
represent the `int16` value. Wrapping and saturating conversions likewise
receive the suffix-selected source type.

## Wrapping and saturating integer conversion

Integer targets also provide explicit conversion modes:

```cloth
int8 wrapped = int8::wrap(300);       // 44
int8 limited = int8::sat(300);        // 127
uint8 residue = uint8::wrap(-1);      // 255
uint8 nonnegative = uint8::sat(-1);   // 0
```

`Target::wrap(value)` computes the mathematical value modulo `2` raised to the
target width. A signed target interprets the resulting bits as two's-complement.
`Target::sat(value)` clamps the mathematical value to the target's inclusive
minimum and maximum.

The target and operand must both be non-nullable integer types. `int`, `uint`,
and `byte` are accepted according to their ordinary aliases and widths. The
operand is evaluated exactly once without adopting the target type, so
`int8::wrap(300)` converts the default `int32` literal. These modes do not trap
for range; the ordinary `Target(value)` form remains the checked default.

## Reference widening

Derived classes widen to base classes. Conforming classes widen to interfaces,
and interfaces widen to their parent interfaces. All managed references widen
to `object`. Compatible nullable forms widen to nullable destinations.

Arrays are invariant. `User[]` does not become `object[]`; it may widen as
a whole to `object`. Primitives, enums, and structs box when converted to
`object`; normal locals, fields, parameters, results, and arrays retain unboxed
value storage.

## Runtime type tests

```cloth
object value = "Cloth";
bool isText = value is string;
string? text = value as string?;
if (text != null) {
  println(text);
}

object count = 42;
bool isInt32 = count is int32;
int32? restored = count as int32?;
```

`is T` requires a non-null runtime-checkable reference target. It returns
false for null and for a mismatching runtime type; it does not trap or smart-cast
the binding to `T`.

`as T?` requires a nullable target. It preserves the reference on success and
returns null on failure. `as T` with a non-null target is invalid.

Checks follow class ancestry and declared interface conformance. Statically
impossible conversions, such as between unrelated concrete class types, are
rejected. A primitive, enum, or struct target checks the exact boxed type and
copies the payload on successful `as`; it never performs numeric widening or
narrowing. Runtime checks targeting arrays are unsupported.
