# Trusted Computing
**Trusted computing** is an idea that gives hardware manufacturers control over what software does and does not run on a system by refusing to run unsigned software. With trusted computing, the computer will consistently behave in expected ways, and those behaviors will be enforced by computer hardware and software. Enforcing this behavior is achieved by loading the hardware with a unique encryption key that is inaccessible to the rest of the system and the owner.[^wiki]

Trusted Computing is a technology developed and promoted by the Trusted Computing Group.

## Trusted Platform Module
**Trusted Platform Module (TPM)** is an international standard for a secure cryptoprocessor, a dedicated microcontroller designed to secure hardware through integrated cryptographic keys. The term can also refer to a chip conforming to the standard.[^tpm-wiki]

Implementations:
- [Official TPM 2.0 Reference Implementation (by Microsoft)](https://github.com/microsoft/ms-tpm-20-ref)
- [TSS.MSR: The TPM Software Stack from Microsoft Research](https://github.com/microsoft/TSS.MSR)
- [tpm2-tss: OSS implementation of the TCG TPM2 Software Stack (TSS2)](https://github.com/tpm2-software/tpm2-tss)
- [Go-TPM](https://github.com/google/go-tpm)

[^wiki]: [Trusted Computing - Wikipedia](https://en.wikipedia.org/wiki/Trusted_Computing)
[^tpm-wiki]: [Trusted Platform Module - Wikipedia](https://en.wikipedia.org/wiki/Trusted_Platform_Module)