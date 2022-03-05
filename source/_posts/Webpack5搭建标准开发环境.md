---
title: Webpack5搭建标准开发环境
date: 2022-03-05 11:00:17
categories: 实战
tags: ['Webpack','前端工程化']
---

## Webpack安装&使用
### 安装

```shell
npm install webpack webpack-cli -D
```

### 使用方式

#### 方式一

```shell
./node_modules/.bin/webpack --version
```

#### 方式二

```shell
npx webpack --version
```



## 入口(entry)

> **入口起点(entry point)**指示 webpack 应该使用哪个模块，来作为构建其内部*依赖图*的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。
## 出口(output)

>**output** 属性告诉 webpack 在哪里输出它所创建的 *bundles*，以及如何命名这些文件，默认值为 `./dist`
```js
const config = {
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.join(__dirname, './dist')
    }
}
```


## loader

> *loader* 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效[模块](https://www.webpackjs.com/concepts/modules)，然后你就可以利用 webpack 的打包能力，对它们进行处理。

### 让webpack处理CSS文件

>*webpack 根据正则表达式，来确定应该查找哪些文件，并将其提供给指定的 loader。在这种情况下，以* `.css` *结尾的全部文件，都将被提供给* `style-loader` *和* `css-loader`*。*

- 下载依赖loader

  ```shell
  npm install --save-dev css-loader
  npm install --save-dev style-loader
  ```

- 编写规则，匹配哪些后缀使用哪些`loader`  `webpack.config.js`

  ```js
  module:{
      rules:[
          {
              test:/\.css$/,
              use:['style-loader','css-loader']
          }
      ]
  }
  ```

- `loader`链式传递，先从后面的loader开始



### 让webpack处理scss文件

- 下载依赖loader

  ```shell
  npm install sass-loader node-sass -D
  ```

- 编写规则

  ```js
  module:{
      rules:[
          {
              test:/\.(scss|sass)$/,
              use:['style-loader','css-loader','sass-loader']
          }
      ]
  },
  ```




<mark>**file-loader** 和 **url-loader** 可以接收并加载任何文件，然后将其输出到构建目录。</mark>

### 让webpack处理图片

- 下载依赖loader

  ```shell
  npm install file-loader -D
  ```

- 编写规则

  ```js
  {
      test: /\.(png|jpg|svg|gif)$/,
      use:['file-loader']
  }
  ```

- 可以在`index.js`中引入

  ```js
  import Icon from './icon.jpg';
  
  //将图像添加到我们现有的div
  const myIcon = new Image();
  myIcon.src = Icon;
  element.appendChild(myIcon);
  ```

- 也可在`index.scss`中引入

  ```css
  .hello{
    color: red;
    background: url("./icon.jpg");
  }
  ```

### 让webpack处理字体

- 下载依赖`loader`

  ```shell
  npm install file-loader -D
  ```

- 编写规则

  ```js
  {
      test:/\.(woff|woff2|eot|ttf|otf)$/,
      use:['file-loader']
  }
  ```

- 在`index.scss`中引入

  ```scss
  @font-face {
    font-family: 'Myfont';
    src: url("./myfont.TTF") format('ttf');
    font-weight: 600;
    font-style: normal;
  }
  .hello{
    color: red;
    font-family: Myfont;
  }
  ```

### 让webpack处理`CSV`、`TSV` 、`XML`

- 下载依赖`loader`

  ```shell
  npm install csv-loader xml-loader -D
  ```

- 编写规则

  ```js
  {
      test:/\.(csv|tsv)$/,
      use:['csv-loader']
  },
  {
      test:/\.xml$/,
      use:['xml-loader']
  }
  ```

- 在`src`下创建`data.xml`并在`index.js`中引入

  - `data.xml`

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <note>
      <to>mary</to>
      <from>john</from>
      <heading>reminder</heading>
      <body>call cindy on tuesday</body>
  </note>
  ```

  - `index.js`

  ```js
  import Data from './data.xml';
  
  console.log(Data);
  ```

  

## 插件(plugins)

> loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。[插件接口](https://www.webpackjs.com/api/plugins)功能极其强大，可以用来处理各种各样的任务。

### 使用plugins处理html

- 下载依赖plugins

  ```shell
  npm install html-webpack-plugin -D
  ```

- **由于插件可以携带参数/选项，所以必须在webpack配置中，向`plugins`属性传入`new`实例**

- ```js
  //处理src下的html文件
  plugins: [
          new HtmlWebpackPlugin({template: "./src/index.html"})
  ],
  ```

## 模式(mode)

> 通过选择 `development` 或 `production` 之中的一个，来设置 `mode` 参数，你可以启用相应模式下的 webpack 内置的优化

```jsx
module.exports = {
  mode: 'production'
};
```



## 模块热替换

### 过程

#### 在应用程序中置换模块

1. 应用程序代码要求HMR runtime检查更新
2. HMR runtime(异步)下载更新，然后通知应用程序代码
3. 应用程序代码要求HMR runtime应用更新
4. HMR runtime(同步)应用更新



#### 在编译器中

除了普通资源，编译器需要发出`update`，以允许更新之前的版本到新的版本，`update`由两部分组成：

1. 更新后的`manifest`(JSON)
2. 一个或多个更新后的`chunk`(JavaScript)



## 配置标准开发环境

```shell
ni babel-loader @babel/core @babel/preset-env @babel/plugin-transform-runtime -D
ni @babel/runtime 
```

### 创建.babelrc

```json
{
  "presets": [
    "@babel/preset-env"
  ],
  "plugins": [
    "@babel/plugin-transform-runtime"
  ]
}
```

### 在webpack.config.js中rules添加规则

```js
{
    test:/\.js$/,
    loader: "babel-loader"
}
```

> 第一里程碑

### 自动清理dist目录webpack5.x之后

在输出中添加`clean:true`即可

```shell
output: {
    filename: 'bundle.js',
    path: path.join(__dirname, './dist'),
    clean: true
},
```

![image-20220101101422744](https://gitee.com/LUNIONT/img-url/raw/master/202201011014825.png)

[CleanWebpackPlugin does not clean in Webpack 5 - fsou (nilmap.com)](https://stackoverflow.nilmap.com/question?dest_url=https://stackoverflow.com/questions/64617228/cleanwebpackplugin-does-not-clean-in-webpack-5)



### 复制资源到dist目录

- 引入对应插件

```shell
npm install --save-dev copy-webpack-plugin
```

- 编写新的plugin语法，旧语法有问题，因为`CopyWebpackPlugin`构造函数还支持其他选项

```js
const CopyWebpackPlugin = require('copy-webpack-plugin');

new CopyWebpackPlugin(
    {
        patterns: [
            {
                from: path.join(__dirname, 'assets')
                to: 'assets'
            }
        ]
    }
)
```

![image-20220101102511174](https://gitee.com/LUNIONT/img-url/raw/master/202201011025251.png)

>错误信息：[webpack-cli] Invalid options object. Copy Plugin has been initialized using an options object that does not match the API schema.



## 对js和css压缩 丑化JS和CSS

### 压缩css和js

- 安装依赖

  ```shell
  ni css-minimizer-webpack-plugin -D
  ni terser-webpack-plugin -D   //让他来增强...扩展运算符
  ni mini-css-extract-plugin -D  //支持头部单独引用不许安装
  ```

- 引入对应插件 

  ```js
  const MiniCssExtractPlugin = require('mini-css-extract-plugin');
  const CssMinimizerPlugin = require('css-minimizer-webpack-plugin');
  const TerserJSPlugin=require('terser-webpack-plugin')
  ```

- rules

  ```js
  {
      test: /\.(scss|sass)$/,
      use: [MiniCssExtractPlugin.loader, 'css-loader', 'sass-loader']
  },
  ```

- optimization

  ```js
  optimization: {
      minimize: true,  //设置开发环境可用，若不设置默认false只能支持生产环境
      minimizer: [
          `...`,   //使用扩展运算符增强
          new CssMinimizerPlugin(),
        	new TerserJSPlugin()
      ]
  },
  ```

- plugins

  ```js
  new MiniCssExtractPlugin({
      filename: '[name].css',
      chunkFilename:'[id].css',
  }),
  new MiniCssExtractPlugin(),
  new TerserJSPlugin()
  ```

  
