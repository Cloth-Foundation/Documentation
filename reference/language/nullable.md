# Nullability

Managed references are non-null by default. Add `?` to admit absence:

```cloth
string name = "Cloth";
string? selected = null;
selected = name;
```

`T` can widen to `T?`, but `T?` cannot be used as `T` without a proof or
check. `null` is assignable only to nullable references. Compatible class,
interface, and `object` widening composes with nullability.

Primitives, enums, structs, and void cannot be nullable. Array nullability is
independent of element nullability: `User?[]`, `User[]?`, and `User?[]?`
mean different things. Overloads cannot differ only by nullability.

## Narrowing locals and parameters

```cloth
func Display(string? value): string {
  if (value == null) {
    return "unknown";
  }
  return value;
}
```

A direct null comparison proves presence on the appropriate path. Reversed
operands, parentheses, negation, and short-circuit logical expressions compose
these proofs.

The binding still has its declared nullable type. Reads on a proven path use the
non-null type; assignment invalidates that proof. Fields are not narrowed because
an alias or function call could change them. Copy a field into a local before
checking it when you need stable narrowing.

## Presence conditions

A nullable reference can be used as a condition:

```cloth
string? value = "Cloth";
if (value) {
  println(value::length);
}
if (!value) {
  println("missing");
}
```

Presence is true when non-null. Non-null reference conditions are rejected as
always true. Nullable operands in `&&` and `||` use the same presence rule.

## Safe field access

`receiver?.Field` evaluates the receiver once and returns null if it is absent.
Otherwise it reads the field.

```cloth
// With User exposing a string Name field:
User? user = null;
string? name = user?.Name;
```

The field must be reference-valued. A `T` field becomes `T?`; a `T?` field
stays nullable. Safe primitive-field access, safe method calls, and safe meta
queries are unsupported. Narrow the receiver before those operations.

## Fallback and assertion

```cloth
string? selected = null;
string display = selected ?? "Unknown";
```

`??` evaluates its left operand once and evaluates the fallback only if it is
null. A non-null compatible fallback yields a non-null result; a nullable fallback
keeps the result nullable. The operator associates to the right.

Postfix `value!` asserts presence and yields the non-null value. It traps with
`non-null assertion failed` if absent. A successful assertion also narrows
subsequent reads of a stable local or parameter.

Without narrowing or an assertion, nullable references cannot use ordinary
member access, indexing, meta queries, or iteration.
