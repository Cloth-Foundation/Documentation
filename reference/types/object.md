# Object

`object` is the universal non-null managed-reference type. Classes, interfaces,
strings, and arrays widen to it without changing their identity.

```cloth
object value = "Cloth";
object? optional = value;
println(value::typeName);
```

`object?` admits null. Primitives, enums, and structs do not widen to `object`;
Cloth does not implicitly box values.

## Identity and available operations

An `object` reference does not expose the concrete type's fields and functions.
Use a checked cast to obtain a more specific reference.

Equality on `object` compares reference identity, including for values that
happen to be strings. Expressions statically typed as `string` use content
equality instead.

Every non-null managed reference supports `::typeName`. Classes report their
qualified source name, strings report `string`, and arrays report `array`.
Widening a reference does not change its runtime type.

## Type tests and safe casts

```cloth
object value = "Cloth";
bool isText = value is string;
string? text = value as string?;
if (text != null) {
  println(text::length);
}
```

`is` requires a non-null target and produces a boolean. `as` requires a
nullable target and produces null when the runtime type does not match.
`is` does not itself narrow a binding to the tested type.

Class checks include base classes; interface checks follow declared conformance.
Null does not match a non-null type. Checked array targets are unsupported.
See [conversions and casts](/docs/reference/language/casts).
