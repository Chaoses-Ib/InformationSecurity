# Code Signing
[Wikipedia](https://en.wikipedia.org/wiki/Code_signing)

Code signing, program signing

> The Code Signing Working Group of the CA/Browser Forum decided that starting June 1, 2023, all code signing certificates (not only the EA ones) should mandate private key storage on a physical media, such as in a hardware crypto module conforming to at least FIPS 140-2 Level 2 or Common Criteria EAL 4+. The CAs subsequently issued announcements on compliance with the decision.

## Certificates
Generation:
- VS
- MakeCert

  [Certificate Creation Tool (Makecert.exe) | Microsoft Learn](https://learn.microsoft.com/en-us/previous-versions/dotnet/netframework-2.0/bfsktky3(v=vs.80)?redirectedfrom=MSDN)

  [Creating Test Certificates - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/install/creating-test-certificates)

  [security - How to create a self signed certificate with the private key inside in a file in one simple step? - Stack Overflow](https://stackoverflow.com/questions/1689450/how-to-create-a-self-signed-certificate-with-the-private-key-inside-in-a-file-in)

  [ssl certificate - Make exportable private key with makecert - Stack Overflow](https://stackoverflow.com/questions/8413385/make-exportable-private-key-with-makecert)

## Windows PE
- Subject Interface Package (SIP)
- Timestamp is optional (and not reliable)
- A PE program can have multiple signatures
  - A signature can contain multiple certificates
  - `SignerInfo`
    - > While the PKCS #7 standard enables multiple signers, Microsoft specifications require **one** and **only one** signer.
    - Can also contain cert
    - [LIEF](https://lief.re/doc/latest/formats/pe/python.html#lief.PE.SignerInfo)
- [PKCS #7](Formats.md#pkcs-7)
  - `.p7b` extracted from signed exe compared to the original one:
    - `digest_algorithms` is no longer empty, but `1.3.14.3.2.26` (`ID_SHA_1`) or others
    - `EncapsulatedContentInfo` changed from `1.2.840.113549.1.7.1` (`ID_DATA`) to `1.3.6.1.4.1.311.2.1.4` (`SPC_INDIRECT_DATA_OBJID`) with 60-bytes `econtent`
    - `SignerInfos` is no longer empty
- Hash algorithms: MD5, SHA-1, SHA-2 (SHA-256, SHA-512)

[Authenticode (I): Understanding Windows Authenticode -- RME-DisCo Research Group](https://reversea.me/index.php/authenticode-i-understanding-windows-authenticode/)

Signing:
- [SignerSignEx3()](https://learn.microsoft.com/en-us/windows/win32/seccrypto/signersignex3)
- [SignTool - Win32 apps | Microsoft Learn](https://learn.microsoft.com/en-us/windows/win32/seccrypto/signtool)
  - Windows only
    - Scoop: [TheRandomLabs/Scoop-Bucket/bucket/windows-sdk-signing-tools.json](https://github.com/TheRandomLabs/Scoop-Bucket/blob/master/bucket/windows-sdk-signing-tools.json)
    - Wine: [Not supported](https://appdb.winehq.org/objectManager.php?sClass=application&iId=13644)
  - 0.5 MiB
  - EKU filter, expiry filter, private key filter, hash filter (`/sha1`)
  - `/f` cannot contain non-ASCII characters (but spaces are ok)
  - Addtional certs: `/ac`

    [How to include entire certification path when signing code with signtool? - Stack Overflow](https://stackoverflow.com/questions/6575424/how-to-include-entire-certification-path-when-signing-code-with-signtool)
  - Rust
    - [16Hexa/pe-signing](https://github.com/16Hexa/pe-signing)
    - [tugger-windows-codesign - crates.io: Rust Package Registry](https://crates.io/crates/tugger-windows-codesign) (Discontinued)
      - [ntamas/tugger-windows-codesign](https://github.com/ntamas/PyOxidizer/tree/main/tugger-windows-codesign)
      - locate ([find-winsdk](https://github.com/FaultyRAM/find-winsdk)), `/f` `/p` / `SystemStore` / `/sha1`, `/t` / CSP, extra args
      - Generate (rcgen), PKCS #12
      - Used by PyOxidizer
    - [SecSamDev/signtool-rs: A library to simplify the usage of Microsoft code signing library (SignTool) for Rust](https://github.com/SecSamDev/signtool-rs)
      - locate, `/fd Enum`, `/t Enum`, `/sha1` / `/f` `/p` / CSP
    - [rust-codesign: Microsoft code signing library (and utility) for Rust](https://github.com/forbjok/rust-codesign)
      - `locate_latest()`, `/fd String`, `/sha1 String`, `/t String`

  [Using SignTool to Sign a File - Win32 apps | Microsoft Learn](https://learn.microsoft.com/en-us/windows/win32/seccrypto/using-signtool-to-sign-a-file)

  Seperate signing:
  - `/dg`: Generates the digest to be signed and the unsigned PKCS7 files. The output digest and PKCS7 files are `<Path>\<FileName>.dig` and `<Path>\<FileName>.p7u`. To output an extra XML file, use `/dxml`.
    - Still needs to specify a cert to generate the PKCS7 file.
      - `.p7b` extracted from the signed exe can be used?
    - Because of no private key filter, `/sha1` must be used if there are multiple certs. (`/a1` doesn't work)
    - `.p7u` is different for different programs and different certs (but the same for the same program and cert combination)
    - `.p7u` is the same as the PKCS #7 data extracted from the `/dg`ed file, but different from the directly signed one (and maybe also the separately signed one).
      - `EncapsulatedContentInfo.econtent` is patially different
      - `SignerInfo.signed_attrs`'s `1.2.840.113549.1.9.4` (`ID_MESSAGE_DIGEST`) is different
      - `SignerInfo.signature` is different
  - `/ds`: Signs the digest only. The input file should be the digest generated by the `/dg` option. The output file is: `<File>.signed`.
  - `/di`: Creates the signature by ingesting the signed digest to the unsigned PKCS7 file. The input signed digest and unsigned PKCS7 files should be `<Path>\<FileName>.dig.signed` and `<Path>\<FileName>.p7u`.
  - `/dlib`: Specifies the DLL that implements the [`AuthenticodeDigestSign`](https://learn.microsoft.com/en-us/windows/win32/seccrypto/pfn-authenticode-digest-sign) function to sign the digest with. This option is equivalent to using SignTool separately with the `/dg`, `/ds`, and `/di` options. This option invokes all three as one atomic operation.

    [novotnyllc/KeyVaultSignToolWrapper: An extension around SignTool to call into Azure Key Vault for the signing](https://github.com/novotnyllc/KeyVaultSignToolWrapper)

    [AzureKeyVaultSignTool/src/lib.rs](https://github.com/vcsjones/AzureKeyVaultSignTool/blob/master/src/lib.rs)

  [digital signature - Signtool.exe /dg /ds /di options and timestamping - Stack Overflow](https://stackoverflow.com/questions/57930959/signtool-exe-dg-ds-di-options-and-timestamping)
  > The timestamping is an operation applied directly to the file itself, so the file must exist locally. Your remote signing service only works because only the digest needs to be signed, not the full binary.

  [Code signing how-to @ Matthew-Jones.com](https://www.matthew-jones.com/articles/codesigning.html)

- PowerShell
  - [Set-AuthenticodeSignature (Microsoft.PowerShell.Security) - PowerShell | Microsoft Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-authenticodesignature?view=powershell-7.5)
  - [Get-AuthenticodeSignature (Microsoft.PowerShell.Security) - PowerShell | Microsoft Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.security/get-authenticodesignature?view=powershell-7.5)
  - Only available on Windows

- [osslsigncode: OpenSSL based Authenticode signing for PE/MSI/Java CAB files](https://github.com/mtrojnar/osslsigncode)
  - `scoop install osslsigncode`
  - Compability pit
    - [Signed executables are not trusted by Windows - Issue #419 - mtrojnar/osslsigncode](https://github.com/mtrojnar/osslsigncode/issues/419)
    - [Sign a driver. - Issue #86 - mtrojnar/osslsigncode](https://github.com/mtrojnar/osslsigncode/issues/86)
  - [mono-signtool: Drop in replacement for Microsoft's signtool not working in Wine](https://github.com/dustinblackman/mono-signtool)
  - [Limelighter: A tool for generating fake code signing certificates or signing real ones](https://github.com/Tylous/Limelighter)

  Separate signing:
  - [feature require: signtool /dg and /di - Issue #85 - mtrojnar/osslsigncode](https://github.com/mtrojnar/osslsigncode/issues/85)
  - [Sign digest only (/ds) - Issue #356 - mtrojnar/osslsigncode](https://github.com/mtrojnar/osslsigncode/issues/356)

- Rust
  - [nix-community/goblin-signing: Sign PE with Goblin! \[maintainers=@raitobezarius @baloo\]](https://github.com/nix-community/goblin-signing)
    - Not on crates.io
    - UEFI only or not?

- [rhboot/pesign: Linux tools for signed PE-COFF binaries](https://github.com/rhboot/pesign)
- [ebourg/jsign: Java implementation of Microsoft Authenticode for signing Windows executables, installers & scripts](https://github.com/ebourg/jsign)
  - [How to sign a digest generated by signtool /dg - ebourg/jsign - Discussion #199](https://github.com/ebourg/jsign/discussions/199)

- Azure
  - Azure Key Vault
    - [vcsjones/AzureSignTool: SignTool Library and Azure Key Vault Support](https://github.com/vcsjones/AzureSignTool)

      [Azure SignTool](https://vcsjones.dev/azure-signtool/)
    
    [sebastiean/volt: An open source Azure Key Vault API compatible server (emulator).](https://github.com/sebastiean/volt)
  - [Trusted Signing](https://learn.microsoft.com/en-us/azure/trusted-signing/quickstart?tabs=registerrp-portal,account-portal,certificateprofile-portal,deleteresources-portal)
    - [Levminer/trusted-signing-cli](https://github.com/levminer/trusted-signing-cli)

Verifying:
- Windows
  - [codesign-verify-rs: A small Rust crate to verify code signatures on macOS and Windows](https://github.com/vkrasnov/codesign-verify-rs)
    - [I-Alzamil/verifysign: A rust crate used to verify digital code signature on files.](https://github.com/I-Alzamil/verifysign)
- [Sigcheck - Sysinternals | Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/sigcheck)
  - Windows only
- Rust
  - [pe-sign: A cross-platform rust no-std library for verifying and extracting signature information from PE files.](https://github.com/0xlane/pe-sign)
    - `--embed` -> `pesign.signed_data.signer_info.get_nested_signature()` (`unsigned_attrs` where `oid == "1.3.6.1.4.1.311.2.4.1"`)
    - [SignedData::verify](https://github.com/0xlane/pe-sign/blob/b94777828990e21d065ed2482fb31b432a359a2f/src/signed_data.rs#L532)
      - [SignerInfo::verify](https://github.com/0xlane/pe-sign/blob/b94777828990e21d065ed2482fb31b432a359a2f/src/signed_data.rs#L144)
    - [Make reqwest optional - Issue #4](https://github.com/0xlane/pe-sign/issues/4)
    - Wasm: ~1 MiB
  - [google/authenticode-rs: Rust tools for working with Authenticode](https://github.com/google/authenticode-rs)
    - Digest only
  - [rustysec/codesigned-rs: Small package to verify code signed binaries on Windows](https://github.com/rustysec/codesigned-rs)
  - [pelite::security - Rust](https://docs.rs/pelite/latest/pelite/security/index.html)
- LIEF: [13 - PE Authenticode --- LIEF Documentation](https://lief.re/doc/latest/tutorials/13_pe_authenticode.html)

  > These checks include:
  > 1.  Check the integrity of the signature ([`lief.PE.Signature.check()`](https://lief.re/doc/latest/formats/pe/python.html#lief.PE.Signature.check "lief.PE.Signature.check")):
  >     1.  There is ONE and only ONE [`SignerInfo`](https://lief.re/doc/latest/formats/pe/python.html#lief.PE.SignerInfo "lief.PE.SignerInfo")
  >     2.  Digest algorithms are consistent ([`Signature.digest_algorithm`](https://lief.re/doc/latest/formats/pe/python.html#lief.PE.Signature.digest_algorithm "lief.PE.Signature.digest_algorithm") `==` [`ContentInfo.digest_algorithm`](https://lief.re/doc/latest/formats/pe/python.html#lief.PE.ContentInfo.digest_algorithm "lief.PE.ContentInfo.digest_algorithm") `==` [`SignerInfo.digest_algorithm`](https://lief.re/doc/latest/formats/pe/python.html#lief.PE.SignerInfo.digest_algorithm "lief.PE.SignerInfo.digest_algorithm"))
  >     3.  If the [`SignerInfo`](https://lief.re/doc/latest/formats/pe/python.html#lief.PE.SignerInfo "lief.PE.SignerInfo") has authenticated attributes, check their integrity. Otherwise, check the integrity of the [`ContentInfo`](https://lief.re/doc/latest/formats/pe/python.html#lief.PE.ContentInfo "lief.PE.ContentInfo") against the Signer's certificate.
  >     4.  If there are authenticated attributes, check that there is a [`lief.PE.PKCS9MessageDigest`](https://lief.re/doc/latest/formats/pe/python.html#lief.PE.PKCS9MessageDigest "lief.PE.PKCS9MessageDigest") attribute for which the [`digest`](https://lief.re/doc/latest/formats/pe/python.html#lief.PE.PKCS9MessageDigest.digest "lief.PE.PKCS9MessageDigest.digest") matches the hash of the [`ContentInfo`](https://lief.re/doc/latest/formats/pe/python.html#lief.PE.ContentInfo "lief.PE.ContentInfo")
  >     5.  If there is a counter signature in the **un-authenticated attributes**, verify its integrity and check that it wraps a valid *timestamping*.
  >     6.  Check the expiration of the certificates according to the potential *timestamping*
  > 2.  If the signature is valid, check that [`lief.PE.ContentInfo.digest`](https://lief.re/doc/latest/formats/pe/python.html#lief.PE.ContentInfo.digest "lief.PE.ContentInfo.digest") matches the computed [`authentihash()`](https://lief.re/doc/latest/formats/pe/python.html#lief.PE.Binary.authentihash "lief.PE.Binary.authentihash")

- [leeqwind/PESignAnalyzer: This program can retrieve signature information from PE files which signed by one or more certificates on Windows. Supporting multi-signed (nested) infomation and certificate-chain.](https://github.com/leeqwind/PESignAnalyzer)
- [secana/CertDump: Dump certificates from PE files in different formats](https://github.com/secana/CertDump)

Others:
- [med0x2e/SigFlip: SigFlip is a tool for patching authenticode signed PE files (exe, dll, sys ..etc) without invalidating or breaking the existing signature.](https://github.com/med0x2e/SigFlip)

### Timestamping
[Package signatures are not timestamped - Issue #2304 - dotnet/arcade](https://github.com/dotnet/arcade/issues/2304)
> Timestamping only helps to validate that a file was signed prior to the certificate's revocation time. Without a timestamp, the signature can only be trusted until the revocation time. After that, regardless of when the file was actually signed, you cannot trust the certificate/signature.
> 
> The problem is that the timestamp itself needs to be trusted - the time stamp usually have a counter signature and needs to come from a trusted timestamping service. I'd be surprised if PRSS sets those up for test certificates.

- SignTool
  - Error if the cert is expired: `SignTool Error: No certificates were found that met all the given criteria.`
    - `/debug`
    - [namazso/MagicSigner: Signtool for expired certificates](https://github.com/namazso/MagicSigner)
      - [SignTool Error: The signer's certificate is not valid for signing. - Issue #5](https://github.com/namazso/MagicSigner/issues/5)
    - [hackerhouse-opensource/SignToolEx: Patching "signtool.exe" to accept expired certificates for code-signing.](https://github.com/hackerhouse-opensource/SignToolEx)
    - [mathisvickie/sign-expired: Signtool modification for expired certificates](https://github.com/mathisvickie/sign-expired)

    [digital signature - Is there a way to sign a binary file using an expired certificate? - Stack Overflow](https://stackoverflow.com/questions/54201341/is-there-a-way-to-sign-a-binary-file-using-an-expired-certificate)
- [emlinhax/DriverTimestampFaker: Fake Timestamps of Driver Certificates while keeping validity.](https://github.com/emlinhax/DriverTimestampFaker)
- [PIKACHUIM/FakeSign: 自建时间戳服务器实现伪签名驱动证书 Implementing Pseudo Signature with Self-Sign Timestamp Servers](https://github.com/PIKACHUIM/FakeSign)

[wanttobeno/FuckCertVerifyTime: 一些使用过期或者注销证书的技术](https://github.com/wanttobeno/FuckCertVerifyTime)

## ELF
- Linux: No official support
- Solaris

Tools:
- [signelf](https://github.com/hermannch/signelf)

  [signelf -- Digitally signing ELF binaries - CodeNoise](https://blog.codenoise.com/signelf-digitally-signing-elf-binaries)
- [NUAA-WatchDog/linux-elf-binary-signer: ✒️ Adding digital signature into ELF binary files.](https://github.com/NUAA-WatchDog/linux-elf-binary-signer)

### Linux
[security - Signed executables under Linux - Stack Overflow](https://stackoverflow.com/questions/1732927/signed-executables-under-linux)
> I wrote signed executable support for the Linux kernel (around version 2.4.3) a while back, and had the entire toolchain in place for signing executables, checking the signatures at `execve(2)` time, caching the signature validation information (clearing the validation when the file was opened for writing or otherwise modified), embedding the signatures into arbitrary ELF programs, etc. It did introduce some performance penalties upon the first execution of every program (because the kernel had to load in the *entire* file, rather than just demand-page the needed pages) but once the system was in a steady-state, it worked well.
> 
> But we decided to stop pursuing it because it faced several problems that were too large to justify the complexity:
> 
> - We had not yet built support for *signed libraries*. Signed libraries would require also modifying the `ld.so` loader and the `dlopen(3)` mechanism. This wasn't impossible but did complicate the interface: should we have the loader ask the kernel to validate a signature or should the computation be done entirely in userspace? How would one protect against a `strace(2)`d process if this portion of the validation is done in userspace? Would we be forced to forbid `strace(2)` entirely on such a system?
> 
>   What would we do about [programs that supply their own loader](http://www.catonmat.net/blog/ldd-arbitrary-code-execution/)?
> 
> - A great many programs are written in languages that do not compile to ELF objects. We would need to provide *language-specific* modifications to `bash`, `perl`, `python`, `java`, `awk`, `sed`, and so on, for each of the interpreters to be able to *also* validate signatures. Since most of these programs are free-format plain text they lack the structure that made embedding digital signatures into ELF object files so easy. Where would the signatures be stored? In the scripts? In extended attributes? In an external database of signatures?
> 
> - Many interpreters are *wide open* about what they allow; `bash(1)` can communicate with remote systems *entirely on its own* using `echo` and `/dev/tcp`, and can easily be tricked into executing anything an attacker needs doing. Signed or not, you couldn't trust them once they were under control of a hacker.
> 
> - The prime motivator for signed executables support comes from rootkits replacing the system-provided `/bin/ps`, `/bin/ps`, `/bin/kill`, and so on. Yes, there are other useful reasons to have signed executables. However, rootkits got significantly more impressive over time, with many relying on *kernel* hacks to hide their activities from administrators. Once the kernel has been hacked, the whole game is over. As a result of the sophistication of rootkits the tools we were hoping to prevent from being used were falling out of favor in the hacking community.
> 
> - The kernel's module loading interface was wide-open. Once a process has `root` privilege, it was easy to inject a kernel module without any checking. We could have also written another verifier for kernel modules but the kernel's infrastructure around modules was very primitive.

## Apple
- [apple-codesign - crates.io: Rust Package Registry](https://crates.io/crates/apple-codesign)
  
  [Gregory Szorc's Digital Home | Pure Rust Implementation of Apple Code Signing](https://gregoryszorc.com/blog/2021/04/14/pure-rust-implementation-of-apple-code-signing/)

## Java
