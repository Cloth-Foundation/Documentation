# Functions

A function declares its parameters after its name and its result after `:`.

```cloth
static func Add(int32 left, int32 right): int32 {
  return left + right;
}
```

Arguments are positional and evaluated left to right. Parameters have explicit
types and may be `final`. Parameter bindings are local to the function.
Passing a class or array copies its reference; passing a struct copies its value.

## Returns

An omitted result annotation means `void`; results are not inferred from
the body. Void functions may fall through or use `return;`.
Value-returning functions must return a compatible value on every reachable exit.

```cloth
func Label(bool enabled): string {
  if (enabled) {
    return "enabled";
  } else {
    return "disabled";
  }
}
```

Abstract functions and interface contracts are the specific bodyless forms.
An ordinary concrete function requires a block.

## Instance and static calls

Functions are instance members unless marked `static`. An instance function
can use `self` and unqualified instance members. Call it through an object.

A static function has no `self`. Call it through its file type, or unqualified
inside its defining file. Calling a static member through an object, or an
instance function through a type, is invalid.

See [static members](/docs/reference/language/static-members) and
[inheritance](/docs/reference/language/inheritance) for dispatch rules.

## Overloads

Overloads differ by canonical parameter types. Return types, parameter names,
`final`, and static ownership do not create distinct overloads. Aliases such
as `int` and `int32` are the same type. Overloads cannot differ only by
reference nullability.

Selection first prefers a complete exact parameter match. Numeric literals use
their default types for that exact-match preference. If no exact candidate exists,
one candidate must be uniquely compatible through literal fitting or ordinary
widening. Several compatible candidates are ambiguous; declaration order is not
a tie-breaker.

For example, a literal `1` selects an `int32` overload when one exists.
If only compatible `int16` and `int64` overloads exist, that literal call is
ambiguous. Specify the intended argument type with `int16(1)` or `int64(1)`.

All file types and member signatures are registered before function bodies are
checked. A function can call a function declared later in the file.
