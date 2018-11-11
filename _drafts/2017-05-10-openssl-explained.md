---
layout: post
title: OpenSSL Explained
categories:
- Linux
tags:
- openssl
---

# References

* [SSL and TLS Explained](https://blog.barracuda.com/2015/07/09/ssl-and-tls-explained/)
* [Cryptography - Public Key Cryptography](https://en.wikipedia.org/wiki/Cryptography#Public-key_cryptography)

# Cryptography

[Cryptography](https://en.wikipedia.org/wiki/Cryptography) is the practice and study of techniques for securing communications so that third parties or the public cannot read them. Since the introduction of encryption technologies during World War I and II, the methods used to encrypt communications have become very complex.

A message that has not been encrypted is usually referred to as "plain text". A message that has been encrypted, and thus unreadable, is referred to as a "cipher" or "code".

# Symmetric Cryptography

[Symmetric-key cryptography](https://en.wikipedia.org/wiki/Cryptography#Symmetric-key_cryptography) refers to methods of encryption that involve both the sender and receiver having the same secret (a shared key) that can be used to encrypt or decrypt messages.

# Public-key Cryptography

[Public-key cryptography](https://en.wikipedia.org/wiki/Cryptography#Public-key_cryptography) (also known as asymmetric key cryptography) involves two different yet mathematically related keys - a public key and a private key. Even though the keys are related, the calculation of one key from the other should be computationally infeasible in such systems.

The public-key may be freely distributed, however the private key must remain a secret. The public key is used for encryption, where-as the private key must be used for decryption.

It is through what is known as a [Diffie-Hellman key exchange protocol](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange) that two parties are able to secretly agree on a shared encryption key. The first algorithm based on this protocol was the [RSA algorithm](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) introduced in 1978.

# History

SSL was originally created by Netscape in 1994 as a solution to concerns over security on the internet, as a platform or operating system agnostic solution. SSL does not only ensure that communications are encrypted, but also ensures that the identity of the server being communicated with is validated.

SSL was released publicly as versions 2 and 3 before being renamed as Transport Layer Security (TLS) version 1. The latest adopted specification is [TLS v1.2](https://en.wikipedia.org/wiki/Transport_Layer_Security#TLS_1.2). Both SSL and TLS use the [X.509](https://en.wikipedia.org/wiki/X.509) standard for [public key certificates](https://en.wikipedia.org/wiki/Public_key_certificate).

# Certificate Authorities

# Low Assurance SSL Certificates

Certificates act as digital documents that verify that a specific public key does in fact belong to a specific entity. This helps the user to distinguish between authenticated and fraudulent websites.

Trusted organizations known as Certificate Authorities are key in establishing this trust, as they act as a 3rd party that validates the identity associated with a trusted certificate.

Unfortunately Certificate Authorities issue certificates with a weak process to authenticate and verify the identity of those requesting a certificate. Usually this is based on the email address or phone numbers listed as authoritative contacts on the WHOIS record for the domain associated with the certificate. Because this is not a thorough validation process, these certificates issues by Certificate Authorities only receive a Yellow padlock icon when a browser detects them in use.

# Extended Validation SSL Certificates

Extended Validation (EV) SSL certificates were introduced to close this trust gap. Web browsers display a green indicator when EV SSL certificates are used by websites.

In addition to confirming that the request is coming from the owner of the domain name used with the certificate, the verification process includes authenticating the authority of the person requesting the certificate, and verifying the registration of the business with government or third party business registries.





