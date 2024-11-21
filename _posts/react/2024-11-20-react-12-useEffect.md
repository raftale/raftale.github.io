---
title: "react-Scaling Up with Reducer and Context"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
- react
---

React中，渲染代码是纯粹的，没有副作用的。

副作用指的是一些不满足预期的后果。

React中，副作用通常属于事件处理程序。

常规的事件处理程序是用户触发的，但有一些事件处理程序需要由渲染自身触发，比如聊天室中建立服务器连接等。React中，Effect就是指这些由渲染引起的副作用。

## 如何编写Effect

1. 声明Effect

```jsx
import { useEffect } from 'react';

function MyComponent() {
  useEffect(() => {
    // 每次渲染后都会执行此处的代码
  });
  return <div />;
}
```

1. 指定Effect的依赖项

默认Effect会在每次渲染后运行，但有时可能只需要mount时或者某些state变化的时候才运行Effect。

```javascript
useEffect(() => {
  // 这里的代码会在每次渲染后运行
});

useEffect(() => {
  // 这里的代码只会在组件挂载（首次出现）时运行
}, []);

useEffect(() => {
  // 这里的代码不但会在组件挂载时运行，而且当 a 或 b 的值自上次渲染后发生变化后也会运行
}, [a, b]);
```

1. 按需添加cleanup函数

cleanup的使用场景是组件每次unmount的时候需要做一些清理的操作，比如聊天室关闭的时候需要断开服务器的连接。

```jsx
 useEffect(() => {
    const connection = createConnection();
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, []);
```

`return () => { connection.disconnect(); };`就是cleanup函数，它会在执行下一轮渲染的Effect之前清理上一轮渲染的Effect。
> 这个我感觉背后是queue

strict模式下React会故意将组件mount两次，这种情况下使用cleanup函数可以解决一些因为两次渲染造成的困惑。


## 不需要Effect的场景

常见的两种场景是不需要使用Effect的：

1. 转换渲染的数据
2. 处理用户事件

总结下来就是：
- 如果你可以在渲染期间计算某些内容，则不需要使用 Effect。
- 想要缓存昂贵的计算，请使用 `useMemo` 而不是 `useEffect`。
- 想要重置整个组件树的 state，请传入不同的 `key`。
- 想要在 prop 变化时重置某些特定的 state，请在渲染期间处理。
- 组件 **显示** 时就需要执行的代码应该放在`Effect`中，否则应该放在事件处理函数中。
- 如果你需要更新多个组件的 state，最好在单个事件处理函数中处理。
- 当你尝试在不同组件中同步 state 变量时，请考虑状态提升。
- 你可以使用`Effect`获取数据，但你需要实现清除逻辑以避免竞态条件。
- 如果必须使用useEffect调用接口，考虑到缓存，可以自定义一个hook，但一般的是使用一些框架自带的机制。‘

## Effect的生命周期
Effect是渲染之后执行的。

Effect与组件有不同的生命周期，组件可以挂载、更新或卸载。

但Effect只做两件事：开始同步某些东西，然后停止同步它。

```tsx
const serverUrl = 'https://localhost:1234';

function ChatRoom({ roomId }) {
  useEffect(() => {
    // 开始同步
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    // 停止同步
    return () => {
      connection.disconnect();
    };
  }, [roomId]);
  // ...
}
```

### 每个Effect是一个独立的同步过程

意味着在useEffect中的逻辑应该是单一职责的，否则会因为依赖性的变更引起混乱。

全局变量不能作为依赖项。
