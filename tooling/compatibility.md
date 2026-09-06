# Targets and compatibility

Cloth's source-level types and rules are portable. Numeric aliases do not depend
on pointer width, and binary-data operations select byte order explicitly.

## Outputs

| Operation | x86-64 | wasm32 |
| --- | --- | --- |
| Source checking | Supported | Supported |
| LLVM IR emission | Supported | Supported |
| Native executable generation | Supported | Not currently supported |

Structs support native lowering and compiled dependencies. Their inline storage
and contained managed references are handled by the compiler and runtime.

## Versioned boundaries

The current contracts are:

| Boundary | Version |
| --- | --- |
| Shuttle manifest schema | 1 |
| Shuttle compiler process protocol | 2 |
| Compiler artifact format | 5 |
| Compiler ABI | 5 |
| Runtime ABI | 5 |
| Build receipt schema | 1 |
| Toolchain metadata schema | 1 |

These versions are independent. The process protocol describes communication
between tools; the artifact and ABI versions describe the compiled data and code
they exchange.

Shuttle advertises and validates artifact format 5 through public capabilities
and receipts. Runtime ABI 5 is compiler-owned metadata inside the opaque
artifact; `clothc` validates it during inspection, reuse, and linking.

Rebuild older artifacts when moving to the current compiler/runtime contract.
Incompatible versions must fail validation rather than being guessed or silently
adapted. Source-free dependencies preserve type identity, public contracts, and
the private layout information needed for correct linking and reference tracing.

Ordinary source code should use the language's types, members, and meta operations.
Internal tags, field offsets, symbol encodings, and printed type names are not
portable persistence formats.
