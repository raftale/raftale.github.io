---
title: "区块链技术与应用-P21-proof-of-stake"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
  - blockchain
  - eth
---

## Electrical Energy of Pow

1. [比特币耗电量](https://digiconomist.net/bitcoin-energy-consumption): 172 TWh(terawatt-hours) per year, 相当于波兰一年的耗电量。
2. [以太坊历史耗电量](https://digiconomist.net/ethereum-energy-consumption): 93 TWh per year when POW

> 1Twh = 10^9 Kwh

## Proof of stake(POS)
基本思想：
- 挖矿为什么要挖矿？
- 出块奖励，激励矿工记账

挖矿的设备是靠外部的货币购买的，矿工的算力本质上就是拼钱来决定的，那如果直接用钱来比一比，跳过挖矿耗能这个过程不好吗？ 所以POS有时也被称为virtual mining。

采用`Proof of stake`的货币分配：
1. 预留一些货币给开发者
2. 出售一些货币换取开发所需要的资金

在系统进入稳定状态后，每个人可以按照持有货币的数量进行质押投票。

Pos比Pow的优点：
1. 节能，减少温室气体排放。
2. 算力较小的POW很容易被恶意攻击(AltCoin Infanticide)，POW的攻击资源来自于外部的算力购买；但POS中发动攻击的资源只能由从加密系统内部获取。

### 混合模型
POW和POS可以混合使用，持有货币越多，POW挖矿越容易出块。

POS的缺点是持有货币最多的人挖矿最容易。

Proof of Deposit： 所以投入的货币有一定的锁定期，不能够重复使用，比如挖当前区块的时候投入一定的币降低难度，但挖下一个区块的时候上一个区块投入的币就不能用了，需要等待一定的时间才能再次使用。

### Nothing At Stake
权益证明系统设计的一大挑战：Nothing At Stake（无利害攻击）。

POW只能选择一侧挖，但Pos中质押节点可以在多条分叉上同时进行stake，无论哪条链成为最长合法链，质押节点都可以获取收益。
```txt

                     ┌──┐ ┌──┐
 ┌──┐◄──┬──┐◄──┬──┐◄─┴──┘◄┴──┘
 └──┘   └──┘   └──┘◄─┬──┐   ▲ 
                     └──┘   │ 
                      ▲     │ 
                      └─stake 
```
但如果大家都这样做对系统是有害的，不利于达成共识。

## Casper
以太坊中准备采用的权益证明协议为：`Casper the friendly Finality Gadget(FFG)`，该协议是要在POW阶段是要混合使用的，为POW提供`Finality`。

### Finality
`Finality` is the property that there are blocks in the chain that are guaranteed never to be reverted: they will be part of the chain forever.

POW中交易是可能回滚的。 比特币中为了防范分叉攻击，挖到的区块很可能作废，所以一个交易要经过6个区块确认，但确认后只是说明回滚的概率小，并不能完全避免。 所以POW系统缺乏这种`Finality`。

以太坊的系统中，`Casper FFG`实现了`Finality`。

### Validator
Casper引入了`Validator`，一个用户要成为`Validator`，需要投入一定数量的以太币作为「保证金」，这笔保证金会被系统给锁定住。保证金的数量决定了投票的权重。

`Validator`的职责是要推动系统达成共识，投票选出最长合法链。

### Epochs and checkpoints

1. Epoch: 混用POW的时候还是会有挖矿的，每挖出一百个区块就作为一个`epoch`。
2. checkpoint: 为了确保验证者能对同一个区块进行投票，checkpoint就是用来进行投票的标记点，它是每个epoch的第一个slot。

### Two-phase commit
投票过程采用的`two-phase commit`的思想：
1. Prepare message
2. Commit message

原始版本的Casper中，两轮投票都在同一个checkpoint，每一轮投票必须要2/3以上的`Validator`同意才能通过。
```txt
          │◄───  epoch ──────►│               
          │                   │               
          │                   │               
 ┌──┐◄┬──◄┼┬──◄── ◄─┬──◄──┬──◄┼┬──◄──┬──┐◄┬──┐
 └──┘ └──┘│└──┘ ... └──┘  └──┘│└──┘  └──┘ └──┘
          │                   │               
          │                   │prepare 2/3    
          │                   │commit  2/3    
```

实际系统中，不区分这两个message，而且把这个epoch从一百个区块减少到50个区块。

每个epoch只用一轮投票就行，这轮投票对于上一个epoch是`commit message`，对于下一轮`epoch`来说是`prepare message`。

连续两轮`epoch`投票后，两轮`epoch`都得到`2/3`以上的多数后，第一轮`epoch`的交易才算有效，实现了`Finality`。

```txt
 epoch │◄───epoch────►│◄────epoch────►│    
       │              │               │    
       │              │               │    
       │              │               │    
┌───┐◄─┼┬──◄── ◄─┬──◄─┼─┬──◄── ◄─┬──┐◄┼┬──┐
└───┘  │└──┘ ... └──┘ │ └──┘ ... └──┘ │└──┘
       │              │               │    
       │              │               │    
       │              │               │    
      2/3             2/3            2/3   
```

### Validator的奖惩
`Validator`参与这个投票过程有什么好处呢？

- `Validator`履行职责的话可以得到相应的奖励。

但如果`Validator`有：
1. 行政不作为：该投票时没有投票，会扣除一部分保证金
2. 乱作为：乱投票，给两个分叉的都投票，这种情况会没收全部的保证金，没收的保证金会被销毁掉。

每个`Validator`有一定的任期限制，任期结束后会进入等待期，等待期间等待其他节点检举揭发验证者是否存在不良行为，若通过等待期，则可以取回保证金和应该得到的奖励。


### 安全性

Casper协议可以给挖矿出来的区块链的某种状态做一个`checkpoint`，那么这个`checkpoint`是绝对安全的吗？

换句话说，通过验证者投票达成的`Finality`有没有可能被推翻？

恶意攻击者如果单纯是矿工的话，无法推翻；但如果是大量的`Validator`两边下注，给两个冲突的`Finality`两边投票，就可能导致系统被篡改。

以太坊系统设想，随着时间推移，挖矿奖励逐渐减少而权益证明奖励逐渐增多，从而实现POW到POS的过渡，最终实现完全不用挖矿。

为什么不一开始就直接用权益证明，因为一开始不是很成熟。

## Delegated Proof of Stake

EOS采用的共识机制是`DPOS: Delegated Proof of Stake`。

先用投票的方法选21个超级节点，再由超级节点产生区块。

目前，权益证明仍然处于探索阶段。

## 挖矿耗能一定不好吗
挖矿消耗大量电能，一定是不好的吗？也有人持相反观点。

1. 电能本身很难进行存储和传输，挖矿提供了将电能转换为钱的手段；
2. 未使用完的清洁能源的电能是很难传回主电网的，挖矿使用清洁能源，有效的化解了过剩的电能。

挖矿对环境的影响是有限的，并且能推动当地的经济发展。

