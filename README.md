# `etk`-Foundry Template

Most smart contract development tooling is geared towards using compiled
languages, such as Solidity. These tools are extremely important when it comes
to testing and debugging contracts, so we have developed a template for using
`etk` with the [Forge][forge] testing framework from [Foundry][forge].

## Getting Started

To start a new `etk` project with this template, go ahead and clone the
repository.

```console
$ git clone https://github.com/quilt/etk-foundry-template
$ cd etk-foundry-template
```

This will load the template, which is a derivative of `forge init` to work
around the fact that Forge doesn't support custom compiler definitions. The
most notable contribution is the script `builder-wrapper` which assembles the
`etk` project, then injects the assembled bytecode into the testing contract
defined at `test/Contract.t.sol.in`. Since `test/Contract.t.sol` is overwritten
each time `build-wrapper` is called, it's important to ensure that all
modifications are made to the `test/Contract.t.sol.in` file.

To run a forge command, call the `builder-wrapper` script with the same arguments
as Forge:

```console
$ ./build-wrapper test

[⠢] Compiling...
No files changed, compilation skipped

Running 1 test for test/Contract.t.sol:ContractTest
[PASS] testExample() (gas: 30472)
Test result: ok. 1 passed; 0 failed; finished in 1.76ms
```

[foundry]: https://github.com/foundry-rs/foundry
[forge]: https://github.com/foundry-rs/foundry/blob/master/forge
