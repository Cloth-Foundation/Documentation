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

The literal must fit the selected type. Negative literals cannot initialize
unsigned types. The compiler diagnoses an out-of-range literal.

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

Use [checked conversion](/docs/reference/language/casts) for narrowing or changes
of signedness. [Operators](/docs/reference/language/operators) covers bitwise and
shift operations; [binary data](/docs/reference/language/binary-data) covers
explicit byte order. `bool` and `char` are not integer operands.
