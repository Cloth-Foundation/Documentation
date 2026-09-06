# Floating-point numbers

Cloth provides IEEE-754 `float32` and `float64`. The alias `float` always
means `float32`, independent of the target.

```cloth
var precise = 0.5;       // float64
float ratio = 0.5;       // float32
var narrow = 0.5f32;     // float32
var whole = 1f64;        // float64
var large = 1.25e10;     // float64
var small = 6.25e-2f32;  // float32
float64 wider = ratio;   // lossless widening
```

A decimal floating literal defaults to `float64`. When an expected floating
type is available, the compiler rounds the literal once to that format and
rejects it if the result is not finite.

The adjacent suffixes `f32` and `f64` fix the initial type. They accept a
decimal floating core (`1.0f32`) or an integer core (`1f32`); the latter is a
floating literal directly, not an integer conversion. A suffix-selected value
does not adopt another contextual type, although `float32` still widens to
`float64` normally.

Suffix spelling is lowercase and atomic. `1F32`, `1f16`, and `1.0i32` are
invalid.

Scientific notation uses `e` or `E`, an optional exponent sign, and required
decimal exponent digits. It is a floating literal even when the mantissa has no
decimal point: `1e3` is `float64`, while `1e3f32` is `float32`. A single
underscore may separate adjacent digits in the integer, fractional, or exponent
run, as in `6.022_140_76e2_3`. It cannot touch the decimal point, exponent
marker, exponent sign, or suffix.

Scientific values follow the same exact round-to-nearest, ties-to-even rule as
ordinary decimal literals. A nonzero value that rounds to zero or infinity is
rejected. Zero remains zero even with a very large exponent. Base-prefixed
integers do not accept floating suffixes or hexadecimal floating-point syntax.

## Conversions

`float32` widens implicitly to `float64`. Integer-to-floating,
floating-to-integer, and `float64`-to-`float32` conversions require explicit
target-type syntax:

```cloth
int32 count = 12;
float64 measurement = float64(count);
float approximate = float(measurement);
int32 whole = int32(measurement);
```

Integer-to-floating conversion rounds to nearest, ties to even. Floating-to-integer
conversion truncates toward zero and traps if the result is out of range or the
input is NaN or infinite.

Runtime narrowing to `float32` rounds once, preserves NaN and infinities, and
traps if a finite input overflows the finite destination range. Underflow and
ordinary precision loss are accepted for explicit conversions.

See [conversions and casts](/docs/reference/language/casts) for the distinction
between runtime conversion and compile-time literal checking.

Use `float32::parse(text)`, `float64::parse(text)`, or the `float` alias for
strict runtime text conversion. Parsing consumes the complete string and throws
`ParseError` for malformed or unrepresentable values. See
[input and primitive parsing](/docs/reference/language/input-and-parsing).

## Output

`print` and `println` use locale-independent, shortest round-trippable decimal
output for finite values. Special values print as `inf`, `-inf`, and `nan`.
