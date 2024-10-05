---
title: "区块链技术与应用 - P07 - 比特币的挖矿难度调整"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
  - blockchain
  - btc
---
# 比特币的挖矿难度调整
## 挖矿本质
挖矿本质上就是尝试`block header`中的`nonce`值，使得`H(block header) <= target`，`target`就是给定的目标阈值。

`target`越小，挖矿难度越大。调整挖矿难度，就是在调整目标空间在整个输出空间占的比例。 

挖矿难度不调整的话，随着算力越来越强，出块时间越来越短。

> 时间越来越短并不是好事，可能出现两个节点都同时挖到区块，然后造成硬分叉。分叉过多，越难达成共识，最长链攻击也就越容易。

比特币用的哈希算法是SHA-256.

## 难度调整
每2016个区块（大概14天）调整一次难度。

`target = target * (actual_time / expected_time)`
1. actual_time: time spent mining the last 2016 blocks
2. expected_time: 期望挖矿时间

实际挖矿时间过长，`target`就会增大，挖矿难度就变小。

实际代码中，上调和下调都有4倍的限制。

如果有的矿工不调整呢？
如果不调整的话，诚实的矿工不会认，代表target的nbits的校验就不会通过。


