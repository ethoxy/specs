---
title: 24-ECIPURI
subtitle: URI Marker for Federated Ethereum Classic Improvement Proposals
author: Wei Tang <hi@that.world>
status: Draft
type: Standard Track
discussions-to: https://github.com/ethoxy/specs/issues/4
category: Informational
created: 2019-03-07
abstract: >
  This specification proposes a solution for a decentralized Ethereum
  Classic Improvement Proposal via federation, while preserving the
  current proposal structures and allowing easy aggregator setup.
---

Overview
--------

This specification proposes a solution for a decentralized Ethereum
Classic Improvement Proposal via federation, while preserving the
current proposal structures and allowing easy aggregator setup. New
repositories can be easily added into the federation without
permissions, and be picked up by existing aggregators. Each URI also has
a single source of truth associated with it, via a well known address
query.

URI
---

An URI for an improvement proposal is a normal Uniform Resource
Identifier, where the scheme can be omitted. For example, the URI for
this specification is `ecips.that.world/24-ECIPURI`.

Registry
--------

The domain part of the specification URI acts as the specification\'s
registry. Note that the specification author can easily change registry
by simply registering at a new registry.

Authoritative Specifications
----------------------------

The actual document for the specification can be hosted on a server, on
a git repository, or on IPFS. When presented with a specification URI,
the client can query a well-known location
`.well-known/ecips/specs.json` (for example
<https://ecips.that.world/.well-known/ecips/specs.json>) to get all
specifications presented in the registry, and understand the URI\'s
authoritativeness. A request to `.well-known/ecips/specs.json` is
expected to return a JSON array, where each item is a JSON object
containing the following fields:

-   `uri`: The URI for the specification
-   `documentUrl`: The location of the authoritative source for the
    specification.
-   `discussionUrl`: The location of discussion for the specification.
-   `status`: A status description for the specification.
-   `author`: Contact of the author for the specification.
-   `createdAt`: Date when the specification was created.

Aggregation
-----------

Each registry can also optionally provide information of other registry
it knows about. This is done by the well known location
`.well-known/ecips/known.json`. A request to
`.well-known/ecips/known.json` is expected to return a JSON array, where
each item is a string pointing to other known registries.

An ECIP aggregator can use this well known location to build up the
complete list of known registries, and then query each registry for
specifications.
