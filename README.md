**NOTE**: _The contents of this repository have been moved to
https://github.com/lightclient/sys-asm._

# `4788asm`

This is an [`etk`][etk] implementation of the EIP-4788
system contract. It is 100 bytes once assembled.

## Getting Started

To setup a dev environment capable of assembling, analyzing, and executing the
repository's assembly you will need to install [`foundry`][foundry] and
[`etk`][etk]. This can be accomplished by running:

```console
$ curl -L https://foundry.paradigm.xyz | bash
$ cargo install --features cli etk-asm etk-dasm
```

## Building

To assemble `src/main.etk` you will need to invoke `eas`:

```console
$ eas src/main.etk
3373fffffffffffffffffffffffffffffffffffffffe14604d57602036146024575f5ffd5b5f35801560495762001fff810690815414603c575f5ffd5b62001fff01545f5260205ff35b5f5ffd5b62001fff42064281555f359062001fff015500
```

It's also possible to remove the `etk` preproccessing by doing a roundtrip --
first assembling the program, then disassembling the program:

```console
$ disease --code 0x$(eas src/main.etk)
   0:   push1 0x61
   2:   dup1
   3:   push1 0x09
   5:   push0
   6:   codecopy
   7:   push0
   8:   return

   9:   caller
   a:   push20 0xfffffffffffffffffffffffffffffffffffffffe
  1f:   eq
  20:   push1 0x4d
  22:   jumpi

  23:   push1 0x20
  25:   calldatasize
  26:   eq
  27:   push1 0x24
  29:   jumpi

  2a:   push0
  2b:   push0
  2c:   revert

  2d:   jumpdest
  2e:   push0
  2f:   calldataload
  30:   dup1
  31:   iszero
  32:   push1 0x49
  34:   jumpi

  35:   push3 0x001fff
  39:   dup2
  3a:   mod
  3b:   swap1
  3c:   dup2
  3d:   sload
  3e:   eq
  3f:   push1 0x3c
  41:   jumpi

  42:   push0
  43:   push0
  44:   revert

  45:   jumpdest
  46:   push3 0x001fff
  4a:   add
  4b:   sload
  4c:   push0
  4d:   mstore
  4e:   push1 0x20
  50:   push0
  51:   return

  52:   jumpdest
  53:   push0
  54:   push0
  55:   revert

  56:   jumpdest
  57:   push3 0x001fff
  5b:   timestamp
  5c:   mod
  5d:   timestamp
  5e:   dup2
  5f:   sstore
  60:   push0
  61:   calldataload
  62:   swap1
  63:   push3 0x001fff
  67:   add
  68:   sstore
  69:   stop
```

### Control-flow Graph

There are several tools to generate a control-flow graph. We'll demostrate
[`evm-cfg`][evm-cfg] here:

```console
evm-cfg $(eas src/main.etk) | dot -Tpng -o cfg.png
```

*note: you will also need [`graphviz`][graphviz] installed to use the `dot` utility.*

###### `cfg.png`
![cfg.png](./docs/cfg.png)

## Testing

The tests can be executed using the `builder-wrapper` script with the same
arguments as [forge][forge]:

```console
$ ./build-wrapper test
[⠆] Compiling...
No files changed, compilation skipped

Running 2 tests for test/Contract.t.sol:ContractTest
[PASS] testRead() (gas: 18514)
[PASS] testUpdate() (gas: 53953)
Test result: ok. 2 passed; 0 failed; 0 skipped; finished in 3.35ms
Ran 1 test suites: 2 tests passed, 0 failed, 0 skipped (2 total tests)
```

A step-by-step debugger can be brought up using `./build-wrapper test --debug {test name}`.

[cfg]: https://en.wikipedia.org/wiki/Control-flow_graph
[etk]: https://github.com/quilt/etk
[evm-cfg]: https://github.com/plotchy/evm-cfg
[forge]: https://github.com/foundry-rs/foundry/blob/master/forge
[foundry]: https://getfoundry.sh/
[graphviz]: https://graphviz.org/download/
