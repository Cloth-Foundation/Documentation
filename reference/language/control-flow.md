# Control flow

## Conditions

`if` and `while` accept booleans and nullable-reference presence checks.
Numbers are not booleans, and non-null references are rejected as always-true
conditions.

```cloth
bool enabled = true;
if (enabled) {
  println("enabled");
} else {
  println("disabled");
}
```

Braces are required. To express an additional condition, place another `if`
inside the `else` block.

## While loops

```cloth
var count = 0;
while (count < 3) {
  println(count);
  count++;
}
```

The condition runs before each iteration. `continue` returns to the condition;
`break` exits the innermost loop or switch.

## Classical for loops

```cloth
for (var index = 0; index < 3; index++) {
  println(index);
}
```

The initializer executes once. Each iteration tests the condition, runs the body,
and then runs the updates. Multiple updates are comma-separated and run left
to right.

Initializer, condition, and updates are optional. An omitted condition means the
loop continues until control exits it. `continue` runs the updates before
rechecking the condition; `break` skips them and exits. A local declared in the
initializer belongs to the loop's scope.

## Array iteration

```cloth
int32[] values = [1, 2, 3];
for (var value in values) {
  if (value == 2) {
    continue;
  }
  println(value);
}
```

The array expression is evaluated exactly once. Each iteration loads the next
element into a local copy. `var` infers the element type; an explicit type uses
ordinary assignment compatibility. `final` may qualify the iteration binding.

Reassigning the binding does not replace the element. Index the array to update
stored elements. See [arrays](/docs/reference/types/arrays).

## Switch statements

Use `switch` to select one block from an integer or enum value. It is supported
by `clothc --check`, LLVM/native builds, and Shuttle package compilation.

```cloth
// Status.co
enum { Pending, Running, Done }

// Describe.co
static func Describe(Status status): string {
  switch (status) {
    case Status.Pending, Status.Running: {
      return "active";
    }
    case Status.Done: {
      return "done";
    }
  }
}
```

Selectors must be integers or enums. Parentheses around the selector, the colon
after each label list, and each arm's braces are required. A switch needs at
least one arm. An optional `default: { ... }` must appear once, last.

Enum switches must cover every case or include a default, even in void functions
and constructors. Integer switches may omit default; unmatched values then
continue after the switch. A default remains a possible flow path even when
every current enum case is explicitly listed.

Omit default when each new enum case should require a decision at compile time.
Use default when future cases should share a fallback. Adding a dependency's
enum case rejects an uncovered switch; removing or renaming a referenced case
is still an error even with a default. Shuttle rebuilds affected consumers when
enum declarations or imported constants change, including when cases are reordered.

An enum default handles otherwise-unlisted valid cases, not invalid internal
tags. Invalid tags trap instead of entering a case or fallback body.

Integer labels accept decimal literals, optionally parenthesized or preceded by
one unary minus, if they fit the selector type. Other labels must be qualified
cases of the same enum or accessible, verified `static final` integer/enum
constants. Typed integer constants may widen losslessly but cannot narrow just
because their value fits. Locals, calls, conversions, arithmetic, ranges, and
patterns are not labels. Equal normalized values are duplicates, including
`1` and `01`, or a case and a constant naming that case.

The selector is evaluated once. Each arm is a separate scope and runs to the
end of its block without falling through into the next arm. A trailing `break;`
is optional. `break` exits the nearest loop or switch; `continue` skips switches
and continues the nearest loop, including a classical for-loop's updates.
Without an enclosing loop, `continue` is invalid.

Return completeness, required/final field initialization, and nullable-flow
checking include early breaks and integer no-match paths. Code after a transfer
cannot establish facts for reachable code. A switch allows at most 65,536 value
labels and 65,537 arms, counting default.

## Return and reachability

`return;` exits a void function or constructor. `return value;` exits a
value-returning function. Statements after a guaranteed `return`, `break`,
or `continue` produce unreachable-code warnings.

A value-returning function cannot fall through. A literal `while (true)` or a
conditionless `for` with no reachable `break` does not fall through either.

Loops are limited to these forms. Numeric ranges, destructuring, asynchronous
iteration, switch expressions, and pattern matching are not currently supported.
