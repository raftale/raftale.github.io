---
title: "区块链技术与应用 - P06 - 比特币网络"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
  - blockchain
---

# The Bitcoin NetWork

## P2P Overlay Network
比特币工作在应用层，底层的网络是一个P2P网络。
```txt
-----------------------------------------
application layer: | BitCoin Block Chain
-----------------------------------------
network layer:     | P2P Overlay Network 
-----------------------------------------
```
网络中所有的节点都是对等的，使用TCP协议通信，有利于突破传统的防火墙。
节点之间一段时间没有通信，会删除对方。

## 设计原则
simple, robust, but not efficient

## 消息传播
消息传播采用flooding的方式，节点第一次听到消息的时候，把消息传播给邻居节点。邻居节点的选取是随机的，没有考虑底层的拓扑结构。增强了鲁棒性，但牺牲了效率。

## race condition
每个节点维护一个等待上链的交易集合，第一次收到合法交易，则将其加入交易集合并转发给邻居节点。
两个产生race condition的交易，具体接受哪个交易取决于节点先收到哪个交易，后续的交易会被废弃。

## 区块大小限制
区块大小限制为1M，区块越小，传播越快。

## Best effort
比特币网络传播属于Best effort（尽力而为），不能保证一定传输成功。

## 可信第三方
支付系统内部没有投诉机制，如果出现线下交易纠纷，电商平台可作为担保，发布新交易作为补偿。