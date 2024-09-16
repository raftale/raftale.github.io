---
title: "nestjs-introduction"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
- nodejs
- nestjs
---

# nodejs是什么？
1. nodejs不是一门编程语言
2. nodejs不是框架

nodejs是一门基于Google V8引擎的JavaScript运行环境，支持运行js，可以理解为JVM支持运行java吧，运行在服务端，可以跨平台。

## 如何学习
- 跟着官方文档学: https://docs.nestjs.com/controllers
- 配合文档练习：https://github.com/raftale/nestjs-practice

## Controller
Controllers are responsible for handling incoming requests and returning responses to the client.

## Module

### controllers
在该模块中定义的必须实例化的控制器集

### providers

这些提供器将被 Nest 注入器实例化，并且至少可以在整个模块中共享。
provider 类似于spring中的bean。

### imports
导入此模块中所需的提供程序的导入模块列表。


### exports
```ts
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
  exports: [CatsService],
})
export class CatsModule {}
```

如果要实现CatsService的实例共享，`exports: [CatsService],` 可以保证任何导入CatsModule都可以共享
`CatsService`。


### 动态模块
Nest 模块系统包含一个强大的特性，称为动态模块。 这个特性使您能够轻松地创建可定制的模块，这些模块可以动态地注册和配置提供器。

## mongoose
