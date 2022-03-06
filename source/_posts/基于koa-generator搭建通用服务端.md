---
title: 基于koa-generator搭建通用服务端
date: 2022-03-05 10:51:22
updated: 2022-03-06 12:20
categories: 实战
tags: ['实战','koa']
feature: true
---

## 安装koa-generator

### 全局安装koa-generator

```shell
npm i koa-generator -g
```

#### 初始化项目

```shell
koa2 goudong-server 
```

### 进入并安装依赖

```shell
cd goudong-server 
npm install
```



## 改造项目环境

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

- 在`src`目录下创建四个目录`controller` 、`db` 、`middleware`、`models`





## 实现登录功能

### 配置开发环境

#### 安装`koa-generic-session`依赖

```shell
ni koa-generic-session 
```

#### 使用

```js
const session = require('koa-generic-session')

//session配置
app.keys = ['liyunfuAAA'] //密钥用于加密
app.use(session({
  //配置cookie
  cookie: {
    path: '/',
    httpOnly: true,
    maxAge: 24 * 60 * 60 * 1000
  }
}))
```



### 跨域

#### 安装`koa2-cors`依赖

```shell
ni koa2-cors
```

#### 使用

```js
const cors = require('koa2-cors')

//cors配置
app.use(cors({
  origin: 'http://localhost:8080',  //前端origin
  credentials: true //允许跨域带cookie
}))
```

### 连接数据库

#### 安装`mongoose`

```shell
ni mongoose
```

#### 在`db`目录下新建`db.js`

```js
/**
 * @description mongoose 连接数据库
 * @author liyunfu
 */
const mongoose = require('mongoose')

const DB_URL = 'mongodb://root:example@47.99.147.11.27017/jingdong?authSource=admin'

// 开始连接
mongoose.connect(DB_URL, {
  useNewUrlParser: true,
  useUnifiedTopology: true
})

// 连接对象
const db = mongoose.connection

db.on('error', err => {
  console.error('mongoose connect error', err)
})
db.once('open', () => {
  console.log('mongoose 连接成功')
})

module.exports = mongoose
```



## 设计Schema和Model

- 在`models`目录下新建`User.js`

  ```js
  /**
   * @description user Model
   * @author liyunfu
   */
  
  const mongoose = require('../db/db')
  
  const Schema = mongoose.Schema({
    username: {
      type: String,
      require: true,
      unique: true
    },
    password: String
  }, { timestamps: true })
  
  const User = mongoose.model('user', Schema)
  
  module.exports = User
  ```

- 在`models`下新建`Address.js`

  ```js
  /**
   * @description Address Model
   * @author liyunfu
   */
  
  const mongoose = require('../db/db')
  
  const Schema = mongoose.Schema({
    username: {
      type: String,
      require: true
    },
    city: String,
    department: String,
    houseNumber: String,
    name: String,
    phone: String
  }, { timeStamps: true })
  
  const Address = mongoose.model('address', Schema)
  
  module.exports = Address
  ```

- 在`models`下新建`Shop.js`

  ```js
  /**
   * @description Shop Model
   * @author liyunfu
   */
  
  const mongoose = require('../db/db')
  
  const Schema = mongoose.Schema({
    name: String,
    imgUrl: String,
    sales: Number,
    expressLimit: {
      type: Number,
      default: 0
    },
    expressPrice: Number,
    slogan: String
  }, { timeStamps: true })
  
  const Shop = mongoose.model('shop', Schema)
  
  module.exports = Shop
  ```

- 在`models`下新建`Product.js`

  ```js
  /**
   * @description Product Model
   * @author liyunfu
   */
  
  const mongoose = require('../db/db')
  
  const Schema = mongoose.Schema({
    ShopId: {
      type: String,
      require: true
    },
    name: String,
    imgUrl: String,
    sales: Number,
    price: Number,
    oldPrice: Number,
    tabs: [String]  //示例 tabs:['all','seckill']
  }, { timestamps: true })
  
  const Product = mongoose.model('product', Schema)
  
  module.exports = Product
  ```
  
- 在`models`下新建`Order.js` 

  ```js
  /**
   * @description Order Model
   * @author liyunfu
   */
  
  const mongoose = require('../db/db')
  
  const Schema = mongoose.Schema({
    username: {
      type: String,
      require: true
    },
    shopId: String,
    shopName: String,
  
    idCanceled: {
      type: Boolean,
      default: false
    },
    address: {
      username: String,
      city: String,
      department: String,
      houseNumber: String,
      name: String,
      phone: String
    },
    products: [
      {
        product: {
          shopId: {
            type: String,
            require: true
          },
          name: String,
          imgUrl: String,
          sales: Number,
          price: Number,
          oldPrice: Number,
          tabs: [String]
        },
        orderSales: Number
      }
    ]
  }, { timestamps: true })
  
  const Order = mongoose.model('order', Schema)
  
  module.exports = Order
  ```

- 在`models` 下新建`index.js` 

  ```js
  /**
   * @description Model 入口文件
   * @author liyunfu
   */
  const Address = require('./Address')
  const Order = require('./Order')
  const Product = require('./Product')
  const Shop = require('./Shop')
  const User = require("./User")
  
  module.exports = {
    Address,
    Order,
    Product,
    Shop,
    User
  }
  ```



## 标准化请求成功与失败的响应信息

- 在`res-model` 下新建`ErrorModel.js`

  ```js
  /**
   * @description 错误返回的数据结构
   * @author liyunfu
   */
  
  class ErrorModel {
    constructor(errno = -1, message = 'error') {
      this.errno = errno
      this.message = message
    }
  }
  
  module.exports = ErrorModel
  ```

- 在`res-model` 下新建`SuccessModel.js`

  ```js
  /**
   * @description 成功返回的数据类型
   * @author liyunfu
   */
  
  class SuccessModel {
    constructor(data) {
      this.errno = 0
      if (data !== null) {
        this.data = data
      }
    }
  }
  
  module.exports = SuccessModel
  ```

- 在`res-model` 下新建入口文件`index.js`

  ```js
  /**
   * @description 返回数据类型 入口文件
   * @author liyunfu
   */
  const SuccessModel = require('./SuccessModel')
  const ErrorModel = require('./ErrorModel')
  
  module.exports = { SuccessModel, ErrorModel }
  ```



## 编写登录验证中间件

- 在`middleware` 下新建`loginCheck.js`

  ```js
  /**
   * @description 登录验证中间件
   * @author liyunfu
   */
  
  const { ErrorModel } = require('../res-model/index')
  
  module.exports = async (ctx, next) => {
    const session = ctx.session
  
    if (session && session.userInfo) {
      await next()
      return
    }
    ctx.body = new ErrorModel(10003, '中间件登录验证失败')
  }
  ```



## 用户操作接口

- 在`controller` 下新建 `user.js`

  ```js
  /**
   * @description user controller
   * @author liyunfu
   */
  
  const { User } = require('../models/index')
  
  /**
   * 注册方法
   * @param {Object} userInfo 用户信息
   * @returns 
   */
  async function register(userInfo = {}) {
    // 注意验证一下username unique
    const newUser = await User.create(userInfo)
    return newUser
  }
  
  async function login(username, password) {
    const user = await User.findOne({ username, password })
    if (user != null) {
      // 登录成功
      return true
    }
    return false
  }
  
  module.exports = {
    register, login
  }
  ```

- 在`routes` 下新建`users.js`

  ```js
  const router = require('koa-router')()
  
  
  const { register, login } = require('../controller/user')
  const { SuccessModel, ErrorModel } = require('../res-model/index')
  const loginCheck = require('../middleware/loginCheck')
  
  router.prefix('/api/user')
  
  // 注册
  router.post('/register', async function (ctx, next) {
    const userInfo = ctx.request.body
    try {
      await register(userInfo)
      // 返回成功
      ctx.body = new SuccessModel()
    } catch (ex) {
      console.log(ex)
      // 返回失败
      ctx.body = new ErrorModel(10001, `注册失败 - ${ex.message}`)
    }
  })
  
  // 登录
  router.post('/login', async (ctx, next) => {
    const { username, password } = ctx.request.body
    // 查询单个用户
    const res = await login(username, password)
  
    if (res) {
      // 登录成功
      ctx.session.userInfo = { username }  //设置session
  
      ctx.body = new SuccessModel()
    } else {
      ctx.body = new ErrorModel(10002, `登录验证失败`)
    }
  })
  
  router.get('/info', loginCheck, async function (ctx, next) {
    // 加了loginCheck之后，因为保证了必须登录
    const session = ctx.session
    ctx.body = new SuccessModel(session.userInfo)
  })
  module.exports = router
  ```



## 地址操作接口

- 在`controller` 下新建`address.js`

  ```js
  /**
   * @description address controller
   * @author liyunfu
   */
  
  const { Address } = require('../models/index')
  
  /**
   * 创建地址 
   * @param {string} username 用户名
   * @param {Object} data 地址的详细信息
   * @returns 
   */
  async function createAddress(username, data) {
    const address = await Address.create({ username, ...data })
  
    return address
  }
  
  /**
   * 获取地址列表
   * @param {string} username 用户名
   * @returns 
   */
  async function getAddressList(username) {
    const list = await Address.find({ username }).sort({ updatedAt: -1 })
    return list
  }
  
  /**
   * 获取单个收获地址
   * @param {string} id id
   * @returns 
   */
  async function getAddressById(id) {
    const address = await Address.findById(id)
    return address
  }
  
  async function updateAddress(id, username, data) {
    const address = await Address.findOneAndUpdate(
      {
        // 查询条件
        _id: id,
        username,
      },
      {
        username, ...data
      },
      {
        new: true  //返回更新之后的最新数据，默认时false，返回更新之前的数据
      }
    )
    return address
  }
  
  module.exports = {
    createAddress,
    getAddressList,
    getAddressById,
    updateAddress
  }
  ```

- 在`routes` 下新建`address.js`

  ```js
  /**
   * @description address router
   * @author liyunfu
   */
  
  const router = require('koa-router')()
  const { createAddress, getAddressList, getAddressById, updateAddress } = require('../controller/address')
  const { SuccessModel, ErrorModel } = require('../res-model/index')
  const loginCheck = require('../middleware/loginCheck')
  
  router.prefix('/api/user/address')
  
  // 创建收货地址
  router.post('/', loginCheck, async (ctx, next) => {
    // 获取用户信息
    const userInfo = ctx.session.userInfo
    const username = userInfo.username
    const data = ctx.request.body
  
    // 创建数据
    try {
      const newAddress = await createAddress(username, data)
      ctx.body = new SuccessModel(newAddress)
    } catch (error) {
      console.log(error)
      ctx.body = new ErrorModel(10004, '创建收货地址失败')
    }
  })
  
  // 获取收货地址列表
  router.get('/', loginCheck, async (ctx, next) => {
    const userInfo = ctx.session.userInfo
    const username = userInfo.username
  
    // 获取列表
    const list = await getAddressList(username)
    ctx.body = new SuccessModel(list)
  })
  
  // 获取单个收获地址
  router.get('/:id', loginCheck, async (ctx, next) => {
    const id = ctx.params.id
    const address = await getAddressById(id)
  
    ctx.body = new SuccessModel(address)
  })
  
  // 更新收货地址
  router.patch('/:id', loginCheck, async (ctx, next) => {
    const id = ctx.params.id
    const data = ctx.request.body
    const userInfo = ctx.session.userInfo
    const username = userInfo.username
    // 更新
    const newAddress = await updateAddress(id, username, data)
    ctx.body = new SuccessModel(newAddress)
  })
  
  module.exports = router
  ```

## 商店商品接口

- 在`controller` 下新建`shop.js`

  ```js
  /**
   * @description shop controller
   * @author liyunfu
   */
  
  const { } = require('../models/index')
  
  // 热门商店列表
  async function getHotList() {
    const list = await Shop.find().sort({ _id: -1 }) //逆序
    return list
  }
  
  // 根据id获取单个商店信息
  async function getShopInfo(id) {
    const shop = await Shop.findById(id)
    return shop
  }
  
  // 根据商店id获取商品
  async function getProductByShopId(id, tab = '') {
    const pList = await Product.find({
      shopId: id,
      tabs: {
        $in: tab  //匹配tabs
      }
    }).sort({ _id: -1 })  //逆序
    return pList
  }
  
  module.exports = {
    getHotList,
    getShopInfo,
    getProductByShopId
  }
  ```

- 在`routes`下新建`shop.js`

  ```js
  const router = require('koa-router')()
  
  const { SuccessModel } = require('../res-model/SuccessModel')
  
  const {
    getHotList,
    getShopInfo,
    getProductByShopId
  } = require('../controller/shop')
  
  router.prefix('/api/shop')
  
  // 热门商店（首页商店列表）
  router.get('/hot-list', async function (ctx, next) {
    const list = await getHotList()
    ctx.body = new SuccessModel(list)
  })
  
  // 根据 id 查询单个商店信息
  router.get('/:id', async function (ctx, next) {
    const id = ctx.params.id  //商店id
    const shop = await getShopInfo(id)
    ctx.body = new SuccessModel(shop)
  })
  
  router.get('/:id/product', async function (ctx, next) {
    const id = ctx.params.id
    const tab = ctx.query.tab || 'all'
    const products = await getProductByShopId(id, tab)
    ctx.body = new SuccessModel(products)
  })
  ```



## 订单接口

- 在`controller` 下新建`order.js`

  ```js
  /**
   * @description order controller
   * @author liyunfu
   */
  
  const { Order, Product, Address } = require('../models/index')
  
  // 创建订单(要从Address，Product里拷贝数据，比较麻烦)
  async function createOrder(username, data = {}) {
    console.log(username, data)
    // 结构data(前端传来的订单信息)
    const {
      addressId,
      shopId,
      shopName,
      isCanceled = false,
      products = []
    } = data
  
    // 根据addressId获取地址信息
    const address = await Address.findById(addressId)
  
    // 获取商品列表
    const pIds = products.map(p => p.id)
    const productList = await Product.find({
      // 条件1：商品id
      _id: {
        $in: pIds
      },
      // 条件2：商店id
      shopId
    })
  
    // 给商品列表增加销售数量(订单里，每个商品都有销量)
    const productListWithSales = productList.map(p => {
      // 商品id
      const id = p._id.toString()
  
      // 找到商品销量
      const filterProducts = products.filter(item => item.id === id)
      if (filterProducts.length === 0) {
        // 没有找到匹配的数量 报错
        throw new Error('未找到匹配的销量数据')
      }
  
      return {
        orderSales: filterProducts[0].num,
        product: p
      }
    })
  
    // 创建订单
    const newOrder = await Order.create({
      username,
      address,
      shopId,
      shopName,
      isCanceled,
      products: productListWithSales
    })
    return newOrder
  }
  
  // 获取订单列表
  async function getOrderList(username) {
    console.log('username', username)
    const list = await Order.find({ username }).sort({ _id: -1 })
    console.log('list', 'list')
    return list
  }
  
  module.exports = {
    createOrder,
    getOrderList
  }
  ```

- 在`routes` 下新建`order.js`

  ```js
  const router = require('koa-router')()
  
  const { SuccessModel, ErrorModel } = require('../res-model/index')
  const loginCheck = require('../middleware/loginCheck')
  const { createOrder, getOrderList } = require('../controller/order')
  
  router.prefix('/api/order')
  
  // 创建订单
  router.post('/', loginCheck, async function (ctx, next) {
    // 有登录验证 可以直接获取session
    const userInfo = ctx.session.userInfo
    const username = userInfo.username
  
    // 订单数据
    const data = ctx.request.body
  
    try {
      const newOrder = await createOrder(username, data)
      ctx.body = new SuccessModel(newOrder)
    } catch (ex) {
      console.error(ex)
      ctx.body = new ErrorModel(10005, '订单创建失败')
    }
  })
  
  // 获取订单列表
  router.get('/', loginCheck, async function (ctx, next) {
    // 有登录验证，可以直接获取session
    const userInfo = ctx.session.userInfo
    const username = userInfo.username
  
    const list = await getOrderList(username)
  
    ctx.body = new SuccessModel(list)
  })
  
  module.exports = router
  ```



## 改为适合部署到Vercel的项目

1. 在根目录下新建`vercel.json`

   ```json
   {
     "version": 2,
     "builds": [
       {
         "src": "./index.js",
         "use": "@vercel/node"
       }
     ],
     "routes": [
       {
         "src": "/(.*)",
         "dest": "/"
       }
     ]
   }
   ```

2. 在根目录下新建`index.js` 将原`bin/www`内容移到此处

3. 修改`package.json`

   ```json
   "start": "node index.js",
   "dev": "./node_modules/.bin/nodemon index.js",
   "prd": "pm2 start index.js",
   ```


## 改造项目，实现跨域

1. 修改根目录下`vercel.json`

   ```json
   {
     "version": 2,
     "builds": [
       {
         "src": "./index.js",
         "use": "@vercel/node"
       }
     ],
     "routes": [
       {
         "src": "/(.*)",
         "dest": "/",
         "headers": {
           "Access-Control-Allow-Credentials": "true",
           "Access-Control-Allow-Origin": "*",
           "Access-Control-Allow-Methods": "GET,OPTIONS,PATCH,DELETE,POST,PUT",
           "Access-Control-Allow-Headers": "X-CSRF-Token, X-Requested-With, Accept, Accept-Version, Content-Length, Content-MD5, Content-Type, Date, X-Api-Version"
         }
       }
     ]
   }
   ```

   

​      

   
