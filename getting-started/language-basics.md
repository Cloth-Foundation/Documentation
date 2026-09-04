# Language basics

Replace `src/Main.co` from the first tutorial with this program:

```cloth
// Main.co
static func Double(int32 value): int32 {
  return value * 2;
}

static func Main() {
  final string label = "Doubled values:";
  int32[] values = [1, 2, 3];
  var total = 0;

  println(label);
  for (var value in values) {
    int32 doubled = Double(value);
    println(doubled);
    total += doubled;
  }

  if (total == 12) {
    println("Total is twelve.");
  } else {
    println("Unexpected total.");
  }
}
```

Run it with `shuttle run`. It prints the label, then `2`,
`4`, `6`, and `Total is twelve.`, each on its own line.

## Values and bindings

A declaration writes the type before the name: `int32 doubled`.
`var total = 0` infers `int32` from the initializer. Inference does not make
the binding dynamically typed.

`final` prevents reassignment. For a reference such as an array, it protects the
binding while allowing the referenced array's elements to change.

Integer literals normally infer `int32`; decimal floating literals infer
`float64`. Explicit types can give unsuffixed literals a different numeric
context. A width suffix instead fixes the literal's type:

```cloth
int64 count = 10;
float ratio = 0.5;  // float is an alias of float32.
var small = 10i8;   // int8
var precise = 0.5f32;
var mask = 0b1111_0000;
var distance = 1_000_000;
var tiny = 1.5e-2;
```

Integer suffixes are `i8`, `i16`, `i32`, `i64`, `u8`, `u16`, `u32`, and
`u64`; floating suffixes are `f32` and `f64`. See
[integer types](/docs/reference/types/integers) and
[floating-point types](/docs/reference/types/floating-point) for range and
conversion rules.

Binary, octal, and hexadecimal integers use lowercase `0b`, `0o`, and `0x`
prefixes. Scientific notation uses `e` or `E`. A single underscore may separate
adjacent digits, including within an exponent. These spellings do not introduce
new numeric types; normal contextual typing and suffix rules still apply.

## Functions and flow

`Double` accepts one `int32` and declares an `int32` result after `:`.
Value-returning functions must return a compatible value on every reachable
exit. A function without a result annotation returns `void`.

`for (var value in values)` visits the array in index order. Each iteration
gets a local copy of the element. Use `values[index] = value` to change an
array element.

Conditions can use booleans or nullable-reference presence checks. Braces are
required around `if`, `else`, and loop bodies.

See [variables](/docs/reference/language/variables-and-visibility),
[functions](/docs/reference/language/functions), and
[control flow](/docs/reference/language/control-flow) for the complete rules.
