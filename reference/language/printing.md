# Printing

The core functions `print(value)` and `println(value)` write one value.
`println` adds one line-feed byte; `println()` writes only that line feed.

```cloth
print("Count: ");
println(3);
println();
```

Output:

```text
Count: 3

```

Neither function inserts spaces, accepts multiple arguments, or interprets
format placeholders. They return void. A local or member named `print` or
`println` shadows the corresponding core overload set.

## Value representations

| Value | Printed form |
| --- | --- |
| String | Its UTF-8 bytes unchanged |
| Boolean | `true` or `false` |
| Character | Unicode scalar encoded as UTF-8 |
| Integer | Base-10 digits, with a minus sign when negative |
| Finite floating-point | Locale-independent shortest round-trippable decimal |
| Special floating-point | `inf`, `-inf`, or `nan` |
| Class instance through `object` | Its virtual `ToString()` result |
| Array | `<Array>` |
| Enum | `qualified.TypeName.CaseName` |
| Struct value | `<qualified.TypeName>` |
| Boxed struct | `qualified.TypeName{Field=value, ...}` |
| Null literal | `null` |

The default class representation is `<qualified.TypeName>` and contains no
address or hash. A class may override `ToString()`. Direct struct and enum
printing does not box values. Import aliases do not change qualified output
names.

Nullable object output prints `null` when absent and dynamically formats a
present value. The bare `null` literal has its own output support.

String bytes, including embedded zero bytes, are preserved. Native Windows output
uses the same line-feed contract as other supported hosts.

User-defined string conversion, interpolation, array-content formatting, and
general formatting APIs are not currently supported. For text concatenation,
both operands of `+` must be strings.
