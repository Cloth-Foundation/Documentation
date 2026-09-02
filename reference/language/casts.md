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
integers and floating-point types. Numeric literals may instead adopt an expected
type directly when representable.

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
No numeric conversion wraps, saturates, reinterprets bits, or converts to
`bool`.

A literal conversion is checked at compile time and adopts its destination type
directly. For example, `int8(127)` is valid and `int8(128)` is a compile-time
error.

## Reference widening

Derived classes widen to base classes. Conforming classes widen to interfaces,
and interfaces widen to their parent interfaces. All managed references widen
to `object`. Compatible nullable forms widen to nullable destinations.

Arrays are invariant. `User[]` does not become `object[]`; it may widen as
a whole to `object`. Primitives, enums, and structs do not box into `object`.

## Runtime type tests

```cloth
object value = "Cloth";
bool isText = value is string;
string? text = value as string?;
if (text != null) {
  println(text);
}
```

`is T` requires a non-null runtime-checkable reference target. It returns
false for null and for a mismatching runtime type; it does not trap or smart-cast
the binding to `T`.

`as T?` requires a nullable target. It preserves the reference on success and
returns null on failure. `as T` with a non-null target is invalid.

Checks follow class ancestry and declared interface conformance. Statically
impossible conversions, such as between unrelated concrete class types, are
rejected. Runtime checks targeting arrays are unsupported. Neither operator is
a numeric, enum, or struct conversion.
