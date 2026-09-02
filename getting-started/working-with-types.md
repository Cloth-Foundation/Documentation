# Working with types

Add a class to the tutorial project:

```text
src/
  Main.co
  models/
    User.co
```

## Define a class

Write `src/models/User.co`:

```cloth
// User.co
final string Name;

User(string name) {
  Name = name;
}

func Greeting(): string {
  return "Hello, " + Name;
}
```

The file defines `models.User`. Its members belong to that class without an
enclosing named declaration.

`Name`, `User`, and `Greeting` are public because they begin with uppercase
letters. The constructor initializes the final field exactly once.
`string` is non-null, so every constructor must provide its value.

## Construct and call

Replace `src/Main.co` with:

```cloth
// Main.co
import models::User;

static func Main() {
  User user = User("Cloth");
  println(user.Greeting());

  User? selected = null;
  println(selected?.Name ?? "No user selected.");

  selected = user;
  if (selected != null) {
    println(selected.Greeting());
  }
}
```

Run the project. It prints:

```text
Hello, Cloth
No user selected.
Hello, Cloth
```

The import selects the file type beneath `models/`. Constructor calls use the
type name without `new`. Instance functions use `.`.

## Represent absence explicitly

`User?` may hold `null`. The expression `selected?.Name` returns the field
when a user exists, or `null` otherwise. `??` evaluates its fallback only if
the left value is null.

Inside the final `if`, the compiler knows the local `selected` is non-null.
That permits an ordinary function call. Safe function calls such as
`selected?.Greeting()` are not supported; narrow the receiver first.

Continue with [classes](/docs/reference/language/classes),
[packages and imports](/docs/reference/language/modules-and-imports), and
[nullability](/docs/reference/language/nullable).
