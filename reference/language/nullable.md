# Nullability

Cloth types are non-null by default. Add one `?` to admit absence:

```cloth
string? name = null;
int32? count = 3;
Status? status = Status.Ready;
Point? point = Point(1, 2);
```

Primitive, enum, struct, class, interface, string, object, and array values can
be nullable. `void`, `null`, and an already nullable type cannot be followed by
`?`. Overloads cannot differ only by nullability.

Nullable references retain their reference representation. Nullable primitive,
enum, and struct values are inline tagged values; they are not boxed objects.
This representation is compiler-owned and is not a persistence format.

## Assignment, conversion, and inference

`T` widens to `T?`, and `null` converts to any `T?`. A nullable value cannot be
used as `T` without a proof, fallback, or assertion. Numeric widening lifts
through nullability:

```cloth
int16? small = 12;
int32? wide = small;
var inferred = true ? 12 : null;  // int32?
```

The lifted conversion preserves absence and converts only a present payload.
Compatible class, interface, and `object` widening also composes with
nullability. Narrowing conversions still require an explicit cast.

Array nullability is independent of element nullability: `User?[]`, `User[]?`,
and `User?[]?` mean different things. Nullable value elements such as `int32?[]`
and `Point?[]` are supported.

Mutable nullable locals and mutable nullable fields default to `null` when no
initializer is present. Final locals still require an initializer. Constructors
must initialize every struct field, including nullable fields.

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

The binding retains its declared nullable type. Reads on a proven path use the
non-null type; assignment invalidates that proof. Fields are not narrowed because
an alias or function call could change them. Copy a field into a local before
checking it when stable narrowing is required.

## Presence conditions

Any nullable value can be used as a condition:

```cloth
bool? enabled = false;
if (enabled) {
  println("present, even though the payload is false");
}
```

The condition tests presence, not the payload. This rule is especially important
for `bool?`: both `true` and `false` are present. `!value`, `&&`, and `||` use the
same presence rule. A non-null value condition is rejected unless its ordinary
type is `bool`.

## Safe fields and calls

`receiver?.Field` evaluates the receiver once. If absent, it returns null;
otherwise, it reads the field. A `T` result becomes `T?`, while a `T?` result
stays nullable.

```cloth
User? user = FindUser();
string? name = user?.Name;
int32? age = user?.Age;
Point? position = user?.Position;
```

The same operator safely invokes declared instance functions:

```cloth
int32? age = user?.GetAge();
Point? moved = point?.Moved(1, 2);
logger?.Flush();
```

On absence, arguments are not evaluated and a throwing call cannot throw.
A safe `void` call performs no action and produces `void`. Static functions,
constructors, and unresolved members cannot be called safely.

Safe indexing and slicing are not supported. Narrow or assert the receiver
before those operations.

## Safe meta queries

Use `?::` for a non-callable meta query:

```cloth
int32? length = text?::length;
string? kind = value?::typeName;
```

The receiver is evaluated once. Absence yields null; presence performs the same
query as `::`. Callable meta operations such as `parse`, `slice`, `wrap`, and
`sat` do not support `?::`.

## Fallback and assertion

```cloth
string? selected = null;
string display = selected ?? "Unknown";
int32 count = maybeCount!;
```

`??` evaluates its left operand once and evaluates the fallback only when it is
absent. A non-null compatible fallback yields a non-null result; a nullable
fallback keeps the result nullable. The operator associates to the right.

Postfix `value!` asserts presence and yields the non-null payload. It terminates
with `non-null assertion failed` if absent. A successful assertion also narrows
subsequent reads of a stable local or parameter.

Two nullable values of the same underlying type support `==` and `!=`. Two
absent values are equal; one absent and one present value are unequal; two
present values use the underlying type's equality rule.
