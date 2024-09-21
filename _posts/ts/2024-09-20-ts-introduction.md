---
title: "TypeScript - 基础"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
  - ts
---

## TypeScript
`TypeScript`是JavaScript的一个超集，主要提供了类型系统和对ES6对支持。

## 类型系统
JavaScript是一种动态类型语言，变量类型是在运行时才确定的，可能导致运行时错误。
TypeScript是一种静态类型语言，编译期间就能发现类型相关的错误，使得代码更具有可维护性。

## 原始数据类型

1. boolean: `let isDone: boolean = false`
2. number: `let num: number = 5`
3. string: `let name: string = 'tom'`
4. void: 函数返回类型为void表示没有任何返回值，变量为void表示只能为`undefined`和`null`.
   - `let x: void = undefined`
5. null/undefined: 它们是所有类型的字类型，意味着它们可以赋值给其他类型。
   - `let num: number = undefined;`

## Any
Any用来表示允许赋值为任意类型。
声明一个值为Any后，对它的任何操作，返回的内容的类型都是任意值。

## 类型推断
1. 如果定义变量的时候有赋值，但没有明确的指定类型，那么`TypeScript`会自动推断类型。
2. 如果没有赋值，也没有指定类型，那么会被推断成Any

## Union Types

```ts
// 允许 myFavoriteNumber 的类型是 string 或者 number，但是不能是其他类型。
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

当不确定是Union Types中的哪一种，只能访问共有的属性和方法。

## interfaces
```ts
interface Person {
  readonly id: number;
  name: string;
  age?: number;
}
```

1. readonly: 不可变，初次赋值后只读
2. ?: 可选属性，实现类可以不定义该属性
3. 实现类不能定义除interface外的其他属性，少一些属性也不行(可选属性除外)






