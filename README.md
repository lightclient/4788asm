# `4788asm`

This is an [`ekt`][etk] implementation of the EIP-4788
system contract. It is 100 bytes once assembled.

## Getting Started

To setup a dev environment capable of assembling, analyzing, and executing the
repository's assembly you will need to install [`foundry`][foundry] and
[`etk`][etk]. This can be accomplished by running:

```console
$ curl -L https://foundry.paradigm.xyz | bash
$ cargo install --features cli etk-asm etk-dasm etk-analyze
```

## Building

To assemble `src/main.etk` you will need to invoke `eas`:

```console
$ eas src/main.etk
3373fffffffffffffffffffffffffffffffffffffffe146048576020361460265760006000fd5b6201800060003506805460003514156042576201800001546000525b60206000f35b426201800042065560003562018000420662018000015500
```

It's also possible to remove the `etk` preproccessing by doing a roundtrip --
first assembling the program, then disassembling the program:

```console
$ disease --code 0x$(eas src/main.etk)
   0:   caller
   1:   push20 0xfffffffffffffffffffffffffffffffffffffffe
  16:   eq
  17:   push1 0x48
  19:   jumpi

  1a:   push1 0x20
  1c:   calldatasize
  1d:   eq
  1e:   push1 0x26
  20:   jumpi

  21:   push1 0x00 # https://www.4byte.directory/signatures/?bytes4_signature=0x00000000
  23:   push1 0x00 # https://www.4byte.directory/signatures/?bytes4_signature=0x00000000
  25:   revert

  26:   jumpdest
  27:   push3 0x018000
  2b:   push1 0x00 # https://www.4byte.directory/signatures/?bytes4_signature=0x00000000
  2d:   calldataload
  2e:   mod
  2f:   dup1
  30:   sload
  31:   push1 0x00 # https://www.4byte.directory/signatures/?bytes4_signature=0x00000000
  33:   calldataload
  34:   eq
  35:   iszero
  36:   push1 0x42
  38:   jumpi

  39:   push3 0x018000
  3d:   add
  3e:   sload
  3f:   push1 0x00 # https://www.4byte.directory/signatures/?bytes4_signature=0x00000000
  41:   mstore

  42:   jumpdest
  43:   push1 0x20
  45:   push1 0x00 # https://www.4byte.directory/signatures/?bytes4_signature=0x00000000
  47:   return

  48:   jumpdest
  49:   timestamp
  4a:   push3 0x018000
  4e:   timestamp
  4f:   mod
  50:   sstore
  51:   push1 0x00 # https://www.4byte.directory/signatures/?bytes4_signature=0x00000000
  53:   calldataload
  54:   push3 0x018000
  58:   timestamp
  59:   mod
  5a:   push3 0x018000
  5e:   add
  5f:   sstore
  60:   stop
```

### Control-flow Graph

`etk` has the ability to generate a [control-flow graph][cfg] to analyze the
possible paths the code may execute under. To generate, you  need `graphviz`.
Installation instructions can be found [here][graphviz].

The diagram can then be generated using the following:

```console
ecfg -c 0x$(eas ./src/main.etk) | dot -Tpng -o out.png
```

Open `out.png` with your choice of image viewer.

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
[forge]: https://github.com/foundry-rs/foundry/blob/master/forge
[foundry]: https://getfoundry.sh/
[graphviz]: https://graphviz.org/download/
