---
title: "React Quick Start: Thinking in React"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
  - react
---


## Step 1: Break the UI into a component hierarchy

一个页面可以拆分成多个`component`，正确的拆分是良好设计的开端。

![product-outline.png](https://react.dev/images/docs/s_thinking-in-react_ui_outline.png)

上图中就是一个良好的层级拆分，为了可维护性，一个component应该保持单一职责的原则。

## Step 2: Build a static version in React 
实现代码的第一步首先是用静态数据 先构建一个静态的版本，然后再在上面添加交互。

大型项目自底向上构建更简单，一般的项目直接从顶向下构建更方便。

## Step 3: Find the minimal but complete representation of UI state
交互涉及到数据模型的变更，react是通过state来改变。

state要尽可能的减少冗余，用最少的state实现交互。举个例子就是如果你要展示一个list中元素的个数，不需要再多维护一个数目，而是用list.length来表示。

## Step 4: Identify where your state should live 

React是使用单向数据流，将数据从父组件传递到子组件，沿着组件层次结构向下传递。

如何判断state放到哪个component？

首先是找到哪个组件需要这个state，如果这个state不需要被共享，那就放在这个`component`中；但如果state被多个component使用，就将state放到最近的公共父component中，然后父component将其作为参数传递给子component。

## Step 5: Add inverse data flow 

因为state是存在父component中，也只有父component可以改变state，所以setState也是需要作为参数传递给子component，然后由子component来触发数据变更进行渲染。



