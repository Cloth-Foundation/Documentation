# Installation

The source build provides the `clothc` compiler and the separate Shuttle project
manager. The compiler is written in C++23; Shuttle is written in Rust.

## Prerequisites

Install these tools and make them available in your terminal:

- Git.
- CMake 3.25 or newer and Ninja.
- A C++23 compiler.
- LLVM `llc` and a C++ linker driver for native Cloth executables.
- Rust 1.85 or newer with Cargo to build Shuttle.

LLVM `opt` is optional for backend verification tests.

## Build the toolchain

From a checkout of the Cloth compiler repository, initialize its submodules and
run:

```sh
git submodule update --init --recursive
cmake --preset dev
cmake --build --preset dev
cargo build --manifest-path shuttle/Cargo.toml --locked
```

The compiler is `build/dev/clothc` (`build/dev/clothc.exe` on Windows).
Shuttle is `shuttle/target/debug/shuttle` (`shuttle.exe` on Windows).
Add these directories to your `PATH` to use the short commands in this guide,
or invoke each executable by its path.

For compiler-only development, configure with
`cmake --preset dev -DCLOTH_TEST_SHUTTLE=OFF`. To run the development tests after
building, use `ctest --preset dev`.

## Check a program

Save this as `Main.co`:

```cloth
static func Main() {
  println("Hello, Cloth!");
}
```

With `clothc` on your `PATH`:

```sh
clothc --check Main.co
```

A successful check prints the compiler's typed program summary. Diagnostics go
to standard error. This command checks source without invoking the native linker.

Build a native executable on Linux or macOS:

```sh
clothc --build=hello Main.co
./hello
```

On Windows PowerShell:

```powershell
clothc --build=hello.exe Main.co
.\hello.exe
```

Native executable generation currently targets x86-64. The compiler can also
emit LLVM IR for the wasm32 layout; this is not a complete WebAssembly execution
workflow. Continue with [Hello World](/docs/getting-started/hello-world) to create
a Shuttle project.
