---
title: "react - useState"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
- react
---

[toc]

## Reacting to Input with State

很基本的思想吧，总结起来就是：

1. 定位组件中不同的视图状态；
2. 确定是什么触发了这些状态的改变
3. 通过useState表示内存中的state
4. 删除不必要的state变量
5. 连接事件处理函数设置state

## Choosing the State Structure

构建state的原则：

1. 合并关联的state
2. 避免相互矛盾
3. 避免冗余
4. 避免重复
5. 避免深度嵌套

## share states between components

主要还是讲的将状态提升到父组件。

## Preserving and Resetting State

state并不保存子component中，react是将state和component映射起来。

当react在移除一个组件时，也会消除它的state。重新渲染的时候state也会重新初始化。



### 相同位置的相同组件会使得state被保留下来。

```javascript
import { useState } from 'react';

export default function App() {
  const [isFancy, setIsFancy] = useState(false);
  return (
    <div>
      {isFancy ? (
        <Counter isFancy={true} /> 
      ) : (
        <Counter isFancy={false} /> 
      )}
      <label>
        <input
          type="checkbox"
          checked={isFancy}
          onChange={e => {
            setIsFancy(e.target.checked)
          }}
        />
        使用好看的样式
      </label>
    </div>
  );
}
```

处于相同位置的counter，内部有状态，isFancy变化时其内部状态仍然保持不变。



### Different components at the same position reset state

如题所示。

### Resetting state at the same position

默认情况下，React 会在一个组件保持在同一位置时保留它的 state，但有时候，你可能想要重置一个组件的 state 并且保持在同一个位置。这种情况有两种选择：

1. 同一个component放到不同的位置，并用同一个变量的true false来控制显示；

```javascript
export default function Scoreboard() {
  const [isPlayerA, setIsPlayerA] = useState(true);
  return (
    <div>
      {isPlayerA &&
        <Counter person="Taylor" />
      }
      {!isPlayerA &&
        <Counter person="Sarah" />
      }
      <button onClick={() => {
        setIsPlayerA(!isPlayerA);
      }}>
        下一位玩家！
      </button>
    </div>
  );
}
```

1. 使用key来重置state：任何组件都有key，不同的key会被react认为是不同的组件。

```javascript
export default function Scoreboard() {
  const [isPlayerA, setIsPlayerA] = useState(true);
  return (
    <div>
      {isPlayerA ? (
        <Counter key="Taylor" person="Taylor" />
      ) : (
        <Counter key="Sarah" person="Sarah" />
      )}
      <button onClick={() => {
        setIsPlayerA(!isPlayerA);
      }}>
        下一位玩家！
      </button>
    </div>
  );
}
```


