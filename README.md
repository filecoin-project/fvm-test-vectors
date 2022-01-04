# Test vectors for FVM implementations

The Filecoin Virtual Machine is WASM-based polyglot virtual machine that
introduces the ability to deploy user-specified smart contracts to the
Filecoin network. For more information, refer to [filecoin-project/fvm-project](https://github.com/filecoin-project/fvm-project/).

This repository follows the steps of the original [filecoin-project/test-vectors](https://github.com/filecoin-project/test-vectors/),
but containing test vectors that specifically target the FVM. We expect
the corpus in `filecoin-project/test-vectors` to become deprecated in favour of
the corpus herein when the non-programmable VM is replaced with the FVM on mainnet.

The FVM hosts builtin actors (e.g. power actor, miner actors, market actor)
as WASM modules. **Only v6+ actors and network versions 14+ are supported.**

Because of that, the corpus in this repo is bootstrapped with actors v6 vectors.

## Structure and schema

The structure of this repo and the test vector schema are identical to those in
[filecoin-project/test-vectors](https://github.com/filecoin-project/test-vectors/).

## License

Dual-licensed under
[MIT](https://github.com/filecoin-project/fvm-test-vectors/blob/master/LICENSE-MIT) +
[Apache 2.0](https://github.com/filecoin-project/fvm-test-vectors/blob/master/LICENSE-APACHE).