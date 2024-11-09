---
title: "Cryptography I - 04: stream ciphers - Attack on stream ciphers and the one time pad"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
  - cryptography
  - coursera
---

## Attack1: two time pad is insecure!!

Never use stream cipher key more than once !!

$C_1 =  m1 \oplus PRG(k)$

$C_2 =  m2 \oplus PRG(k)$

Eavesdropper does: 

$C_1 \oplus C_2 = m_1 \oplus m_2$

Enough redundancy English and ASCII encoding that:

$m_1 \oplus m_2$ => you can recover m1, m2

So Never use stream cipher key more than once !!

### Real World Example

- project venona: they use same pads to encrypt multiple messages.
- MS-PPTP(windows NT): point to point transfer protocol, client and server using same key encrypt message.
- 802.11b.WEP: 
- Disk encryption:  



### Two time pad: summary
Never use stream cipher key more than once!!

- Network traffic: negotiate new key for every session.
- Disk encryption: typically do not use a stream cipher.

## Attack2: no integrity(OTP is malleable)
one time pad and stream cipher don't provide integrity。

Modifications to ciphertext are undetected and
have predictable impact on plaintext.
