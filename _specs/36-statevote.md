---
title: 36-STATEVOTE
subtitle: State-based Hard Fork Signaling
author: Wei Tang <hi@that.world>
status: Raw
type: Standard Track
category: Core
created: 2019-06-04
abstract: >
  This ECIP defines a hard fork procedure where rather than hard coding a block number, we fetch the hard fork status from a particular on-chain contract.
---

## Abstract

This ECIP defines a hard fork procedure where rather than hard coding a block number, we fetch the hard fork status from a particular on-chain contract. This allows advanced signaling procedure to be defined. For example, miner hash-power based signaling can be defined by this, as well as carbon vote or more advanced DAO procedure.

## Specification

When defining hard fork meta ECIPs, rather than associate the hard fork with a block number, associate it with a contract address `HARD_FORK_CONTRACT` and a storage location `HARD_FORK_STORAGE`. Before processing a new block, check whether `HARD_FORK_STORAGE` in `HARD_FORK_CONTRACT` is not 0 and if current block number is greater than or equal to the storage value. If so, enable the feature set defined by the hard fork meta ECIP.

## Rationale

This state-based hard fork signaling method keeps best compatibility with existing clients, compared with contract-based signaling or extra-data-based-signaling. Some light clients might not have all headers stored in the database, thus using extra data field signaling (like `27-MINERVOTE`) may not be viable for them. By using a storage location rather than executing a contract to determine whether a hard fork happened, we also avoid the complication of determine the feature set enabled for that particular execution.

Once the hard fork happens, if the contract is clearly defined, then the hard fork block number can be easily known. Thus those client that require hard-coded block number can continue to function.

## Special Considerations for Miner Signaling

When using this method to implement miner signaling, we can just ask miners to include a zero price transaction that "vote" on the mined block's hash power. However, there're some particular considerations to keep in mind:

* Usually, miner won't keep the private key hot. Thus we cannot sign the transction with the coinbase private key. Rather, we sign it with a well-known private key or any other dummy private key chosen by the miner.
* The signaling contract checks that current transaction's gas price is 0. When gas price is 0, all current client configurations ensure that the transaction cannot be broadcasted or included externally, thus it must have been submitted locally.
* The signaling contract checks that the execution frame is top-level, by making sure `ORIGIN` is `CALLER`.
* The signaling contract then counts the block's hash power as voted, and it notes the current block number to avoid double vote.

Below is an example contract that counts signaling based on last 50000 blocks, and enable the hard fork when 80% of blocks signals to be ready, with a lock-in period of another 5000 blocks.

```
pragma solidity ^0.5.9;

contract HardFork {
    uint constant votePeriod = 50000;
    uint constant lockinPeriod = 50000;
    uint constant voteThreshold = 40000;
    
    uint public enabledAt = 0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff;
    bool public lockedIn = false;
    uint lastVoted = 0;
    bool[] votes;
    
    function() external {
        assert(tx.origin == msg.sender);
        assert(tx.gasprice == 0);
        assert(lastVoted < block.number);
        assert(!lockedIn);
        
        uint _current = lastVoted;
        while (_current < block.number) {
            if (_current == block.number) {
                votes.push(true);
            } else {
                votes.push(false);
            }
            _current += 1;
        }
        
        uint _votedCount = 0;
        uint _totalCount = 0;
        _current = votes.length - 1;
        
        while (_current > 0 && _totalCount <= votePeriod) {
            if (votes[_current]) {
                _votedCount += 1;
            }
            _totalCount += 1;
        }
        
        if (_votedCount >= voteThreshold) {
            enabledAt = block.number + lockinPeriod;
            lockedIn = true;
        }
        
        lastVoted = block.number;
    }
}
```
