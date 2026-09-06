# Memory and errors

Classes, strings, and arrays use managed references. The runtime traces reachable
objects and reclaims unreachable allocations. Ordinary Cloth programs do not
manually free memory, maintain reference counts, or annotate ownership.

The current collector is a precise, non-moving mark-and-sweep collector.
The compiler supplies reference information, including references contained
inside struct fields and array elements. Cyclic object graphs can be reclaimed
when unreachable.

## Values and sharing

Class and array assignment shares a reference. String references share immutable
text. Struct assignment copies inline fields recursively while sharing managed
references stored inside those fields.

`final` prevents rebinding; it does not recursively freeze referenced objects.
See [structs](/docs/reference/types/structs) for inline read-only behavior.

Collection timing is not deterministic destruction. No destructor or finalizer
contract is exposed by the current language. Collector diagnostics are runtime
facilities for testing and embedding, not source-language functions.

## Typed errors

An `error` file defines a managed error type. It derives from the
compiler-provided `Error` root unless another error base is named.

```cloth
// InvalidUser.co
error {
  InvalidUser(string message): Error(message) {}
}
```

`Error` is abstract and supplies the public final `string Message` field.
Errors otherwise follow class rules: they may contain fields and functions,
implement interfaces, derive from one error, and use capitalization for
visibility. Construct an error with its file type name; Cloth does not use
`new`.

The standard-library prelude provides four general and operation-specific
errors:

- `ArgumentError` reports a value a caller is not permitted to supply.
- `StateError` reports an operation that is invalid for the receiver's current
  state.
- `IoError` reports a failure in a portable I/O operation.
- `ParseError` reports malformed or unrepresentable primitive text.

Their canonical package is `cloth.lang.errors`; recursive prelude lookup keeps
the short names available without an import.

All four provide `()` and `(string message)` constructors and may be extended by
more specific application errors. They require no import:

```cloth
func SetLimit(int32 limit) throws ArgumentError {
  if (limit < 0) {
    throw ArgumentError("limit must be non-negative");
  }
}
```

Use a specific error when the API has a more precise failure contract. These
types do not replace compiler-owned `Error`, `DivisionByZero`, or runtime traps.

Use `throw` to complete the current callable with a non-null error. A callable
that can expose an error declares its set after the return type:

```cloth
func Load(): object throws IoError, ParseError {
  return ReadSource();
}

User(string name) throws InvalidUser: Human(name) {
  Name = Validate(name) ?? throw InvalidUser(name);
}
```

Calls use ordinary syntax. When a called function succeeds, evaluation
continues with its result. When it throws, Cloth automatically propagates the
error; no later argument, assignment, or statement on that path executes.
Public functions and constructors must state every exposed error. Private
callables may omit `throws`; the compiler infers their transitive set.

The compiler-provided sealed `DivisionByZero` error is produced when executed
integer `/`, `%`, `/=`, or `%=` uses a zero divisor. A required constant with a
zero divisor remains a compile-time error. Floating division keeps IEEE
behavior.

Null still represents expected absence. `?? throw` promotes absence to an
error without a separate conditional:

```cloth
User user = FindUser(id) ?? throw InvalidUser("unknown user");
```

Cloth currently has no local `try`, `catch`, or `recover` construct. A thrown
error continues to the caller until it reaches a declared throwing `Main`.

## Terminal runtime failures

Some invalid operations are compile-time errors. Others depend on runtime values
and terminate through a runtime trap:

- Out-of-bounds array access.
- A failed non-null assertion.
- An out-of-range checked numeric conversion.
- An invalid dynamic shift count.
- An integer byte operation whose complete range does not fit the array.

A failed safe reference cast is different: `value as T?` returns null. Use
[nullability](/docs/reference/language/nullable) to handle that result.

## Program termination

A void `Main` finishes with status zero. An `int32` `Main` supplies its
returned process status. `Main` may declare `throws`; an escaping error is
written to standard error as `cloth error: Type` with `: Message` appended when
the message is non-empty, and the process returns a nonzero status. Runtime
traps terminate execution instead of returning a successful result.
