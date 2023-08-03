# `4788asm`

This is an [`ekt`][etk] implementation of the EIP-4788
system contract. It is 100 bytes once assembled.

## Tests

To run the tests you'll need [`foundry`][foundry] and `etk`. You can install by running:

```console
curl -L https://foundry.paradigm.xyz | bash
cargo install --features cli etk-asm
```

Then the tests can be executed using the `builder-wrapper` script with the same
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

[etk]: https://github.com/quilt/etk
[forge]: https://github.com/foundry-rs/foundry/blob/master/forge
[foundry]: https://getfoundry.sh/
