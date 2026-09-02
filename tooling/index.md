# Tools and projects

`clothc` compiles Cloth source. Shuttle organizes projects, resolves local
dependencies, and invokes the compiler.

Use [Shuttle projects](/docs/tooling/shuttle) for a manifest-based application or
library. Use [the compiler directly](/docs/tooling/compiler) for a standalone
source file, checking, or LLVM IR output.

The tools have separate responsibilities:

| Shuttle | clothc |
| --- | --- |
| Reads `Shuttle.toml` | Parses and checks `.co` source |
| Resolves and validates dependency graphs | Resolves supplied source imports and visibility |
| Chooses project output locations | Generates and validates compiled artifacts |
| Coordinates check, build, and run | Emits LLVM IR and native code |

The compiler does not read manifests or resolve remote packages.
Shuttle does not redefine language syntax, type rules, or object layout.

See [installation](/docs/installation) for source builds and
[targets and compatibility](/docs/tooling/compatibility) for current output limits.
