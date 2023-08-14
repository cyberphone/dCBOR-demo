<a id="cborjs">![Saturn is great](https://cyberphone.github.io/CBOR.js/doc/cbor.js.svg)

Note: This is the **dCBOR** version.

This repository contains a
[CBOR JavaScript API](https://cyberphone.github.io/dCBOR-demo/doc/).  The API loosely mimics the "JSON" object by _exposing a single global object_,
unsurprisingly named "CBOR".  To minimize the need for application developers having detailed knowledge of CBOR,
  the API provides a set of high level wrapper objects.

  The wrapper objects are used for encoding CBOR data items,
  as well as being the result of CBOR decoding.

### "CBOR" Components
- Self-encoding wrapper objects
- Decoder
- Diagnostic Notation decoder
- Utility functions

### Encoding Example

```javascript
let cbor = CBOR.Map()
               .set(CBOR.Int(1), CBOR.Float(45.7))
               .set(CBOR.Int(2), CBOR.String("Hi there!")).encode();

console.log(CBOR.toHex(cbor));
------------------------------
a201fb4046d9999999999a0269486920746865726521
```
Note: there are no requirments "chaining" objects as shown above; items
may be added to `CBOR.Map` and `CBOR.Array` objects in separate steps.

### Decoding Example

```javascript
let map = CBOR.decode(cbor);
console.log(map.toString());  // Diagnostic notation
----------------------------------------------------
{
  1: 45.7,
  2: "Hi there!"
}

console.log('Value=' + map.get(CBOR.Int(1)));
---------------------------------------------
Value=45.7
```

### On-line Testing

On https://cyberphone.github.io/dCBOR-demo/doc/playground.html you will find a simple Web application,
permitting testing the encoder, decoder, and diagnostic notation implementation.

### Deterministic Encoding Rules

The JavaScript API implements deterministic encoding based on section 4.2 of [RFC8949](https://www.rfc-editor.org/rfc/rfc8949.html).
For maximum interoperability, the API also depends on Rule&nbsp;1 of section 4.2.2.

To shield developers from having to know the inner workings of deterministic encoding, the CBOR.js API performs
all the necessary transformations _automatically_.  This for example means that if the `set` operations
in the [Encoding&nbsp;Example](#encoding-example) were swapped, the generated CBOR would still be the same.

### Diagnostic Notation Support

Diagnostic notation as described in section 8 of [RFC8949](https://www.rfc-editor.org/rfc/rfc8949.html)
permits displaying CBOR data as human-readable text.  This is practical for _logging_,
_documentation_, and _debugging_ purposes.  Diagnostic notation is an intrinsic part of the CBOR.js API through the `toString()` method.

However, the  CBOR.js API extends the scope of diagnostic notation by supporting using it as
_input_ for creating CBOR based _test data_ and
_configuration files_ from text.  Example:
```javascript
let cbor = CBOR.diagDecode(`{
# Comments are also permitted
  1: 45.7,
  2: "Hi there!"
}`).encode();

console.log(CBOR.toHex(cbor));
------------------------------
a201fb4046d9999999999a0269486920746865726521
```

### Implementation Note

The code represents a _Reference Implementation_, not code for inclusion in JavaScript engines.  The latter would (for _performance_ reasons), most certainly require parts to be rewritten in native code.

### Other Compatible Implementations

|Language|URL|
|-|-|
|Rust|https://github.com/BlockchainCommons/bc-dcbor-rust|
|Swift|https://github.com/BlockchainCommons/BCSwiftDCBOR|
|TypeScript|https://github.com/BlockchainCommons/bc-dcbor-ts|

### Internet Draft
https://datatracker.ietf.org/doc/draft-mcnally-deterministic-cbor/

Updated: 2023-08-14
