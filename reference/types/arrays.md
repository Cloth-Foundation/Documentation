# Arrays

An array is a managed reference to a fixed-length sequence of mutable elements.
Write its type as `T[]`.

```cloth
int32[] values = [1, 2, 3];
values[1] = 4;
println(values[1]);
println(values::length);
```

The index must have type `int32` (`int` is its alias). `::length` is a
read-only `int32` query. Negative indices and indices at or beyond the length
trap on both reads and writes.

## Literals and element types

Elements are evaluated from left to right. Numeric literals can use an explicit
array element type, as in `int64[] values = [1, 2, 3];`.

Reference elements infer a type from the first non-null element. Null or nullable
elements make it nullable. Different managed-reference types join at `object`.

```cloth
string?[] labels = ["first", null];
object[] mixed = ["text", [1, 2]];
```

Empty literals and null-only literals are unsupported, including with an explicit
array type. Repeated `[]` suffixes, resizable arrays, slices, and multidimensional
array syntax are also unsupported.

## References and copying

Array assignment copies the reference. Equality compares array identity, not
element contents. An indexed struct read copies the struct value; reference
elements continue to refer to the same objects.

Arrays are invariant: `User[]` cannot be assigned to `object[]`.
An entire array can widen to `object`, which does not permit writing arbitrary
elements into it.

`final` protects an array binding but allows element updates:

```cloth
final int32[] values = [1, 2];
values[0] = 3;
```

## Nullability

| Type | Meaning |
| --- | --- |
| `User[]` | Non-null array of non-null users |
| `User?[]` | Non-null array of nullable users |
| `User[]?` | Nullable array of non-null users |
| `User?[]?` | Nullable array of nullable users |

Narrow a nullable array before indexing, querying length, or iterating.
`Status[]?` and `Point[]?` are valid even though enum and struct elements
cannot themselves be nullable.

## Iteration

```cloth
int32[] values = [1, 2, 3];
for (final var value in values) {
  println(value);
}
```

The array expression is evaluated once. The loop visits increasing indices and
creates a local element copy for each iteration. Reassigning that binding does
not replace the array element. `break` exits; `continue` advances to the next
element. An explicit binding type uses ordinary assignment compatibility.

There is currently no general iterator protocol or string iteration. Runtime
`is` and `as` checks targeting array types are unsupported.
