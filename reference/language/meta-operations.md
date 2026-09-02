# Meta operations

Declared members use `.`. Language-defined meta operations use `::` and
depend on the receiver's semantic type.

```cloth
string text = "Cloth";
println(text::length);
println(text::typeName);
```

Meta names are case-sensitive and do not follow member visibility rules.
They cannot be shadowed, overloaded, or assigned. The receiver is evaluated
once and must be non-null.

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
