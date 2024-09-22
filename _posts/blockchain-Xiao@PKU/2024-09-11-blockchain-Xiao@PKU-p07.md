---
title: "区块链技术与应用 - P07 - 比特币的挖矿难度调整"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
  - blockchain
---

挖矿本质上就是尝试`block header`中的`nonce`值，使得`H(block header) <= target`，`target`就是给定的目标阈值。

`target`越小，挖矿难度越大。调整挖矿难度，就是在调整目标空间在整个输出空间占的比例。 

比特币用的哈希算法是
