---
title: 基于koa-generator实现验证码功能
date: 2022-03-05 10:56:54
categories: 实战
tags: ['Koa']
---

### 第一步

- 全局安装`koa-generator`

- ```shell
  npm install -g koa-generator
  ```

- 生成基本框架

- ```shell
  koa2 apiname    
  # 使用koa2后面接目录名即可自动创建名为apiname的目录
  ```

- 根据命令行提示，进入创建的文件夹，执行`npm install`

- ```shell
  cd apiname
  npm install
  ```

- 下载依赖完成之后，可以启动看看

- ```shell
  npm run start
  ```

- 打开浏览器访问`3000`端口

- ```shell
  http://localhost:3000
  ```

- 如果浏览器中页面显示出数据即创建成功，可以进入下一步



### 第二步

- 在根目录下新建`src`目录

- 将`public` 、`routes` 、`views` 、`app.js`拖入`src`目录

- 修改`bin/www`中的`var app = require('../app');`为`var app = require('../src/app');`

- 改造后目录

- ```shell
  |- bin
    |- www
  |-node_modules
  |-src
    |-public
    |-routes
    |-views
    |-app.js
  |-package.json
  ```

- 在`src`目录下创建目录`controller`



### 第三步

- 安装用于聚合router的包

- 安装`koa-combine-routers`包

- ```shell
  npm install koa-combine-routers
  ```

- 安装`svg-captcha`  包 用于生成`svg`验证码

- ```shell
  npm install svg-captcha
  ```

- 在`controller`目录下创建`publicController.js` 用于生成验证码 并输入以下代码

- ```js
  const svgCaptcha = require("svg-captcha");
  
  function  publicController(ctx) {
    //创建一个新验证码svg对象 
    const newCaptcha = svgCaptcha.create({
      size: 4,  //验证码长度
      ignoreChars: "0o1il", //排除易混淆的几个0o1il字符
      color: true,   //验证码有颜色
      noise: Math.floor(Math.random() * 5), //干扰线
      width: 150,  
      height: 50,
    });
    ctx.body = {
      msg: newCaptcha.data,
    };
  }
  
  module.exports = publicController;
  ```

- 在`routes`目录下新建`publicRouter.js ` 输入以下代码

- ```js
  const router = require("koa-router")();
  const getCaptcha = require("../controller/PublicController");
  
  router.get("/getCaptcha", getCaptcha);
  
  module.exports = router;
  ```

- 在`routes`目录下新建`routes.js` 输入以下代码

- ```js
  const combineRoutes=require('koa-combine-routers')
  
  const aRoutes=require('./publicRouter')
  
  module.exports=combineRoutes(
    aRoutes   //如果有多个 可以引入多个，并写在此处用逗号隔开
  )
  ```

- 在`app.js`引入`routes.js` 并使用

- ```js
  const router = require("./routes/routes");
  app.use(router())
  ```

- 实现跨域请求，下载并引入`koa2-cors`包

- ```shell
  npm install koa2-cors
  
  const cors = require("koa2-cors");
  
  //cors配置
  app.use(
    cors({
      origin: "http://localhost:8080", //前端origin
      credentials: true, //允许跨域带cookie
    })
  );
  ```



### 第四步

使用`vue`项目，尝试请求验证码  下载`axios`包

```vue
<template>
	<div id="app">
    <div class="svg" @click="getCaptcha" v-html="svg">验证码</div>
  </div>
</template>

<script>
const axios = require('axios')
export default {
  name: 'app',
  data () {
    return {
      svg: ''
    }
  },
  mounted () {
    this.getCaptcha()
  },
  methods: {
    getCaptcha () {
      axios.get('http://localhost:3000/getCaptcha').then((res) => {
        if (res.status === 200) {
          this.svg = res.data.msg
        }
      })
    }
  }
}
</script>
```

![image-20220111225233945](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/202201112252049.png)
