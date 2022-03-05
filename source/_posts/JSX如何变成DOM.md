---
title: JSX如何"摇身一变"成为DOM的
date: 2022-02-27 17:46:50
categories: React源码
tags: ['React','源码']
---

## JSX代码如何变成DOM

### 抛出问题

-   JSX的本质是什么，它和JS之间到底是什么关系？
-   为什么要用JSX？不用会有什么后果？

-   JSX背后的功能模块是什么，这个功能模块都做了那些事

### 尝试解答

-   JSX的本质是JS的拓展，但是浏览器不能天然支持JSX，所以需要Babel将它编译为React.createElement()的调用，语法糖返回一个叫`React Element`的JS对象

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/76b3770e68e64c12abc1e7a84e0dbce7~tplv-k3u1fbpfcp-zoom-1.image)

-   既然最后编译为React.createElement()的调用，为什么不直接使用React.createElement()呢？
-   答：
    -   由于实现同样的功能的情况下，JSX代码层次分明，语言简练，而React.createElement()代码繁重
    -   JSX语法糖允许前端开发者使用我们最为熟悉的类HTML标签语法来创建虚拟DOM，在降低学习成本的同时，也提升了研发效率和研发体验


\


-   ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0ab7dbc78f714dfaac62a91ce3a08553~tplv-k3u1fbpfcp-zoom-1.image)

\


## createElement解析

```
/**
  React的创建元素方法
 */

function createElement(type, config, children) {
  // propName用于储存后面需要用到的元素属性
  var propName;
  // props用于储存元素属性的键值对集合
  var props = {};
  // key、ref、self、source均为React元素的属性
  var key = null;
  var ref = null;
  var self = null;
  var source = null;

  // config 对象中存储的是元素的属性
  if (config != null) {
    // 进来之后的第一件事，依次对ref、key、self和source属性赋值
    if (hasValidRef(config)) {
      ref = config.ref;

      {
        warnIfStringRefCannotBeAutoConverted(config);
      }
    }

    // 此处将key 值字符串化
    if (hasValidKey(config)) {
      key = '' + config.key;
    }

    self = config.__self === undefined ? null : config.__self;
    source = config.__source === undefined ? null : config.__source; // Remaining properties are added to a new props object

    // 接着就是要把config里面的属性都一个一个挪到props对象里面
    for (propName in config) {
      if (hasOwnProperty.call(config, propName) && !RESERVED_PROPS.hasOwnProperty(propName)) {
        props[propName] = config[propName];
      }
    }
  }

  // childrenLength 指的是当前元素的子元素的个数，减去的2是type和config两个参数占用的长度
  var childrenLength = arguments.length - 2;

  // 如果抛去type和config，就只剩下一个参数，一般意味着文本节点出现了
  if (childrenLength === 1) {
    // 直接把这个值赋值给props.children
    props.children = children;
  } else if (childrenLength > 1) {
    // 处理嵌套多个子元素的情况
    // 声明一个数组，把所有剩余对象参数都遍历传入，最后把数组赋值给props.children对象
    var childArray = Array(childrenLength);

    for (var i = 0; i < childrenLength; i++) {
      childArray[i] = arguments[i + 2];
    }

    {
      if (Object.freeze) {
        Object.freeze(childArray);
      }
    }

    props.children = childArray;
  } // Resolve default props

  // 处理defaultProps
  if (type && type.defaultProps) {
    var defaultProps = type.defaultProps;

    for (propName in defaultProps) {
      if (props[propName] === undefined) {
        props[propName] = defaultProps[propName];
      }
    }
  }

  {
    if (key || ref) {
      var displayName = typeof type === 'function' ? type.displayName || type.name || 'Unknown' : type;

      if (key) {
        defineKeyPropWarningGetter(props, displayName);
      }

      if (ref) {
        defineRefPropWarningGetter(props, displayName);
      }
    }
  }
  // 最后返回一个调用ReactElement执行方法，并传入刚才处理过的参数
  return ReactElement(type, key, ref, self, source, ReactCurrentOwner.current, props);
}
```

### 参数说明：

-   `type`:用于标识节点的类型，它可以是HTML标签字符串，也可以是React组件类型
-   `config`: 以对象形式传入，组件所有的属性都会以键值对的形式存储在`config`对象中

-   `children`: 以对象形式传入，它记录的是组件标签之间嵌套的内容，也就是所谓的"子节点""子元素"

### 例子

DOM结构

```
<ul className="list" id="lis">
  <li key={1}></li>
  <li key={2}></li>
</ul>
```

React.createElement语法糖

```
React.createElement("ul", {
  // 传入属性键值对
  className: "list",
  id: "lis"
  // 从第三个入参开始往后，传入的参数都是children
}, React.createElement("li", {
  key: 1
}), React, createElement("li", {
  key: 2
}))
```

### 流程

1.  处理key、ref、self、source四个属性值
1.  遍历config，筛选出可以提进props里的属性

3.  提取子元素，推入props.children
3.  格式化defaultProps

5.  将以上数据作为入参，发起ReactElement调用

### 总结

createElement就像是开发者和ReactElement调用之间的一个“转换器”，在开发者出接收相对简单的参数，然年后将这些参数按照ReactElement的预期做一层格式化，最终通过调用ReactElement来实现元素的创建

## ReactElement解析

```
var ReactElement = function (type, key, ref, self, source, owner, props) {
  var element = {
    // 用来标识该对象是一个ReactElement
    $$typeof: REACT_ELEMENT_TYPE,
    // 内置属性赋值
    type: type,
    key: key,
    ref: ref,
    props: props,
    // 记录创建该元素的组件
    _owner: owner
  };

  return element;
};
```

ReactElement只做了一件事情，就是组装，把传入的参数按照一定的规范，组装进element对象里，并把它返回给React.createElement，最终React.createElement又把它交回到开发者手中

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/465b9816b7be49cca27a49305f04ea60~tplv-k3u1fbpfcp-zoom-1.image)



## render初识

```
function render(element, container, callback) {
  if (!isValidContainer(container)) {
    {
      throw Error( "Target container is not a DOM element." );
    }
  }

  {
    var isModernRoot = isContainerMarkedAsRoot(container) && container._reactRootContainer === undefined;

    if (isModernRoot) {
      error('You are calling ReactDOM.render() on a container that was previously ' + 'passed to ReactDOM.createRoot(). This is not supported. ' + 'Did you mean to call root.render(element)?');
    }
  }

  return legacyRenderSubtreeIntoContainer(null, element, container, false, callback);
}
```

### 参数说明

-   `element`:需要渲染的元素(ReactElement)
-   `container`:元素挂载的目标容器(一个真实DOM)

-   `callback`: 回调函数，可选参数，可以用来处理渲染结束后的逻辑



## 总结

![image-20220227181403603](https://gitee.com/LUNIONT/img-url/raw/master/image-20220227181403603.png)
