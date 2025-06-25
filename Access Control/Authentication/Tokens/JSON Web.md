# [JSON Web Token](https://jwt.io/) (JWT)
[Wikipedia](https://en.wikipedia.org/wiki/JSON_Web_Token)

**JSON Web Token (JWT)** is a proposed Internet standard for creating data with optional signature and/or optional encryption whose payload holds JSON that asserts some number of claims.

[RFC 7519 - JSON Web Token (JWT)](https://datatracker.ietf.org/doc/html/rfc7519)
- [RFC 8725 - JSON Web Token Best Current Practices](https://datatracker.ietf.org/doc/html/rfc8725)

[No Way, JOSE! Javascript Object Signing and Encryption is a Bad Standard That Everyone Should Avoid - Paragon Initiative Enterprises Blog](https://paragonie.com/blog/2017/03/jwt-json-web-tokens-is-bad-standard-that-everyone-should-avoid)
- [Stop using JWT for sessions - joepie91's Ramblings](http://cryto.net/~joepie91/blog/2016/06/13/stop-using-jwt-for-sessions/)

Discussions:
- 2021-04 [为什么那么多 web 系统使用 jwt token 来做身份认证 - V2EX](https://fast.v2ex.com/t/774127)

  > 我个人觉得大量的 web 系统在滥用 jwt token 技术。jwt token 签发后可使用私钥来验证其是否过期和篡改，这个技术用来做下载链接、邮箱验证、或短时间内的一次性验证业务是非常好的。但如果用来做 web 系统的身份认证，那简直糟糕透顶。在真正无状态下，很难平衡签发时间和安全之间的矛盾，还有无法续期导致的用户体验问题。当然，可以打补丁，甚至变得有状态，最后解决以上问题。但是，为什么一开始不用 cookie + session 呢？想听听大家的看法。

- 2021-05 [关于国内腾讯，阿里等等互联网公司的主流业务全都不使用 jwt 做鉴权的一些思考 - V2EX](https://s.v2ex.com/t/776114)
- 2021-09 [使用 token 不是还是要每次都需要从数据库加载用户信息么和传统 session 有什么区别？ - V2EX](https://www.v2ex.com/t/801448)

## Algorithms
[RFC 7518 - JSON Web Algorithms (JWA)](https://datatracker.ietf.org/doc/html/rfc7518#section-3)

[JSON Web Token (JWT) Signing Algorithms Overview](https://auth0.com/blog/json-web-token-signing-algorithms-overview/)

> Most JWTs in the wild are just signed. The most common algorithms are:
- `HS256`: HMAC + SHA256
- `RS256`: RSASSA-PKCS1-v1_5 + SHA256
- `ES256`: ECDSA + P-256 + SHA256
- `PS256`: RSASSA-PSS using SHA-256 MGF1 with SHA-256 (optional)
- `none` (optional)

[jwt-cracker: Simple HS256, HS384 & HS512 JWT token brute force cracker.](https://github.com/lmammino/jwt-cracker)

[JWT Authentication Bypass leads to Admin Control Panel | by Hohky | InfoSec Write-ups](https://infosecwriteups.com/jwt-authentication-bypass-leads-to-admin-control-panel-dfa6efcdcbf5)

[Another JWT Algorithm Confusion Vulnerability: CVE-2024-54150](https://pentesterlab.com/blog/another-jwt-algorithm-confusion-cve-2024-54150)

## Claims
[JSON Web Token (JWT) Claims](https://www.iana.org/assignments/jwt/jwt.xhtml#claims)

[JSON Web Token Claims](https://auth0.com/docs/secure/tokens/json-web-tokens/json-web-token-claims)
- `iss` (issuer): Issuer of the JWT
- `sub` (subject): Subject of the JWT (the user)
- `aud` (audience): Recipient for which the JWT is intended
- `exp` (expiration time): Time after which the JWT expires
- `nbf` (not before time): Time before which the JWT must not be accepted for processing
- `iat` (issued at time): Time at which the JWT was issued; can be used to determine age of the JWT
- `jti` (JWT ID): Unique identifier; can be used to prevent the JWT from being replayed (allows a token to be used only once)

  [How to use jti claim in a JWT - Stack Overflow](https://stackoverflow.com/questions/28907831/how-to-use-jti-claim-in-a-jwt)
  > Indeed, storing all issued JWT IDs undermines the stateless nature of using JWTs. However, the purpose of JWT IDs is to be able to revoke previously-issued JWTs. This can most easily be achieved by blacklisting instead of whitelisting. If you've included the "exp" claim (you should), then you can eventually clean up blacklisted JWTs as they expire naturally. Of course you can implement other revocation options alongside (e.g. revoke all tokens of one client based on a combination of "iat" and "aud").

Date time:
> Implementers MAY provide for some small leeway, usually no more than a few minutes, to account for clock skew.
> Its value MUST be a number containing a NumericDate value.
>
> NumericDate:
> A JSON numeric value representing the number of seconds from
> 1970-01-01T00:00:00Z UTC until the specified UTC date/time,
> ignoring leap seconds.  This is equivalent to the IEEE Std 1003.1,
> 2013 Edition [POSIX.1] definition "Seconds Since the Epoch", in
> which each day is accounted for by exactly 86400 seconds, other
> than that non-integer values can be represented.  See RFC 3339
> [RFC3339] for details regarding date/times in general and UTC in
> particular.

[authentication - What is difference between private and public claims on jwt - Stack Overflow](https://stackoverflow.com/questions/49215866/what-is-difference-between-private-and-public-claims-on-jwt)

## Invalidation
[Invalidating JSON Web Tokens - Stack Overflow](https://stackoverflow.com/questions/21978658/invalidating-json-web-tokens)
- Simply remove the token from the client
- Create a token blocklist
- Just keep token expiry times short and rotate them often
- > A common approach for invalidating tokens when a user changes their password is to sign the token with a hash of their password. Thus if the password changes, any previous tokens automatically fail to verify. You can extend this to logout by including a last-logout-time in the user's record and using a combination of the last-logout-time and password hash to sign the token. This requires a DB lookup each time you need to verify the token signature, but presumably you're looking up the user anyway.

  尽管仍然需要每次查询，但 JWT 无法伪造，防止了用于暴力破解。

[JWT (JSON Web Token) automatic prolongation of expiration - Stack Overflow](https://stackoverflow.com/questions/26739167/jwt-json-web-token-automatic-prolongation-of-expiration)
> Today, lots of people opt for doing session management with JWTs without being aware of what they are giving up for the sake of *perceived* simplicity. My answer elaborates on the 2nd part of the questions:
> 
> > What is the real benefit then? Why not have only one token (not JWT) and keep the expiration on the server?
> >
> > Are there other options? Is using JWT not suited for this scenario?
> 
> JWTs are capable of supporting basic session management with some limitations. Being self-describing tokens, they don't require any state on the server-side. This makes them appealing. For instance, if the service doesn't have a persistence layer, it doesn't need to bring one in just for session management.
> 
> However, statelessness is also the leading cause of their shortcomings. Since they are only issued once with fixed content and expiration, you can't do things you would like to with a typical session management setup.
> 
> Namely, you can't invalidate them on-demand. This means you **can't implement a secure logout** as there is no way to expire already issued tokens. You also **can't implement idle timeout** for the same reason. One solution is to keep a blacklist, but that introduces state.
> 
> I wrote a [post explaining these drawbacks](https://www.securitydrops.com/session-management/) in more detail. To be clear, you can work around these by adding more complexity (sliding sessions, refresh tokens, etc.)
> 
> As for other options, if your clients only interact with your service via a browser, I strongly recommend using a cookie-based session management solution. I also [compiled a list authentication methods](https://www.securitydrops.com/the-web-api-authentication-guide/) currently widely used on the web.

[JWT and server side token storage - Stack Overflow](https://stackoverflow.com/questions/34620867/jwt-and-server-side-token-storage)

## JSON Web Encryption
[Wikipedia](https://en.wikipedia.org/wiki/JSON_Web_Encryption)

[Understanding JSON Web Encryption (JWE)](https://www.scottbrady91.com/jose/json-web-encryption)

[encryption - Should jwt web token be encrypted? - Stack Overflow](https://stackoverflow.com/questions/34235875/should-jwt-web-token-be-encrypted)

## Implementations
- Nginx: [Setting up JWT Authentication | NGINX Documentation](https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-jwt-authentication/)

### Rust
- [jsonwebtoken](https://github.com/Keats/jsonwebtoken)
  - [jwt-authorizer: JWT authorization layer for Axum.](https://github.com/cduvray/jwt-authorizer)

  [Implementing JWT Authentication in Rust](https://www.shuttle.rs/blog/2024/02/21/using-jwt-auth-rust)

- [Frank JSW](https://github.com/GildedHonour/frank_jwt)
- [JWT-Simple: A secure, standard-conformant, easy to use JWT implementation for Rust.](https://github.com/jedisct1/rust-jwt-simple)

  > This crate is not an endorsement of JWT. JWT is [an awful design](https://tools.ietf.org/html/rfc8725), and one of the many examples that "but this is a standard" doesn't necessarily mean that it is good.
  > 
  > I would highly recommend [PASETO](https://github.com/paragonie/paseto) or [Biscuit](https://github.com/CleverCloud/biscuit) instead if you control both token creation and verification.
  > 
  > However, JWT is still widely used in the industry, and remains absolutely mandatory to communicate with popular APIs.

- [JWT](https://github.com/mikkyang/rust-jwt) (discontinued)

[How to add JWT and API Key authorized to Axum : r/rust](https://www.reddit.com/r/rust/comments/1d81lfk/how_to_add_jwt_and_api_key_authorized_to_axum/)