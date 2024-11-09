---
title: "Cryptography I - 05: stream ciphers - real-world -examples"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
  - cryptography
  - coursera
---

## Old example: RC4 (1987)

- used in HTTPS and WEB

Weaknesses:
1. Bias in initial output: Pr[ 2nd byte = 0 ] = 2/256
2. Prob. of (0,0) is 1/2562 + 1/2563
3. Related key attacks

RC4 is unsecure!!

## Old example: CSS (badly broken)

CSS: content scrambling system

CSS is based on Linear feedback shift register (LFSR):

LFSR seed: init status of LFSR

all broken:
1. DVD encryption (CSS): 2 LFSRs
2. GSM encryption (A5/1,2): 3 LFSRs
3. Bluetooth (E0): 4 LFSRs
   
CSS: seed = 5 bytes = 40 bits
 
CSS is unsecure!!

## Modern stream ciphers: eStream

PRG: $\{0, 1\}^s * R \rightarrow \{0, 1\}^n, n>>s$

R: nonce
- a non-repeating value for a given key

$E(k, m; r) = m \oplus PRG(k; r)$

the pair (k, r) is never used more than once.

### eStream: Salsa 20 (Software + Hardware)

Salsa: 
