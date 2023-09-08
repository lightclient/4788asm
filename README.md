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
$  eas src/main.etk
3373fffffffffffffffffffffffffffffffffffffffe1460475760203614156043575f35801560435762016da08106908154141560435762016da001545f5260205ff35b5f5ffd5b62016da042064281555f359062016da0015500
```

It's also possible to remove the `etk` preproccessing by doing a roundtrip --
first assembling the program, then disassembling the program:

```console
$ disease --code 0x$(eas src/main.etk)
   0:   caller
   1:   push20 0xfffffffffffffffffffffffffffffffffffffffe
  16:   eq
  17:   push1 0x47
  19:   jumpi

  1a:   push1 0x20
  1c:   calldatasize
  1d:   eq
  1e:   iszero
  1f:   push1 0x43
  21:   jumpi

  22:   push0
  23:   calldataload
  24:   dup1
  25:   iszero
  26:   push1 0x43
  28:   jumpi

  29:   push3 0x016da0
  2d:   dup2
  2e:   mod
  2f:   swap1
  30:   dup2
  31:   sload
  32:   eq
  33:   iszero
  34:   push1 0x43
  36:   jumpi

  37:   push3 0x016da0
  3b:   add
  3c:   sload
  3d:   push0
  3e:   mstore
  3f:   push1 0x20
  41:   push0
  42:   return

  43:   jumpdest
  44:   push0
  45:   push0
  46:   revert

  47:   jumpdest
  48:   push3 0x016da0
  4c:   timestamp
  4d:   mod
  4e:   timestamp
  4f:   dup2
  50:   sstore
  51:   push0
  52:   calldataload
  53:   swap1
  54:   push3 0x016da0
  58:   add
  59:   sstore
  5a:   stop


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
