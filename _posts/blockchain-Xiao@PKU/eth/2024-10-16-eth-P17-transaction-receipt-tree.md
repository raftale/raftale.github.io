---
title: "区块链技术与应用-P17-交易树和收据树"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
  - blockchain
  - eth
---

# 交易树和收据树
## 交易树和收据树
发布区块的时候，区块包含的交易会形成一个交易树。

每个交易执行完成后，会形成一个收据，记录交易的信息。

数据结构：MPT

交易树和收据树的MPT中，key就是交易在区块中的序号。交易的排列顺序是交易的节点决定的。

与状态树的区别：
1. 交易树和收据树只需要将当前区块的交易组织起来。它们不在节点之间共享。
2. 状态树要把系统中所有的状态包含进来。

交易树和收据树的作用：
1. 提供merkle proof；
2. 复杂的查找操作：找到以太坊中最近10天跟某个智能合约的交易。或者所有的众筹事件。


## bloom filter

为了支持复杂的查询，以太坊中引入了bloom filter这样的数据结构。

bloom filter的特点：
1. 元素不在集合中时的判断一定是准确的；
2. 元素在集合中的判断可能产生误报；
3. 无法支持删除元素；
4. 内存开销小

以太坊中的bloom filter的作用：

每个交易完成后会形成一个收据，收据包含一个Bloom filter，记录这个交易的类型、地址等信息。

发布的区块block header中也包含一个总的Bloom filter，其为该区块中所有交易的Bloom filter的一个并集。

比如要查找过去十天跟这个智能合约相关的所有交易。

先查block header中的bloom filter是否有这个交易的类型，如果存在就再查看这个区块包含的交易中所对应的收据树的bloom filter，如果存在，再查看交易进行确认；不存在则说明不在这个block。

Bloom filter这样一个结构，能够快速过滤掉大量的无关区块，提高查找效率。


## transaction-driven state machine
以太坊的运行过程可以看作一个transaction-driven state machine. 交易的执行会驱动账户的状态转移。

比特币也可以看作一个transaction-driven state machine，比特币中的状态就是`UTXO`。

他们的状态转移都是确定性的。


## Q & A

### 收款账户是否有可能不在状态树中
有可能。

只有在产生交易的时候才会被记录到状态树中。

### 状态树能否只记录当前区块的交易账户状态

不能。原因在于A 转账 B 时，
1. 查找A、B的账户状态不方便，需要往前找到匹配的区块。
2. 如果B是新建的账户，需要一直找到创世区块才知道该账户为新账户。




