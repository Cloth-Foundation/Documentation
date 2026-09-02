# Hello World

> To complete this tutorial, you will need to have Shuttle and Cloth installed.

In this tutorial, we will learn how to write a simple program, using Cloth Object
files and writing source code, which will will then compile to produce an executable
binary. However, this tutorial is written for beginners, it is not intended to be a
comprehensive introduction to Cloth itself. The goal is to sketch out the basics of
Cloth and avoid getting into too much detail.

## Setup

To setup a Cloth project, we will use the Shuttle build system.
Run the following command in the directory you want your project to exist:

```bash
shuttle init hello-world
```

> Spaces are not allowed in project names, neither are capitalized letters.

Running `shuttle init` creates a new directory with the following structure:

```
hello-world/
├── Shuttle.toml
└── src/
```

The `src/` directory is where all your source files will live. The `Shuttle.toml`
file at the root of your project describes how the project should be built.

## The Build File

Open `Shuttle.toml`, you will see the following:

```toml
manifest-version = 1

[package]
name = "hello-world"
version = "0.1.0"

[executable]
entry = "Main.co"

[dependencies]
stdlib = "2026.0.1A"

```

his file is the single source of truth for your project. Let's walk through each
section:

- **`[package]`** — the name and version of your project. The version follows a
`major.minor.patch` format.
- **`[executable]`** — tells the compiler what to produce.
  - `entry` is the location your `Main` function lives.
- **`[dependencies]`** — lists the libraries your project depends on. The `cloth`
entry refers to the Cloth standard library, which provides built-in types and functions.

> The Standard Library is not specifically required, however most projects will
need it.

You should not need to modify `build.toml` for this tutorial. The compiler will
automatically detect the architecture of your computer and use the correct target.
