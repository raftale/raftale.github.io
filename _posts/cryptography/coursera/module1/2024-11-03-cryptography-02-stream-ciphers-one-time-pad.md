---
title: "Cryptography I - 02: stream ciphers - one time pad"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
  - cryptography
  - coursera
---

## Symmetric Ciphers: definition
A cipher is defined over (K, M, C)
1. K is key space
2. M is this set of all possible messages 
3. C is this set of all possible ciphertexts.

the cipher itself is a pair of efficient algorithms E and D. 
1. E is the encryption algorithm, E(K, M) = C
2. D is the decryption algorithm, D(K, C) = M

consistent requirement: D(K , E(K, M)) = M

## The One Time Paid (Vernam 1917)

the first example of a secure cipher: The One Time Paid.

encrypt: $E(K, M) = K \oplus M$

1. consistency: $D(K, E(K, M)) = D(K, K \oplus M) = K \oplus (K \oplus M) = (K \oplus K) \oplus M = 0 \oplus M = M$
2. security: given M and C, it's very easy to recover what the key is, $K = M \oplus C$


## Information Theoretic Security (Shannon 1949)

what makes cipher secure?

Basic idea: the cipher text should reveal no information about the plain text.

shannon formulize and formally expalain what does information about the plain text actually mean.

definition: A cipher (E, D) over (K, M, C) has **perfect secrecy** if 

${\forall} m_0, m_1 \in M, len(m0) = len(m1)$ and $\forall c \in C$

$Pr[E(k, m_0) = c] = Pr[E(k, m_1) = c]$ 

where the k is uniform in the key space K.

if attacker have intercepted c, the probability of getting c is equally likely whether m0 is being encrypted or m1 is being encrypted.


basically what this proves is that there's no, there's no **cypher text-only attack** on a cypher that has perfect secrecy.

### one time paid security proof 
lemma: OTP has perfect secrecy.
Proof: 
$\forall m, c: Pr[E(k, m) = c] = (\#k, k \in K, such.that. E(k, m) = c) / |K|$

- #keys : number of keys
- |K| : the total number of keys

所以只需要证明：
$\forall m, c, \#\{k \in K, E(k, m) = c\} = const$

就能证明perfect secrecy。

for OTP, $if(E(k, m) = c)$, => $k\oplus m = c$ => $k = m \oplus c$

=> $\#\{k \in K, E(k, m) = c\} = 1 $ , $\forall m, c$

所以OTP has perfect secrecy.

Shannon proved another theorem that says that in fact if a cypher has perfect secrecy, the number of keys in the cypher must be at least the number of messages that the cypher can handle

|K| >= |M|

key-len >= len-msg

### bad news
unfortunately, it's very difficult to use in practice, because the keys is essentially as long as message.



