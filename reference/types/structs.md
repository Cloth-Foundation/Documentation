# Structs

A struct is a named aggregate value. Assigning or passing a struct copies it.
Structs support checking, native execution, garbage-collected reference fields,
and compiled package dependencies.

```cloth
// Point.co
struct {
  int32 X;
  int32 Y;

  Point(int32 x, int32 y) {
    X = x;
    Y = y;
  }

  func Moved(int32 dx, int32 dy): Point {
    return Point(X + dx, Y + dy);
  }
}
```

The filename supplies the name. Imports may precede the envelope; declarations
cannot follow it. Members and constructors use the usual capitalization rules.

## Initialize complete values

Every instance field, including primitives, needs a declaration initializer or a
direct assignment on every constructor exit. An embedded struct field must receive
a complete value before its subfields can be used.

Struct locals and struct-valued class fields also require initialization.
There are no default struct values or synthesized constructors. Empty structs
are allowed but still require a constructor to create a value.

Before required initialization is complete, you cannot read uninitialized fields,
copy or expose `self`, or call its instance functions. Final fields require
exactly one initialization.

## Copying and writable storage

```cloth
Point original = Point(1, 2);
Point copy = original;
copy.X = 9;
println(original.X);  // 1

Point[] points = [original];
points[0].X++;
println(points[0].X); // 2
```

Assignments, arguments, returns, array reads, and iteration bindings copy values.
Inline fields copy recursively; class and array references inside a struct copy
shallowly. Copies therefore share the referenced objects.

Only writable storage can be updated. `points[0].X++` changes the stored
element. `Point(1, 2).X++` is invalid because the temporary is not writable
storage.

A final struct binding also protects its inline fields. This protection stops at
a managed reference: a referenced object's fields or array elements may change,
but the reference field in the final struct cannot be replaced.

## Instance methods

An instance method receives a read-only snapshot, captured before its explicit
arguments are evaluated. It can read and return `self`, mutate local copies,
perform I/O, and mutate referenced objects. It cannot change its inline receiver
fields.

The `Moved` method above returns a new point rather than modifying its receiver.
Constructors receive writable, incomplete `self`; assigning an entirely new
value to `self` is invalid.

## Equality and output

`==` and `!=` require the same struct type and compare all instance fields in
declaration order, including private fields. Nested structs compare recursively,
strings by content, and other references by identity. Floating NaN behavior is
preserved. Static fields and padding do not participate.

`println(point)` prints `<qualified.TypeName>`.
`point::typeName` returns that qualified name. Neither operation boxes the value.

## Restrictions

Structs do not inherit, implement interfaces, widen to `object`, support
reference `is`/`as`, or provide truthiness or arithmetic. `Point?` is
invalid; `Point[]?` is valid. Instance calls are direct, with no virtual,
abstract, or final-override functions.

Inline field cycles are rejected. A class or array reference breaks such a cycle
because it does not embed the referenced value's layout.
Static fields retain the scalar-literal or enum-case rules; static struct
constants are unsupported.
