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

## Character

`char` represents a Unicode scalar value using 32-bit storage. Character literals
use single quotes; strings use double quotes.

```cloth
char letter = 'A';
println(letter);
```

Character output encodes the scalar as UTF-8. An invalid Unicode scalar traps.
`char` is separate from the numeric integer types and cannot be used as an
integer operand for bitwise or byte-order operations.

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
