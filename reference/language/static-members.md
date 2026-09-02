# Static members

`static` makes a member belong to its file type rather than to an instance.

```cloth
// Counter.co
static final int32 Initial = 0;

static func Next(int32 value): int32 {
  return value + 1;
}
```

Other files can use `Counter.Initial` and `Counter.Next(1)`.
Inside `Counter.co`, those names can be unqualified.

## Static functions

A static function has no `self`. It may use parameters, locals, static members,
imported types, and core operations. To access an instance member, it needs an
explicit object.

Calling a static function through an object is invalid.
Calling an instance function through a type is invalid.

## Static constants

A static field must also be `final` and have an initializer. Supported values
are scalar primitives initialized with literals and enums initialized with
direct cases. Parenthesized forms are allowed.

```cloth
static final bool Enabled = true;
static final int32 Limit = 16;
// With Status in scope:
static final Status InitialStatus = Status.Pending;
```

Calls, references to other static fields, and general constant expressions do
not qualify. Mutable static fields, reference-valued static fields, aggregate
constants, and dynamic initialization are unsupported.

Static fields do not occupy instance storage and cannot be accessed through
objects.

## Program entry point

A native program needs exactly one eligible public static `Main` with no
parameters:

```cloth
static func Main(): int32 {
  println("Done");
  return 0;
}
```

An omitted return annotation or explicit `void` produces status zero.
An `int32` return supplies the process status. The entry point runs without
allocating an instance.
