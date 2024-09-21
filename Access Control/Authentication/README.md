# Authentication
[Wikipedia](https://en.wikipedia.org/wiki/Authentication)

[The Web API Authentication guide - Security Drops](https://www.securitydrops.com/the-web-api-authentication-guide/)

[Summary of Authentication Methods For Red Hat Ansible Tower | Ansible Collaborative](https://www.ansible.com/blog/summary-of-authentication-methods-in-red-hat-ansible-tower/)

[关于鉴权，看懂这篇就够了 - 琅琊甲乙木 - 博客园](https://www.cnblogs.com/erichi101/p/15225947.html)

Discussions:
- 2018-01 [前后端是怎么验证身份的？ - V2EX](https://global.v2ex.com/t/425736)
- 2018-03 [问前端，你们单页应用时，用户登录鉴权一般是怎么做的？ - V2EX](https://cn.v2ex.com/t/439490)
- 2023-07 [微服务,认证,鉴权,授权 机制如何设计比较通用合理. - V2EX](https://cn.v2ex.com/t/959513)
- 2024-05 [网关与微服务间鉴权的疑惑 - V2EX](https://www.v2ex.com/t/1038123)

## Libraries
Rust:
- `cookies: cookie::CookieJar` + `let user = state.auth_verify(&cookies)?;`
- [tower_http::validate_request](https://docs.rs/tower-http/latest/tower_http/validate_request/index.html)
  - [tower_http::auth::async_require_authorization](https://docs.rs/tower-http/latest/tower_http/auth/async_require_authorization/index.html)
  - How to pass the user to the handler?
- [axum-login: 🪪 User identification, authentication, and authorization for Axum.](https://github.com/maxcountryman/axum-login)
  - Discussions instead of Issues
  - Must use sessions and cookies
- 2024-02 [Should i use axum-login or do it myself : r/rust](https://www.reddit.com/r/rust/comments/1ajpcv2/should_i_use_axumlogin_or_do_it_myself/)
- 2024-05 [Axum authentication and authorization : r/rust](https://www.reddit.com/r/rust/comments/1d1qmnb/axum_authentication_and_authorization/)