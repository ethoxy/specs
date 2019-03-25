---
title: 26-VER
subtitle: Use Version Byte Prefix for Contract Account Versioning
author: Wei Tang <hi@that.world>
status: Raw
type: Standard Track
category: Core
created: 2019-01-17
discussions-to: https://github.com/sorpaas/EIPs/issues/3
abstract: >
  Provide an alternative scheme for account versioning with the least amount of changes required.
---

## Simple Summary

Provide an alternative proposal compared with EIP-1702 / ECIP-1040,
with the following advantages:

* We don't need to modify existing account state format.
* We don't need to modify how precompiled contracts are invoked.
* Compatible with the standard WebAssembly binary format.

## Specification

After `FORK_BLOCK`, before an account or contract creation transaction
code is executed, check that whether:

* The first byte is `\0` (`0x00`).
* The code length is greater or equal to 4.

If so, we define the second to fourth bytes as version bits. Instead
of executing on the default EVM, pass the *whole* code array to a VM
defined by the version bits.

* If version bytes are `\0\0\1`, then invoke "EVM1", where the first 4
  bytes are stripped, and the rest of the code bytes are executed in
  an EVM with "EVM1" config.
* If version bytes are `asm`, then invoke WebAssembly virtual
  machine. This is compatible with the standard WebAssembly binary
  format because it always starts with `\0asm`.
  
If the above does not match, execute it on the default EVM. Note that
if the first byte is `\0`, the client can short circuit and stop
immediately.

Additionally, if [25-DIOP](https://ecips.that.world/25-DIOP) is
deployed, before executing a contract creation transaction, or adding
contract code to the state, do the following check:

* Check whether the first byte is `\0`.
  * If so, check whether the code length is greater or equal to 4. If
    not, throw out of gas for the code deployment.
  * Fetch version bytes as defined above, check whether the version
    bytes are defined and active. If not, throw out of gas for the code
    deployment.
