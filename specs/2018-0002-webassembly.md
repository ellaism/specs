# WebAssembly Smart Contracts

    Number: ella-2018-0002
    Title: WebAssembly Smart Contracts
    Author: "ellaismer" <ellaismer@protonmail.ch>
    Status: Proposed
    Type: Standards Track
    Layer: Consensus
    Discussion: https://github.com/ellaism/specs/issues/10
    Created: 2018-04-30

## Motivation

This lies out a basic specification documentation for the WebAssembly scripting implementation in [Parity Kovan](https://kovan-testnet.github.io/website/) network and possibly Ellaism network in the future.

## Encoding and Parsing

The contract is encoded as specified in [WebAssembly Binary Encoding](https://github.com/WebAssembly/design/blob/master/BinaryEncoding.md). To distinguish WASM contracts and EVM contracts, WASM modules always start with "\0asm" (`0x6d736100`) magic number. In a contract creation transaction, data are appended after the WASM module encoding.

## Invocation

WASM are instantiated as normal. The exported function `call` will be called without arguments.

## Host Functions

| Index | Name | Arguments | Return Value |
|------|---------------|----------------------------------------------------------------------------------------------------------------|------------------|
| 0    | storage_write | `(key_ptr: u32, value_ptr: u32)`                                                                               | `void`           |
| 10   | storage_read  | `(key_ptr: u32, value_ptr: u32)`                                                                               | `void`           |
| 20   | ret           | `(ptr: u32, len: u32)`                                                                                         | (Stop execution) |
| 30   | gas           | `(amount: u32)`                                                                                                | `void`           |
| 40   | fetch_input   | `(ptr: u32)`                                                                                                   | `void`           |
| 50   | input_length  | `()`                                                                                                           | `i32`            |
| 60   | ccall         | `(gas: u64, address_ptr: u32, val_ptr: u32, input_ptr: u32, input_len: u32, result_ptr: u32, result_len: u32)` | `i32`            |
| 70   | scall         | `(gas: u64, address_ptr: u32, input_ptr: u32, input_len: u32, result_ptr: u32, result_len: u32)`               | `i32`            |
| 80   | dcall         | `(gas: u64, address_ptr: u32, input_ptr: u32, input_len: u32, result_ptr: u32, result_len: u32)`               | `i32`            |
| 90   | value         | `(ptr: u32)`                                                                                                   | `void`           |
| 100  | create        | `(endowment_ptr: u32, code_ptr: u32, code_len: u32, result_ptr: u32)`                                          | `i32`            |
| 110  | suicide       | `(refund_ptr: u32)`                                                                                            | (Stop execution) |
| 120  | blockhash     | `(index: u64, hash_ptr: u32)`                                                                                  | `void`           |
| 130  | blocknumber   | `()`                                                                                                           | `u64`            |
| 140  | coinbase      | `(dest_ptr: u32)`                                                                                              | `void`           |
| 150  | difficulty    | `(dest_ptr: u32)`                                                                                              | `void`           |
| 160  | gaslimit      | `(dest_ptr: u32)`                                                                                              | `void`           |
| 170  | timestamp     | `()`                                                                                                           | `u64`            |
| 180  | address       | `(dest_ptr: u32)`                                                                                              | `void`           |
| 190  | sender        | `(dest_ptr: u32)`                                                                                              | `void`           |
| 200  | origin        | `(dest_ptr: u32)`                                                                                              | `void`           |
| 210  | elog          | `(topic_ptr: u32, topic_count: u32, data_ptr: u32, data_len: u32)`                                             | `void`           |
| 1000 | panic         | `(ptr: u32, len: u32)`                                                                                         | (Stop execution) |
| 1010 | debug         | `(ptr: u32, len: u32)`                                                                                         | `void`           |

Module `env` can be imported with the above names.

## Gas Costs, Stack Limits and Opcodes

`Load` and `Store` type instructions have fixed cost of `2`. This includes:

```
I32Load, I64Load, I32Load8S, I32Load8U, I32Load16S, I32Load16U, I64Load8S, I64Load8U, I64Load16S, I64Load16U, I64Load32S, I64Load32U, I32Store, I64Store, I32Store8, I32Store16, I64Store8, I64Store16, I64Store32
```

`Div` type instructions have fixed cost of `16`. This includes:

```
I32DivS, I32DivU, I32RemS, I32RemU, I64DivS, I64DivU, I64RemS, I64RemU
```

`Mul` type instructions have fixed cost of `4`. This includes:

```
I32Mul, I64Mul
```

All floating type instructions are disabled. For other instructions, the cost is `1`.

The initial memory region has a cost of `4096` per page, and growing memories have costs of `8192` per page.

For external host functions, gas costs as in EVM applies, in addition:

* Copying values into the memory has costs `1` per `u64`.
* Reading an unsigned 256-bit integer from memory has cost `64`.
* Reading an address from memory has cost `40`.

Gas passed to WASM are also adjusted by 8/3 compared with EVM. Stack size limit for WASM is 64KB.
