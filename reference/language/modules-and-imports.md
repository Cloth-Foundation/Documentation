# Packages and imports

Packages follow directories beneath a source root. A source file does not
declare a module or embed its filesystem path.

```text
project/
  Shuttle.toml
  src/
    Main.co
    models/
      User.co
    services/
      api/
        Client.co
```

With `src` as the source root, the file types have identities `Main`,
`models.User`, and `services.api.Client`.

Directory components and file stems must be valid Cloth identifiers. Qualified
identities differing only by ASCII case are rejected on every host.

## Import syntax

```cloth
import models::User;
import models::User as ModelUser;
import services.api.*;
import RootType;
```

`.` traverses packages, `::` selects one file type, and `.*` imports public
types directly in a package. Wildcards do not recurse. A single identifier
selects a type from the root package.

Imports must precede members or the file envelope. Aliases are file-local;
they change lookup names, not nominal identity or printed type names.
Imports never re-export names and never import individual members or enum cases.

## Lookup and visibility

Name resolution considers these scopes in order:

1. Locals and parameters.
2. Current file members.
3. Public types in the current source package.
4. Explicit imports and aliases.
5. Wildcard imports.
6. Core symbols.

An explicit import wins over a wildcard. Conflicting wildcard names are ambiguous
unless an explicit import or alias resolves them.
A private file type is available only in its defining file, even to a sibling
in the same package.

Import cycles are allowed: imports do not execute code or paste source text.
Type names and member signatures are registered before bodies are checked.

## Source discovery

Shuttle supplies source roots and recursively includes the package's `.co`
files. Directory symbolic links are not followed.

Direct `clothc --source-root=src ...` closes the entry files' import graph and
includes same-package siblings. Without `--source-root`, the first entry's
directory is the standalone root and unrelated siblings are not automatically
loaded. Files outside the root and unresolved imports are errors.

## Dependencies

A Shuttle dependency's lowercase alias becomes the leading import component:

```cloth
import models::User;
import models.data::Record;
```

For dependency alias `models`, these refer to `User.co` and `data/Record.co`
in that dependency's source root. Only direct dependencies are visible.
Aliases cannot collide with local top-level source packages.

Manifest package names and versions do not appear in source import syntax.
They contribute to compiled type identity; an import alias does not change it.
See [Shuttle projects](/docs/tooling/shuttle) for local dependency configuration.
