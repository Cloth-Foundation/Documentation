# Void

`void` means that a function returns no value. An omitted return annotation
means exactly the same thing.

```cloth
func Implicit() {}
func Explicit(): void {}
```

Return types are not inferred from `return` statements. A void function may
fall through or use `return;`, but it cannot return an expression.

A void call can be an expression statement:

```cloth
println("Cloth");
```

It cannot supply an initializer, argument, condition, operator operand, array
element, or value return.

`void` is invalid for fields, parameters, locals, arrays, iteration bindings,
and nullable types. Use `()` for an empty parameter list, not `(void)`.

Constructor bodies follow void return rules, while a constructor call produces
the newly constructed class reference or struct value.

A native `static func Main()` or `static func Main(): void` produces process
status zero. A `static func Main(): int32` supplies the status explicitly.
