# crypto

Cryptographic hashes, HMAC, secure random, and UUID generation. All hash digests are returned as **lowercase hex Strings** (so they're ready to compare or store as identifiers). Random bytes use the O

This package is also bundled with the Quirk compiler at
`<QUIRK_HOME>/packages/crypto/`, so `use crypto` works out of the box on a
fresh install. This repo is the canonical home — when you want a version
newer than the one your compiler shipped with, install it explicitly:

```bash
quirk pkg install crypto             # latest tag from this repo
quirk pkg install crypto@1.0.0       # pinned
```

The compiler-shipped registry maps the bare name `crypto` to this repo
(since v1.4.0), so no `quirk pkg registry add` is needed.

## Origin

Split from `quirk-compiler/packages/crypto/` in compiler v1.5.0 as part of
the stdlib-in-independent-repos rollout. Source history before that lives
in [AlexVachon/quirk](https://github.com/AlexVachon/quirk).

## License

MIT (see [`LICENSE`](LICENSE)).
