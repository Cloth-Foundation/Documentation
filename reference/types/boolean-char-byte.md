# Boolean, character, and byte

## Boolean

`bool` has the values `true` and `false`. Comparisons produce booleans, and
`!`, `&&`, and `||` provide logical negation, conjunction, and disjunction.

```cloth
bool enabled = true;
bool ready = enabled && 3 > 1;
if (ready) {
  println("Ready");
}
```

Logical operations short-circuit. Numeric values do not convert to `bool`.
Nullable-reference presence is also accepted in conditions, as explained under
[nullability](/docs/reference/language/nullable).
`bool::parse(text)` accepts exactly `true` or `false`.

## Character

`char` represents exactly one Unicode scalar value using 32-bit storage. Valid
values range from U+0000 through U+10FFFF, excluding the surrogate range
U+D800 through U+DFFF. Character literals use single quotes; strings use double
quotes.

```cloth
char letter = 'A';
char thread = '🧵';
char maximum = '\u{10FFFF}';
println(letter);
```

A raw literal must contain exactly one well-formed UTF-8 scalar. The Unicode
escape `\u{HEX}` accepts one through six hexadecimal digits, either letter case,
and leading zeroes. For example, `'\u{1F9F5}'` is identical to `'🧵'`, and
`'\u{0}'` is identical to `'\0'`. Empty and multi-scalar literals, malformed
UTF-8, surrogates, and out-of-range escapes are compile-time errors.

Character output encodes the scalar as UTF-8. An invalid Unicode scalar traps.
`char` is separate from the numeric integer types and cannot be used as an
integer operand for bitwise or byte-order operations.
`char::parse(text)` accepts exactly one Unicode scalar value.
Indexing a `string` produces a `char`, and `for in` traverses a string as
Unicode scalar values. See [strings](/docs/reference/types/string).

## Byte

`byte` is an unsigned eight-bit integer with values from 0 to 255. A `byte[]`
is a managed array of bytes.

```cloth
byte[] bytes = [0, 0, 0, 0];
int32 number = 42;
number::writeLittleEndian(bytes, 0);
println(bytes::readInt32LittleEndian(0));
```

See [binary data](/docs/reference/language/binary-data) for range checks and the
complete set of read and write operations.
Use `byte::parse(text)` for strict text conversion, including decimal and
prefixed integer syntax. See
[input and primitive parsing](/docs/reference/language/input-and-parsing).
