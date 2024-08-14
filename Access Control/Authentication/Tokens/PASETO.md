# [PASETO: Platform-Agnostic Security Tokens](https://paseto.io/)
[PASETO: Platform-Agnostic Security Tokens](https://github.com/paragonie/paseto)

[PASETO specification](https://github.com/paseto-standard/paseto-spec)

How to encrypt the payload?

Discussions:
- 2024-07 [Is Paseto really better than JWT : r/KeyCloak](https://www.reddit.com/r/KeyCloak/comments/1e2h5w7/is_paseto_really_better_than_jwt/)

## PASERK: Platform-Agnostic Serialized Keys
[paseto-standard/paserk: Platform Agnostic SERialized Keys](https://github.com/paseto-standard/paserk)

> The use-cases that PASETO doesn't address out of the box are:
> 
> - Key-wrapping
> - Asymmetric encryption
> - Password-based key encryption

## Implementations
PHP: [paragonie/paseto](https://github.com/paragonie/paseto)

Rust:
- [PASETOrs: PASETO tokens in pure Rust](https://github.com/brycx/pasetors)
  - PASERK
- [rusty\_paseto: A type-driven, ergonomic RUST implementation of the PASETO protocol for secure stateless tokens.](https://github.com/rrrodzilla/rusty_paseto)
  - [rusty-paserk: PASERK implementation in Rust](https://github.com/conradludgate/rusty-paserk/tree/main)
  - Much less downloads compared to PASETOrs.