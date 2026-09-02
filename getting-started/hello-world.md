# Hello, World!

Create a directory named `hello-world` with this structure:

```text
hello-world/
  Shuttle.toml
  src/
    Main.co
```

## Describe the project

Write this in `Shuttle.toml`:

```toml
manifest-version = 1

[package]
name = "hello-world"
version = "0.1.0"
source-root = "src"

[executable]
name = "hello-world"
entry = "Main.co"
```

The source root is relative to the directory containing the manifest. The entry
file is relative to that source root. Package names use lowercase words separated
by hyphens; source type names use Cloth identifiers.

No dependencies are needed for this example.

## Write the program

Put this in `src/Main.co`:

```cloth
static func Main() {
  println("Hello, World!");
}
```

The filename defines the type `Main`. The function is also named `Main`: it is
the program's entry point. `static` lets it run without constructing an instance.
An omitted return type means `void`, so this program finishes with status zero.

`println` writes its argument followed by one line feed. Statements end in
semicolons, and function bodies use braces.

## Check and run

From `hello-world/`, run:

```sh
shuttle check
shuttle run
```

The program writes:

```text
Hello, World!
```

Use `--compiler` with the path to your compiler if it is not on `PATH`.
Use `shuttle build` to build without running. The executable
is written beneath `target/x86_64/`.

You can also build this dependency-free program directly:

```sh
clothc --source-root=src --build=hello src/Main.co
```

On Windows, use `--build=hello.exe`. Shuttle reads the manifest; direct
`clothc` commands use only the roots and inputs you supply.

Next, extend the program with [language basics](/docs/getting-started/language-basics).
