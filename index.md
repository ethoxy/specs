---
title: Ethereum Classic Improvement Proposals
subtitle: In That World and Ethoxy
layout: default
toc:
  - raw
  - draft
  - stable
  - deprecated
---

This is the Ethereum Classic Improvement Proposals project in That
World and Ethoxy. Please feel free to [contribute new
specifications](https://github.com/ethoxy/specs/compare). You can join
discussions about ECIPs at
[#ecips:matrix.org](https://matrix.to/#/#ecips:matrix.org).

We collect specifications for APIs, file formats, protocols, processes
and compositions of them. We welcome and encourage Ethereum-compatible
blockchain developers to contribute specifications for wider community
to review.

## Goals

- **Shared innovation**: it wastes review efforts to duplicate
  specifications between EIPs and ECIPs. Sharing innovations across
  teams and blockchains can be more effective.
- **Focus on standardization**: Sepcifications should be less
  political. Arguments won\'t be on whether a certain technology
  **should** happen, but **how** it would happen.

## Process

We currently use an informal IETF-like process, following [RFC
2026](https://www.ietf.org/rfc/rfc2026.txt). Every specification has its
maturity level:

- A new specification is sent in a pull request. The editor checks and
  helps the author to move the specification into an acceptable
  quality. After that, the specification is merged in **Raw** maturity
  level. In this process, the specification gets its RFC number in the
  format of `$number-$feature`.
- When at least one implementation is done for a particular
  specification, it is moved to **Draft** maturity level.
- When significant implementations and successful operational
  experiences have been obtained, the specification is moved to
  **Stable** maturity level.
- Specifications can be moved to **Deprecated**, **Retired** or
  **Deleted** status based on various conditions.

The current editor is `Wei Tang <hi@that.world>`.

Technically this process also support non-standards track, but currently
we assume all submitted specifications are on the standards track.

## Core

{% capture content %}
{% for page in site.specs %}
{% if page.category == "Core" %}
{% include toc-item.html item=page %}
{% endif %}
{% endfor %}
{% endcapture %}

{% include toc-raw.html content=content %}

## Informational

{% capture content %}
{% for page in site.specs %}
{% if page.category == "Informational" %}
{% include toc-item.html item=page %}
{% endif %}
{% endfor %}
{% endcapture %}

{% include toc-raw.html content=content %}

## Interface

{% capture content %}
{% for page in site.specs %}
{% if page.category == "Interface" %}
{% include toc-item.html item=page %}
{% endif %}
{% endfor %}
{% endcapture %}

{% include toc-raw.html content=content %}

## Other ECIP Repositories

The ECIP process is decentralized, meaning this is only one of the ECIP
repositories in the wild. Currently, other known ECIP repositories
include:

- [Ethereum Classic
  Organization](https://github.com/ethereumclassic/ECIPs)
- [ETC Labs Core](https://github.com/etclabscore/ECLIPS)

Below is our best attempt to aggregate all current ECIPs in those
locations.

<div class="small-column" markdown="1">

{% capture content %}
{% for page in site.data.org %}
{% include toc-item.html item=page %}
{% endfor %}
{% endcapture %}

{% include toc-raw.html content=content %}

</div>
