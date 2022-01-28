# Simple Benchmarks for the EVM

## Setup

Ensure you have all the dependencies needed for compiling [geth](https://github.com/ethereum/go-ethereum) locally.

Run `./build_evm.sh` to build geth in the submodule of the repo.

## Benchmarks

### cpu_ram_soak

This test uses the following contract:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Bencher {
    function cpu_ram_soak_test(uint32 loop_limit) public pure {
        uint8[102400] memory buf;
        uint32 len = 102400;
        for (uint32 i=0; i < loop_limit; i++) {
            uint32 j = (i * 7 + len / 2) % len;
            uint32 k = (i * 3) % len;
            uint8 tmp = buf[k];
            buf[k] = buf[j];
            buf[j] = tmp;
        }
    }
}
```

The compiled EVM bytecode is located at `./benches/cpu_ram_soak.hex`.
The input to run with `loop_limit = 100_000` is located at `./benches/cpu_ram_soak.input`.
To run this benchmark simply use the command `./run.sh`.
Sample output:

```
EVM gas used:    192796554
execution time:  541.418155ms
allocations:     39
allocated bytes: 6568188
0x
```
