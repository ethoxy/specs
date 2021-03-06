---
title: 23-RLPMEDIA
subtitle: Recursive Length Prefix Media Type
author: Wei Tang <hi@that.world>
status: Raw
type: Standard Track
discussions-to: https://github.com/ethoxy/specs/issues/2
category: Interface
created: 2017-11-22
abstract: >
  Defining a
  media type for Recursive Length Prefix (RLP) to allow a RESTful
  HTTP server to return raw RLP information directly.
---

Abstract
--------

Recursive Length Prefix (see [Yellow
Paper](https://ethereum.github.io/yellowpaper/paper.pdf) Appendix B)
is the data type used for all common Ethereum Classic structures
including blocks, raw transactions, receipts and accounts. Defining a
media type for Recursive Length Prefix (RLP) would allow a RESTful
HTTP server to return raw RLP information directly. This is more
efficient than the current JSON format for RPC calls.

Specification
-------------

Define a new media type according to RFC6838 under
`application/vnd.ecip.rlp` ([IANA media type
assignment](https://www.iana.org/assignments/media-types/application/vnd.ecip.rlp)). This
media type has no required or optional parameters. It uses binary
encoding. The specification for RLP can be found at [Yellow
Paper](https://ethereum.github.io/yellowpaper/paper.pdf) Appendix B.

Example
-------

*Note this section is only served as an example of how this media type might be used, and is not part of the specification.*

A client can implement a RESTful API that returns RLP binary data
directly. We use the routes below, that partially implements the
functionality of the current JSON RPC protocol.

* `GET /{0xhex or number or latest or pending}`: returns RLP binary data of a given block.
* `GET /{0xhex or number or latest or pending}/transactions/{0xhex or number}`: returns RLP binary data of a transaction.
* `GET /{0xhex or number or latest or pending}/receipts/{0xhex or number}`: returns RLP binary data of a transaction receipt.
* `PUT /pending/transactions`: accepts a raw RLP transaction input, and put it into the transaction mempool.
* `GET /{0xhex or number or latest or pending}/accounts/{0xhex}`: returns the RLP binary data of an account.
