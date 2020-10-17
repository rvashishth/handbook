# Overview

- [Overview](#overview)
  - [Keywords](#keywords)
  - [Basics](#basics)
    - [Core Concepts](#core-concepts)
    - [Types of attacks](#types-of-attacks)
    - [Types of security methods](#types-of-security-methods)
    - [ACM - Access control mechanisms](#acm---access-control-mechanisms)
    - [Utility Tools](#utility-tools)
    - [Algorithms](#algorithms)
  - [protocol/standards](#protocolstandards)
  - [TLS Certificates](#tls-certificates)
  - [TODO](#todo)
  - [Good Reads](#good-reads)

##  Keywords
**ACM**(Access control mechanism), **SPI**(Service provider interface)

## Basics

- Confidentiality(access is allowed to only authorized entity, what is private stays private)
- Authenticity(data is from authentic source),
  - Hashing
- Integrity(data is not modified except by authorized entities)
  - MAC(message authentication code)
  - MD(message digest) or Hash
  - Digital signature
- Non-repudiation (integrity + authenticity = msg sender can not deny the authenticity of their signature or content of their msg)

### Core Concepts

> Encoding, Hashing, Encryption

- [Hashing](https://www.youtube.com/watch?v=2BldESGZKB8)

  - always return a fixed size of string/bytes for any size of given input(file/string). It's a one way process, we hash a data when we don't need to get back the original data. i.e. store password in hashed format.
  - Hashing does not encrypt
  - A hashing algo will always produce the same output if given input is same.
  - MD(message digest) and SHA(Secure hashing algorithm) are two hashing algorithms
  - **BCrypt** uses blowfish algorithm for password hashing mainly
  - use **salting** with bcrypt

- Encoding
- Encryption

  - Symmetric uses the same key to lock unlock the data
  - Some symmetric algorithms are AES, DES, Blowfish
  - Asymmetric algorithms RSA, Diffie-Hallman

- Cipher - when we want to encrypt and decrypt the data
- Block Cipher (when data size is known) and Stream Cipher (when data size is not known and streamed)

- HTTP, FTP, SMTP are application layer protocols while SSL/TLS is transport layer protocol
- **TLS** only switch to secure connection after handshake is complete. Initial communication starts on plain non secure protocol.

-  **Signature** encryption(hash(data))

**How a web server get certificates flow works**

1. first mysite.com sends a **CSR**(certificate signing request) to a **RA**(registration authority)
2. RA will verify the CSR. RA sends CSR to **CA**(certificate authority)
3. CA will generate and send the cert to requesting application.
4. CA sends the cert status to verifying system **OCSP**(Online certificate status protocol)
5. Clients will open mysite.com, this will load the cert in browser
6. browser send certificate validation request via http to OCSP

**How public key exchange/TLS handshake works**

1.  user opens a https://mysite.com
2.  mysite sends it's asymmetric public key to user
3.  user generate a symmetric key encrypted by mysites's public key to mysite
4.  mysite decrypt the users msg using it's private key, get hold of symmetric key
5.  now both entities can talk to each other via symmetric key

- **PKI** Public key infrastructure
- **CA** Certificate authority
- **TPC** Third Party Cookie
- **Mutual TLS** Authentication
- **Digital Signature** provides non-repudiation

### Types of attacks

- CSRF - Cross Site Request Forgery
- Men in middle attack
- Dictionary Attack

### Types of security methods

- Pinning - associate a web server with a public key
- TLS Handshake

### ACM - Access control mechanisms

- RBAC
- UBAC
- ABAC

- Client Credential Flow (used by gateway api clients )
- Policy Enforcement Point

### Utility Tools

- **GPG** GNU Private Guard is an implementation of PGP. Use GPG4win for windows. We can generate RSA or DSA keys using GPG tool. Use this key to encrypt the msg and later use passpharase to decrypt this msg
- **PGP** Pretty good privacy
- sha256, md5 other such hashing algorithm's lib are also available
- wireshark

### Algorithms

Symmetric

- AES

Asymmetric

- RSA (certificate communication)

Hashing

- MD5
- SHA - SHA-1, SHA-256, SHA-512
- HMAC - also uses a secret key

## protocol/standards

- OpenID Connect
- OAuth
- OpenLDAP
- [UMA](https://docs.kantarainitiative.org/uma/wg/oauth-uma-federated-authz-2.0-09.html)

* OAuth.net - specification
* Openid.net - specification
* www.w3.org/TR/webauthn-2/ - specification
* WebAuthn.io - utility/demo/docs
* JWT.io - utility/demo/docs
* Keycloak - Open Source
* Auth0.com - product/services
* Okta.com - product/services
* OAuth.com - product/services
* SiteMinder SPS - Secure Server Proxy

- [JWS - Signature](https://tools.ietf.org/html/rfc7515)
- [JWE - Encryption](https://tools.ietf.org/html/rfc7516)
- [JWK - Key](https://tools.ietf.org/html/rfc7517)
- [JWA - Algorithms](https://tools.ietf.org/html/rfc7518)
- [JWT - Token](https://tools.ietf.org/html/rfc7519)

## TLS Certificates

A cert contains - owner info, issuer info, signature(encrypted hash) and public key

Self Signed Certificates - signed by owner. Where Subject Name details and Issuer details are same

Multi domain certificates - single certificate is used by  multiple domain i.e. google.com

Intermediate certi - ?
Chain of trust - ?

## TODO

- [ ] Read RBAC https://profsandhu.com/journals/tissec/ANSI+INCITS+359-2004.pdf
- [ ] what is user agent
- [ ] write example for all grant type
- [ ] TLS Handshake
- [ ] PGP public key vs certificate
- [ ] RSA DSA keys
- [ ] How bcrypt + salting safeguard against dictionary attacks

## Good Reads

- https://medium.com/sitewards/the-magic-of-tls-x509-and-mutual-authentication-explained-b2162dec4401
