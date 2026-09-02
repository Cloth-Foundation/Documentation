# Memory and runtime failures

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

## Checked failures

Some invalid operations are compile-time errors. Others depend on runtime values
and terminate through a runtime trap:

- Out-of-bounds array access.
- A failed non-null assertion.
- An out-of-range checked numeric conversion.
- An invalid dynamic shift count.
- An integer byte operation whose complete range does not fit the array.

A failed safe reference cast is different: `value as T?` returns null.
Use [nullability](/docs/reference/language/nullable) to handle that result.

Cloth does not currently provide recoverable exceptions, `try`/`catch`,
or checked-exception declarations. Represent expected absence or failure in your
program's data, such as a nullable reference or an explicitly defined status,
and validate inputs before operations that would trap.

## Program termination

A void `Main` finishes with status zero. An `int32` `Main` supplies its
returned process status. Runtime traps terminate execution instead of returning
a successful result.
