---
title: Webpack配置及打包vue
date: 2022-04-10 20:08:54
tags: ['工程化','vue']
categories: 知识点
---

1.  安装`webpack``webpack-cli`

```
npm install webpack webpack-cli -D
```

2.  配置入口出口

```js
// webpack.config.js
module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, './dist')
  }
}
```

3.  处理sass

1. 安装`sass``sass-loader``postcss-loader``css-loader``style-loader`

```shell
npm install sass sass-loader postcss-loader css-loader style-loader -D
```

2. 添加`postcss.config.js`

```js
module.exports = {
  plugins: [
    require('postcss-preset-env')
  ]
}
```

3. 添加rules

```js
module: {
  rules: [
    {
      test: /.(s[ac]ss|css)$/,
      use: [
        { loader: 'style-loader' },
        { loader: 'css-loader' },
        { loader: 'postcss-loader' },
        { loader: 'sass-loader' }
      ]
    },
  ]
}
```

4.  处理图片等文件资源和字体

-   v5已经可以试用`asset`替代`file-loader` `url-loader``raw-loader`了
-   添加rules

```js
  module: {
    rules: [
      {
        test: /.(png|svg|jpg|jpeg|git)$/i,
        type: "asset",
        generator: {
          filename: "img/[name].[hash:6][ext]"
        },
        parser: {
          dataUrlCondition: {
            maxSize: 100 * 1024
          }
        }
      },
      {
        test: /.(woff2?|eot|ttf)$/,
        type: "asset/resource",
        generator: {
          filename: "font/[name].[hash:6][ext]"
        }
      }
    ]
  },
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/318952a7e517412380390cac9adf8450~tplv-k3u1fbpfcp-zoom-1.image)

5.  自动清理dist目录

```js
npm install clean-webpack-plugin -D

const { CleanWebpackPlugin } = require('clean-webpack-plugin')

  plugins: [
    new CleanWebpackPlugin()
  ]
```

6.  打包html

```js
npm install html-webpack-plugin -D

const HtmlWebpackPlugin = require('html-webpack-plugin')
plugins:[
  new HtmlWebpackPlugin({
        title: 'webpack案例',
        template: './index.html'
  }),
]
```

7.  自定义模板需要数据填充，需要`defineplugin` 已内置

```
new DefinePlugin({
      BASE_URL: '"./"'
}),
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/36afeedcc9e841f985a788c40f0343d5~tplv-k3u1fbpfcp-zoom-1.image)

8.  自动复制public的内容到dist

```js
npm install copy-webpack-plugin -D

    new CopyWebpackPlugin({
      patterns: [
        {
          from: 'public',
          globOptions: {
            ignore: [
              '**/.DS_Store',
              '**/index.html'
            ]
          }
        }
      ]
    })
```

9.  支持ES6 安装babel

<!---->

1.  1.  安装babel

```js
npm install babel-loader @babel/core -D
```

1.  2.  安装预设

```js
npm install @babel/preset-env
```

1.  3.  新增rules

```js
{
  test: /.m?js$/,
  use: {
    loader: "babel-loader",
    options: {
      presets: [
        ["@babel/preset-env"]
      ]
    }
  }
}
```

1.  4.  也可以在`babel.config.js`配置预设

```js
module.exports = {
  presets: [
    ["@babel/preset-env"]
  ]
}
```

\


\


## 打包vue

\


1.  添加`@vue/compiler-sfc` `vue-loader`

```js
ni @vue/compiler-sfc vue-loader -D

const { VueLoaderPlugin } = require('vue-loader/dist/index')


{
  test: /.vue$/,
  loader: "vue-loader"
}

new VueLoaderPlugin()
```

2.  public/index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>hello</title>
  </head>
  <body>
    <div id="app"></div>
  </body>
</html>
```

3.  src/app.vue

```js
<template>
  <div>
    <h1>aoo</h1>
    <p>hello</p>
  </div>
</template>

<script>
export default {}
</script>

<style lang="scss" scoped></style>
```

4.  src/index.js

```js
import { createApp } from 'vue/dist/vue.esm-bundler'
import App from './App.vue'
import './style.scss'

// vue代码
createApp(App).mount('#app')
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9aa97e50e6bf477f96cda737d42ad215~tplv-k3u1fbpfcp-zoom-1.image)

\


## 搭建本地服务器

### webpack-dev-server

-   安装

```js
npm install webpack-dev-server -D
```

-   配置webpack.config.js

```js
devServer: {
    static: {
      directory: path.join(__dirname, './')
    },
    compress: true,
    hot: true,
    //host: '0.0.0.0',  // 表示在同一个网段下所有主机
    port: 8000,
    open: true,
    proxy: {
      "/api": {
        target: "http://localhost:8888",
        pathRewrite: {
          "^/api": ""
        },
        secure: false,
        changeOrigin: true   //修改源
      }
    }
  },
```

-   配置package.json

```js
"scripts": {
    "build": "webpack --watch",
    "serve": "webpack serve"
  },
```

-   使用nr serve启动\

-   开启HMR

<!---->

-   -   修改webpack配置

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bf048a58f23f4d6da13e0b4196ce57dc~tplv-k3u1fbpfcp-zoom-1.image)

#### 配置devServer

```js
 devServer: {
    static: {
      directory: path.join(__dirname, './')
    },
    compress: true,
    hot: true,
    //host: '0.0.0.0',  // 表示在同一个网段下所有主机
    port: 8000,
    open: true,
    proxy: {
      "/api": {
        target: "http://localhost:8888",
        pathRewrite: {
          "^/api": ""
        },
        secure: false,
        changeOrigin: true   //修改源
      }
    }
  },
```

### resolve模块解析

-   extensions解析到文件时自动添加扩展名 即可以省略后缀引入
-   alias取别名

```js
resolve: {
    extensions: ['.js', '.json'],
    alias: path.resolve(__dirname, './src/js')
  }
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ee85a48857d042cc910badc72967ed3f~tplv-k3u1fbpfcp-zoom-1.image)

\


### 分离不同环境的配置

-   webpack.comm.config.js

```js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { DefinePlugin } = require("webpack");
const { VueLoaderPlugin } = require('vue-loader/dist/index');

module.exports = {
  target: "web",
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "../build"),
    filename: "js/bundle.js",
  },
  resolve: {
    extensions: [".js", ".json", ".mjs", ".vue", ".ts", ".jsx", ".tsx"],
    alias: {
      "@": path.resolve(__dirname, "../src"),
      "js": path.resolve(__dirname, "../src/js")
    }
  },
  module: {
    rules: [
      {
        test: /.css$/,
        use: ["style-loader", "css-loader", "postcss-loader"],
      },
      {
        test: /.less$/,
        use: ["style-loader", "css-loader", "less-loader"],
      },
      // },
      {
        test: /.(jpe?g|png|gif|svg)$/,
        type: "asset",
        generator: {
          filename: "img/[name]_[hash:6][ext]",
        },
        parser: {
          dataUrlCondition: {
            maxSize: 10 * 1024,
          },
        },
      },
      {
        test: /.(eot|ttf|woff2?)$/,
        type: "asset/resource",
        generator: {
          filename: "font/[name]_[hash:6][ext]",
        },
      },
      {
        test: /.js$/,
        loader: "babel-loader"
      },
      {
        test: /.vue$/,
        loader: "vue-loader"
      }
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./public/index.html",
      title: "哈哈哈哈"
    }),
    new DefinePlugin({
      BASE_URL: "'./'",
      __VUE_OPTIONS_API__: true,
      __VUE_PROD_DEVTOOLS__: false
    }),
    new VueLoaderPlugin()
  ],
};
```

-   webpack.dev.config.js

```js
const { merge } = require('webpack-merge');

const commonConfig = require('./webpack.comm.config');

module.exports = merge(commonConfig, {
  mode: "development",
  devtool: "source-map",
  devServer: {
    contentBase: "./public",
    hot: true,
    // host: "0.0.0.0",
    port: 7777,
    open: true,
    // compress: true,
    proxy: {
      "/api": {
        target: "http://localhost:8888",
        pathRewrite: {
          "^/api": ""
        },
        secure: false,
        changeOrigin: true
      }
    }
  },
})
```

-   webpack.prod.config.js

```js
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const CopyWebpackPlugin = require('copy-webpack-plugin');
const {merge} = require('webpack-merge');

const commonConfig = require('./webpack.comm.config');

module.exports = merge(commonConfig, {
  mode: "production",
  plugins: [
    new CleanWebpackPlugin(),
    new CopyWebpackPlugin({
      patterns: [
        {
          from: "./public",
          globOptions: {
            ignore: [
              "**/index.html"
            ]
          }
        }
      ]
    }),
  ]
})
```

-   package.json

```
"scripts": {
    "build": "webpack --config ./config/webpack.prod.config.js",
    "serve": "webpack serve --config ./config/webpack.dev.config.js"
  },
```

\


### 完整webpack配置

-   webpack.config.js

```js
const path = require("path");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { DefinePlugin } = require("webpack");
const CopyWebpackPlugin = require('copy-webpack-plugin');
const { VueLoaderPlugin } = require('vue-loader/dist/index');

module.exports = {
  target: "web",
  mode: "development",
  devtool: "source-map",
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "./build"),
    filename: "js/bundle.js",
  },
  devServer: {
    contentBase: "./public",
    hot: true,
    // host: "0.0.0.0",
    port: 7777,
    open: true,
    // compress: true,
    proxy: {
      "/api": {
        target: "http://localhost:8888",
        pathRewrite: {
          "^/api": ""
        },
        secure: false,
        changeOrigin: true
      }
    }
  },
  resolve: {
    extensions: [".js", ".json", ".mjs", ".vue", ".ts", ".jsx", ".tsx"],
    alias: {
      "@": path.resolve(__dirname, "./src"),
      "js": path.resolve(__dirname, "./src/js")
    }
  },
  module: {
    rules: [
      {
        test: /.css$/,
        use: ["style-loader", "css-loader", "postcss-loader"],
      },
      {
        test: /.less$/,
        use: ["style-loader", "css-loader", "less-loader"],
      },
      // },
      {
        test: /.(jpe?g|png|gif|svg)$/,
        type: "asset",
        generator: {
          filename: "img/[name]_[hash:6][ext]",
        },
        parser: {
          dataUrlCondition: {
            maxSize: 10 * 1024,
          },
        },
      },
      {
        test: /.(eot|ttf|woff2?)$/,
        type: "asset/resource",
        generator: {
          filename: "font/[name]_[hash:6][ext]",
        },
      },
      {
        test: /.js$/,
        loader: "babel-loader"
      },
      {
        test: /.vue$/,
        loader: "vue-loader"
      }
    ],
  },
  plugins: [
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      template: "./public/index.html",
      title: "哈哈哈哈"
    }),
    new DefinePlugin({
      BASE_URL: "'./'",
      __VUE_OPTIONS_API__: true,
      __VUE_PROD_DEVTOOLS__: false
    }),
    // new CopyWebpackPlugin({
    //   patterns: [
    //     {
    //       from: "public",
    //       to: "./",
    //       globOptions: {
    //         ignore: [
    //           "**/index.html"
    //         ]
    //       }
    //     }
    //   ]
    // }),
    new VueLoaderPlugin()
  ],
};
```
