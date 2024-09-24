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

Go:
- [caddy: Fast and extensible multi-platform HTTP/1-2-3 web server with automatic HTTPS](https://github.com/caddyserver/caddy)
  - `scoop install caddy`, 14.2 MiB

  [HTTPS quick-start --- Caddy Documentation](https://caddyserver.com/docs/quick-starts/https)
- [lego: Let's Encrypt/ACME client and library written in Go](https://github.com/go-acme/lego)
  - [DNS validation](https://go-acme.github.io/lego/dns/index.html): Cloudflare, AWS Route53, Azure, GCP, DigitalOcean, Linode, Hetzner, GoDaddy, 阿里云, 腾讯云, 华为云, ...
  - `scoop install lego`, 23 MiB
- [certificates: 🛡️ A private certificate authority (X.509 & SSH) & ACME server for secure automated certificate management, so you can use TLS everywhere & SSO for SSH.](https://github.com/smallstep/certificates)

.NET:
- [win-acme: A simple ACME client for Windows](https://github.com/win-acme/win-acme)
  - DNS validation: Cloudflare, Azure, GCP, DigitalOcean, Linode, Hetzner, GoDaddy, [阿里云](https://www.win-acme.com/reference/plugins/validation/dns/alibaba), 腾讯云, ...
  - Servers: IIS, Apache, Exchange
  - `scoop install win-acme`, 13.6 MiB, *trimmed* (cannot use plugins)
  - `wacs`
  - `C:\ProgramData\win-acme\acme-v02.api.letsencrypt.org\Certificates\`
- [certify: Certify The Web - ACME Certificate Manager UI for Windows](https://github.com/webprofusion/certify)
- [ACMESharp: An ACME client library and PowerShell client for the .NET platform (Let's Encrypt)](https://github.com/ebekker/ACMESharp) (discontinued)

Web:
- [Cert Warden (formerly LeGo CertHub)](https://www.certwarden.com/)

  [LeGo CertHub - Centralized Let's Encrypt : r/selfhosted](https://www.reddit.com/r/selfhosted/comments/133dpd6/lego_certhub_centralized_lets_encrypt/)