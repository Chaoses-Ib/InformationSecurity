# Mandatory Access Control
In MAC, users do not have much freedom to determine who has access to their files. For example, security clearance of users and classification of data (as confidential, secret or top secret) are used as security labels to define the level of trust.[^access-wiki]

Security levels:
- Top Secret
- Secret
- Confidential
- Unclassified

Operations:
- Read up: Low level read high level
- Read down: High level read low level
- Write up: Low level write high level
- Write down: High level write low level

Models:
- [Bell-LaPadula model (BLP model)](https://en.wikipedia.org/wiki/Bell%E2%80%93LaPadula_model): Write up, read down (WURD)
  
  Confidentiality.
- [Biba model](https://en.wikipedia.org/wiki/Biba_model): Read up, write down (RUWD)
  
  Integrity.
- [Clark-Wilson model](https://en.wikipedia.org/wiki/Clark%E2%80%93Wilson_model)
- [Brewer and Nash model (Chinese wall model)](https://en.wikipedia.org/wiki/Brewer_and_Nash_model)
- [Access control matrix](https://en.wikipedia.org/wiki/Access_control_matrix)
  - [Graham-Denning model](https://en.wikipedia.org/wiki/Graham%E2%80%93Denning_model)

[^access-wiki]: [Access control - Wikipedia](https://en.wikipedia.org/wiki/Access_control#Computer_security)