# Operators

The table lists precedence from lowest to highest. Operators on one row share
a precedence level; parentheses override the grouping.

| Level | Operators | Associativity |
| --- | --- | --- |
| 1 | `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `<<=`, `>>=`, `&=`, `\|=`, `^=` | Right |
| 2 | `??` | Right |
| 3 | `\|\|` | Left |
| 4 | `&&` | Left |
| 5 | `\|` | Left |
| 6 | `^` | Left |
| 7 | `&` | Left |
| 8 | `==`, `!=` | Left |
| 9 | `<`, `<=`, `>`, `>=`, `is T`, `as T` | Left |
| 10 | `<<`, `>>` | Left |
| 11 | `+`, `-` | Left |
| 12 | `*`, `/`, `%` | Left |
| 13 | Prefix `!`, `+`, `-`, `~`, `++`, `--` | Right |
| 14 | Calls, members, meta operations, indexing, postfix `!`, `++`, `--` | Left |

## Numeric operations and assignment

Binary numeric operations use a common type when one operand can widen
losslessly to the other. Compound assignments must retain compatibility with the
target type. See [numeric conversions](/docs/reference/language/casts).
Numeric suffixes fix an operand's initial type, so `1i8 + 2i64` uses `int64`;
they do not introduce a separate arithmetic rule.

`%` and `%=` require integer operands. Strings also support `+` and `+=` with
another string; these concatenate text without converting other value types.

Prefix `++value` and `--value` yield the updated numeric value. Postfix
`value++` and `value--` yield the previous value. The target must be writable.

Compound assignments evaluate the target once. For a nested struct storage path,
the owner and index are captured before the right-hand side; the current field
value is loaded after the right-hand side is evaluated.

Ordinary integer `+`, `-`, `*`, and unary `-` are checked at the resolved fixed
width. An unrepresentable result terminates with
`cloth runtime error: integer arithmetic overflow`. Executed integer `/`, `%`,
`/=`, and `%=` with a zero divisor throw `DivisionByZero` and therefore require
a covering `throws` contract unless the divisor is proven nonzero during
semantic analysis. Signed minimum divided or remaindered by `-1` is overflow.
A leading minus forms a signed literal value, so every signed minimum literal
remains valid; negating that value later is checked.

The same checks apply to integer `++`, `--`, and arithmetic compound assignment,
and a failing update performs no store. Floating-point arithmetic retains IEEE
behavior rather than using these integer guards.

## Bitwise operations and shifts

`&`, `|`, `^`, and `~` accept fixed-width integers, including `byte`.
They do not accept booleans, characters, or floats. Binary bitwise operations
use the same common-type rule as integer arithmetic; `~` preserves its type.

For `value << count` and `value >> count`, the left operand supplies the
result type and width. The count may have any integer type and must be at least
zero and strictly less than that width.

Out-of-range literal counts are compile-time errors; invalid dynamic counts
trap. Left shift discards bits beyond the fixed width and fills with zero.
Right shift extends the sign for signed integers and fills with zero for unsigned
integers.

## Equality

| Operand type | Meaning of equality |
| --- | --- |
| Primitive | The primitive type's value comparison |
| String | UTF-8 content equality |
| Class, interface, array, or `object` | Reference identity |
| Enum | Same case of the same enum type |
| Struct | Fieldwise comparison of the same struct type |

Struct comparison recursively uses each field's equality. Strings viewed as
`object` use identity. Null is supported by nullable-reference equality.

## Logical and null operations

`&&` and `||` short-circuit. Prefix `!` negates a boolean or tests a
nullable reference for absence.

`?.` safely reads a reference-valued field, `??` selects a lazy null fallback,
and postfix `!` asserts presence. See [nullability](/docs/reference/language/nullable).
`is` and `as` operate on runtime reference types; they are not numeric
conversions.
