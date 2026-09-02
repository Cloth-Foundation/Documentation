# Binary data

Cloth provides explicit integer encoding and decoding in a `byte[]`.
The operations do not expose host byte order or memory addresses.

```cloth
byte[] bytes = [0, 0, 0, 0];
int32 value = 16909060;
value::writeBigEndian(bytes, 0);
println(bytes[0]); // 1
println(bytes[3]); // 4

int32 restored = bytes::readInt32BigEndian(0);
println(restored); // 16909060
```

## Writing

`value::writeLittleEndian(destination, offset)` and
`value::writeBigEndian(destination, offset)` return void.

The receiver must be an integer. The destination must be a non-null `byte[]`,
and the offset must be `int32`. The operation writes exactly the integer's
bit width divided by eight. Signed values use their fixed-width two's-complement
bit pattern.

## Reading

Read operations use the byte array as receiver and take an `int32` offset.
Each prefix below supports both `LittleEndian` and `BigEndian` suffixes:

| Prefix | Result type |
| --- | --- |
| `readByte` | `byte` |
| `readInt8` | `int8` |
| `readInt16` | `int16` |
| `readInt32` | `int32` |
| `readInt64` | `int64` |
| `readUint8` | `uint8` |
| `readUint16` | `uint16` |
| `readUint32` | `uint32` |
| `readUint64` | `uint64` |

For example, `bytes::readUint64LittleEndian(offset)` returns `uint64`.
Alias-based names such as `readIntLittleEndian` and `readUintBigEndian`
do not exist. Eight-bit little-endian and big-endian operations produce the same
bytes, but both spellings are available.

## Evaluation and bounds

Receivers and arguments evaluate once, from left to right. The offset must be
non-negative and the entire operation must fit within the array.

The runtime checks the full range before reading or writing. An invalid operation
traps with `integer byte range is out of bounds`. An invalid write changes
no bytes.

Floating-point bit access, unsafe views, and general serialization are not
provided by these operations.
