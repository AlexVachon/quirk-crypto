# crypto

Cryptographic hashes, HMAC, password hashing, constant-time comparison,
secure random and UUIDs for [Quirk](https://github.com/AlexVachon/quirk).
Digests are **lowercase hex Strings**. Random bytes come from OpenSSL's
CSPRNG (seeded by the OS) — an error, never a weak fallback, if there are none.

```quirk
use crypto

crypto.sha256("hello")                 // 2cf24dba5fb0a30e…
crypto.hmac_sha256("secret", "payload")
crypto.random_hex(32)                  // 64 hex chars: a token, a session id
crypto.uuid()
```

## Passwords

```quirk
// signing up: store this, never the password
stored := crypto.hash_password(password)

// logging in
if crypto.verify_password(password, stored) {
    if crypto.needs_rehash(stored) { stored = crypto.hash_password(password) }   // and save it
    ...
}
```

- `hash_password` is **scrypt** with a 16-byte random salt, N = 2^15, r = 8,
  p = 3 (32 MiB — one of OWASP's settings), stored as one String:
  `$scrypt$ln=15,r=8,p=3$<salt>$<hash>`. About a tenth of a second, on purpose:
  whoever steals the table pays that for every guess. The same password gives a
  different String each time.
- `verify_password` compares in constant time; it's false for anything that
  isn't such a hash (and the settings in a stored hash are checked before they're
  used, so a tampered row can't make it use gigabytes).
- `needs_rehash` is true for a hash made with weaker settings than today's: hash
  the password again when its owner next logs in.
- Hashing doesn't block other `async` tasks.
- Never use `sha256(password)` for passwords: it's made to be fast.

## Comparing secrets

`crypto.equal(a, b)` is `a == b` in constant time. Use it for signatures,
tokens and API keys: with `==`, how long the comparison takes tells an attacker
how much of their guess was right.

## Requirements

Quirk 7.55.0 or later for the passwords and `equal` (the runtime has them).

This package is also bundled with the Quirk compiler at
`<QUIRK_HOME>/packages/crypto/`, so `use crypto` works out of the box on a
fresh install. This repo is the canonical home — when you want a version
newer than the one your compiler shipped with, install it explicitly:

```bash
quirk pkg install crypto             # latest tag from this repo
quirk pkg install crypto@1.1.0       # pinned
```

The compiler-shipped registry maps the bare name `crypto` to this repo
(since v1.4.0), so no `quirk pkg registry add` is needed.

## Origin

Split from `quirk-compiler/packages/crypto/` in compiler v1.5.0 as part of
the stdlib-in-independent-repos rollout. Source history before that lives
in [AlexVachon/quirk](https://github.com/AlexVachon/quirk).

## License

MIT (see [`LICENSE`](LICENSE)).
