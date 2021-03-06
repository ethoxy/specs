---
title: 28-CARBONVOTE
subtitle: Combined CarbonVote and MinerVote for Consensus Soft and Hard Forks
author: Wei Tang <hi@that.world>
status: Raw
type: Standard Track
category: Core
created: 2017-06-28
abstract: >
  The following ECIP tries to combine a enforced version of Ethereum CarbonVote and the best practices about how Bitcoin deals with consensus hard fork into Ethereum Classic using smart contracts.
---

## Abstract

The following ECIP tries to combine a enforced version of Ethereum CarbonVote and the best practices about how Bitcoin deals with consensus hard fork ([BIP-9](https://github.com/bitcoin/bips/blob/master/bip-0009.mediawiki) and [BIP-135](https://github.com/bitcoin/bips/blob/master/bip-0135.mediawiki)) into Ethereum Classic using smart contracts. Rather than hard-code a block number as we currently do, each miner and ETC coin holders emits support of the new consensus hard-fork. Only when a large enough portion of the network support it, the hard-fork is "locked-in" and will be activated.

This should be used in combination with ECIP-1022 for its best effect.

## Motivation

In addition to the motivation provided in ECIP-1022:

**ETC coin holders are important**. Holders of coins should also have sayings about whether a hard fork should occur. CarbonVote, if it can be enforced, is a good way to do this.

**Minimal cost for miners to emit support**. The cost for miners to emit support should be minimal.

## Specification

### Fork deployment parameters

In addition to the fork deployment parameters defined in ECIP-1022, the following should also exist:

* **carbonVoteYesAddress** the address for carbon vote YES.
* **carbonVoteNoAddress** the address for carbon vote NO.
* **carbonThreshold** specifies the portion * 1000 required, in the entire start..(start + timeout) blocks, which should vote YES.

### Carbon Vote

Besides defining the signaling bits for miners to vote, the implementor of the hard fork should also deploy two new smart contracts (YES and NO) to the ETC network with the code below:

```
contract Vote {
    event LogVote(address indexed addr);

    function() {    
        LogVote(msg.sender);

        if (msg.value > 0) {
            msg.sender.send(msg.value);
        }
    }
}
```

ETC holders can vote YES and NO by sending a zero value transaction to any of those contracts.

### Miner vote

The miner will use the voting method defined in ECIP-1022 to conduct the voting.

### Deployment states

To make the voting pass (i.e. status changing from **STARTED** to **LOCKED_IN**), at each voting window, besides requiring the threshold out of windowsize blocks to have the associated bit set in `signalingBits` in `extraData` block header, it also requires the portion of ETC coins that voted YES in the Carbon Vote smart contracts >= (carbonThreshold / 1000000).

Before implementing a hard fork, the implementor should deploy a smart contract for voting (referred to as *The Contract*) to the ETC network that has the following properties.

## Rationale

Miners continue to use `signalingBits` in the `extraData` field to vote, as this is the way with minimal cost for them (and also does not require any protocol change). The consensus is reached jointly by miners and coin holders for a hard fork to pass.
