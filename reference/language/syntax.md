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

Use `interface { ... }`, `enum { ... }`, or `struct { ... }` for the other
file kinds. Never repeat the filename as `class User { ... }`.
Imports must come first; an explicit envelope consumes the rest of the file.

An explicit class envelope is required for inheritance, interface conformance,
or an `abstract` or `sealed` modifier.

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

Integer literals use decimal digits, such as `42`. Floating literals use digits
on both sides of a decimal point, such as `0.5`. Scientific notation, numeric
base prefixes, digit separators, and type suffixes are not supported. Use a typed
declaration or numeric conversion to choose a numeric type.

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
