# [JSON Web Token](https://jwt.io/) (JWT)
[Wikipedia](https://en.wikipedia.org/wiki/JSON_Web_Token)

[RFC 7519 - JSON Web Token (JWT)](https://datatracker.ietf.org/doc/html/rfc7519)

**JSON Web Token (JWT)** is a proposed Internet standard for creating data with optional signature and/or optional encryption whose payload holds JSON that asserts some number of claims.

## Invalidation
[Invalidating JSON Web Tokens - Stack Overflow](https://stackoverflow.com/questions/21978658/invalidating-json-web-tokens)
- Simply remove the token from the client
- Create a token blocklist
- Just keep token expiry times short and rotate them often
- > A common approach for invalidating tokens when a user changes their password is to sign the token with a hash of their password. Thus if the password changes, any previous tokens automatically fail to verify. You can extend this to logout by including a last-logout-time in the user's record and using a combination of the last-logout-time and password hash to sign the token. This requires a DB lookup each time you need to verify the token signature, but presumably you're looking up the user anyway.

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

## Implementations
### Rust
- [jsonwebtoken](https://github.com/Keats/jsonwebtoken)
- [Frank JSW](https://github.com/GildedHonour/frank_jwt)
- [JWT](https://github.com/mikkyang/rust-jwt)

[Implementing JWT Authentication in Rust](https://www.shuttle.rs/blog/2024/02/21/using-jwt-auth-rust)