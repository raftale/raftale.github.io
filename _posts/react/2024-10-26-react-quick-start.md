---
title: "React Quick Start"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
  - react
---

最近公司让写一些react代码，用了下cursor，真的是大为震撼，给原型图后就可以直接生成可以跑的代码。

虽然这样确实提高了效率，但是却对你的基础功有要求，它给的代码不一定100%能跑，也不一定就是最优的，所以能看的懂和理解本质才能更好的提高效率。

来复习一下React，跟着官网看看。

## JSX
JSX就是一些类似`<br />, <div></div>`之类的html markup syntax。

## Hooks


## re-renders

看一段重复渲染的代码：
```tsx
export default function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));

  function handleClick(i) {
    const nextSquares = squares.slice();
    nextSquares[i] = "X";
    setSquares(nextSquares);
  }

  return (
      <Square value={squares[0]} onSquareClick={ handleClick(0) } />
  )
}
```

`<Square value={squares[0]} onSquareClick={ handleClick(0) } />` 的handleClick(0)在页面渲染的时候就会执行。

然后`handleClick(0)` function 又会执行`setSquares`，造成re-renders的无限循环。

解决方法有两种：
1. 将`handleClick(0)`再包装到一个函数`handleFirstSquareClick`中，然后`onSquareClick={ handleFirstSquareClick }`；
2. 通过闭包将 `onSquareClick={() => handleClick(0)}` 传递给子组件。


## 不变性
上面的handleClick中，修改数组数据是创建了一个新的数组，而不是在原数组上更改。

这样有几个好处：
1. 保留原数据，可以实现回溯的功能；
2. 保持数据不变性，默认情况下，当父组件的state发生变化时，所有子组件都会自动重新渲染。不变性使得组件比较其数据是否已更改的成本非常低。
