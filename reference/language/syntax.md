# Source files and syntax

Each `.co` file defines one type whose name is its filename stem.
`User.co` defines `User`. By default, that type is a class.

```cloth
// User.co
string Name;

User(string name) {
  Name = name;
}
```

Fields, functions, and constructors at file scope belong to the file type.

## File envelopes

An optional unnamed envelope makes the file kind explicit:

```cloth
// User.co
class {
  string Name;

  User(string name) {
    Name = name;
  }
}
```

Use `interface { ... }`, `enum { ... }`, `struct { ... }`, or `error { ... }`
for the other file kinds. Never repeat the filename as `class User { ... }`.
Imports must come first; an explicit envelope consumes the rest of the file.

An explicit class or error envelope is required for inheritance, interface
conformance, or an `abstract` or `sealed` modifier.

Nested type declarations are not currently supported. Put each type in its own
file. There are no module declarations, annotations, traits, fragments, generic
declarations, or tuple types in the implemented syntax.

## Identifiers

An identifier begins with an ASCII letter or underscore and continues with
ASCII letters, decimal digits, or underscores. Keywords cannot be identifiers.
Package directory components and source file stems obey the same rules.

Identifiers are case-sensitive. Declarations beginning with `A` through `Z`
are public; lowercase and underscore-prefixed declarations are private.
[Enum cases](/docs/reference/types/enums) are the exception: all are public.
Local variables and parameters have lexical scope regardless of capitalization.

## Comments and literals

`//` starts a comment extending to the end of the line. `/* ... */` encloses a
block comment; block comments do not nest.

Integer literals use decimal digits, such as `42`. Floating literals ordinarily
use digits on both sides of a decimal point, such as `0.5`. An adjacent lowercase
suffix selects an exact existing numeric type: `42i8`, `42u64`, `0.5f32`, and
`1f64` are valid. The complete suffix sets are documented under
[integers](/docs/reference/types/integers) and
[floating-point numbers](/docs/reference/types/floating-point).

A suffix is part of its numeric token and must end before another identifier
character. Scientific notation uses `e` or `E`, as in `1.5e-2` and `1e3f32`.
Lowercase `0b`, `0o`, and `0x` prefixes select binary, octal, and hexadecimal
integer notation. Hexadecimal digits accept either case. A single underscore
may separate adjacent digits in any digit run: `1_000`, `0xFF_80`, and
`1.25e1_0` are valid.

Separators cannot touch a prefix, decimal point, exponent marker, exponent sign,
or suffix, and cannot appear consecutively. Base-prefixed values are integers;
they do not accept decimal points, exponents, or floating suffixes. Prefixes and
suffixes are lowercase. `0x1f32` is a hexadecimal integer because hexadecimal
digits are consumed before suffix recognition.

Strings use double quotes and characters use single quotes. Supported escapes
are `\n` (line feed), `\r` (carriage return), `\t` (tab), `\0` (zero), `\\`
(backslash), `\"` (double quote), and `\'` (single quote). A literal cannot
contain an unescaped line break. See [strings](/docs/reference/types/string) for
UTF-8 content rules.

## Statements and expressions

Statements such as declarations, assignments, calls, and returns end in
semicolons. Bodies use braces, including single-statement `if` and loop bodies.

```cloth
static func Main() {
  var count = 2;
  if (count > 0) {
    println(count);
  }
}
```

Calls use parentheses, declared members use `.`, and language-defined
[meta operations](/docs/reference/language/meta-operations) use `::`.
Package imports also use `::`, in declaration syntax.

Use parentheses to group expressions. The complete precedence table is under
[operators](/docs/reference/language/operators).
