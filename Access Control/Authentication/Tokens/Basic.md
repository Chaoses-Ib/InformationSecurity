# HTTP Basic Authentication
[Wikipedia](https://en.wikipedia.org/wiki/Basic_access_authentication), [MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication)

`Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==`

> Basic authentication is typically used in conjunction with HTTPS to provide confidentiality.

[http - What is the "realm" in basic authentication - Stack Overflow](https://stackoverflow.com/questions/12701085/what-is-the-realm-in-basic-authentication)

JS:
- [Basic Authentication Using JavaScript - Stack Overflow](https://stackoverflow.com/questions/34860814/basic-authentication-using-javascript)
- [javascript - Basic authentication with fetch? - Stack Overflow](https://stackoverflow.com/questions/43842793/basic-authentication-with-fetch)

Rust:
- [axum-auth: High-level http auth extractors for axum](https://github.com/owez/axum-auth)

## Servers
- Nginx: [`ngx_http_auth_basic_module`](https://nginx.org/en/docs/http/ngx_http_auth_basic_module.html)

  ```nginx
  auth_basic "realm";
  # conf/.htpasswd
  auth_basic_user_file .htpasswd;
  ```
  - 404 if `auth_basic_user_file` not found
  - No cache so doesn't need reload
  - [nginx-control](https://github.com/16Hexa/nginx-control)

  [Restricting Access with HTTP Basic Authentication | NGINX Documentation](https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/)

## htpasswd
[htpasswd - Manage user files for basic authentication - Apache HTTP Server Version 2.4](https://httpd.apache.org/docs/2.4/programs/htpasswd.html)

`.htpasswd`:
```
# comment
name1:password1
name2:password2:comment
name3:password3
```

[Password Formats - Apache HTTP Server Version 2.4](https://httpd.apache.org/docs/2.4/misc/password_encryptions.html)
- Apache: BCrypt, Apache MD5, SHA1, Unix crypt, plain text
- [Nginx](https://nginx.org/en/docs/http/ngx_http_auth_basic_module.html): Apache MD5, SSHA1, SHA1, Unix crypt, plain text
  - Unix: BCrypt, SHA-512, SHA-256

  [\[patch\]: document SHA-2 support in glibc crypt()](https://mailman.nginx.org/pipermail/nginx-devel/2017-October/010535.html)

  [HTTP/nginx - Rockstable Wiki](https://wiki.rockstable.it/HTTP/nginx)

  [Nginx gives an Internal Server Error 500 after I have configured basic auth - Stack Overflow](https://stackoverflow.com/questions/31833583/nginx-gives-an-internal-server-error-500-after-i-have-configured-basic-auth)
  - `[error] 5936#3940: *236 user "admin": password mismatch` on Windows

  [problem with nginx and bcrypt for htpasswd - Issue #643 - nginx-proxy/nginx-proxy](https://github.com/nginx-proxy/nginx-proxy/issues/643)

  [Use of debian as base image disallows use of bcrypt for password hashes - Issue #29 - nginx/docker-nginx](https://github.com/nginx/docker-nginx/issues/29)

Libraries:
- Rust
  - [pwhash: A collection of password hashing routines in pure Rust](https://github.com/inejge/pwhash)
    - BCrypt, MD5, SHA1 (`$sha1${rounds}${salt}${checksum}`), SHA256, SHA512, Unix crypt
  - [passivized\_htpasswd: Generate Apache htpasswd files](https://github.com/iamjpotts/passivized_htpasswd)
    - BCrypt, SHA-512
  - ~~[blfpd/htpasswd-rs: Basic generator of .htpasswd file written in Rust](https://github.com/blfpd/htpasswd-rs)~~
  - [htpasswd-verify](https://github.com/aQaTL/htpasswd-verify) (discontinued)
    - Apache MD5, BCrypt, SHA1, Unix crypt
    - > It also allows to encrypt with md5
    - [Multiple improvements by twistedfall - Pull Request #7](https://github.com/aQaTL/htpasswd-verify/pull/7)
  - [axum-htpasswd: htpasswd Authentication Layer for Axum](https://github.com/Sarek/axum-htpasswd)

Tools:
- `htpasswd`
  - Windows
    - `scoop install apache`
    - Apache MD5, BCrypt, SHA1 (SHA-2 and Unix crypt are not supported)
- `openssl passwd`
- `{scheme}data` + SHA1 tools
- [Htpasswd Generator - Create Htaccess .htpasswd file with all 5 Algorithms!](https://www.askapache.com/online-tools/htpasswd-generator/)
