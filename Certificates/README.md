# Digital Certificates
[Wikipedia](https://en.wikipedia.org/wiki/Public_key_certificate)

## Formats
- `.key` files are generally the private key
- `.pem` files are generally the public key
- `.p12`/`.pfx` files have both halves of the key embedded, so that administrators can easily manage halves of keys.

  [security - What is the difference between a cer, pvk, and pfx file? - Stack Overflow](https://stackoverflow.com/questions/2292495/what-is-the-difference-between-a-cer-pvk-and-pfx-file?rq=3)

- `.cert` or `.crt` files are the signed certificates -- basically the "magic" that allows certain sites to be marked as trustworthy by a third party.
- `.csr` is a certificate signing request, a challenge used by a trusted third party to verify the ownership of a keypair without having direct access to the private key (this is what allows end users, who have no direct knowledge of your website, confident that the certificate is valid). In the self-signed scenario you will use the certificate signing request with your own private key to verify your private key (thus self-signed). Depending on your specific application, this might not be needed. (needed for web servers or RPC servers, but not much else).
- A JKS keystore is a native file format for Java to store and manage some or all of the components above, and keep a database of related capabilities that are allowed or rejected for each key.

[ssl - Difference between pem, crt, key files - Stack Overflow](https://stackoverflow.com/questions/63195304/difference-between-pem-crt-key-files)

## Wildcard certificates
A public key certificate which uses an asterisk `*` (the wildcard) in its domain name fragment is called a Wildcard certificate. Through the use of `*`, a single certificate may be used for multiple sub-domains. It is commonly used for transport layer security in computer networking.

Only a single level of subdomain matching is supported in accordance with RFC 2818.

Wildcards can be added as domains in multi-domain certificates or [Unified Communications Certificates](https://en.wikipedia.org/wiki/Unified_Communications_Certificate "Unified Communications Certificate") (UCC). In addition, wildcards themselves can have `subjectAltName` extensions, including other wildcards. For example, the wildcard certificate `*.wikipedia.org` has `*.m.wikimedia.org` as a Subject Alternative Name. Thus it secures `www.wikipedia.org` as well as the completely different website name `meta.m.wikimedia.org`.

[Issuing/Distributing Wildcard Cert? - Help - Let's Encrypt Community Support](https://community.letsencrypt.org/t/issuing-distributing-wildcard-cert/132244)

[How to sync letsencrypt wildcard certificates from one server to others on local network : r/selfhosted](https://www.reddit.com/r/selfhosted/comments/137psu9/how_to_sync_letsencrypt_wildcard_certificates/)

## Certificate Transparency
[Wikipedia](https://community.letsencrypt.org/t/opt-out-of-certificate-transparency/58046)

> Certificate Transparency (CT) is an Internet security standard for monitoring and auditing the issuance of digital certificates. Certificate Transparency makes public all issued certificates in the form of a distributed ledger, giving website owners and auditors the ability to detect and expose inappropriately issued certificates.

> When a CA issues a new SSL/TLS certificate, it is required to submit the certificate to one or more CT logs. These logs are append-only, meaning that once a certificate is added, it cannot be removed or modified.

Hiding:
- Wildcard certificates
- Self-signed certificates

[Opt-out of Certificate Transparency - Feature Requests - Let's Encrypt Community Support](https://community.letsencrypt.org/t/opt-out-of-certificate-transparency/58046)

Tools:
- [crt.sh | Certificate Search](https://crt.sh/)
- [Entrust Certificate Search - Entrust, Inc.](https://ui.ctsearch.entrust.com/ui/ctsearchui)
  - Better UI
- [Subdomain search engine | Merklemap](https://www.merklemap.com/) ([Hacker News](https://news.ycombinator.com/item?id=41476383))
  
  [How MerkleMap Works - Merklemap Documentation](https://www.merklemap.com/documentation/how-it-works)
- [Certificate Transparency Monitoring | Cloudflare SSL/TLS docs](https://developers.cloudflare.com/ssl/edge-certificates/additional-options/certificate-transparency-monitoring/)

## Services
- 阿里云
- 腾讯云
- [FreeSSL.cn - 一个提供免费HTTPS证书申请的网站](https://freessl.cn/)

Discussions:
- 2023-12 [阿里云免费 SSL 证书有哪些替代方案？ - V2EX](https://v2ex.com/t/999627)

  > 国外免费证书中，在国内使用效果最佳的是 google 免费证书，这有点毁三观，但事实如此。globalsign 交叉根，中国本地的 ocsp 服务，中国本地的 crl 下载。除了发证书的 acme 服务需要魔法，其他都很好。

- 2024-09 [为什么 SSL 证书有很多免费的，代码签名证书没有？ - V2EX](https://www.v2ex.com/t/1075481#reply6)