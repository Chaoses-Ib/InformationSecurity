# Automatic Certificate Management Environment (ACME)
[Wikipedia](https://en.wikipedia.org/wiki/Automatic_Certificate_Management_Environment)

> The Automatic Certificate Management Environment (ACME) protocol is a communications protocol for automating interactions between certificate authorities and their users' servers, allowing the automated deployment of public key infrastructure at very low cost. It was designed by the Internet Security Research Group (ISRG) for their Let's Encrypt service.

## Challenges
[Challenge Types - Let's Encrypt](https://letsencrypt.org/docs/challenge-types/)
- HTTP-01 challenge

- DNS-01 challenge

  [You should probably know about LetsEncrypt DNS challenge validation : r/selfhosted](https://www.reddit.com/r/selfhosted/comments/ga02px/you_should_probably_know_about_letsencrypt_dns/)
  > The downside of the DNS-01 challenge is that you need to have an API key stored on your server in plain-text that has write access to your DNS zone. The API key is essentially a password, and if mismanaged could let an attacker take over your DNS zone (and bad things could happen then). Not necessarily a reason to panic, but something to consider, depending on your setup.

  - [acme-dns: Limited DNS server with RESTful HTTP API to handle ACME DNS challenges easily and securely.](https://github.com/joohoi/acme-dns)
    - 安全、优雅，不需要储存 API token，也不需要为每个 DNS hosting service 单独适配
    - Servers:
      - https://auth.acme-dns.io
    - [caddy-dns/acmedns](https://github.com/caddy-dns/acmedns)
  - [agnos: Obtain (wildcard) certificates from let's encrypt using dns-01 without the need for API access to your DNS provider.](https://github.com/krtab/agnos)

- TLS-SNI-01 (deprecated)

- TLS-ALPN-01
  - Caddy

## Clients
[ACME Client Implementations - Let's Encrypt](https://letsencrypt.org/docs/client-options/)

Sh:
- [acme.sh: A pure Unix shell script implementing ACME client protocol](https://github.com/acmesh-official/acme.sh)
  - Windows: Cygwin

    [Windows使用acme.sh申请let's encrypt泛域名SSL证书方法(附带自动续期) - 哔哩哔哩](https://www.bilibili.com/read/cv1174912/)
- [dehydrated: letsencrypt/acme client implemented as a shell-script – just add water](https://github.com/dehydrated-io/dehydrated)

Nginx:
- Reload

  Nginx needs `reload` to update certificates.

  [ssl - Do I Need to Restart Nginx if I Renew My Security Certificate(s)? - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/247418/do-i-need-to-restart-nginx-if-i-renew-my-security-certificates)

  [django - Restart Nginx or reload certificate cache on cert change - Stack Overflow](https://stackoverflow.com/questions/62431911/restart-nginx-or-reload-certificate-cache-on-cert-change)

- [nginx-proxy/acme-companion: Automated ACME SSL certificate generation for nginx-proxy](https://github.com/nginx-proxy/acme-companion)
- JS: [nginx/njs-acme: Nginx NJS module runtime to work with ACME providers like Let's Encrypt for automated no-reload TLS certificate issue/renewal.](https://github.com/nginx/njs-acme)
- Lua
  - [lua-resty-auto-ssl: On the fly (and free) SSL registration and renewal inside OpenResty/nginx with Let's Encrypt.](https://github.com/auto-ssl/lua-resty-auto-ssl)
  - [lua-resty-acme: Automatic Let's Encrypt certificate serving and Lua implementation of ACMEv2 procotol](https://github.com/fffonion/lua-resty-acme)
- [acme-nginx: python acme client for nginx](https://github.com/kshcherban/acme-nginx)
  - DNS validation: Cloudflare, AWS Route53, DigitalOcean
  - It's written in pure Python depends on pyOpenSSL and pycrypto and the only binary it calls is ps to determine nginx master process id to send `SIGHUP` to it during challenge completion.

Python:
- [certbot: EFF's tool to obtain certs from Let's Encrypt and (optionally) auto-enable HTTPS on your server. It can also act as a client for any other CA that uses the ACME protocol.](https://github.com/certbot/certbot)
  - [Third-party plugins](https://eff-certbot.readthedocs.io/en/latest/using.html#third-party-plugins)

Go:
- [caddy: Fast and extensible multi-platform HTTP/1-2-3 web server with automatic HTTPS](https://github.com/caddyserver/caddy)
  - `scoop install caddy`, 14.2 MiB

  [HTTPS quick-start --- Caddy Documentation](https://caddyserver.com/docs/quick-starts/https)
- [lego: Let's Encrypt/ACME client and library written in Go](https://github.com/go-acme/lego)
  - [DNS validation](https://go-acme.github.io/lego/dns/index.html): [acme-dns](https://go-acme.github.io/lego/dns/acme-dns/), Cloudflare, AWS Route53, Azure, GCP, DigitalOcean, Linode, Hetzner, GoDaddy, 阿里云, 腾讯云, 华为云, ...
  - `scoop install lego`, 23 MiB
- [step-ca: 🛡️ A private certificate authority (X.509 & SSH) & ACME server for secure automated certificate management, so you can use TLS everywhere & SSO for SSH.](https://github.com/smallstep/certificates)
- [certimate: 开源的SSL证书管理工具，可以帮助你自动申请、部署SSL证书，并在证书即将过期时自动续期。An open-source SSL certificate management tool that helps you automatically apply for and deploy SSL certificates, as well as automatically renew them when they are about to expire.](https://github.com/usual2970/certimate)
  - Doesn't support acme-dns
  - Deployment
  - MIT

  <details>

  Without arguments:
  ```go
  >certimate.exe
  panic: runtime error: slice bounds out of range [2:1]

  goroutine 1 [running]:
  main.main()
          /home/runner/work/certimate/certimate/main.go:33 +0x316
  ```
  </details>

.NET:
- [win-acme: A simple ACME client for Windows](https://github.com/win-acme/win-acme)
  - DNS validation: acme-dns, Cloudflare, Azure, GCP, DigitalOcean, Linode, Hetzner, GoDaddy, [阿里云](https://www.win-acme.com/reference/plugins/validation/dns/alibaba), 腾讯云, ...
  - Servers: IIS, Apache, Exchange
  - `scoop install win-acme`, 13.6 MiB, *trimmed* (cannot use plugins)
  - `wacs`
  - Default renewal period: 55 days (9:00 AM)
  - `C:\ProgramData\win-acme\acme-v02.api.letsencrypt.org\Certificates\`
- [certify: Certify The Web - ACME Certificate Manager UI for Windows](https://github.com/webprofusion/certify)
  - Only free for personal use (and up to 5 certs)
  - DNS validation: acme-dns, ...
  - [Deployment](https://docs.certifytheweb.com/docs/deployment/tasks_intro#built-in-task-types): Exchange, RDS, multi-server, CCS, Apache, nginx, export, webhooks, Hashicorp Vault, Azure KeyVault...
- [ACMESharp: An ACME client library and PowerShell client for the .NET platform (Let's Encrypt)](https://github.com/ebekker/ACMESharp) (discontinued)

JS:
- [Certd: 开源SSL证书管理工具；全自动证书申请、更新、续期；通配符证书，泛域名证书申请；证书自动化部署到阿里云、腾讯云、主机、群晖、宝塔；https证书，pfx证书，der证书，TLS证书，nginx证书自动续签自动部署](https://github.com/certd/certd)
  - Deployment
  - AGPL

Web:
- [Cert Warden (formerly LeGo CertHub)](https://www.certwarden.com/)

  [LeGo CertHub - Centralized Let's Encrypt : r/selfhosted](https://www.reddit.com/r/selfhosted/comments/133dpd6/lego_certhub_centralized_lets_encrypt/)

## Deployment
- certimate
- Certd
- [cdn-auto-cert: CDN HTTPS 证书自动更新，支持阿里云、腾讯云、华为云](https://github.com/shuhanghang/cdn-auto-cert)

阿里云:
- `AliyunCDNFullAccess`/`AliyunDCDNFullAccess`
- [aliyun-cli: Alibaba Cloud CLI](https://github.com/aliyun/aliyun-cli)
  - Go
  - `scoop install aliyun`
  - [CDN](https://help.aliyun.com/zh/cdn/developer-reference/api-cdn-2018-05-10-setcdndomainsslcertificate)
    ```pwsh
    aliyun cdn DescribeDomainCertificateInfo --DomainName example.com

    $sslPub = (Get-Content -Path "crt.pem" -Raw)
    $sslPri = (Get-Content -Path "key.pem" -Raw)
    aliyun cdn SetCdnDomainSSLCertificate --DomainName example.com --SSLProtocol on --SSLPub="$sslPub" --SSLPri="$sslPri"
    ```
  - [DCDN](https://help.aliyun.com/zh/edge-security-acceleration/dcdn/developer-reference/api-dcdn-2018-01-15-setdcdndomainsslcertificate)
    ```pwsh
    aliyun dcdn DescribeDcdnDomainCertificateInfo --endpoint dcdn.aliyuncs.com --DomainName example.com
    aliyun dcdn SetDcdnDomainSSLCertificate --endpoint dcdn.aliyuncs.com --DomainName example.com --SSLProtocol on --SSLPub="$sslPub" --SSLPri="$sslPri"
    ```
  - 不能写成 `--SSLPub "..."`，尽管其它参数都能这样写，但 `SSLPub`/`SSLPri` 这样写会导致以下屎一般的报错，必须要使用 `--SSLPub="..."` 才能让傻逼阿里云正常解析（至少在 v3.0.210~251 上）
    - `ERRPR: '-----...-----' is not a valid parameter or flag.`
    - `ErrorCode: InvalidSSLPub`
  - 每次 `Set` 都会导致[证书管理](https://yundun.console.aliyun.com/?p=cas#/certExtend/upload/)中增加新的证书，不能通过指定 `--CertName` 覆盖

  [使用Aliyun CLI自动更新CDN证书\_阿里云免费证书自动更新-CSDN博客](https://blog.csdn.net/skyline66/article/details/140489748)

  [申请域名SSL证书并自动推送至阿里云 CDN\_阿里云cdn 自动更新证书-CSDN博客](https://blog.csdn.net/leijuly/article/details/135394463)

  [1Panel 推送 SSL 证书到阿里云、腾讯云 - Anyeの小站](https://www.anye.xyz/archives/XIo2GGcD)
- [Hill-98/aliyun-openapi-bash-sdk: \[Unofficial\] Alibaba Cloud SDK for Bash](https://github.com/Hill-98/aliyun-openapi-bash-sdk)

  [申请通配符域名证书并自动推送至阿里云 CDN](https://www.wqy.ac.cn/p/2311-acme-aliyuncdn/)
- [OpenSight/aliyun-cert: 为阿里云 CDN 域名获取和续期 let's encrypt 免费证书 Request and renew free certificates from let's encrypt for Aliyun CDN domains](https://github.com/OpenSight/aliyun-cert)
- [自动推送 Let's Encrypt 证书到阿里云 CDN -- Jacky's Blog](https://jackyu.cn/tech/push-lets-encrypt-cert-to-aliyun-cdn/)
- [git9527/aliyun-cdn-https-cert-updater: 阿里云CDN Https证书更新脚本](https://github.com/git9527/aliyun-cdn-https-cert-updater)

  [个人开发记录 -- 同步Caddy证书到阿里云CDN](https://www.getce.cn/show/246.html)
- [如何将证书推送到阿里云的CDN？ - Issue #1461 - acmesh-official/acme.sh](https://github.com/acmesh-official/acme.sh/issues/1461)

腾讯云:
- [tencentcloud-cli: Tencent Cloud API 3.0 Command Line Interface](https://github.com/TencentCloud/tencentcloud-cli)
  - Python

  [1Panel 推送 SSL 证书到阿里云、腾讯云 - Anyeの小站](https://www.anye.xyz/archives/XIo2GGcD)
- [yangzan0532/qcloud\_ssl\_deploy: ssl证书自动化部署工具](https://github.com/yangzan0532/qcloud_ssl_deploy)
