# [PASETO: Platform-Agnostic Security Tokens](https://paseto.io/)
[PASETO: Platform-Agnostic Security Tokens](https://github.com/paragonie/paseto)

[PASETO specification](https://github.com/paseto-standard/paseto-spec)
- [Post-Quantum PASETO (v5, v6) by paragonie-security - Pull Request #36](https://github.com/paseto-standard/paseto-spec/pull/36)

How to encrypt the payload?

[PASETO: A Comprehensive Guide to Platform-Agnostic Security Tokens - Rutvik Bhatt - Technologist & Engineering Leader](https://www.rutvikbhatt.com/paseto-a-comprehensive-guide-to-platform-agnostic-security-tokens/)

Discussions:
- 2024-07 [Is Paseto really better than JWT : r/KeyCloak](https://www.reddit.com/r/KeyCloak/comments/1e2h5w7/is_paseto_really_better_than_jwt/)

## Size
- Header
  - [Token Opacity by paragonie-security - Pull Request #38 - paseto-standard/paseto-spec](https://github.com/paseto-standard/paseto-spec/pull/38)

- payload: 2 bytes at minimum
  - PASETOrs: ~~57 bytes at minimum (`{"nbf":"2025-06-19T08:19:21","iat":"2025-06-19T08:19:21"}`)~~
  - [Support for Non-JSON Formatted Payload - Issue #40 - paseto-standard/paseto-spec](https://github.com/paseto-standard/paseto-spec/issues/40)
  - [Payload Compression - Issue #3 - paseto-standard/paseto-spec](https://github.com/paseto-standard/paseto-spec/issues/3)

- `v1.local`: +80 bytes
- `v1.public`: +256 bytes
- `v2.local`: +24 bytes
- `v2.public`: +64 bytes
- `v3.local`: +80 bytes
- `v3.public`: +96 bytes
- `v4.local`: +64 bytes
- `v4.public`: +64 bytes, 3+7+(payload+64)/0.75 ≥ 98 bytes
  - PASETOrs: ~~≥ 172 bytes~~

    `v4.public.eyJuYmYiOiIyMDI1LTA2LTE5VDA4OjE5OjIxIiwiaWF0IjoiMjAyNS0wNi0xOVQwODoxOToyMSJ9MDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMA==`

  While a UUID only needs 36 bytes at maximum and 22 bytes with Base64.

JWT:
- PASETO's header is much (27 bytes) smaller than JWT, but the signature must be at least 64 bytes, while it can be 32 bytes in JWT, in which case the total size of PASETO will be 15 bytes larger (86-44-27).
- JWT uses Unix timestamp for date times, but PASETO uses RFC 3339, which is 10 bytes longer (before Base64).

## Claims
[paseto-spec/docs/02-Implementation-Guide/04-Claims.md at master - paseto-standard/paseto-spec](https://github.com/paseto-standard/paseto-spec/blob/master/docs/02-Implementation-Guide/04-Claims.md)
> While arbitrary fractional seconds SHOULD be supported in parsers, is RECOMMENDED to omit them on creation.

See also [JWT claims](JSON%20Web.md#claims).

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
  - [Draft: Implement `v5`/`v6` by brycx - Pull Request #132](https://github.com/brycx/pasetors/pull/132)
  - Claims
    - Set `exp`, `nbf` and `iat` by default
      - ~~Must set `nbf` and `iat` unless use `Claims::from_string()` directly~~
      - [Add `remove_claim()` to `Claims` by Chaoses-Ib - Pull Request #201](https://github.com/brycx/pasetors/pull/201)
    - [Omitting fractional seconds in DateTime claims on creation - Issue #197](https://github.com/brycx/pasetors/issues/197)
      - [Omit fractional seconds in DateTime claims on creation by Chaoses-Ib - Pull Request #200](https://github.com/brycx/pasetors/pull/200)
    - Registered claims have built-in methods for setting, but not for getting, only for `ClaimsValidationRules`.
    - No docs for registered claims
  - [paseto-cli](https://github.com/bettermarks/paseto-cli)
  - Used by Cargo, [Lapdev](https://github.com/lapce/lapdev), [Oxidrive](https://github.com/oxidrive/oxidrive)
    - Cargo: [Tracking Issue for RFC 3231: Asymmetric Tokens - Issue #10519 - rust-lang/cargo](https://github.com/rust-lang/cargo/issues/10519)

      [Asymmetric tokens by Eh2406 - Pull Request #10771 - rust-lang/cargo](https://github.com/rust-lang/cargo/pull/10771)

- [rusty\_paseto: A type-driven, ergonomic RUST implementation of the PASETO protocol for secure stateless tokens.](https://github.com/rrrodzilla/rusty_paseto)
  - [GenericBuilder in rusty\_paseto::generic - Rust](https://docs.rs/rusty_paseto/latest/rusty_paseto/generic/struct.GenericBuilder.html)
  - [Compile time error when the feature `v3_public` is enabled with any other public feature - Issue #48](https://github.com/rrrodzilla/rusty_paseto/issues/48)
  - [rusty-paserk: PASERK implementation in Rust](https://github.com/conradludgate/rusty-paserk)
  - [itsscb/paseto\_maker: Rust lib to create and verify paseto tokens and add and extract claims](https://github.com/itsscb/paseto_maker) (discontinued)
  - [Govcraft/paseto\_cli: A command-line tool for generating and validating PASETO v4.local tokens](https://github.com/GovCraft/paseto_cli) (discontinued)
  - Much less downloads compared to PASETOrs.
  - `.DS_Store`
  - Used by [Atuin](https://github.com/atuinsh/atuin), [Graft](https://github.com/orbitinghail/graft)

- [instructure/paseto: A paseto implementation in rust.](https://github.com/instructure/paseto) (discontinued)

- No middleware available

JS:
- [auth70/paseto-ts: PASETO v4 (encrypt, decrypt, sign & verify) in TypeScript](https://github.com/auth70/paseto-ts)
- Node.js: [panva/paseto: PASETO (Platform-Agnostic SEcurity TOkens) for Node.js with no dependencies](https://github.com/panva/paseto) (discontinued)
