---
title: "react - useContext"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
- react
---

## Passing Data Deeply with Context

React的组件渲染是一个多叉树，如果你的信息是从父组件经过中间组件传递到某个组件，最直接的是传递`props`。

但`props`有时候会很麻烦。

`context`允许父组件向下传递无论多深的节点，都不需要通过`props`来进行显式传递。

### 如何使用


1. 创建context：
```js
// LevelContext.js

import { createContext } from 'react';
export const LevelContext = createContext(1);
```
`createContext`接受任何参数类型，这里的`1`是初始默认参数，如果没有提供上层`context`，那使用的时候就会是这个默认值。

2. 使用context
```js
import { useContext } from 'react';
import { LevelContext } from './LevelContext.js';

export default function Heading({ children }) {
  const level = useContext(LevelContext);
  // ...
}
```
3. 提供context
```js
import { LevelContext } from './LevelContext.js';

export default function Section({ level, children }) {
  return (
    <section className="section">
      <LevelContext.Provider value={level}>
        {children}
      </LevelContext.Provider>
    </section>
  );
}
```
这里只简单讲了一些语法，只看这部分代码会比较困惑，最好看下完整的代码和文档。

[完整的代码在这](https://github.com/raftale/react-practice/blob/main/src/react-dev-demo/use-context/HeadPage.jsx)


### 使用场景
使用`props`都好处是数据传输比较清晰，context会隐藏一些细节，所以需要注意使用的场景。

1. 主题：应用的外观，在应用的顶层设置`context provider`，就可以自由的应用在各个子页面中；
2. 当前账户：方便各个页面能方便的获取用户登陆信息
3. 路由：
4. 状态管理：下层的页面可以更方便的修改顶层的state
