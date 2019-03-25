---
title: 21-LEASM
subtitle: Low-level Ethereum Virtual Machine Assembly
author: Wei Tang <hi@that.world>
status: Raw
type: Standard Track
category: Interface
abstract: >
  This RFC aims to provide an assembly language that closely assembles EVM
  opcodes.
---

This RFC aims to provide an assembly language that closely assembles EVM
opcodes.

Normal Opcodes
--------------

Instructions should be in lower case letters [\[a-z\]]{.title-ref} and
ends with space or a new line. Opcodes that are normal are everything
except [PUSH]{.title-ref}.

This includes *Stop and Arithmetic Operations*:

-   stop
-   add
-   mul
-   sub
-   div
-   sdiv
-   mod
-   smod
-   addmod
-   mulmod
-   exp
-   sigextend

*Comparison & Bitwise Logic Operations*:

-   lt
-   gt
-   slt
-   sgt
-   eq
-   iszero
-   and
-   or
-   xor
-   not
-   byte

*SHA3*:

-   sha3

*Environmental Information*:

-   address
-   balance
-   origin
-   caller
-   callvalue
-   calldataload
-   calldatasize
-   calldatacopy
-   codesize
-   codecopy
-   gasprice
-   extcodesize
-   extcodecopy

*Block Information*:

-   blockhash
-   coinbase
-   timestamp
-   number
-   difficulty
-   gaslimit

*Stack, Memory, Storage and Flow Operations*:

-   pop
-   mload
-   mstore
-   mstore8
-   sload
-   sstore
-   jump
-   jumpi
-   pc
-   msize
-   gas
-   jumpdest

*Duplication, Exchange and Logging Operations*:

-   dup\[1-16\]
-   swap\[1-16\]
-   log\[0-4\]

*System Operations*:

-   create
-   call
-   callcode
-   return
-   delegatecall
-   suicide

Push Opcodes
------------

For *Push Operations* push\[1-32\], the instruction should start with
the instruction name, followed by [(]{.title-ref}, followed by the byte
to be pushed, prefixed by [0x]{.title-ref}, and then followed by
[)]{.title-ref}. There should not be any space or newline character in
between.
