---
title: "Cryptography I - Section 1: What is Cryptography about?"
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

encrypt:
${\displaystyle C_{i}=E_{K}(M_{i})=(M_{i}+K_{i}){\bmod {2}}6}$

decrypt:
${\displaystyle M_{i}=D_{K}(C_{i})=(C_{i}-K_{i}){\bmod {2}}6,}$

```java
public class VigenereCipher {

   public static void main(String[] args) {
      String m = "WHAT A DAY".toUpperCase();
      String key = "CRYPTO".toUpperCase();
      String ciphertext = encrypt(m, key);
      System.out.println(ciphertext);
      String decrypt = decrypt(ciphertext, key);
      System.out.println(decrypt);
   }

   private static String decrypt(String ciphertext, String key) {
      StringBuilder sb = new StringBuilder(ciphertext.length());
      for (int i = 0, j = 0; i < ciphertext.length(); i++) {
         char c = ciphertext.charAt(i);
         if (Character.isAlphabetic(c)) {
            char shift = key.charAt(j);
            char decryptedChar = unShift(c, shift);
            sb.append(decryptedChar);
            j = (j + 1) % key.length();
         } else {
            sb.append(c);
         }
      }
      return sb.toString();
   }

   public static String encrypt(String plaintext, final String key) {
      StringBuilder sb = new StringBuilder(plaintext.length());
      for (int i = 0, j = 0; i < plaintext.length(); i++) {
         char m = plaintext.charAt(i);
         if (Character.isAlphabetic(m)) {
            char c = shift(m, key.charAt(j));
            sb.append(c);
            j = (j + 1) % key.length();
         } else {
            sb.append(m);
         }
      }
      return sb.toString();
   }

   // c = (m + k) % 26
   public static char shift(char m, char s) {
      return (char) ((m - 'A' + s - 'A') % 26 + 'A');
   }

   // m = (c - k) % 26
   public static char unShift(char c, char s) {
      return (char) ((c - s  + 26) % 26 + 'A');
   }
}
```
