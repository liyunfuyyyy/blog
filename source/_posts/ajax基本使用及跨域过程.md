---
title: ajax基本使用及跨域过程
date: 2022-03-05 11:06:30
categories: 知识点
tags: ['ajax','跨域']
---

## ajax

- 是什么：ajax是Asynchronous JavaScript and XML（异步 JavaScript 和 XMl）的简写
- 异步：异步得像服务器发送请求，在等待响应的过程中，不会阻塞当前页面，浏览器可以做自己的事情。直到成功获取响应后，浏览器才开始处理响应数据
- Ajax 需要服务器环境，非服务器环境下，很多浏览器无法正常使用ajax

## XMLHttpRequest

```js
//创建xhr对象
const xhr=new XMLHttpRequest();

//监听事件，处理响应
xhr.addEventListener('readystatechange',()=>{},false)
//或
xhr.onreadystatechange=()=>{}

//处理响应
xhr.onreadystatechange=()=>{
    if(xhr.readyState!==4)return;
    
    //http code
    //获取到响应后，响应的内容会自动填充xhr对象的属性
    if(xhr.status>=200&&xhr.status<300||xhr.status===304){
        console.log(xhr.responseText)
    }
}

/*
readystatechange 事件监听readyState这个状态的变化
0: 未初始化，尚未调用open()
1: 启动，已经调用open() 但尚未调用send()
2: 发送，已经调用send() 但尚未接收到响应
3: 接收，已经接收到部分响应数据
4: 完成，已经接收到全部响应数据，而且已经可以在浏览器中使用了
*/

//准备发送请求
xhr.open(
	'HTTP 方法 GET、POST、PUT、DELETE',
    '地址 URL',
    true //是否异步
)

//发送请求 send的参数是通过请求体携带的数据 只有post能携带请求体
xhr.send(null)
```

### 属性

- responseType 和 response 属性

  ```js
  xhr.onreadystatechange = () => {
    if (xhr.readyState != 4) return;
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
      // 文本形式的响应内容
      // responseText 只能在没有设置 responseType 或者 responseType = '' 或 'text' 的时候才能使用
      // console.log('responseText:', xhr.responseText);
      // 可以用来替代 responseText
      console.log('response:', xhr.response);
      // console.log(JSON.parse(xhr.responseText));
    }
  };
  
  xhr.responseType = 'text';
  ```

- timeout属性

  ```js
  //设置请求的超时时间（单位ms） 在发送之前
   xhr.open('GET', url, true);
  
  xhr.timeout = 10000;
  
  xhr.send(null);
  ```

- withCredentials属性

  ```js
  //指定使用ajax发送请求时是否携带cookie
  xhr.open('GET', url, true);
  
  xhr.withCredentials = true;
  
  xhr.send(null);
  ```



### 方法

- `abort()` 终止当前请求

  ```js
  xhr.open('GET', url, true);
  xhr.send(null);
  xhr.abort();
  //放在发送之后
  ```

- `setRequestHeader()`设置请求头消息

  ```js
  xhr.setRequestHeader(头部字段的名称, 头部字段的值);
  xhr.setRequestHeader('Content-Type','application/json')
  ```



### 事件

- `load`事件 响应数据可用时触发

  ```js
  xhr.onload=()=>{}
  xhr.addEventListener('load',()=>{})
  //代替readystatechange 可以有效减少判断标识为4 的状态
  ```

- `error`事件 请求发生错误时触发

  ```js
  xhr.addEventListener(
    'load',
    () => {
      if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        console.log(xhr.response);
      }
    },
    false
  );
  xhr.addEventListener(
    'error',
    () => {
      console.log('error');
    },
    false
  )
  ```

- `abort`事件 调用abort方法后触发

  ```js
  xhr.addEventListener(
    'abort',
    () => {
      console.log('abort');
    },
    false
  );
  ```

  

## Json

<mark>不支持undefined</mark>

- JSON.parse()
  - 将json字符串转化为JS的数据类型，对象或者数组
- JSON.stringify()
  - 将JS的基本数据类型，对象或者数组转化为JSON的字符串



## CORS

- 使用CORS 跨域的过程

  ① 浏览器发送请求

  ② 后端在响应头中添加Access-Control-Allow-Origin 头信息

  ③ 浏览器接收到响应

  ④ 如果是同域下的请求，浏览器不会额外做什么，这次前后端通信就圆满了

  ⑤ 如果是跨域请求，<mark>浏览器会从响应头中查找是否允许跨域访问</mark>

  ⑥ 如果允许跨域，通信圆满完成

  ⑦ 如果没找到或步包含想要跨域的域名，就丢弃响应结果

  

  

  
