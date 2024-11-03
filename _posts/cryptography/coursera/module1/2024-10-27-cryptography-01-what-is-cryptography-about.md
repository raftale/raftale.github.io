---
title: "Cryptography I - 01: What is Cryptography about?"
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

## History


### Symmetric Ciphers
c := E(key, m)
1. c: ciphertext
2. E: cipher
3. k: shared key
4. m: plaintext

m = D(key, c)

### Few Historic Examples

#### Substitution cipher

how to break a substitution cipher?

1. use frequency of English letters: 利用了大数据的原理，单个字母在文本出现的概率是有统计的，根据这个概率排序就可以确定加密的letter映射的是哪一个letter
2. use frequency of pairs of letters(digrams): 多个字母连在一起是组成单词的，考虑到多个字母连在一起的出现的概率也是可以进行推断的。 

#### Vigenère cipher (16'th century, Rome)

if the plaintext is `WHAT A DAY`, and the key is `CRYPTO`，so the ciphertext is `YYYI T RCP`。

![img.png](img.png)

假设预先知道key的长度l，那么每隔l个字母它们的cipher是一样的，因此可以猜测出现频率最高的可能是E(毕竟我们预先知道最高的是E)，最后也就能猜测key。如果最后的message是有意义的，那就说明破解是没有问题的。
即使我们不知道key的长度，也可以通过不断的假设key的长度，通过穷举key的长度来找到有意义的message。


### Rotor machines(1870-1943)
Early example: the Hebern machine (single rotor)
![img_1.png](img_1.png)


Most famous: the Enigma (3-5 rotors)

![img_2.png](img_2.png)

### Data Encryption Standard (1974)
using computer

DES: # keys  = 2^56, block size = 64bits
DES is not secure today.

Today: 
1. AES(2001), # keys = 2^128
2. Salsa20
3. others
