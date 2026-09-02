# Interfaces

An interface defines public function contracts without storage or construction.
It uses the filename as its type name.

```cloth
// Named.co
interface {
  func GetName(): string;
}
```

Interface functions end in semicolons. They must be public instance contracts,
with no modifiers or bodies. Interfaces have no fields, constructors, static
functions, or default implementations.

## Class conformance

A class explicitly lists its interfaces after `is`:

```cloth
// Person.co
class is Named {
  final string name;

  Person(string name) {
    self.name = name;
  }

  override func GetName(): string {
    return name;
  }
}
```

A class may also have one implementation base:
`class : Base is Named, Renderable { ... }`.

Matching functions alone do not establish interface subtyping. The class must
declare or inherit conformance. A locally declared implementation requires
`override`; an inherited implementation needs no redeclaration.

```cloth
Named named = Person("Cloth");
println(named.GetName());
```

The call reaches the object's most-derived implementation. A derived override
also updates behavior through inherited interface references.

## Combining contracts

An interface can extend multiple interfaces using
`interface : Named, Renderable { ... }`.
Cycles and duplicate direct parents are invalid.

A contract is identified by its name and canonical parameter types. Overloads
with different parameter lists remain separate contracts. Identical inherited
signatures are merged. Compatible covariant reference results select the most
specific contract; incompatible results are an error.

A child interface may refine an inherited result with a plain `func`
declaration. Interfaces do not use `override`.

A concrete class must satisfy every transitive requirement. An abstract class
may leave requirements unresolved; if it explicitly restates one, it uses
`abstract override func`.

## Conversions

A class reference widens to every interface it conforms to. An interface
reference widens to its parent interfaces and to `object`. Nullability
composes with these conversions.

Reverse and cross-interface conversions use `as T?`; runtime tests use
`is T`. One class may implement otherwise unrelated interfaces.
The conversions preserve object identity.

`super` only selects class implementations; it cannot call an interface
contract. Structs cannot implement interfaces. Traits, generic contracts, and
default interface methods are not currently supported.
