# Alternative Scheme of Precompiled Contract Versioning for Hard Fork

    Number: ella-2018-0001
    Title: Alternative Scheme of Precompiled Contract Versioning for Hard Fork
    Author: Wei Tang <hi@that.world>
    Status: Proposed
    Type: Standards Track
    Layer: Meta/Consensus
    Discussion: https://github.com/ellaism/specs/issues/5
    Created: 2018-01-02
    
## Motivation

This defines a scheme for precompiled contract versioning as an alternative for the precompiled contract handling defined in [ella-2017-0001](./2017-0001-account-version.md). This adds certain complexity. However, it has the advantage that it makes effect of precompiled contracts to be visible on the state trie and might to simplify future virtual machine design.

## Specification

Even with [ella-2017-0001](./2017-0001-account-version.md) applied, the effect of precompiled contracts are hidden in the state trie. As an alternative, however, we can make it visiable. We do this by having a separate set of account versions for precompiled contracts.

We define *precompiled contract deployment system transactions*. This type of transaction is in the same category of *block reward system transactions* and it is not recorded in a block's `transactions` set. Precompiled contract deployment system transactions are only valid for a particular block number, and for a particular address. Not having this system transaction is also invalid. Once executed, this type of transaction deploys a precompiled contract with a given account version starting from `10001`. It inherits the current account balance, set the code hash to to the empty code hash, stroage to the empty storage, and set the given account version. To transit to this scheme, we use the following backward-compatible method.

* The account version 0 inherits the old behavior. When contracts with account version 0 calls address from `0x1` to `0x4`, instead of refering to the actual account, it executes a pre-defined code together with a message call transaction.
* With the hard fork that we transit to use the new account versioning scheme, issue four precompiled contract deployment system transactions to address `0x1` to `0x4` with account version number `0x10001` to `0x10004`. This will create account `0x1` to `0x4` if not exist, and the deployment transaction will then only change the account version.
* From this point, no matter which account version the VM is executing, the behavior of precompiled contracts are defined by version `0x10001` to `0x10004` and will remain the same.
