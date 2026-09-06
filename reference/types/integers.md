# Integers

Integer types have fixed widths on every target.

| Type | Range |
| --- | --- |
| `int8` | -128 to 127 |
| `int16` | -32,768 to 32,767 |
| `int32`, `int` | -2,147,483,648 to 2,147,483,647 |
| `int64` | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 |
| `uint8`, `byte` | 0 to 255 |
| `uint16` | 0 to 65,535 |
| `uint32`, `uint` | 0 to 4,294,967,295 |
| `uint64` | 0 to 18,446,744,073,709,551,615 |

`int` and `uint` are aliases of `int32` and `uint32`. `byte` is an
unsigned eight-bit integer type; it also supplies the element type for binary-data
operations.

## Literals

An integer literal defaults to `int32`. A typed initializer, argument, return,
assignment, or enclosing numeric expression can supply another expected type.

```cloth
var count = 10;                     // int32
int64 distance = 10;                // int64 literal
uint maximum = 4294967295;
uint64 largest = 18446744073709551615;
int8 minimum = -128;
```

Binary, octal, and hexadecimal integers use lowercase `0b`, `0o`, and `0x`
prefixes. Hexadecimal digits may use either case. A leading zero without a base
prefix remains decimal.

```cloth
var mask = 0b1111_0000;           // 240
var permissions = 0o755;          // 493
uint16 color = 0xFF80;
var decimal = 012;                // 12, not octal
```

A single underscore may separate adjacent digits in any integer digit run. It
cannot touch a prefix or suffix, appear twice, or begin or end the run.

An adjacent lowercase suffix fixes the literal's initial type:

| Suffix | Type | Suffix | Type |
| --- | --- | --- | --- |
| `i8` | `int8` | `u8` | `uint8` |
| `i16` | `int16` | `u16` | `uint16` |
| `i32` | `int32` | `u32` | `uint32` |
| `i64` | `int64` | `u64` | `uint64` |

```cloth
var small = 10i8;                  // int8
var largest = 18446744073709551615u64;
int64 widened = 10i8;              // Lossless widening.
int8 rejected = 10i32;             // Invalid implicit narrowing.
```

The literal must fit the selected or contextual type. A leading minus remains
an operator but may form the signed minimum, such as `-128i8`. Negative values
cannot use unsigned suffixes. `byte` remains distinct from `uint8` and has no
suffix; use a contextual `byte` declaration or `byte(value)`.

Suffixes are part of the numeric token. They are case-sensitive and must end at
an identifier boundary: `1I32`, `1i32value`, and `1i8u8` are invalid. There are
no short aliases such as `1i` or `1u`.

Base-prefixed values are always integer literals. Integer suffixes remain
available, as in `0b1111u8` and `0xFFFFu16`; floating suffixes are not. Because
hexadecimal digits are consumed first, `0x1f32` means hexadecimal `1F32`, not
an `f32` literal. Use an explicit checked conversion such as
`float32(0x1F32)` when a floating result is intended.

## Widening and arithmetic

A signed integer widens to a larger signed type. An unsigned integer widens to
a larger unsigned type or a signed type with strictly greater width.
Signed-to-unsigned conversion is never implicit.

```cloth
int16 small = 10;
int32 wide = small;
int32 sum = small + wide;
wide += small;
```

Binary arithmetic uses a common operand type when one operand can widen
losslessly to the other. Compound assignment must preserve the target type;
`small += wide;` is invalid.

Ordinary integer arithmetic is checked at that exact fixed width. If its result
cannot be represented, Cloth terminates before storing a partial value. Division
and remainder throw `DivisionByZero` for an executed zero divisor. See
[operators](/docs/reference/language/operators) for the complete operation and
failure contract.

Use [numeric conversion](/docs/reference/language/casts) for narrowing or changes
of signedness. `Target(value)` checks range, `Target::wrap(value)` reduces modulo
the target width, and `Target::sat(value)` clamps to the target range.
[Operators](/docs/reference/language/operators) covers bitwise and shift
operations; [binary data](/docs/reference/language/binary-data) covers explicit
byte order. `bool` and `char` are not integer operands.

Use `T::parse(text)` when the value comes from runtime text. It accepts the
integer bases and separator rules above, consumes the complete string, and
throws `ParseError` for malformed or out-of-range input. See
[input and primitive parsing](/docs/reference/language/input-and-parsing).
