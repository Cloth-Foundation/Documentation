# Classes and constructors

A class is the default type defined by a `.co` file. Instances use managed
references: assigning an instance copies the reference.

```cloth
// User.co
final string Name;
int32 visits;

User(string name) {
  Name = name;
}

func Visit(): int32 {
  visits++;
  return visits;
}
```

Construct with `User("Cloth")` and call with `user.Visit()`. Constructors
are declared explicitly; Cloth does not synthesize a default constructor.

## Members and self

Fields and functions are instance members by default. An instance context can
name them directly or through `self`. Use `self.Name` when a parameter or local
shadows a field.

Capitalization controls visibility. `Name` is public, while `visits` is private
to `User.co`. A public class may have private members and private constructors.

## Constructors

The constructor's name is derived from the file stem. In `User.co`:

- `User(...)` declares a public constructor.
- `user(...)` and `_User(...)` declare private constructors.

Call sites always use `User(...)`, including private construction inside the
defining file. A static factory may expose controlled construction.
Constructors cannot overload solely by their public/private spelling.

Constructor bodies have no return annotation. They may use `return;` but
cannot return a value. Constructor calls produce the new object reference.

## Field initialization

| Instance field | Initial value requirement |
| --- | --- |
| Mutable primitive class field | Zero-value default unless explicitly initialized |
| Mutable nullable reference field | Null default unless explicitly initialized |
| Non-null reference, enum, or struct field | Declaration initializer or direct assignment on every constructor exit |
| Final field | Exactly one initialization, at declaration or in each constructor |

Field initializers run in declaration order. Constructor initialization must
directly assign a field of the current instance. Branches must initialize it on
every reachable path, including early returns. An assignment inside a loop does
not establish definite initialization after the loop.

Until required initialization is complete, uninitialized fields cannot be read,
`self` cannot escape, and instance functions cannot be called on the current
object. These rules prevent partially initialized objects from being observed.

Structs have stronger rules: every instance field requires initialization.
See [structs](/docs/reference/types/structs).

## Explicit envelopes

`class { ... }` is equivalent to an unwrapped root class. An envelope allows
a base, interfaces, or class modifiers:

```cloth
// User.co, with Human and Named available
class : Human is Named {
  User(string name): Human(name) {}

  override func GetName(): string {
    return "User";
  }
}
```

See [inheritance](/docs/reference/language/inheritance) for base initialization
and dispatch, and [interfaces](/docs/reference/language/interfaces) for conformance.
