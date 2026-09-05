# Shuttle projects

Shuttle manages a package described by `Shuttle.toml`. A Shuttle package is a
build and distribution unit; it may contain many directory-based Cloth source
packages.

## A minimal application

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

The source root is relative to the manifest's directory.
The entry path is relative to the source root and names the file containing the
eligible `Main`.

`manifest-version` and `[package]` are required.
`[executable]` and `[dependencies]` are optional. A package without an
executable can be checked and consumed as a dependency.
One executable is supported; its name defaults to the package name.

Package names use lowercase letters and digits with hyphens between words.
Versions are exact semantic versions. Paths in manifests use `/`.

The source root must stay inside the package directory after resolution.
Source filenames and package-directory components must be Cloth identifiers.
Unknown manifest fields are errors.

## Check, build, and run

```sh
shuttle check
shuttle build
shuttle run
```

These commands discover `clothc` beside Shuttle or on `PATH`. The `--compiler`
option instead selects an explicit filesystem path, relative to the current
directory or absolute; it does not search `PATH` for the supplied name.

By default, Shuttle searches the current directory and its parents for the
nearest `Shuttle.toml`. Select a manifest explicitly with:

```sh
shuttle run --manifest-path my-app/Shuttle.toml --compiler path/to/clothc
```

On Windows, supply the compiler's `.exe` path when needed.

Checking keeps validated interface artifacts beneath
`target/TARGET/check/packages/`. Native builds keep object artifacts beneath
`target/x86_64/packages/` and the executable beneath `target/x86_64/`.

Shuttle reuses unchanged local artifacts after compiler validation. Independent
ready packages can build concurrently; `--jobs 4` limits the scheduler to four
jobs.

## Local dependencies

Add direct local dependencies with source-visible aliases:

```toml
[dependencies]
models = { path = "../models" }
text_utils = { path = "../text-utils" }
```

Each relative path names a directory containing its own `Shuttle.toml`.
Sibling dependency paths may leave the current package directory.
Aliases begin with a lowercase letter and continue with lowercase letters,
digits, or underscores; keywords are invalid aliases. The alias `cloth` is
reserved and cannot appear in a user manifest.

In Cloth source:

```cloth
import models::User;
import text_utils.formatting::Formatter;
```

The alias is the leading package component. It is independent of the manifest
package name. Only direct dependencies are visible; transitive dependencies
are not automatically imported. A dependency's own executable does not supply
the application's entry point.

## Standard library

Shuttle automatically adds the standard library paired with the selected
compiler as the direct dependency `cloth` for every package. Do not add it to
`[dependencies]`.

Standard-library types are still imported explicitly:

```cloth
import cloth.math::Math;
```

There is no general prelude or implicit wildcard import. The compiler and its
adjacent toolchain metadata select one exact compatible library; Shuttle does
not search the current directory, user home, or network for an alternative.

## Current boundary

Local dependency graphs, compiler-paired standard-library injection, and
compiled artifact reuse are supported. Remote registries, version solving, Git
dependencies, and manifest workspaces are not currently provided.

Manifest schema, process protocol, and compiled artifact versions are distinct.
See [compatibility](/docs/tooling/compatibility) when updating a toolchain.
