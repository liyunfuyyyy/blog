---
title: nestjs全解[windows版]
date: 2022-04-23 09:38:46
tags: ['服务端','数据库','nestjs','node']
categories: 服务端
---



## 前期准备

### 基于docker配置数据库环境

1. 进入docker官网下载docker desktop

   ```js
   https://www.docker.com/get-started/
   ```

2. 使用docker-compose简化运行方式

   由于docker desktop安装时已经自带docker-compose，我们只需要在`C:\Users\[yourname]\.docker`  C盘你的用户目录下的`.docker` 文件夹下添加`docker-compose.yml` 即可

   ```yaml
   version: "3.8"
   
   services:
     mysql:
       image: mysql:8.0.23
       command: --default-authentication-plugin=mysql_native_password
       restart: always
       environment:
         MYSQL_ROOT_PASSWORD: example
       ports:
         - 3306:3306
   
     postgres:
       image: postgres:13.1
       restart: always
       environment:
         POSTGRES_PASSWORD: example
       ports:
         - 5432:5432
   
     adminer:
       image: adminer
       restart: always
       ports:
         - 8080:8080
   
   ```

   其中`adminer` 为可视化数据库管理系统，当启动images时，可以在`localhost:8080` 访问页面，另外两个为数据库

3. 在`C:\Users\[yourname]\.docker` 当前目录下，执行`docker-compose up` 会自动拉取并执行images

   ```js
   docker-compose up
   ```



## Nest.js

### 认识Nest.js

#### 安装

1. 全局安装`@nestjs/cli` 工具，并新建项目`learn_nest` 

   ```shell
   npm i -g @nestjs/cli
   ```

   ```shell
   nest new learn_nest
   ```

2. 安装好之后的`src` 核心文件

   ```js
   src
   |-- app.controller.spec.ts
   |-- app.controller.ts
   |-- app.module.ts
   |-- app.service.ts
   |-- main.ts
   ```

3. 核心文件概述

   | 核心文件                 | 概述                         |
   | ------------------------ | ---------------------------- |
   | `app.controller.ts`      | 带有单个路由的基本控制器示例 |
   | `app.controller.spec.ts` | 对于基本控制器的单元测试样例 |
   | `app.module.ts`          | 应用程序的跟模块             |
   | `app.service.ts`         | 带有单个方法的基本服务       |
   | `main.ts`                | 应用程序入口文件             |

4. `main.ts` 认识

   ```typescript
   import { NestFactory } from '@nestjs/core';
   import { AppModule } from './app.module';
   
   async function bootstrap() {
     const app = await NestFactory.create(AppModule);
     await app.listen(3000);
   }
   bootstrap();
   
   // 使用NestFactory来创建Nest应用实例，并监听3000端口
   ```



#### 控制器`controller` 

![image-20220423101156577](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/202204231011661.png)

控制器负责处理传入的`请求` 和向客户端返回`响应` 

控制器的目的是接收应用的特定请求。`路由`机制控制哪个控制器接收哪些请求，通常，每个控制器有多个路由，不同的路由可以执行不同的操作。

为了创建一个基本的控制器，我们使用类和`装饰器` 。装饰器将类与所需的元数据相关联，并使Nest能够创建路由映射(将请求绑定到相应的控制器)

1. 路由示例

   ```typescript
   /* event.controller.ts */
   import {Controller,Get} from '@/nestjs/common'
   
   @Controller('event')
   export class EventController{
       @Get('/list')
       findAll():string{
           return 'this is event all'
       }
   }
   ```

   上面示例就可以在postman中请求url`localhost:3000/event/list` 即可得到返回结果

2. Request示例

   ```typescript
   /* event.controller.ts */
   import {Controller,Get} from '@/nestjs/common'
   import {Request} from 'express'
   
   @Controller('event')
   export class EventController{
       @Get('/list')
       findAll(@Req() request:Request):string{
           return 'this is event all'
       }
   }
   
   ```

   **常见底层装饰器对照表**

   | 装饰器                  | 对象                             |
   | ----------------------- | -------------------------------- |
   | `@Request() , @Req()`   | req                              |
   | `@Response() , @Res()`  | res                              |
   | `@Next()`               | next                             |
   | `@Session()`            | `req.session`                    |
   | `@Param(key?:string)`   | `req.params/req.params[key]`     |
   | `@Body(key?:string)`    | `req.body / req.body[key]`       |
   | `@Query(key?:string)`   | `req.query / req.query[key]`     |
   | `@Header(name?:string)` | `req.headers / req.headers[key]` |
   | `@Ip()`                 | `req.ip`                         |
   | `@HostParam()`          | `req.hosts`                      |

3. 资源路由`@Put()  @Delete() @Patch()  @Get()  @Post @All()`

4.  路由通配符 (路由同样支持模式匹配，例如星号被用作通配符)<img src="https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/202204231105247.png" alt="image-20220423110559162" style="zoom:67%;" />

5. 状态码

   ```typescript
   @Post()
   @HttpCode(204)
   create() {
     return 'This action adds a new cat';
   }
   ```

   返回状态码204

6. Headers

   可以指定返回头

   ```typescript
   @Post()
   @Header('Cache-Control', 'none')
   create() {
     return 'This action adds a new cat';
   }
   ```

7. 重定向

   `@Redirect()`装饰器有两个参数，`url` 和`statusCode` ，如果省略，则`statusCode` 默认为`302` 

   ```typescript
   @Get()
   @Redirect('https://nestjs.com', 301
   ```

   有时候想要动态指定重定向地址，可以返回一个标准的对象，重定向到v5版本

   ```typescript
   @Get('docs')
   @Redirect('https://docs.nestjs.com', 302)
   getDocs(@Query('version') version) {
     if (version && version === '5') {
       return { url: 'https://docs.nestjs.com/v5/' };
     }
   }
   ```

8. 路由参数

   当你需要接收`动态数据` 作为请求的一部分时，如`GET /event/1` 来获取id为1的event

   ```typescript
   @Get(':id')
   findOne(@Param() params):string{
       console.log(params.id)
       return `this is return ${params.id}`
   }
   ```

   或

   ```typescript
   @Get(':id')
   findOne(@Param('id'):id):string{
       return `this is return ${id}`
   }
   ```

9. 子域路由

   即要求传入的`HTTP` 主机匹配某个特定的值才会响应

   ```typescript
   @Controller({ host: ':account.example.com' })
   export class AccountController {
     @Get()
     getInfo(@HostParam('account') account: string) {
       return account;
     }
   ```

10. 请求负载(规定接收的客户端参数)

    创建`create-event.dto.ts` 

    ```typescript
    export class CreateEventDto {
      name: string;
      description: string;
      when: string;
      address: string;
    }
    
    ```

    在`event.controller.ts` 中使用

    ```typescript
    @Post()
    async create(@Body() input: CreateEventDto) {
      return await this.repository.save({
        ...input,
        when: new Date(input.when),
      });
    }
    ```

​	**最后一步，需要在App.module.ts中添加自定义的controller**

```typescript
import { Module } from '@nestjs/common';
import { CatsController } from './cats/cats.controller';

@Module({
  controllers: [EventController],
})
export class AppModule {}
```



#### 提供者Providers



#### 模块Module





#### 中间件 Middleware



#### 异常过滤



#### 管道pipes



#### 守卫



#### 拦截器





#### 自定义装饰器





### 基本原理





### 技术





### 安全







