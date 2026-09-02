# Variables and visibility

Declare a local with a type, or use `var` to infer its type:

```cloth
int32 count = 3;
var label = "Cloth";
count = count + 1;
```

`var` requires an initializer and preserves its exact canonical type.
`null` alone and void calls cannot supply an inferred type.
A nested block can shadow an outer local; duplicate declarations in one scope
are invalid.

Fields and parameters use explicit types. Enum and struct locals require
initializers; final locals do too. Initialize locals when declaring them to keep
their starting values clear.

## Visibility through names

| Declaration name | Visibility |
| --- | --- |
| Starts with `A` through `Z` | Public |
| Starts with `a` through `z` or `_` | Private |

The rule applies to file types, fields, functions, and constructors.
`User.co` is public; `user.co` is private to its own file.
Private members are accessible only within their defining file class, not from
other files in the same package or from derived classes.

Locals and parameters are scoped lexically; uppercase names do not export them.
Core types such as `string` and `int32` are language-provided names, so their
lowercase spelling does not make them private. Enum cases are always public
when their containing type is accessible.

## Final bindings

`final` prevents a binding from being assigned again:

```cloth
final int32 limit = 10;
final var title = "Cloth";
final int32[] values = [1, 2];
values[0] = 3;
```

The array element write is valid: `final` protects the reference binding, not
the referenced object. For a struct, it also protects inline fields, stopping
at any managed-reference boundary.

Parameters and iteration bindings may be final:

```cloth
func Write(final int32[] values) {
  for (final var value in values) {
    println(value);
  }
}
```

`final` does not change type identity or overload selection.

## Final fields

A final field needs a declaration initializer or exactly one direct assignment
on every constructor exit. Declaration initializers run in field order.
Reading a later uninitialized final field or assigning an initialized final field
again is invalid.

Constructor assignments use `Field = value;` or `self.Field = value;`.
Each reachable branch must establish initialization. Initialization inside a loop
cannot satisfy the exactly-once requirement, and early `return;` paths are
checked too.

See [classes and constructors](/docs/reference/language/classes) for the other
required field-initialization rules.
