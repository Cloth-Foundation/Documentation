# Types

Cloth distinguishes values from managed references. A type determines assignment
compatibility, available operations, and how copying behaves.

| Kind | Types | Copy behavior |
| --- | --- | --- |
| Primitive values | Boolean, character, integer, floating-point | Copy the value |
| Named values | Enums and structs | Copy the value; struct reference fields remain shared |
| Managed references | Classes, interfaces, `string`, arrays, `object` | Copy the reference |
| No result | `void` | No value to store or copy |

A class, interface, enum, or struct has nominal identity: matching members alone
do not make two named types interchangeable.

## Numeric types

Signed integers are `int8`, `int16`, `int32`, and `int64`. Unsigned integers
are `uint8`, `uint16`, `uint32`, `uint64`, and `byte`.
Floating-point types are `float32` and `float64`.

The aliases `int = int32`, `uint = uint32`, and `float = float32` are
independent of the target. `bool` and `char` are separate primitive types,
not numeric operands.

## References and absence

References are non-null by default. Write `User?`, `string?`, or `int32[]?`
to admit `null`. Nullable primitives, enums, and structs are not supported:
`int32?`, `Status?`, and `Point?` are invalid.

For arrays, element and array nullability are separate:
`User?[]` contains nullable users; `User[]?` is a nullable array of non-null
users. See [nullability](/docs/reference/language/nullable).

## Inference and conversion

`var` infers a local's exact type from its initializer. It needs a value:
`var value = null;` and an initializer returning `void` are invalid.

Numeric literals can adopt an expected type when representable. Existing numeric
values widen implicitly only through specified lossless conversions. Reference
widening follows class inheritance, declared interfaces, and `object`.
Arrays remain invariant. See [conversions](/docs/reference/language/casts).
