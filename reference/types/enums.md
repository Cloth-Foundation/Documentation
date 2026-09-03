# Enums

An enum is a named value type with a closed set of cases. Its filename supplies
the type name.

```cloth
// Status.co
enum {
  Pending,
  running,
  _Done,
}
```

Cases are comma-separated; a trailing comma is allowed. An enum must contain
between 1 and 65,536 distinct case-sensitive identifiers.

## Select and compare cases

```cloth
Status current = Status.Pending;
current = Status.running;
if (current != Status._Done) {
  println(current);
}
```

Select a case with `Type.Case`, not through an instance or with `::`.
Cases are constants, not constructors or writable fields.

Every case is public, including lowercase and underscore-prefixed names. The enum
type itself still follows filename visibility. Imports and aliases select the
type; wildcard imports do not import bare cases.

Assignment, arguments, returns, and equality require the same nominal enum type.
`var` preserves that identity. Enums support `==` and `!=`, but no ordering,
arithmetic, integer conversion, bitwise operations, truthiness, or boxing.

## Initialization and storage

Enum locals require initializers. Instance fields need declaration initializers
or direct assignments on every constructor exit. There is no implicit first-case
default.

A static enum constant directly names a case:

```cloth
static final Status Initial = Status.Pending;
```

Arrays of enums support ordinary indexing, mutation, and iteration.
`Status?` is invalid, while `Status[]?` is a nullable array reference.

## Output and limits

Printing produces the qualified type and case name, such as
`Status.running`. `current::typeName` returns the qualified enum type name.
Import aliases do not change either result.

Enums have a four-byte value representation. Internal tags follow declaration
order and are not stable persistence identifiers or a source-level integer API.

Enums cannot declare fields, functions, constructors, explicit discriminants,
payloads, or a base type. [Switch statements](/docs/reference/language/control-flow#switch-statements)
must cover every case or provide a default. They work in native builds and with
source-free package dependencies. Invalid internal tags trap, even with a default.
Adding a case requires updating switches that have no default. Removing or
renaming a case requires updating references to it, regardless of defaults.
Pattern matching remains unsupported; named-case comparisons with `if` are also
available.
