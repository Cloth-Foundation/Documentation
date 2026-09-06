# Meta operations

Declared members use `.`. Language-defined meta operations use `::` and depend
on a receiver's semantic type or an explicitly named primitive target type.

```cloth
string text = "Cloth";
println(text::length);
println(text::typeName);
```

Meta names are case-sensitive and do not follow member visibility rules.
They cannot be shadowed, overloaded, or assigned. A value receiver is evaluated
once and must be non-null.

## Integer conversion modes

Integer target types provide two callable meta operations:

```cloth
int8 wrapped = int8::wrap(300);
int8 limited = int8::sat(300);
```

`wrap` reduces the operand modulo the target bit width. `sat` clamps it to the
target range. Both require an integer operand, evaluate it once, and return the
named target type. See [conversions and casts](/docs/reference/language/casts)
for signedness and range behavior.

## Primitive parsing

Primitive target types provide `parse` as a callable meta operation:

```cloth
int32 count = int32::parse("2_048");
float ratio = float::parse("6.25e-2");
```

It consumes a complete non-null string, returns the exact target type, and may
throw `ParseError`. It is runtime-only. See
[input and primitive parsing](/docs/reference/language/input-and-parsing) for
the supported targets, grammar, and exact failures.

## Queries

Queries are read-only values and do not take parentheses.

| Receiver | Query | Result |
| --- | --- | --- |
| Array | `::length` | `int32` element count |
| String | `::length` | `int32` Unicode scalar count |
| String | `::byteLength` | `int32` UTF-8 byte count |
| String | `::isEmpty` | `bool` |
| Managed reference | `::typeName` | `string` runtime type name |
| Enum or struct | `::typeName` | `string` qualified nominal type name |

Classes report qualified source identities. Strings report `string`;
arrays report the erased name `array`. These names are diagnostic text,
not serialization identifiers, memory addresses, or a reflection API.

Narrow a nullable receiver before querying it. Safe meta access is unsupported.

## Byte-order operations

Integer and byte-array meta operations take arguments:

```cloth
byte[] bytes = [0, 0, 0, 0];
int32 value = 42;
value::writeBigEndian(bytes, 0);
int32 restored = bytes::readInt32BigEndian(0);
```

These are callable operations, unlike the read-only queries above.
[Binary data](/docs/reference/language/binary-data) lists their names, evaluation
order, and range checks.
