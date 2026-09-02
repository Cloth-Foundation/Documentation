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
`break` exits the innermost loop.

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

## Return and reachability

`return;` exits a void function or constructor. `return value;` exits a
value-returning function. Statements after a guaranteed `return`, `break`,
or `continue` produce unreachable-code warnings.

A value-returning function cannot fall through. A literal `while (true)` or a
conditionless `for` with no reachable `break` does not fall through either.

Loops are limited to these forms. Numeric ranges, destructuring, asynchronous
iteration, switch statements, and pattern matching are not currently supported.
