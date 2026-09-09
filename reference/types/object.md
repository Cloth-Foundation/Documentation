# Object

`object` is Cloth's universal non-null object view. Classes, interfaces,
strings, and arrays widen to it without changing identity. Primitive, enum, and
struct values box into a managed object when they cross an `object` boundary;
their ordinary storage remains unboxed.

```cloth
object text = "Cloth";
object number = 42;
object? optional = number;
println(number.ToString());
```

`object?` admits null. A present nullable value boxes its payload when needed;
an absent nullable value becomes null.

## Identity and available operations

An `object` reference exposes `Equals(Object?)`, `HashCode()`, and `ToString()`.
It does not expose other members of the concrete type. Use a checked cast to
recover an exact boxed value or a more specific reference.

Equality on `object` compares reference identity, including for values that
happen to be strings. Expressions statically typed as `string` use content
equality instead.

`Equals` is separate from `==`. Value boxes compare the exact canonical type
and payload. Their hashes agree whenever `Equals` is true. `ToString` formats
the payload; it never derives text from a hash or native address.

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

object count = 42;
bool isInt32 = count is int32;
int32? restored = count as int32?;
```

`is` requires a non-null target and produces a boolean. `as` requires a
nullable target and produces null when the runtime type does not match.
`is` does not itself narrow a binding to the tested type.

Class checks include base classes; interface checks follow declared conformance.
Value checks require the exact boxed type: an `Int32` box does not unbox as
`int64` or `uint32`. Null does not match a non-null type. Checked array targets
are unsupported.
See [conversions and casts](/docs/reference/language/casts).
