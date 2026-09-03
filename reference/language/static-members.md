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
are booleans, characters, fixed-width integers, floating-point numbers, and
enums. Scalar expressions and references to other constants are supported by
checking, native builds, and package compilation.

```cloth
static final bool Enabled = true;
static final int32 Limit = 16;
// With Status in scope:
static final Status InitialStatus = Status.Pending;
```

Initializers may use scalar expressions and references to other constants:

```cloth
static final int32 BufferSize = 4 * 1024;
static final int32 LastIndex = BufferSize - 1;
static final int8 Minimum = int8(-128);
static final bool Enabled = BufferSize > 0 && LastIndex < BufferSize;
static final float32 Third = 1.0 / 3.0;
```

Values are computed at compile time and stored as constant data. They do not
run initialization code at program startup. Integer and enum constants may also
be used as named `switch` case labels; inline case expressions remain invalid.

Forward references are allowed within normal import and visibility rules.
Dependencies must be acyclic, including references in skipped boolean operands.
All constants are checked, even unused or private ones. A constant reference
keeps its declared type; narrowing still requires an explicit checked conversion.

Arithmetic, comparisons, boolean operations, integer bitwise operations, and
built-in numeric conversions are allowed. Integer arithmetic overflow, evaluated
division by zero, invalid shifts, and failed conversions are errors. Floating
operations round to binary32/binary64 with ties to even; evaluated results must
be finite. Signed zero and subnormals are preserved. Arithmetic underflow may
produce zero, but a nonzero literal that rounds to zero is out of range.

Each evaluated operation must be valid, not just the final result. For example,
`-(-(-2147483648))` is invalid as an `int32` constant: the middle negation
overflows even though cancelling the signs would produce a value that fits.
`-2147483648` itself is a valid signed-minimum literal.

Boolean evaluation short-circuits: `false && (1 / 0 == 0)` is valid. Skipped
operands must still have eligible syntax and types, valid literals, and acyclic
dependencies. Calls and meta operations are not constant expressions.

Mutable static fields, reference-valued static fields, aggregate constants,
local constant propagation, and dynamic initialization remain unsupported.

Constant initializers are limited to 65,536 expression nodes each, nesting depth
256, and 4,096 bytes per numeric literal. Each owning package may declare 65,536
static constants and contain 1,048,576 initializer nodes in total. Parentheses
and skipped operands count toward these limits.

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
