---
title: "Coursera - Cryptography I - Section 1: What is Cryptography about?"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
  - cryptography
  - coursera
---

# Course Overview

## cryptography is everywhere

1. Secure communication: HTTPS, 802.11i WPA2, GSM, Bluetooth
2. Encryption files on disk: EFS, TrueCrypt
3. Content protection: CSS, AACS
4. User authentication


## Secure Sockets Layer / TLS 
two main protocol: 
1. Handshake protocol: Establish shared secret key using public-key cryptography.
2. Record Layer: Transmit data using shared secret key ensure confidentiality and integrity.

## Use cases

1. Single use key: (one time key)
   - Key is only used to encrypt one message
   - encrypted email: new key generated for every email
2. Multi use key: (many time key)
   - Key used to encrypt multiple messages
   - encrypted files: same key used to encrypt many files
   - Need more machinery than for one-time key


## what is cryptography

### crypto core
1. Secret key establishment
2. Secure communication

### but crypto can do much more

1. Digital signatures
2. Anonymous communication
3. Anonymous digital cash

### protocols
Another application of cryptography has to do with more abstract protocols.
1. Elections
2. private auctions: Vickrey auction.
3. Secure multi-party computation

There's a very central theorem in crypto and it really is quite a surprising fact.

That says that any computation you'd like to do, any function F you'd like to compute, that you can compute with a trusted authority, you can also do without a trusted authority.

for example, individual inputs can talk to each other using some protocol, at the end of the protocol all of a sudden the value of the function becomes known to everybody. and yet nothing other than the value of the function is revealed.


### crypto magic

1. privately outsourcing computation
    - GGoogle can actually compute on the encrypted values without knowing what the plain texts are
2. zero knowledge(proof of knowledge)
   -  the number N was constructed is as a product of two large primes, N = P * Q, Alice know N and P and Q, Bob just know N, Alice can prove to Bob that she knows the factorization of N, but Bob absolutely learns nothing at all about the factors P and Q. 


