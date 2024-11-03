---
title: "Cryptography I - 03: stream ciphers - pseudorandom generators"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
  - cryptography
  - coursera
---

## Stream cipher
stream cipher: making the one time paid more practical.

idea: replace "random" key by "pseudorandom" key.

pseudo random generator: PRG

PRG is a function. G: {0, 1}^s -> {0, 1}^n. n >> s


Stream cipher can't have perfect secrecy!!

- need a different definition of security
- security will depend on specific PRG

### PRG must be unpredictable

### Weak PRG

