---
title: "区块链技术与应用 - P09 - The Bitcoin Scripting Language"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
  - blockchain
  - btc
---
# The Bitcoin Scripting Language
比特币交易验证其合法性依赖于脚本的执行。

比特币系统中使用的脚本语言唯一可以访问的内存空间只有栈，所以也被称为「基于栈的语言」。

## 交易结构
一笔交易的结构：
```json
{
  "result": {
    "txid": "921a…dd24",   // transaction id
    "hash": "921a…dd24",  // transaction hash
    "version": 1,         // bitcoin procotol version
    "size": 226,          // transaction size
    "locktime": 0,        // 交易生效的时间，如等多少区块后才生效
    "vin": [{             // 交易输入
      "txid": "c0cb…c57b",   // 币的来源，也就是之前交易的hash
      "vout": 0,             // 这个交易中的第几个输出
      "scriptSig": {         
        "asm": "3045...0018",     // 输入脚本内容
        "hex": "4830...0018"
      }
    }],
    "vout":  [{                   // 交易输出
      "value": 0.22684000,        // 转账多少金额
      "n": 0,                     // 交易中的第几个输出
      "scriptPubKey": {           // 输出脚本
        "asm": "DUP HASH160 628e…d743 EQUALVERIFY CHECKSIG",  // 输出脚本内容
        "hex": "76a9…88ac",
        "reqSigs": 1,                           // 需要多少个签名
        "type": "pubkeyhash",                   // 输出类型，公钥的哈希
        "addresses": [ "19z8LJkNXLrTv2QK5jgTncJCGUEEfpQvSr"]      // 收款人的地址(type对应的实际值)
      }
    },{
      "value": 0.53756644,
      "n": 1,
      "scriptPubKey": {
        "asm": "DUP HASH160 da7d…2cd2 EQUALVERIFY CHECKSIG",
        "hex": "76a9…88ac",
        "reqSigs": 1,
        "type": "pubkeyhash",
        "addresses": ["1LvGTpdyeVLcLCDK2m9f7Pbh7zwhs7NYhX"]
      }
    }],
    "blockhash": "0000000000000000002c510d…5c0b",  // 交易所在区块的hash
    "confirmations": 23,                           // 交易已经有多少个确认信息
    "time": 1530846727,                            // 交易产生的时间
    "blocktime": 1530846727                        // 区块产生时间
  }
}
```


## 输入输出脚本拼接

在[第四章](2024-09-01-btc-P04-Consensus%20Protocols.md)中有讲到过：
1. 输入部分包含转账方的公钥哈希。
2. 输出部分包含收款人的公钥哈希。

验证交易时需要进行合法性校验，具体来说就是校验「这个交易的输入部分的公钥哈希」和「币的来源的交易的输出部分的公钥哈希」是否一致。

校验是通过输入输出的脚本执行，首先就是要将输入输出脚本进行拼接。

```txt
 ┌────────┐◄───────┬───────┐◄────────┬───────┐◄───────┬────────◄─────────┬───────┐ 
 │        │        │       │         │       │        │        │         │       │ 
 │        │        ├───────┤         │       │        ├────────┤         │       │ 
 │        │        │ A -> B│         │       │        │ B -> C │         │       │ 
 │        │        ├───┬───┤         │       │        ├────┬───┤         │       │ 
 │        │        │   │   │         │       │        │    │   │         │       │ 
 └────────┘        └───┼───┘         └───────┘        └────┼───┘         └───────┘ 
                       │                                   │                       
                       │                                   │                       
                       │                                   │                       
                       ▼                                   ▼                       
 ┌──────────────────────────────────┐        ┌──────────────────────────────────┐  
 │                                  │        │                                  │  
 │           TX: A -> B             │        │           TX: A -> B             │  
 ├────────────────┬─────────────────┤        ├────────────────┬─────────────────┤  
 │ vin:           │ vout:           │        │ vin:           │ vout:           │  
 │   txid         │   value         │        │   txid         │   value         │  
 │   vout         │   n             │        │   vout         │   n             │  
 │   input script │ ┌─output script │        │ ┌─input script │   output script │  
 └────────────────┴─┼───────────────┴        └─┼──────────────┴─────────────────┴  
                    └───────────────────────┐  │                                   
                             ┌──────────────┼──┘                                   
                             │              │                                      
                       ┌─────▼─────────┬────▼───────────┐                          
                       │ input script  │ output script  │                          
                       └───────────────┴────────────────┘                          
                                                                                   
```

脚本执行流程中，首先是将币的来源的交易的输出脚本和当前交易的输入脚本拼接起来。早期是拼接，但后来为了安全考虑，执行思路就是先执行`input script`，再执行`output script`。
具体是如何执行的，下面将讲到。

## 输入输出脚本的几种形式

1. P2PK(Pay to Public Key)： 输出脚本直接给出收款人公钥。
2. P2PKH(Pay to Public Key Hash)：输出脚本给出的是收款人的公钥哈希。
3. P2SH（Pay to Script Hash）：输出脚本给出的收款人提供的一个脚本的哈希，该脚本称为`redeemScript`。等到以后转账时，输入脚本要给出`redeemScript`的具体内容和能保证正确运行所需的签名。

第一种其实很好理解，我们重点讲第二种，第三种太复杂了先不讲。

### P2Pk(Pay to Public Key)
1. input script:
   - PUSHDATA(Sig): 付款人用私钥对整个交易的签名
2. output script:
   - PUSHDATA(PubKey)：公钥
   - CHECKSIG：弹出栈顶的两个元素，用公钥检查签名是否正确
### P2PKH(Pay to Public Key Hash)
`input script` 和 `output script`的具体内容：
1. input script:
   - PUSHDATA(Sig)
   - PUSHDATA(PubKey)
2. output script:
   - DUP: 复制一份栈顶的元素并压入栈
   - HASH160: 弹出栈顶的元素，hash后压入栈 
   - PUSHDATA(PubKeyHash)
   - EQUALVERIFY：弹出栈顶的两个元素，比较是否相等
   - CHECKSIG：弹出栈顶的两个元素，用公钥检查签名是否正确
 
我们这里用栈来描述执行过程，上面的input script 和 output script依次入栈：

```txt
stack                                                                                      
    │                │         │                 │            │                    │  
    │                │         │                 │            │                    │  
    │                │ DUP     │ PUSHDATA(PubKey)│ HASH160    │PUSHDATA(PubKeyHash)│  
    │PUSHDATA(PubKey)│────────►│ PUSHDATA(PubKey)│──────────► │PUSHDATA(PubKey)    │  
    │PUSHDATA(Sig)   │         │ PUSHDATA(Sig)   │            │PUSHDATA(Sig)       │  
    └────────────────┘         └─────────────────┘            └────────┬───────────┘  
                                                                       │              
                                                                       │              
                                                                       ▼              
                                                                                      
                              │               │               │                    │  
                     CHECKSIG │               │   EQUALVERIFY │PUSHDATA(PubKeyHash)│  
   │              │◄───────── │               │  ◄─────────── │PUSHDATA(PubKeyHash)│  
   │              │           │PUSHDATA(PubKey│               │PUSHDATA(PubKey)    │  
   │    TRUE      │           │PUSHDATA(Sig)  │               │PUSHDATA(Sig)       │  
   └──────────────┘           └───────────────┘               └────────────────────┘  
                                                                                      
```
如果是合法的交易，最后栈顶留下的就是`TRUE`。如果任何一个环境发生错误，交易就是非法的。
### P2SH（Pay to Script Hash）
这个很复杂，后续再具体研究，但用作多重签名很好使。

## 多重签名
现在的多重签名，大多都采用P2SH的形式

## Proof of Burn
一种特殊的脚本。
脚本内容开头是`return`，这种形式的output被称为`Provably Unspendable/Prunable Outputs`。

假如有一个交易的`input`指向这个`output`，不论`input`里的`input script`如何设计，执行到`return`命令后都会直接返回false，不会执行`return`后面的命令。
所以这个output无法再被花出去，其对应的`UTXO`也无需再保存。

换句话说，当你想把某个地址的coin给销毁掉时，就可以使用这个脚本。

销毁的场景一般是：
1. 部分小币种要求销毁才能得到该小币种
2. 有人想利用该特性往区块链中写入永久保存的内容作为纪念或者其他用途：因为`input scipt`就可以随便写内容，反正最后不会执行。

当然你如果只是想写某些内容存储在区块链上，那你发布交易时也可以将该地址的余额作为交易费全部给矿工，一样能消除`UTXO`中的数据实现销毁的目的。

实际的脚本中，都需要加上`OP`前缀，这里只是为了学习方便刻意去掉的。
所以为什么`return`一般叫做`OP_RETURN`。




