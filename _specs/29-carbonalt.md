---
title: 29-CARBONALT
subtitle: A CarbonVote and MinerVote Contract for Consensus Soft and Hard Forks
author: Wei Tang <hi@that.world>
status: Raw
type: Standard Track
category: Core
created: 2017-06-28
abstract: >
  This contract provides a way to implement CarbonVote and MinerVote based solely on smart contract.
---

## Abstract

This contract provides a way to implement CarbonVote and MinerVote based **solely** on smart contract, unlike ECIP-1022 and ECIP-1023 which needs to use `extraData` block header. It also provides an interface that voting hard fork smart contracts should follow.

## Motivation

See ECIP-1022 and ECIP-1023 for the motivations.

## Specification

### Deployment states

All hard fork voting smart contract should be in any of the five states below:

* **DEFINED** is the first state that each fork starts out as. The genesis block for any chain SHALL by definition be in this state for each deployment.
* **STARTED** for blocks past the starttime.
* **LOCKED_IN** after voting.
* **ACTIVE** for all blocks after the grace period conditions have been met.
* **FAILED** if past the timeout time and LOCKED_IN was not reached.

The smart contract should have an `deploymentState()` function that allows querying the current state of this hard fork.
 
## An Implementation

The above defines the interface for hard fork voting smart contracts. Below we provides a possible implementation (persedu-code) as one way to implement the above interface. Note that here for simplicity, we only have one window for the entire miner vote period, and the threshold for both miners and coin holders are set to 95%.

```
contract Vote {
  address[] carbonYesAddresses;
  address[] carbonNoAddresses;
  uint minerYesCount;
  
  string name;
  uint startBlock;
  uint timeoutNumber;
  uint lockedInNumber;
  enum State { Defined, Started, LockedIn, Active, Failed }
  
  function Vote(string n, uint s, uint t, uint l) {
    startBlock = s;
    timeoutNumber = t;
    lockedInNumber = t;
    name = n;
  }
  
  function carbonYes() {
    if (block.number < startBlock || block.number > startBlock + timeoutNumber) {
      throw;
    }
    carbonYesAddresses.push(msg.sender);
  }
  
  function carbonNo() {
    if (block.number < startBlock || block.number > startBlock + timeoutNumber) {
      throw;
    }
    carbonNoAddresses.push(msg.sender);
  }
  
  function minerYes() {
    if (block.number < startBlock || block.number > startBlock + timeoutNumber) {
      throw;
    }
    if (block.coinbase != msg.sender) {
      throw;
    }
    minerYesCount = minerYesCount + 1;
  }
  
  function passed() {
    if (block.number < startBlock) {
      throw;
    }
    
    // If we get 95% of the minerVote and 95% of the carbonVote, it is considered passed.
    bool passed = true;
    if (minerYesCount * 100 / min(timeoutNumber, block.number - startBlock) < 95) {
      passed = false;
    }
    uint carbonYesCount = 0;
    uint carbonNoCount = 0;
    for (uint i = 0; i < carbonYesAddresses.length; i++) {
      carbonYesCount += carbonYesAddresses[i].balance;
    }
    for (uint i = 0; i < carbonNoAddresses.length; i++) {
      carbonNoCount += carbonNoAddresses[i].balance;
    }
    if (carbonYesCount * 100 / (carbonYesCount + carbonNoCount) < 95) {
      passed = false;
    }
    return passed;
  }
  
  function deploymentState() {
    // .. use the above passed function to implement this. The details are ignored here.
  }
}
```
