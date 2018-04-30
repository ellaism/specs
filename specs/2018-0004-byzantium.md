# Hardfork Meta: Byzantium

    Number: ella-2018-0004
    Title: Hardfork Meta: Byzantium
    Author: "ellaismer" <ellaismer@protonmail.ch>
    Status: Proposed
    Type: Standards Track
    Layer: Meta
    Discussion: <to be added>
    Created: 2018-04-30

## Abstract

Ellaism was created before Ethereum's Byzantium hard fork was finalized. This proposes to apply some of the Byzantium features onto Ellaism blockchain.

## Specification

* Activation: Block >= 2,000,000 on Mainnet.
* Included specifications:
  * [EIP 100](http://eips.ethereum.org/EIPS/eip-100) (Change difficulty adjustment to target mean block time including uncles)
  * [EIP 140](http://eips.ethereum.org/EIPS/eip-140) (REVERT instruction in the Ethereum Virtual Machine)
  * [EIP 196](http://eips.ethereum.org/EIPS/eip-196) (Precompiled contracts for addition and scalar multiplication on the elliptic curve alt_bn128)
  * [EIP 197](http://eips.ethereum.org/EIPS/eip-197) (Precompiled contracts for optimal ate pairing check on the elliptic curve alt_bn128)
  * [EIP 198](http://eips.ethereum.org/EIPS/eip-198) (Precompiled contract for bigint modular exponentiation)
  * [EIP 211](http://eips.ethereum.org/EIPS/eip-211) (New opcodes: RETURNDATASIZE and RETURNDATACOPY)
  * [EIP 214](http://eips.ethereum.org/EIPS/eip-214) (New opcode STATICCALL)
  * [EIP 658](http://eips.ethereum.org/EIPS/eip-658) (Embedding transaction status code in receipts)
