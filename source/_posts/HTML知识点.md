---
title: HTML知识点
date: 2022-03-15 09:13:09
categories: 知识点
tags: ['HTML','面试']
---

## 1. 如何理解HTML语义化

**优点**

- 对机器友好，更适合搜索引擎的爬取，有利于SEO。支持读屏软件，根据文章可以自动生成目录
- 对开发者友好，增强可读性，结构更加清晰，便于维护

**常见语义化标签**

```html
<header>头部</header>
<nav>导航栏</nav>
<section>区块(有语义化的div)</section>
<main>主要区域</main>
<article>主要内容</article>
<aside>侧边栏</aside>
<footer>底部</footer>
```



## 2. 常见块级和内联元素

**块级元素**

- div、h1、h2、table、ul、ol、p等

**内联元素**

- span、img、input、button等



## 3. DOCTYPE(文档类型的作用)

告诉浏览器（解析器）应该以什么样的文档类型（html或xhtml）定义来解析文档

浏览器渲染页面的两种模式（可通过`document.compatMode` 获取）

**CSS1Compat：标准模式(Strick mode)** ，默认模式，浏览器使用W3C标准解析渲染页面，在标准模式下，浏览器以其支持的最高标准呈现页面

**BackCompat：怪异模式(Qiock mode)** ，浏览器以自己的怪异模式解析渲染页面，在怪异模式中，页面以一种比较宽松的向后兼容的方式显示

**触发怪异模式的方式**

- IE浏览器
- 不写DOCTYPE
- `box-sizing:border-box` 



## 4. src和href的区别

- **src**：表示对资源的引用，指向的内容会被下载并嵌入到当前标签所在位置，如js脚本，当浏览器解析到该元素时，会暂停其他资源的下载和处理，指导该资源加载、编译、执行完毕，所以一半js脚本会放在页面底部
- **href**：表示超文本引用，它指向一些网络资源，建立和当前元素或当前文档的链接关系，当浏览器识别到它指向的文件时，会并行下载资源，不会停止对当前文档的处理，常用在a、link等标签上



## 5. script标签中defer和async的区别

如果没有defer或async属性，浏览器会立即加载并执行相应的脚本。它不会等待后续加载的文档元素怒，读取到就会开始加载和执行，这样就阻塞了后续文档的加载。

**defer和async属性都是去异步加载外部的JS脚本文件，它们都不会阻塞页面的解析**，区别如下：

- **执行顺序**：多个带async属性的标签，不能保证加载的顺序；多个带defer属性的标签，按照加载顺序执行
- **脚本是否并行执行**：async属性，并行加载，并行执行；defer属性，并行加载，等到文档所有元素解析完成之后才执行，在DOMContentLoaded触发之前



## 6. 常用的meta标签有哪些

1. `charset` ，用来描述HTML文档的编码类型

```html
<meta charset="utf-8">
```

2. `keywords` ，页面关键词

```html
<meta name="keywords" content="关键词" />
```

3. `description` ，页面描述

```html
<meta name="description" content="页面描述" />
```

4. `refresh` ，页面重定向和刷新

```html
<meta http-equiv="refresh" content="0;url=" />
```

5. `viewport` ，适配移动端，可以控制视口的大小和比例

```html
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1" />
```

`content` 参数有以下几种

- `width`：宽度(数值/device-width)
- `height`：高度(数值/device-height)
- `initial-scale`：初始缩放比例
- `maximum-scale` ：最大缩放比例
- `minimum-scale` ：最小缩放比例
- `user-scalable`：是否允许用户缩放(yes/no)

6. 搜索引擎索引方式

```html
<meta name="robots" content="index,follow" />
```

`content` 参数有以下几种

- `all`：文件将被检索，且页面上的链接可以被查询
- `none`：文件不被检索，且页面上的链接不可以被查询
- `index`：文件将被检索
- `follow`：页面上的链接可以被查询
- `noindex`：文件不被检索
- `nofollow`：页面上的链接不可以被查询



## 7. HTML有哪些更新

### 1. 语义化标签

- `header`：头部
- `nav`：导航
- `footer`：底部
- `article`：文章内容
- `section`：文档中的节
- `aside`：侧边栏

### 2. 媒体标签

1. audio：音频

```html
<audio src='' controls autoplay loop />
```

属性

- `controls` 控制面板
- `autoplay` 自动播放
- `loop` 循环播放



2. video：适配

```html
<video src='' poster='imgs/aa.jpg' controls />
```

属性

- `poster` 指定封面
- `controls` 控制面板
- `width` 宽度
- `height` 高度



3. source标签，兼容不同的浏览器

```html
<video>
  <source src='aa.flv' type='video/flv'></source>
  <source src='aa.mp4' type='video/mp4'></source>
</video>
```

### 3. 表单

**表单类型**

- `email` 能够验证当前输入的邮箱地址是否合法
- `url` 验证url
- `number` 只能输入数字，自带点击增加减小箭头，max属性设置为最大值，min设置为最小值，value为默认值
- `search` 可以一键删除输入内容
- `range` 可以提供一个范围，其中可以设置max和min以及value，其中value属性可以设置为默认值
- `color` 提供一个颜色拾取器
- `time` 时间选择器
- `data` 日期选择器
- `datatime` 时间和日期
- `datatime-local` 日期时间控件
- `week` 周控件
- `month` 月控件



**表单属性**

- `placeholder` 提示信息
- `autofocus` 自动获取焦点
- `autocomplete="on"`或`autocomplete="off"` 必须有name属性，并提交过，可以自动填写
- `required` 不能为空
- `pattern""`里面写入想要的正则模式，例如手机号`pattern="^(+86)?\d{10}$"` 
- `mutiple` 可以选择多个我呢见或者多个邮箱
- `form=form表单的ID`



**表单事件**

-  `oninput` 每当input里的输入库内容发生变化都会触发此事件
- `oninvalid` 当验证不通过时触发此事件



### 4. 进度条、度量器

- `progress` 标签用来表示任务的进入，max表示最大，value表示已完成多少

- `meter` 属性：用来显示剩余容量或剩余库存

  - high/low 规定被视作高/低的范围
  - max/min 规定最大/小值
  - value 规定当前度量值

  设置规则：min < low < hight < max



### 5. DOM查询操作

- `document.querySelector()`
- `document.querySelectorAll()`



### 6. Web存储

- `localStorage` - 没有时间限制的数据存储
- `sessionStorage` - 针对一个session的数据存储



### 7. 其他

- 拖放：拖放是一种常见的特性，即抓取对象以后拖到另一个位置，设置元素可拖放

```html
<img draggable="true" />
```

- 画布：canvas元素使用JS在网页上绘制图像。画布是一个矩形区域，可以控制其每一像素。canvas拥有多种绘制路径、矩形、字符以及添加图像的方法

```html
<canvas id="myCanvas" width="200" height="100"></canvas>
```

- SVG：可伸缩矢量图形，用来定义用于网络的基于矢量的图形，使用XML格式定义图形，图像的放大或改变尺寸的情况下其图像质量不会有损失，它是万维网联盟的标准
- 地理位置：Geolocation用于定位用户的位置

### 移除

- 纯表现的元素：basefont，big，center，font，s，strike，tt，u
- 对可用性产生负面影响的元素：frame，frameset，noframes



## 8. img的srcset属性的作用

响应式页面中经常用到根据屏幕密度设置不同的图片，这时就用到了img标签的srcset属性，srcset属性用于设置不同屏幕密度下，img会自动加载不同的图片，用法如下

```html
<img src="images-128.png" srcset="images-256.png 2x" />
```

```html
<img src="image-128.png"
     srcset="image-128.png 128w, image-256.png 256w, image-512.png 512w"
     sizes="(max-width: 360px) 340px, 128px" />

```

- 其中`srcset`指定图片的地址和对应的图片质量，`sizes` 设置临界点，可以按需加载

## 9. 说一下web worker

web worker为web内容在后台线程中运行脚本提供了一种简单的方法，线程可以执行任务而不干扰用户界面



## 10. HTML5的离线存储怎么使用，它的工作原理是什么

离线存储指的是：在用户没有与因特网连接时，可以正常访问站点或应用，在用户与因特网连接时，更新用户机器上的缓存文件

**原理**： HTML5的离线存储是基于一个新建的`.appcache` 文件的缓存机制，通过这个文件上的解析清单离线存储资源，这些资源就会像cookie一样被存储下来，之后网络处于离线状态下时，浏览器会通过被离线存储的数据进行页面展示

**使用方法**

1. 创建一个和html同名的manifest文件，然后在页面头部加入manifest属性

```html
<html lang="en" manifest="index.manifest">
```

2. 在`cache.manifest` 文件中编写需要离线存储的资源

```json
CACHE MANIFEST
    #v0.11
    CACHE:
    js/app.js
    css/style.css
    NETWORK:
    resourse/logo.png
    FALLBACK:
    / /offline.html

```

- **CACHE** 表示需要离线存储的资源列表，由于包含manifest文件的页面将被自动离线存储，所以不需要把页面自身页列出来
- **NETWORK** 表示在它下面列出来的资源只有在在线的情况下才能访问，它们不会被离线存储，所以在离线情况下无法使用这些资源，不过，如果在CACHE和NETWORK中有一个相同的资源，那么这个资源还是会被离线存储，也就是说CACHE的优先级更高
- **FALLBACK** 表示如果访问第一个资源失败，那么就使用第二个资源来替换他，比如上面这个文件表示的就是如果访问根目录下任何一个资源失败了，那么就去访问`offline.html` 

3. 在离线状态时，操作`window.applicationCache` 进行离线缓存的操作

**如何更新缓存**

1. 更新manifest文件
2. 通过javascript操作
3. 清除浏览器缓存

**注意事项**

1. 浏览器对缓存数据的容量限制可能不一样（某些浏览器设置的限制是每个站点5MD）
2. 如果manifest文件，或者内部列举的某一个文件不能正常下载，整个更新过程都将失败，浏览器继续全部使用老的缓存
3. 引用manifest的html必须与manifest文件同源，在同一个域下
4. FALLBACK中的资源必须和manifest文件同源
5. 当一个资源被缓存后，该浏览器直接请求这个绝对路径也会访问缓存中的资源
6. 站点中的其他页面即使没有设置manifest属性，请求的资源如果在缓存中也从缓存中访问
7. 当manifest文件发生改变时，资源请求本身也会触发更新



## 11. 浏览器是如何对HTML5的离线存储资源进行管理和加载

- **在线的情况下**：浏览器发i西安html头部有manifest属性，它会请求manifest文件，如果是第一次访问页面，那么浏览器就会根据manifest文件的内容下载相应的资源并且进行离线缓存。如果已经访问过页面并且资源已经进行离线缓存，那么浏览器就会使用离线的资源加载页面，然后浏览器会对比新的manifest文件和旧的manifest文件，如果文件没有发生改变，就不做任何操作，如果文件改变了，就会重新下载文件中的资源并进行离线存储
- **离线的情况下**浏览器会直接使用离线存储的资源



## 12. title与h1的区别、b与strong的区别、i与em的区别

- strong标签有语义，起到加强语气强调的效果，b标签没有语义，只是加粗标签，搜索引擎更侧重strong
- title属性没有明确意义只表示标题，H1则表示层次明确的标题，对页面信息的抓取有很大的影响
- i内容展示位斜体，em表示强调的文本



## 13. iframe有哪些优点

**优点**

- 用来加载速度较慢的内容
- 可以使脚本并行下载
- 可以实现跨子域通信

**缺点**

- iframe会阻塞主页面的onload事件
- 无法被一些搜索引擎识别
- 会产生很多页面，不易管理



## 14. label的作用是什么，如何使用

用来定义表单控件的关系：点击label时，自动将焦点定位到与label相关的表单控件上

- 使用方法  for控件的id  或者直接包裹

```html
<label for="mobile">Phone</label>
<input type="text" id="mobile" />
```

```html
<label>
Phone:<input  type="text" />
</label>
```



## 15. Canvas和SVG的区别

**SVG** 可伸缩矢量图形，是基于XML描述的2D图形的语言，SVG基于XML就意味着SVG DOM中的每个元素都是可用的，可以为某个元素附加JS事件处理器，在SVG中，每个被绘制的图形均被视作对象，如果SVG对象的属性发生变化，那么浏览器能够自动重现图形

**特点**

- 不依赖分辨率
- 支持事件处理器
- 最适合带有大型渲染区域的应用程序(比如谷歌地图)
- 复杂度高会减慢渲染速度(任何过度使用DOM的应用都不快)
- 不适合游戏应用



**Canvas** 画布，通过JS来绘制2D图形，是逐像素进行渲染的，其位置发生改变，就会重新进行渲染

**特点**

- 依赖分辨率
- 不支持事件处理器
- 弱的文本渲染能力
- 能够以.png或.jpg格式保存结果图像
- 最适合图像密集型的游戏，其中的许多对象会被反复重绘



## 16. head标签有什么用，其中什么标签必不可少

标签用于定义文档的头部，它是所有头部元素的容器，head中的元素可以引用脚本、指示浏览器在哪里找到样式表、提供元信息等

**可在head中用的标签**

- `<base>`
- `<link>`
- `<meta>`
- `<script>`
- `<style>`
- `<title>` 必需



## 17. 浏览器乱码的原因是什么，如何解决

**产生乱码的原因：**

- 网页源代码是`gbk` 的编码，而内容中的中文字是`utf-8` 编码的，编码不匹配乱码
- `html` 页面编码是`gbk` ，而程序从数据库中调出呈现是`utf-8`编码的内容也会造成乱码
- 浏览器不能自动检测网页编码，造成乱码

**解决办法：**

- 使用软件编辑HTML网页内容
- 使用网页涉资编码类型，如果数据库和网页不匹配编码，可以对中文进行转码，使用转码函数
- 如果浏览器浏览时候出现网页乱码，在浏览器中找到转换编码的菜单进行转换



## 18. 渐进增强和优雅降级之间的区别

**渐进增强(progressice enhancement)**主要针对低版本的浏览器进行页面重构，保证基本的功能的情况下，再针对高级浏览器进行效果、交互等方面的改进和追加功能，以达到更好的用户体验

**优雅降级(graceful degradation)**一开始就构建完整的功能，然后再针对低版本的浏览器进行兼容



## 19. 说一下HTML5 drag API

- `dragstart` 事件主体是被拖放元素，在开始拖放被拖放元素时触发
- `drag` 事件主体是被拖放元素，在正在拖放被拖放元素时触发
- `dragenter` 事件主体时目标元素，在被拖放元素进入某元素时触发
- `dragover` 事件主体是目标元素，在被拖放元素进入某元素时触发
- `dragleave` 事件主体时目标元素，在被拖放元素移出目标元素时触发
- `drop` 事件主体时目标元素，在目标元素完全接受被拖放元素时触发
- `dragend` 事件主体是被拖放元素，在整个拖放操作结束时触发



## 20. 网页开发中，如何实现图片的懒加载

**描述**：懒加载，顾名思义，在当前网页，滑动页面到能看到图片的时候再加载图片

直接将懒加载这事交给浏览器做，为图片加一个属性即可

```html
<img src="kity.png" loading="lazy" />
```



## 21. 浏览器中如何实现剪切板复制内容的功能

**描述**：在一些博客系统中，可以复制代码，它是怎么实现的



目前最为推荐的方式是用第三方库`Clipboard API`进行实现[feross/clipboard-copy: Lightweight copy to clipboard for the web (github.com)](https://github.com/feross/clipboard-copy)

```js
navigator.clipboard.writeText(text)
```

**复制**

```js
document.execCommand("copy")
```



## 22. localhost:3000和localhost:5000的cookie信息是否共享

根据同源策略，cookie是区分端口的，但是浏览器实现来说，`cookie` 区分域，而不区分端口，也就是说同一个ip下的cookie是共享的



## 23. 什么是CSRF攻击

CSRF跨站请求伪造，又称`one-click-attack` 顾名思义，通过恶意引导用户一次点击劫持`cookie`进行攻击，是一种挟制用户在当前已登录的 Web 应用程序上执行非本意的操作的攻击方法。跟跨网站脚本（XSS）相比，XSS 利用的是用户对指定网站的信任，CSRF 利用的是网站对用户网页浏览器的信任。

1. 使用JSON API，当进行CSRF攻击时，请求体通过`<form>` 构建，请求头`application/www-form-urlencoded` 它难以发送JSON数据被服务器所理解
2. CSRF Token，生成一个随机的token，切勿放在cookie中，每次请求手动携带该token进行校验
3. SameSite Cookie，设置为Lax或者Strict，禁止发送第三方Cookie



## 24. 在浏览器中如何监听剪切板中内容

通过`Clipboard API` 可以获取剪切板中内容，但需要获取到`clipboard-read` 权限

```js
// 是否有读取权限
const result=await navigator.permission.query({name:'clipboard-read'})

// 获取剪切板内容
const text=await navigator.clipboard.readText()
```



## 25. 如何把json数据转化为demo.json并下载文件

json视为字符串，可以使用`DataURL` 进行下载，`Text->DataURL` 

除了使用DataURL，还可以转化为Object URL进行下载

`Text - > Blob -> Object URL` 

可以把以下代码直接粘贴到控制台下载文件

```js
function download(url, name) {
  const a = document.createElement("a");
  a.download = name;
  a.rel = "noopener";
  a.href = url;
  // 触发模拟点击
  a.dispatchEvent(new MouseEvent("click"));
  // 或者 a.click()
}

const json = {
  a: 3,
  b: 4,
  c: 5,
};
const str = JSON.stringify(json, null, 2);

// 方案一：Text -> DataURL
const dataUrl = `data:,${str}`;
download(dataUrl, "demo.json");

// 方案二：Text -> Blob -> ObjectURL
const url = URL.createObjectURL(new Blob(str.split("")));
download(url, "demo1.json");
```

**总结**

1. 模拟下载，可以通过新建一个`<a href="url" download>` 标签并设置`url` 即`download` 属性来下载
2. 可以通过把`json` 转化为`dataurl` 来构造URL
3. 可以通过把`json`转化为`Blob` 再转化为`ObjectURL` 来构造URL



## 26. 介绍requestIdleCallback及使用场景

`requestIdleCallback` 维护一个队列，将在浏览器空闲时间内执行，属于后台任务API，可以使用`setTimeout` 来模拟实现

```js
const rIC = window["requestIdleCallback"] || ((f) => setTimeout(f, 1));
```

在`rIC` 中执行任务时需要注意以下几点：

1. 执行重计算而非紧急任务
2. 空闲回调任务时间应该小于50ms，最好更少
3. 空闲回调中不要操作DOM，因为它本来就是利用的重排重绘后的空闲时间，重新操作DOM又会造成重绘重排

React的时间分片便是基于类似`rIC`而实现，然而因为`rIC`的见同行及50ms流畅问题，React自制了一个实现`scheduler` 



## 27. 如何计算白屏时间和首屏时间

白屏时间: window.performance.timing.domLoading - window.performance.timing.navigationStart

首屏时间: window.performance.timing.domInteractive - window.performance.timing.navigationStart



## 28. 什么是重排重绘，如何减少重排重绘

**重排（Reflow）**：元素的位置发生变动时发生重排，也叫回流

**重绘（Repaint）**：元素的样式发生变动，位置不变。

重排必定造成重绘，有以下方法

1. 使用`DocumentFragment` 进行DOM操作，不过现在原生操作很少，基本用不到
2. CSS样式尽量批量修改
3. 避免使用table布局
4. 为元素提前设置好宽高，不因多次渲染改变位置



## 29.  什么时Data URL

Data URL时将图片转换为base64直接嵌入到网页中，使用`<img src="data:[MIME type];base 64" />` 这种方式引用图片，不需要再发送请求获取图片，缺点

- base64编码后的图片会比原来的体积大三分之一左右
- Data URL形式的图片不会缓存下来，每次访问页面都要被下载一次，可以将Data URL写入到CSS文件中随着CSS被缓存下来



## 30. textarea如何禁止拉伸

使用CSS眼视光hi可以避免拉伸

```css
textarea{
  resize:none
}
```



## 31. 在Canvas中如何处理跨域的图片

```js
img.setAttribute('crossOrigin','anonymous')
```



## 32. 如何取消请求的发送

1. XHR使用`xhr.abort()` 

```js
const xhr=new XMLHttpRequest(),method="GET",url="https://www.baidu.com";
xhr.open(method,url,true)

xhr.end()

// 取消发送请求
xhr.abort()
```

2. fetch使用`AbortController`

```js
const controller = new AbortController()
const signal = controller.signal

const downloadBtn = document.querySelector('.download');
const abortBtn = document.querySelector('.abort');

downloadBtn.addEventListener('click', fetchVideo);

// 点击取消按钮时，取消请求的发送
abortBtn.addEventListener('click', function() {
  controller.abort();
  console.log('Download aborted');
});

function fetchVideo() {
  ...
  fetch(url, {signal}).then(function(response) {
    ...
  }).catch(function(e) {
   // 请求被取消之后将会得到一个 AbortError
    reports.textContent = 'Download error: ' + e.message;
  })
}
```

3. Axios使用`cancelToken` 取消

```js
const CancelToken=axios.CancelToken
const source=CancelToken.source()

axios
  .get('/user/1234',{
  cancelToken:souce.token
})
	.catch(function(thrown){
  if(axios.isCancel(thrown)){
    console.log('request canceled',thrown.message)
  }else{
    // handle error
  }
})

axios.post(
	'/user/123',
  {
    name:'new name'
  },
  {
    cancelToken:source.token
  }
)

source.cancel('operation canceled by the user')

```

