# Cloth

Cloth is a statically typed language for portable native programs. It combines
classes, interfaces, and value types with managed memory and explicit nullability.

```cloth
// Main.co
static func Main() {
  println("Hello, Cloth!");
}
```

Every `.co` file defines a type named after the file. `Main.co` defines `Main`;
you do not repeat the name in a class declaration. Packages follow directories
beneath your source root. Names beginning with an uppercase ASCII letter are
public; lowercase and underscore-prefixed names are private.

Classes, strings, and arrays use garbage-collected references. Structs and enums
are values. References are non-null by default; write `T?` when absence is part
of your data.

## Start learning

1. [Install the toolchain](/docs/installation).
2. [Write and run Hello World](/docs/getting-started/hello-world).
3. [Learn files, values, and functions](/docs/getting-started/language-basics).
4. [Build a program with multiple files](/docs/getting-started/working-with-types).

The [language reference](/docs/reference) explains the supported syntax and
semantics. [Tools and projects](/docs/tooling) covers `clothc`, Shuttle manifests,
and local dependencies.

## Reading the examples

Code blocks marked `cloth` contain Cloth source. A comment such as
`// User.co` identifies the filename required by a type declaration. Short
statement examples belong inside a function unless otherwise stated.

Cloth is under active development. These pages describe implemented behavior,
including native structs, and state relevant limitations where you encounter
them. A complete standard library, remote package registry, generics, and
recoverable exceptions are not currently available.
