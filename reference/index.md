# Language reference

The reference describes the syntax and behavior you can use in Cloth programs.
Begin with the [tutorials](/docs/getting-started) if you are new to the language.

[Types](/docs/reference/types) covers primitives, strings, arrays, objects, enums,
structs, and `void`. [Language rules](/docs/reference/language) covers declarations,
functions, control flow, classes, interfaces, conversions, and memory.

For building and organizing projects, use [Tools and projects](/docs/tooling).

## Core conventions

- Source files end in `.co` and define one type named by the file stem.
- Packages come from source directories; source files have no `module` declaration.
- Identifiers are case-sensitive; uppercase declaration names are public.
- References are non-null unless qualified with `?`.
- `int`, `uint`, and `float` always mean `int32`, `uint32`, and `float32`.
- Statements use semicolons; function and control-flow bodies use braces.

Examples that intentionally show invalid code are labeled as such.
