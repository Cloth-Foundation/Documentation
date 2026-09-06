# Input and primitive parsing

`cloth.io.Console` provides portable, line-oriented standard input. Import it
explicitly; I/O is not part of the `cloth.lang` prelude.

```cloth
import cloth.io::Console;

static func Main() throws IoError, ParseError {
  string line = Console.ReadLine() ??
      throw IoError("standard input reached EOF");
  int32 count = int32::parse(line);
  println(count);
}
```

`Console.ReadLine(): string? throws IoError` returns one line without its line
ending. It accepts LF and CRLF, preserves every other character, returns an
empty string for an empty line, and returns `null` at end of input. Redirected
input is decoded as strict UTF-8; malformed input and host read failures throw
`IoError`.

## Primitive parsing

Use `T::parse(text)` to convert a complete, non-null string to a primitive:

```cloth
bool enabled = bool::parse("true");
char marker = char::parse("🙂");
int32 offset = int32::parse("-2_048");
uint16 mask = uint16::parse("0xFF00");
float64 ratio = float64::parse("6.25e-2");
```

The supported targets are `bool`, `char`, `byte`, every fixed-width signed and
unsigned integer, and `float32` or `float64`. The aliases `int`, `uint`, and
`float` select `int32`, `uint32`, and `float32`.

Parsing consumes the entire string and is locale-independent. It does not trim
whitespace. Integers accept decimal, lowercase `0b`, `0o`, and `0x` prefixes;
floats accept strict decimal and scientific notation. A single underscore may
separate adjacent digits. Suffixes such as `i32` and `f64` are not accepted in
text because the meta target already supplies the type.

`bool` accepts exactly `true` or `false`. `char` accepts exactly one Unicode
scalar value. Floating parsing accepts finite values and signed zero, but not
`nan`, `inf`, hexadecimal floats, overflow, or nonzero underflow to zero.

Malformed text throws `ParseError("invalid <type> text")`. A valid numeric
value outside the target range throws
`ParseError("<type> value is out of range")`; aliases use their canonical type
name. Callers declare `ParseError` like any other typed effect. Parsing is a
runtime operation and cannot initialize a constant.

`parse` is case-sensitive, belongs to the language-defined `::` meta namespace,
and is called on a type. `T::Parse(text)` and `value::parse(text)` are invalid.
