# Strings

`string` is Cloth's built-in immutable UTF-8 reference type. It is non-null by
default; `string?` also permits `null`. The lowercase name is significant:
`String` is a different, user-defined name.

```cloth
string greeting = "Hello, " + "Cloth";
bool same = greeting == "Hello, Cloth";
println(greeting);
```

## Concatenation and equality

`+` concatenates two strings into a new value. It does not modify either input
and does not implicitly convert numbers or other types to text.

String `==` and `!=` compare UTF-8 content. Two nullable strings are equal
when both are null; a null and a non-null string are unequal.

When values are statically typed as `object`, equality compares reference
identity instead. See [object](/docs/reference/types/object).

## Unicode and lengths

String literals must decode to well-formed UTF-8. Cloth preserves the decoded
Unicode scalars without normalization, so different scalar sequences can compare
unequal even if they appear visually identical.

The escape `\u{HEX}` inserts one Unicode scalar using one through six
hexadecimal digits. Hexadecimal letters may use either case, and leading zeroes
are allowed:

```cloth
string thread = "\u{1F9F5}"; // Same content as "🧵".
string nul = "A\u{0}B";      // U+0000 is ordinary string content.
```

Signs, spaces, underscores, prefixes, missing braces, surrogates, and values
above U+10FFFF are rejected. The existing `\n`, `\r`, `\t`, `\0`, `\\`, `\"`,
and `\'` escapes remain available.

| Expression | Result |
| --- | --- |
| `text::length` | `int32` count of Unicode scalars |
| `text::byteLength` | `int32` count of UTF-8 bytes |
| `text::isEmpty` | `bool`, true when byte length is zero |
| `text::typeName` | `string`, with value `"string"` |

A scalar count is not necessarily a count of displayed characters. Embedded
U+0000 is ordinary content: it counts toward both lengths and does not terminate
output or comparison.

These queries are read-only and do not take parentheses. Narrow a nullable
string first:

```cloth
string? text = "Cloth";
if (text != null) {
  println(text::length);
}
```

## Scalar indexing

Indexing counts Unicode scalars from zero and returns a `char` value:

```cloth
string text = "A🧵Z";
char first = text[0];
char thread = text[1];
char last = text[2];
```

The index must be assignable to `int32`. A negative index or an index greater
than or equal to `text::length` terminates with
`cloth runtime error: string index is out of bounds`. The result is not a
writable location because strings are immutable. A nullable string must first
be narrowed or asserted non-null.

Indexing is based on scalars, not UTF-8 bytes. It uses constant auxiliary space
and may scan the string bytes to reach the requested scalar.

## Scalar iteration

`for in` visits Unicode scalars in source order:

```cloth
for (var scalar in text) {
  println(scalar);
}

for (final char scalar in text) {
  println(scalar);
}
```

`var` infers `char`. The string expression is evaluated once, an empty string
executes no body, `continue` advances to the next scalar, and `break` stops
without decoding another scalar. Reassigning a non-final iteration variable
changes only that local value. A complete traversal is linear in the string's
UTF-8 byte length and allocates no managed storage for the loop itself.

## Slicing

`slice(start, end)` selects a half-open range of Unicode scalars and returns a
new immutable `string`:

```cloth
string text = "A🧵BC";
string middle = text::slice(1, 3); // "🧵B"
```

Both bounds must be assignable to `int32`. A slice is valid when
`0 <= start <= end <= text::length`; equal bounds return an empty string.
Bounds count Unicode scalars rather than UTF-8 bytes, so combining marks remain
separate positions and embedded U+0000 remains ordinary data.

The receiver, start, and end expressions evaluate left to right exactly once.
Invalid bounds terminate with
`cloth runtime error: string slice is out of bounds`. A nullable receiver must
first be narrowed or asserted non-null. The result is a value, not writable
storage or a view.

Interpolation, searching, and implicit formatting are not currently supported.
To print separate values, make separate
[printing calls](/docs/reference/language/printing).
