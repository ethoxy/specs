---
title: 34-SIGN
subtitle: Message Signing and Verification in JSON RPC
author: Wei Tang <hi@that.world>
status: Raw
type: Standard Track
category: Protocol
created: 2017-09-05
abstract: >
  This ECIP defines two new RPC calls `sign` and `recover` in a different namespace.
---

### Motivation

The current `eth_sign` definition is different across different client implementation, which creates [many](https://github.com/ethereumproject/go-ethereum/issues/323) [problems](https://github.com/paritytech/parity/issues/5490). In addition, there is currently no way to verify a signed message using JSON RPC. This ECIP defines two new RPC calls `sign` and `recover` in a different namespace.

### Namespace

The author of this ECIP recommends to define the two new RPC in a different namespace rather than `eth`. Reusing `eth` may cause confusion with the existing `eth_sign` RPC call and might further pollute the already huge namespace. We can use `etc` namespace, or `key` namespace.

### Specification

#### `[namespace]_sign`

This sign method calculates an Ethereum specific signature of the following Recursive Length Prefix (RLP) structure (call it Message RLP):

* A 16-byte bytearray of magic code: `0x457468657265756d207369676e6d7367` (ASCII string of "Ethereum signmsg").
* The binary bytearray of the message (`message` in parameters).

Similar to [eth_sign](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_sign), by adding a prefix, it prevents misuse where a malicious DApp can sign arbitrary data (e.g. transaction) and use the signature to impersonate the victim.

##### Parameters

1. `account`: 20 bytes address.
2. `message`: N bytes message.
3. `detached`: (Optional) If true, detach the message from the signing result. By default, this is false.

##### Returns

The signing process takes the Keccak256 hash of the above Message RLP structure as the actual message to be signed by secp256k1. It uses the specified account private key to sign a private message. The returned data is also a RLP structure (encoded as hex string):

* A 65-byte bytearray of the signature. The first byte is the recovery id, followed by 64 bytes of the signature data.
* The Keccak256 message hash used in the secp256k1 signing process.
* (Only included if `detached` is false) Message RLP defined above.

#### `[namespace]_recover`

Recover a signature signed by `[namespace]_sign`.

##### Parameters

`signature`: returned data by `[namespace]_sign`.

##### Returns

If the signature is invalid, return `false`. Otherwise, returns the following JSON struct.

1. `address`: Address used to sign this message.
2. `message`: If the signature is not detached, returns the actual message (excluding the prefix "Ethereum signmsg") of the signature.
