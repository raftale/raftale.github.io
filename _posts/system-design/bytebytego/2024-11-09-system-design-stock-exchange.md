---
title: "System design: stock exchange"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
  - system design
---

target: stock exchange system

## Step 1 - Understand the Problem and Establish Design scope

### functional requirements
- trade: only stocks
- order type: limit order 
- normal trading hours
- basic function: 
  - place new limit orders
  - cancel an order
  - receive matched trades in real time
  - view the real-time order book 
  - support at least tens of thousands of users trading at the same time.
  - at least support 100 symbols
  - trading volume: support billions of orders per day
  - risk checks: like a user can only trade a maximum of 1 million shares of Apple stock in one day

### non-functional requirements

1. Availability: at least 99.99%. 
2. Fault tolerance: a fast recovery mechanism
3. Latency: The round-trip latency should be at the millisecond level
4. Security: account management and KYC and prevent DDoS

### back-of-the-envelope estimation

1. 100 symbols
2. 1 billion orders a day
3. 6.5 hours in total a day.
4. QPS: 1 billion / 6.5 / 3600 = ~43,000
5. Peak QPS: 5 * QPS = 215,000.

## Step 2 - Propose High-Level Design and Get Buy-In

### business knowledge 101

1. broker: like Robinhood
2. bid price: the highest price a buyer is willing to pay for a stock
3. ask price: the lowest price a seller is willing to sell the stock

### High level design
![high-level-design.png](/img/system-design/high-level-design.png)


## Step 3 - Design Deep Dive

1. trading flow:
   - matching engine
   - sequencer
   - order manager
   - client gateway
2. Market data flow
3. Reporting flow

### API Design

### Data models

#### production, order, execution


#### order book

The following code snippet shows an implementation of the order book.

```java
class PriceLevel{
    private Price limitPrice;
    private long totalVolume;
    private LinkedList<Order> orders;
}
class Book<Side> {
    private Side side;
    private Map<Price, PriceLevel> limitMap;
}
class OrderBook {
    private Book<Buy> buyBook;
    private Book<Sell> sellBook;
    private PriceLevel bestBid;
    private PriceLevel bestOffer;
    private Map<OrderID, Order> orderMap;
}
```
比较核心的就是这个order book的设计。
1. orders的数据结构设计为双向链表。维护一个Map<orderId, Order>的结构，当删除一个order 的时候就可以直接定位到order，然后在双向链表中进行删除就是O(1)的时间复杂度。

![Place match and cancel an oder in O(1)](/img/system-design/Place%20match%20and%20cancel%20an%20oder%20in%20O(1).png)
