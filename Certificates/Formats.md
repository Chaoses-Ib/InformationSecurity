# Certificate Formats
- [DER](#distinguished-encoding-rules-der) (binary)
  - `.cer`, `.der`
- [PEM](#privacy-enhanced-mail-pem) (Base64)
  - `.cer`
- [PKCS #7](#pkcs-7)
  - Multiple certs
  - Cert only (no private key)
  - `.p7b`, `.p7u` (SignTool)
- [PKCS #12](#pkcs-12)
  - Multiple certs
  - `.pfx`, `.p12`
- Microsoft Serialized Certificate Store (SST)
  - Multiple certs
  - `.sst`

Extensions:
- `.key` files are generally the private key
- `.pem` files are generally the public key
- `.p12`/`.pfx` files have both halves of the key embedded, so that administrators can easily manage halves of keys.

  [security - What is the difference between a cer, pvk, and pfx file? - Stack Overflow](https://stackoverflow.com/questions/2292495/what-is-the-difference-between-a-cer-pvk-and-pfx-file?rq=3)

- `.cert` or `.crt` files are the signed certificates -- basically the "magic" that allows certain sites to be marked as trustworthy by a third party.
- `.csr` is a certificate signing request, a challenge used by a trusted third party to verify the ownership of a keypair without having direct access to the private key (this is what allows end users, who have no direct knowledge of your website, confident that the certificate is valid). In the self-signed scenario you will use the certificate signing request with your own private key to verify your private key (thus self-signed). Depending on your specific application, this might not be needed. (needed for web servers or RPC servers, but not much else).
- A JKS keystore is a native file format for Java to store and manage some or all of the components above, and keep a database of related capabilities that are allowed or rejected for each key.

[Certificate filename extensions - Wikipedia](https://en.wikipedia.org/wiki/X.509#Certificate_filename_extensions)

[ssl - Difference between pem, crt, key files - Stack Overflow](https://stackoverflow.com/questions/63195304/difference-between-pem-crt-key-files)

[A SSL Certificate File Extension Explanation: PEM, PKCS7, DER, and PKCS#12 - Comodo SSL Resources](https://comodosslstore.com/resources/a-ssl-certificate-file-extension-explanation-pem-pkcs7-der-and-pkcs12/)

Tools:
- Windows certmgr
  - Import
  - Export: DER (.CER), Base64 (.CER), PKCS #7 (.P7B), PKCS #12 (.PFX), Microsoft SST (.SST)
    - Can only export passless certs and installed certs
- 亚洲诚信数字签名工具
  - 导入: PFX, PVK+SPC
  - 导出: certmgr
- [SSL Converter - Convert SSL Certificates to different formats](https://www.sslshopper.com/ssl-converter.html)
- [VSign 远程数字签名解决方案\_代码签名工具](https://www.vsign.com/)

## ASN.1
[Wikipedia](https://en.wikipedia.org/wiki/ASN.1)

> ASN.1 is a joint standard of the ITU-T in ITU-T Study Group 17 and ISO/IEC, originally defined in 1984 as part of CCITT X.409:1984. In 1988, ASN.1 moved to its own standard, X.208, due to wide applicability. The substantially revised 1995 version is covered by the X.680 series. The latest revision of the X.680 series of recommendations is the 6.0 Edition, published in 2021.

> It is broadly used in telecommunications and computer networking, and especially in cryptography.

Then way is it not widely used as a general serialization format?

[A Warm Welcome to ASN.1 and DER - Let's Encrypt](https://letsencrypt.org/docs/a-warm-welcome-to-asn1-and-der/)

[The specs behind the specs part 1 - Working Group Two](https://web.archive.org/web/20230122104720/https://engineering.wgtwo.com/the-specs-behind-the-specs-part-1) ([Hacker News](https://news.ycombinator.com/item?id=29905090))

Rust:
- [rasn: A Safe `#[no_std]` ASN.1 Codec Framework](https://github.com/librasn/rasn)

Tools:
- [ASN.1 JavaScript decoder](https://lapo.it/asn1js/)

### Distinguished Encoding Rules (DER)
Rust:
- [RustCrypto/formats/der](https://github.com/RustCrypto/formats/tree/master/der)

### Privacy-Enhanced Mail (PEM)
Rust:
- [jcreekmore/pem-rs](https://github.com/jcreekmore/pem-rs)
- [rustls/pemfile: Basic parser for PEM formatted keys and certificates](https://github.com/rustls/pemfile)

Tools:
- [Certificate Decoder - Decode certificates to view their contents](https://www.sslshopper.com/certificate-decoder.html)

## X.509
[Wikipedia](https://en.wikipedia.org/wiki/X.509)

Rust:
- [RustCrypto/formats/x509-cert](https://github.com/RustCrypto/formats/tree/master/x509-cert)
  - [formats/x509-cert/tests/certificate.rs](https://github.com/RustCrypto/formats/blob/master/x509-cert/tests/certificate.rs)
  - [cms: `TagUnexpected` error, but works fine in OpenSSL - Issue #1452](https://github.com/RustCrypto/formats/issues/1452)
  - `BMPString`

    <details>

    - [x509-cert: support for BMPString? (unknown/unsupported ASN.1 DER tag: 0x1e at DER byte xxx) - Issue #1036](https://github.com/RustCrypto/formats/issues/1036)
    - [x509-cert: `BMPString` support for `DirectoryString` - Issue #1792](https://github.com/RustCrypto/formats/issues/1792)
      - [x509-cert: add `DirectoryString::BmpString` by dishmaker - Pull Request #1794](https://github.com/RustCrypto/formats/pull/1794)
    </details>
  - Dependency hell?

    [der: remove generic `<T>` from `Reader::finish` by dishmaker - Pull Request #1833 - RustCrypto/formats](https://github.com/RustCrypto/formats/pull/1833#issuecomment-2946179404)
  - [x509-cert: Creating x509 certificates without allocations - Issue #689](https://github.com/RustCrypto/formats/issues/689)
  - [bhesh/x509-verify](https://github.com/bhesh/x509-verify/)
  - [pe-sign: A cross-platform rust no-std library for verifying and extracting signature information from PE files.](https://github.com/0xlane/pe-sign)
    - [`impl Display for Certificate`](https://github.com/0xlane/pe-sign/blob/b94777828990e21d065ed2482fb31b432a359a2f/src/cert/certificate.rs#L66)

  [Update and use more RustCrypto crates by plotnick - Pull Request #58 - oxidecomputer/lpc55\_support](https://github.com/oxidecomputer/lpc55_support/pull/58)
  > The motivation for this change is that Permission Slip uses the latter crate (since it needs full ser/de for certificates), and now that it supports owned types, we can pass around e.g., deserialized objects of type `Certificate` instead of DER serialized `Vec<u8>`.
  > 
  > The RustCrypto `x509-cert` crate does lack a few convenience methods that `x509-parser` had (e.g., `public_key`, `verify_signature`), but they were easily replaced with trivial functions. The one behavioral difference I found was that `x509-cert` returns an error if the buffer containing a DER message is larger than the message itself, but `x509-parser` does not (it just ignores the excess bytes); this matters to `lpc55_signverify-signed-image` because the certificates in the image table are padded to 4-byte boundaries. The work-around is to peek at the DER header, compute the actual message length, and pass a slice with just the message bytes.

  [feat: use RustCrypto for x509 related stuff and discharge ring by beltram - Pull Request #61 - wireapp/rusty-jwt-tools](https://github.com/wireapp/rusty-jwt-tools/pull/61)
  > Replace `rcgen` & `x509-parser` by `x509-cert` from RustCrypto and as a consequence stop relying on ring. This has the nice property of allowing elliptic curves on WASM

  [chore: remove x509-parser by conradludgate - Pull Request #11247 - neondatabase/neon](https://github.com/neondatabase/neon/pull/11247)
  > Both crates seem well maintained. x509-cert is part of the high quality RustCrypto project that we already make heavy use of, and I think it makes sense to reduce the dependencies where possible.

  [Replace `x509-parser` with `x509-cert` by Xynnn007 - Pull Request #212 - sigstore/sigstore-rs](https://github.com/sigstore/sigstore-rs/pull/212)

  [x509-cert: Make it easier to work with the contents of a `Name` - Issue #1493 - RustCrypto/formats](https://github.com/RustCrypto/formats/issues/1493)
  > As part of migrating `age-plugin-yubikey` to `yubikey 0.8`, I am required to migrate from the `x509-parser` crate to `x509-cert`.

- [x509-parser: X.509 parser written in pure Rust. Fast, zero-copy, safe.](https://github.com/rusticata/x509-parser)
  - DER, PEM
  - Verification (`ring`)

  Used by rustls, salvo
- [x509\_certificate - Rust](https://docs.rs/x509-certificate/latest/x509_certificate/)
  - BER, DER, PEM
  - Serialization, generation
  - Verification
  - All dependencies are not optional, including `ring`
- [rustls/rcgen: Generate X.509 certificates, CSRs](https://github.com/rustls/rcgen)
  - Generation
- [openssl::x509 - Rust](https://docs.rs/openssl/latest/openssl/x509/index.html)
- [zartarn15/simple\_x509](https://github.com/zartarn15/simple_x509)
  - Serialization, generation
  - Verification
- [str4d/x509.rs: Pure-Rust X.509 serialization](https://github.com/str4d/x509.rs) (discontinued)

[x509 parser for rustls? : r/rust](https://www.reddit.com/r/rust/comments/fnemww/x509_parser_for_rustls/)

Python:
- [pyca/cryptography](https://github.com/pyca/cryptography)
  - [cryptography-x509](https://github.com/pyca/cryptography/tree/main/src/rust/cryptography-x509)
  - [cryptography-x509-verification](https://github.com/pyca/cryptography/tree/main/src/rust/cryptography-x509-verification)
    - `pem` crate
  - Rust

  [We build X.509 chains so you don't have to - The Trail of Bits Blog](https://blog.trailofbits.com/2024/01/25/we-build-x-509-chains-so-you-dont-have-to/)

### Subject Public Key Info (SPKI)
Rust:
- [RustCrypto/formats/spki](https://github.com/RustCrypto/formats/tree/master/spki)

### Cert hash
Cert hash, fingerprint, thumbprint (Microsoft)

Hash of the whole certificate in DER format.

Used in OCSP, Windows.

Rust:
- [x509-cert: support for computing certificate fingerprints - Issue #1662 - RustCrypto/formats](https://github.com/RustCrypto/formats/issues/1662)
  - `>0.3.0-pre.0`

.NET:
- [X509Certificate2.Thumbprint Property (System.Security.Cryptography.X509Certificates) | Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/api/system.security.cryptography.x509certificates.x509certificate2.thumbprint?view=net-9.0)

  [c# - How to get the thumbprint of a X509Certificate certificate? - Stack Overflow](https://stackoverflow.com/questions/76157259/how-to-get-the-thumbprint-of-a-x509certificate-certificate)

Tools:
- `openssl x509 -in CERTIFICATE_FILE -fingerprint -noout`

  [Viewing X.509 PEM Certificate Fingerprints with OpenSSL - Jamie Tanna | Software Engineer](https://www.jvt.me/posts/2019/04/03/openssl-fingerprint-x509-pem/)
- Windows

[security - How to generate X509 certificate thumbprint? - Stack Overflow](https://stackoverflow.com/questions/9296182/how-to-generate-x509-certificate-thumbprint)

## PKCS #7
[Wikipedia](https://en.wikipedia.org/wiki/PKCS_7)

PKCS #7, Cryptographic Message Syntax (CMS)

> It’s used in many cryptographic standards, such as S/MIME, PKCS#12 and the RFC 3161 digital timestamping protocol.

Rust:
- [formats/cms at master - RustCrypto/formats](https://github.com/RustCrypto/formats/tree/master/cms)
  - [pe-sign: A cross-platform rust no-std library for verifying and extracting signature information from PE files.](https://github.com/0xlane/pe-sign)
- [rasn\_cms - Rust](https://docs.rs/rasn-cms/latest/rasn_cms/)
- [picky-rs/picky/src/x509/pkcs7](https://github.com/Devolutions/picky-rs/tree/master/picky/src/x509/pkcs7)

[Why pkcs#7 is so hard to work with in rust ? (and many other languages actualy !) : r/rust](https://www.reddit.com/r/rust/comments/v0jegi/why_pkcs7_is_so_hard_to_work_with_in_rust_and/)

## PKCS 12
[Wikipedia](https://en.wikipedia.org/wiki/PKCS_12)

- The filename extension for PKCS #12 files is `.p12` or `.pfx`.

Rust:
- [RustCrypto/formats/pkcs12](https://github.com/RustCrypto/formats/tree/master/pkcs12)
  - [formats/pkcs12/tests/cert\_tests.rs](https://github.com/RustCrypto/formats/blob/master/pkcs12/tests/cert_tests.rs)

  [PKCS#12 / PFX support - Issue #3 - RustCrypto/formats](https://github.com/RustCrypto/formats/issues/3)
- [Pkcs12 in openssl::pkcs12 - Rust](https://docs.rs/openssl/latest/openssl/pkcs12/struct.Pkcs12.html)
- [p12 - Rust](https://docs.rs/p12/latest/p12/index.html)

[Handle PKCS12 files - Issue #17 - rustls/pemfile](https://github.com/rustls/pemfile/issues/17)

[Can't decode PKCS#12 certificates - Issue #78 - rusticata/x509-parser](https://github.com/rusticata/x509-parser/issues/78)

JS:
- [digitalbazaar/forge: A native implementation of TLS in Javascript and tools to write crypto-based and network-heavy webapps](https://github.com/digitalbazaar/forge#pkcs12)

  [How to load a PKCS#12 Digital Certificate with Javascript WebCrypto API - Stack Overflow](https://stackoverflow.com/questions/36018233/how-to-load-a-pkcs12-digital-certificate-with-javascript-webcrypto-api)

.NET:
- [Get-PfxCertificate (Microsoft.PowerShell.Security) - PowerShell | Microsoft Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.security/get-pfxcertificate?view=powershell-7.5)
  ```pwsh
  Get-PfxCertificate -FilePath "c.pfx" -Password (ConvertTo-SecureString pass -AsPlainText -Force)
  ```
