# Inheritance

A class may extend one other class. Use an explicit envelope and name the base
after `:`.

```cloth
// Animal.co
Animal() {}

func Describe(): string {
  return "Animal";
}
```

```cloth
// Dog.co
class : Animal {
  Dog(): Animal() {}

  override func Describe(): string {
    return super.Describe() + ": dog";
  }
}
```

The base must be a visible class. Self-inheritance and inheritance cycles are
invalid. Constructors are not inherited.

## Base construction

Every declared derived constructor must explicitly select a constructor of its
direct base. There is no automatic zero-argument base call.

Base-initializer arguments may use parameters, literals, and static functions.
They cannot observe `self`, instance fields, or unqualified instance functions.

Construction allocates one complete object. Base arguments are evaluated from
derived toward root. Field initializers and constructor bodies then execute
from root toward derived, with each class's fields initialized in declaration
order before its body.

A private base constructor is inaccessible to a derived class.

## Overrides and dispatch

Every public class instance function is virtual. A local function matching an
inherited name and canonical parameter signature must use `override`.
A marker without a matching class or interface contract is invalid.

```cloth
Animal animal = Dog();
println(animal.Describe()); // Animal: dog
```

The static receiver type determines the available signature. The runtime object
determines which override runs. Private and static functions are not virtual.

Calls on the current object during field initialization and construction bind
directly to the declaration selected for the class being initialized. This
prevents a derived override from observing incomplete derived state.
Calls on unrelated objects retain normal dispatch.

Member lookup uses the nearest class declaring the requested name. That class's
declaration set hides same-named declarations farther up the base chain.
Private members remain private to their declaring file; they do not become
accessible merely through inheritance.

## Calling a base implementation

`super.Method(arguments)` looks up a public instance function through the
direct-base view and calls it without virtual dispatch for that call.

It requires an instance context and cannot be used in a base initializer.
`super.Field`, `super.StaticMethod()`, and `super(...)` are invalid.
A base function called this way can still make ordinary virtual calls in its body.

## Abstract and sealed classes

```cloth
// Shape.co
abstract class {
  Shape() {}
  abstract func Area(): float;
}
```

An abstract class cannot be constructed directly. It may contain ordinary
functions and public bodyless `abstract func` declarations. A concrete subclass
must implement every inherited abstract signature; an abstract subclass may
defer them.

An abstract restatement of an inherited class or interface requirement uses
`abstract override func`. `super` cannot call a bodyless abstract function.

`sealed class { ... }` prevents subclassing. A class cannot be both abstract
and sealed. `final override func` prevents further overrides of that function
while leaving it callable. `final func` without `override` is invalid.

## Covariant results and references

An override may return a managed-reference type assignable to the inherited
result: for example, `Dog` instead of `Animal`, or `Dog` instead of
`Animal?`. It cannot add nullability or widen the result. Primitive, enum,
struct, and void results must match exactly. Arrays remain invariant.

Derived references widen implicitly to base types, including compatible nullable
forms. Reverse conversion uses [checked casts](/docs/reference/language/casts).
