---
title: TS基础入门
date: 2022-04-08 13:52:14
tags: TypeScript
categories: 学习记录
---

## 类型

### number类型

```js
let num:number=10
let num:number=10.1
let num:number=0b110  // 二进制
let num:number=0o555  // 八进制
let num:number=0xf23  // 十六进制
```



### boolean类型

```js
let flag:boolean=true
```



### string类型

```js
let message:string='hello world'
```



### Array类型

```js
const names:string[]=['alen','john','bob']
const names:Array<string> =['alen','john','bob']
```



### Object类型

```js
const myInfo:object={
  name:'john',
  age:18
}
```

- 我们不能从myinfo中获取数据，也不能设置数据，只用来描述一个对象



### Symbol类型

```js
const s1:symbol=Symbol('title')
const s2:symbol=Symbol('title')

const person={
  [s1]:'程序员',
  [s2]:'老师'
}
```



### null和undefined类型

```js
let n:null=null
let u:undefined=undefined
```



### any类型

- 在某些情况下，我们无法确定一个变量的类型，并且可能它会发生一些变化，这个时候我们可以用any类型

```js
let a:any='why'
a=123
a=true

const arr:any[]=['why',18]
```



### unknown类型

- 用于描述不确定的变量

```js
function foo():string{}
function bar():number{}

const flag=true
let result:unknown

if(flag){
  result=foo()
}else{
  result=bar()
}

if(typeof result==='string'){
  console.log(result)
}
```



### void类型

- 指定一个函数没有返回值，可以把null和undefined赋值给void，也就是函数可以返回null或者undefined
- 函数没有写任何类型，默认返回值的类型就是void

```js
function sum(num1:number,num2:number):void{
  console.log(num1+num2)
}
```



### never类型

- never表示永远不会发生值得类型
- 使用never指定死循环或者抛出异常得函数得值类型

```js
function handleMessage(message:number|string){
  switch(typeof message){
    case 'string':
      console.info('foo')
      break
    case 'number':
      console.info('bar')
      break
    default:
      const check:never=message
  }
}
```



### tuple类型

- tuple和数组得区别

  - 数组通常存放相同类型得元素，不同类型得元素是不推荐放在数组中
  - 元素每个类型都有自己特性得类型，根据索引值获取到得值可以确定对应得类型

  ```js
  const info:[string,number,number]=['john',18,10]
  ```



#### tuple的应用场景

- tuple通常可以作为返回的值，在使用的时候会非常的方便

  ```js
  function useState<T>(state:T):[T,(newState:T)=>void]{
  	let currentState=state
    const changeState=(newState:T)=>{
      currentState=newState
    }
  	return [currentState,changeState]
  }
  
  const [counter,setCounter]=useState(10)
  ```

  
