# Using clothc

Direct compiler mode accepts source files and an optional source root.

```text
clothc [--target=x86_64|wasm32]
       [--source-root=<path>]
       [--check | --emit-llvm[=<path>] | --build=<path>]
       <source.co>...
```

The output modes are alternatives. Direct commands do not read `Shuttle.toml`.

## Check source

```sh
clothc --check --source-root=src src/Main.co
```

This performs source discovery, parsing, semantic checking, typed intermediate
representation verification, and control-flow analysis. It prints a typed program
summary on success, sends diagnostics to standard error, and emits no artifact.

Checking supports the same source types, including structs, without requiring
native code generation.

## Build a native executable

On Linux or macOS:

```sh
clothc --source-root=src --build=program src/Main.co
./program
```

On Windows PowerShell:

```powershell
clothc --source-root=src --build=program.exe src/Main.co
.\program.exe
```

Native output currently requires the x86-64 target, LLVM `llc`, and a configured
C++ linker driver. The program must have exactly one eligible public
`static func Main()`, with a void or `int32` return.

## Emit LLVM IR

```sh
clothc --source-root=src --emit-llvm=program.ll src/Main.co
clothc --target=wasm32 --source-root=src --emit-llvm=program-wasm32.ll src/Main.co
```

Both target layouts support LLVM IR emission. wasm32 IR emission does not supply
a WebAssembly runtime or native execution workflow.

Without an output option, the compiler prints token, syntax, typed program,
control-flow, and ABI summaries for compiler inspection.

## Source roots and dependencies

With `--source-root`, the compiler resolves packages beneath that directory
and includes same-package siblings while closing imports. Without it, the first
entry's directory is the standalone root; unrelated siblings are not
automatically included.

Use [Shuttle](/docs/tooling/shuttle) for dependency graphs and compiled package
reuse. Direct source mode does not act as a dependency manager.
