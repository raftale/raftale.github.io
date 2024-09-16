---
title: "nestjs-JWT"
layout: post
author: "raftale"
header-style: text
hidden: false
published: true
tags:
- JWT
- nestjs
---

想写一下JWT的原理和在nestjs中的使用，不过没时间总结，先随便记点什么
## Reference

1. https://wdk-docs.github.io/nest-docs/security/authentication
2. https://docs.nestjs.com/guards

## JWT

![jwt](/img/nestjs/jwt-validation.jpg)

## Guards
`@nestjs/passport` 是最node.js中最流行的身份验证库。

`Guards`在客户端和路由模块之间，来决定一个请求是否会被路由处理器处理。

自定义`Guards`：扩展内置类`AuthGuard`并在子类中重写方法：

```ts
import {
  ExecutionContext,
  Injectable,
  UnauthorizedException,
} from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';

@Injectable()
export class JwtAuthGuard extends AuthGuard('jwt') {
  canActivate(context: ExecutionContext) {
    // Add your custom authentication logic here
    // for example, call super.logIn(request) to establish a session.
    return super.canActivate(context);
  }

  handleRequest(err, user, info) {
    // You can throw an exception based on either "info" or "err" arguments
    if (err || !user) {
      throw err || new UnauthorizedException();
    }
    return user;
  }

  
```

`AuthGuard`的入参是一个策略，它定义了实际的身份验证逻辑。



每个`Guard`都必须实现一个`canActivate`，这个函数返回值为boolean值，表示是否允许当前请求。

### ExecutionContext

`canActivate()` 函数只接受一个参数，即 `ExecutionContext` 实例。

`ExecutionContext`（执行上下文）是在NestJS中用于执行请求处理的上下文对象。它包含了与当前请求相关的一些信息和操作，包括请求对象、响应对象、路由处理方法等。

- getRequest(): 获取请求对象的引用。
- getResponse(): 获取响应对象的引用。
- getClass(): 获取当前处理方法所属的类。
- getHandler(): 获取当前处理方法的引用（即路由处理器）。
- switchToHttp(): 返回一个特定于HTTP的上下文，允许访问请求对象、响应对象和参数等。
- switchToRpc(): 返回一个特定于RPC的上下文，允许访问消息对象、连接和参数等。
- switchToWs(): 返回一个特定于WebSocket的上下文，允许访问连接对象、数据和参数等。


### JwtService
在NestJS中，jwtService是一个由Passport JWT策略提供的服务，用于处理JSON Web Tokens (JWT) 的生成和验证。

jwtService.verify是用于验证JWT令牌的方法，入参为token，出参为`{"username": "", "sub": "", "iat": 1726500940, exp: 1726587340}`。

`sub`为签发时间， `exp`为过期时间。


### @UseGuards()

`Guards`控制的范围可以是`controller, method, global`的。

`@UseGuards()` 用来应用各类`Guards`：

```ts
@Controller('cats')
@UseGuards(RolesGuard)
export class CatsController {}
```





