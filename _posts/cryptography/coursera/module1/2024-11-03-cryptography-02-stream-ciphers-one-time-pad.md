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

Shannon formulize and formally explains what does information about the plain text actually mean.

Shannon's definition: A cipher (E, D) over (K, M, C) has **perfect secrecy** if the following holds:


- when ${\forall} m_0, m_1 \in M, len(m0) = len(m1)$ and $\forall c \in C$

- if the probability of encrypting `m0` and getting c is exactly the same as the probability of encrypting `m1` and getting c: $Pr[E(k, m_0) = c] = Pr[E(k, m_1) = c]$

where the k is random variable that's uniformly in the key space K.

that means:

if a attacker have intercepted a particular cipher text `c`, then in reality, the probability that the cipher text is the encryption of `m0` is exactly the same as the probability that's encryption of `m1`.

so you can't deduce the plaintext behind it based on the cipher text.

basically what this proves is that there's no **cypher text-only attack** on a cypher that has perfect secrecy.

### one time paid security proof
#### how to proof
can we build ciphers that actually have perfect secrecy?

actually **one time pad has perfect secrecy**.
 
let's prove it.

Proof: 

$\forall m, c: Pr[E(k, m) = c] = \frac{|k|, k \in K, s.t. E(k, m) = c)}{|K|}$

- |k| : number of keys matching condition
- |K| : the total number of keys


so $\forall m, c, \{|k|, k \in K, s.t. E(k, m) = c)\} = const$

Because the denominator is the same, the numerator is the same,  therefore the probability is always the same for all `m` and `c`.

and if this property is true, the cipher has perfect secrecy.

#### proof
for one time pad,

$if(E(k, m) = c)$,

=> $k\oplus m = c$

=> $k = m \oplus c$

=> $\{|k| \in K, E(k, m) = c\} = 1 $ , $\forall m, c$

so one time pad has perfect secrecy.

But, in fact, the one time pad is actually not such a secure cipher, there are other attacks that are possible.

it has perfect secrecy, does not mean that the one time pad is the secure cypher to use.


#### long keys

one time pad had really long keys and so the obvious question is are there other ciphers that has perfect secrecy and possibly have much, much shorter keys?

the bad news is the Shannon proved another theorem that says that in fact if a cipher has perfect secrecy, the number of keys in the cipher must be at least the number of messages that the cipher can handle.

that means |K| >= |M|

=> len(key) >= len(msg)

unfortunately, it's very difficult to use in practice, because the keys is essentially as long as message.



