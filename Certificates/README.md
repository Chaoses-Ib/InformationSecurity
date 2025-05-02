# Digital Certificates
[Wikipedia](https://en.wikipedia.org/wiki/Public_key_certificate)

> 😩数字签名真是天坑，序列化用的是40年前的老格式，从封装格式到扩展名全都不统一，密码学库还都写的难用，PE 签名再叠上微软加的私货就更乱了

## Wildcard certificates
A public key certificate which uses an asterisk `*` (the wildcard) in its domain name fragment is called a Wildcard certificate. Through the use of `*`, a single certificate may be used for multiple sub-domains. It is commonly used for transport layer security in computer networking.

- Only a single level of subdomain matching is supported in accordance with RFC 2818.
- Cannot be used for the root domain

  Subject Alternative Name: `*.example.com,example.com`

  [Should a wildcard SSL certificate secure both the root domain as well as the sub-domains? - Server Fault](https://serverfault.com/questions/310530/should-a-wildcard-ssl-certificate-secure-both-the-root-domain-as-well-as-the-sub)

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
  - [lyc8503/AliyunCertRenew: 阿里云个人免费证书自动续期, 免去手动申请繁琐流程, 可用 GitHub Actions 快速部署~](https://github.com/lyc8503/AliyunCertRenew)
- 腾讯云
- [FreeSSL.cn - 一个提供免费HTTPS证书申请的网站](https://freessl.cn/)

Discussions:
- 2023-12 [阿里云免费 SSL 证书有哪些替代方案？ - V2EX](https://v2ex.com/t/999627)

  > 国外免费证书中，在国内使用效果最佳的是 google 免费证书，这有点毁三观，但事实如此。globalsign 交叉根，中国本地的 ocsp 服务，中国本地的 crl 下载。除了发证书的 acme 服务需要魔法，其他都很好。

- 2024-09 [为什么 SSL 证书有很多免费的，代码签名证书没有？ - V2EX](https://www.v2ex.com/t/1075481#reply6)