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

String indexing, slicing, iteration, interpolation, searching, and implicit
formatting are not currently supported. To print separate values, make separate
[printing calls](/docs/reference/language/printing).
