---
title: "区块链技术与应用 - P05 - 实现"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
  - blockchain
---

## Transition-based ledger

比特币采用了基于交易的账本（Transition-based ledger）模式，并维护了一个UTXO(Unspent Transation Output) 的数据结构，来防止双花攻击，也就是说，交易中币的来源必须是在UTXO中。



矿工打包交易的动机：

1. block reward: 每隔21万个区块减半，大概4年一次。
2. transaction fee：`total outpus - total inputs`

### Block Header

1. version
2. Previous block header hash
3. merkle root hash
4. time: 区块产生时间
5. nBits: An encoded version of the target threshold this block's header hash must be less than or equal to.
6. nonce

## 挖矿过程的概率分析
挖矿本身就是在尝试`nonce`，使得产生的区块哈希值小于等于目标阈值。
- 每次尝试都被视为伯努利试验
- 无记忆性，保障了挖矿的公平

整个系统的出块时间服从指数分布。

## 比特币总量
每`210000`区块减半，等比公式计算总量：
`210000 * 50 + 210000 * 25 + ...= 2100,0000`

虽然出块奖励越来越少，但价格变高后手续费也足够支撑矿工挖矿。

## 安全性分析

如果记账权落在了恶意的节点上。

### 问题1:是否能偷币
不能，转账别人的币需要私钥，无法伪造别人的签名。

诚实的节点不会接受非法的交易，诚实的节点会继续沿着上一个节点写入合法的交易。

### 问题2: 能否双花
有最长合法链原则（区块上有高度，高度）保护，但是如果矿工将自己双花的区块所在的链变成最长合法链呢（这个也叫做分叉攻击）？

防范方法：交易多等几个区块确认，比特币默认等6个区块，6个区块确认后，攻击者的重组链追上正常区块链的概率将降低至千分之一以下，所以可以认为是安全的。
```txt
                          one confirmation    two           three        four       
                                                                                    
        ┌──────┐◄──┬──────┐ ◄───┬──────┐◄───┬──────┐◄──────┬───────┐◄──┬───────┐    
        │      │   │      │ ◄┐  │ M->A │    │      │       │       │   │       │    
        └──────┘   └──────┘  │  └──────┘    └──────┘       └───────┘   └───────┘    
                             │                                                      
                             │                                                      
                             └──┬──────┐                                            
                                │M->M' │                                            
                                └──────┘                                            

```

### 问题3：能不能故意不把合法的交易写入区块中？
可以。但是始终有诚实的节点在下一次区块中写入合法的交易。


### 问题4: selfish mining

隐藏挖到的区块，最后再一起发布作为最长链。这个也叫分叉攻击。
但是要实现这样的操作，你的算力必须要占据一半以上。

selfish mining的好处让别人做无用功，减少了竞争，但风险是可能跟不上最长链。


### 问题5: 跳着挖区块
不能，必须要有前一个区块的哈希。
