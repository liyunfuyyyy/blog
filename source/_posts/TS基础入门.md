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

  ```typescript
  function useState<T>(state:T):[T,(newState:T)=>void]{
  	let currentState=state
    const changeState=(newState:T)=>{
      currentState=newState
    }
  	return [currentState,changeState]
  }
  
  const [counter,setCounter]=useState(10)
  ```




### 函数的返回值类型

```typescript
function sum(num1:number , num2:number):number{
  return num1+num2
}
```



### 匿名函数的参数类型

- 匿名函数可以自动推断出参数的类型

```typescript
const names=['abc','def','ghi']
names.forEach(item=>{
  console.info(item.toUpperCase())
})
```



### 对象类型

```typescript
function printCoordinate(point:{x:number,y:number}){
  console.info('x坐标',point.x)
  console.info('y坐标',point.y)
}
printCoordinate({x:10,y:30})
```



### 可选类型

```typescript
function printCoordinate(point:{x:number,y:number,z?:number}){
  console.info('x坐标',point.x)
  console.info('y坐标',point.y)
  if(point.z){
    console.info('z坐标',point.z)
  }
}
printCoordinate({x:10,y:30})
printCoordinate({x:20,y:30,z:40})
```



### 联合类型

- 联合类型是由两个或多个其他类型组成的类型
- 表示可以是这些类型中的任何一个值
- 联合类型中的每一个类型被称之位联合成员

```typescript
function printId(id:number|string){
  console.log('你的id是',id)
}
printId(10)
printId('abc')
```



### 类型别名

- 编写对象类型和联合类型有时需要多次在其他地方使用，可以起别名

```typescript
type Point={
  x:number,
  y:number
}
function printPoint(point:Point){
  console.info(point.x,point.y)
}

function sumPoint(point:Point){
  console.info(point.x+point.y)
}

printPoint({x:20,y:30})
sumPoint({x:20,y:20})
```

```typescript
type ID=number|string
function printId(id:ID){
  console.info('你的id',id)
}
```



## 断言

### 类型断言as

- 有时候TS无法获取具体的类型信息，这时我们就需要使用类型断言
- TS只允许类型断言转换为`更具体` 或`不太具体` 的类型版本

```typescript
const el=document.getElementById('box') as HTMLImageElement

el.src='图片地址'
```



### 非空断言

- 当我们编写可选参数的时候，执行TS的编译阶段会报错

  - 这是因为传入的message有可能是undefined

    ```typescript
    function printMessage(message?:string){
      console.info(message:toUpperCase())
    }
    printMessage('hello')
    ```

- 但是，我们确定传入的参数是有值的，这个时候我们可以使用非空类型断言

  - 非空断言使用的是`!` 表示可以确定某个标识符是有值得，跳过TS在编译阶段对他得检测

    ```typescript
    function printMessage(message?:string){
      console.info(message!.toUpperCase())
    }
    ```



## JS新特性

### 可选链的使用

- 可选链操作符`?.` 

- 作用是当对象的熟悉不存在时，会短路，直接返回undefined，如果存在，那么才会继续执行

  ```typescript
  type Person={
    name:string,
    friend?:{
      name:string,
      age?:number,
      girlFriend?:{
        name:string
      }
    }
  }
  
  const info:Person={
    name:'why',
    friend:{
      name:'kobe',
      girlFriend:{
        name:'lily'
      }
    }
  }
  
  console.info(info.friend?.name)
  console.info(info.friend?.age)
  console.info(info.friend?.girlFirend?.name)
  ```



### ??和!! 的作用

- ！！操作符
  - 将一个其他类型转换成boolean类型
  - 类似与Boolean的方式
  
- ？？操作符

  - 空值合并操作符是一个逻辑操作符，当操作符的1左侧是null或者undefined时，返回其右侧操作数，否则返回左侧操作数

  ```typescript
  const message=""
  let flag=!!message  // true
  
  const message='123'
  const result=message??'321'
  ```



## 字面量

### 字面量类型

- 多个类型联合起来，可以达到类似enum的效果

```typescript
type Direction = 'TOP'|'BOTTOM'|'LEFT'|'RIGHT'
function changeDeriction(direction:Direction){
  console.info('转向',align)
}

changeDeriction('LEFT')
```



### 字面量推理

```typescript
const info={
  url:'https://baidu.com/abc',
  method:'GET'
}
function request(url:string,method:'GET'|'POST'){
  console.info(url,method)
}
request(info.url,info.method)
```

- 因为函数参数需要的时`GET` 或`POST` 字面量，所以不能将string赋值进去，解决方法

```typescript
// 方案1
request(info.url,info.method as 'GET')
```

```typescript
// 方案2
const info={
  url:'https://baidu.com',
  method:'GET'
}as const
```



## 函数

### 函数类型

```typescript
type CalcFunc=(num1:number,num2:number)=>void

function calc(fn:CalcFunc){
  console.info(fn(20,30))
}
```



### 参数的可选类型

- 可选类型必须在必传参数的后面
- 可选类型的类型是指定的类型和undefined联合

```typescript
function foo(x:number,y?:number){
  console.info(x,y)
}
```



### 默认参数

```typescript
function foo(x:number,y:number=6){
  console.info(x,y)
}
foo(10)
```



### 剩余参数

```typescript
function sum(...nums:number[]){
  let total=0
  for(const num of nums){
    total+=num
  }
  return total
}

const result=sum(10,20,30)
```



### this的类型

- 某些时候可推导

```typescript
const info={
  name:'why',
  asyHello(){
    console.info(this.name)
  }
}
info.sayHello()
```

- 不可推导时，需要指定

```typescript
type NameType={
  name:string
}
function sayHello(this:NameType){
  console.info(this.name)
}
```



### 函数的重载

```typescript
function sum(a1:number,a2:number):number;
function sum(a1:string,a2:string):string;
function sum(a1:any,a2:any):any{
  return a1+a2
}

console.info(sum(20,30))
console.info(sum('aaa','bbb'))
```



## 类

### 类的定义

```typescript
class Person{
  name!:string
  age:number
  
  constructor(name:string,age:number){
    this.age=age
  }
  
  runing(){}
}

```

- 如果我们不希望给属性初始化，可以使用`name!:string` 语法，加`!`



### 类的继承

```typescript
class Student extends Person{
  sno:number
  
  constructor(name:string,age:number,sno:number){
    super(name,age)
    this.age=sno
  }
  
  studying(){
    console.info(this.name+'studying')
  }
}
```



### 类的成员修饰符

- **public** 任何地方可见，公有的属性或方法，默认编写的属性就是public
- **private** 修饰的是仅在同一类中可见、私有的属性或方法
- **protected** 仅在自身和子类中可见



### 只读属性readonly

- 如果有一个属性我们不希望外界可以任意的修改，只希望确定值后直接使用，那么可以使用readonly

```typescript
class Person{
  readonly name:string
  
  constructor(name:string){
    this.name=name
  }
}

const p=new Person('why')
console.info(p.name)

p.name='code'  // error
```



### getters/setters

- 在前面一些私有属性我们是不能直接访问的，或者某些属性我们想要监听它的获取(getter)和设置(setter)的过程，这个时候我们可以使用存取器

  ```typescript
  class Person{
    private _name:string
    
    set name(newName){
      this._name=newName
    }
    get name(){
      return this._name
    }
    
    constructor(name:string){
      this.name=name
    }
  }
  
  const p=new Person('why')
  p.name='coder'
  console.info(p.name)
  ```



### 静态成员

```typescript
class Student{
  static time:string='20:00'
  
  static attendClass(){
    console.info('去上课')
  }
}

console.info(Student.time)
Student.attendClass()
```



### 抽象类abstract

- 抽象方法，必须存在于抽象类中
- 抽象类是使用abstruct声明的类

#### 特点

- 抽象类是不能被实例化的(也就是不能通过new创建)
- 抽象方法必须被子类实现，否则该类必须是一个抽象类

```typescript
abstract class Shape{
  abstract getArea():number
}

class Circle extends Shape{
  private r:number
  constructor(r:number){
    super()
    this.r=r
  }
  getArea(){
    return this.r+this.r*3.14
  }
}

class Rectangle extends Shape{
  private width:number
  private height:number
  
  constructor(width:number,height:number){
    super()
    this.width=width
    this.height=height
  }
  getArea(){
    return this.width+this.height
  }
}

const circle=new Circle(10)
const rectangle=new Reactangle(20,30)
function calcArea(shape:Shape){
  console.info(shape.getArea())
}
calcArea(circle)
calcArea(rectangle)
```



### 类的类型

```typescript
class Person{
  name:string
  constructor(name:string){
    this.name=name
  }
  runing(){
    console.info(this.name+'running')
  }
}

const p1:Person=new Person('why')
const p2:Person={
  name:'kobe',
  runing function(){
    console.info(this.name+'runing')
  }
}
```



## 接口

### 接口的声明

```typescript
interfacce Point{
  x:number
  y:number
}
```



### 可选属性

```typescript
interface Person{
  name:string
  age?:number
  friend?:{
    name:string
  }
}

const person:Person={
  name:'why',
  age:19,
  friend:{
    name:'kobe'
  }
}

console.info(person.name)
console.info(person.friend?.name)
```



### 只读属性

- 接口中也可以定义只读属性

  - 这样就意味着我们在初始化之后，这个值是不可以被修改的

  ```typescript
  interface Person{
    readonly name:string
    age?:number
    readonly friend?:{
      name:string
    }
  }
  
  const person:Person={
    name:'why',
    age:19,
    friend:{
      name:'kobe'
    }
  }
  
  person.name='code' //不可以设置
  person.friend={}  //不可以设置
  
  if(person.friend){
    person.friend.name='123'  // 可以
  }
  ```

  

### 索引类型

- 前面我们使用interface来定义对象类型，这个时候其中的属性名、类型、方法都是确定的，但是有时候我们会遇到类似下面的对象

```typescript
interface FrontLanguage{
  [index:number]:string
}

const frontend:FrontLanguage={
  1:'HTML',
  2:'CSS',
  3:'JS'
}

interface LanguageBirth={
  [name:string]:number
}

const language:LanguageBirth={
  "Java":1999,
  "JavaScript":1000,
  "c":1998
}
```



### 函数类型

- 前面我们都是通过interface来定义对象中普通的属性和方法的，实际上它也可以用来定义函数类型

```typescript
interface CalcFunc{
  (num1:number,num2:number):number
}

const add:CalcFunc=(num1,num2)=>{
  return num1+num2
}

const sub:CalcFunc=(num1,num2)=>{
  return num1-num2
}
```

- 推荐使用类型别名来定义函数

```typescript
type CalcFunc=(num1:number,num2:number)=>number
```



### 接口继承

- 接口和类一样是可以进行继承的，也是使用extends关键字

  - 并且我们会发现，接口是支持多继承的

  ```typescript
  interface Person{
    name:string
    eating:()=>void
  }
  
  interface Animal{
    runing:()=>void
  }
  
  interface Student extends Person,Animal{
    sno:number
  }
  
  const stu:Student={
    sno:100,
    name:'why',
    eating:function(){
      
    },
    runing:function(){}
  }
  ```



### 接口实现

- 接口定义后，也是可以被类实现的
  - 如果被一个类实现，那么在之后需要传入接口的地方，都可以将这个类传入
  - 这就是面向接口开发

```typescript
interface ISwim{
  swimming:()=>void
}

interface IRun{
  runing:()=>void
}

class Person implements ISwim,IRun{
  swimming(){
    console.info('swimming')
  }
  running(){
    console.info('running')
  }
}

function swim(swimmer:ISwim){
  swimmer.swimming()
}

const p=new Person()
swim(p)
```





### 交叉类型

- 交叉类似表示需要满足多个类型的条件
- 交叉类型使用&符号
- 在开发中进行交叉时，通常是对对象类型进行交叉的

```typescript
interface Colorful{
  color:string
}

interface IRun{
  runing:()=>void
}

type NewType=Colorful&IRun

const obj:NewType={
  color:'red',
  running:function(){}
}
```



### interface和type的区别

- interface可以重复的对某个接口来进行属性和方法
- type定义的是别名，别名是不能重复的

```typescript
interface IPerson{
  name:string
  running:()=>void
}

interface IPerson{
  age:number
}
```

![image-20220409124829263](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220409124829263.png)



## 枚举

### 枚举类型

```typescript
enum Direction{
  LEFT,
  RIGHT,
  TOP,
  BOTTOM
}

function turnDirection(direction:Direction){
  switch(direction){
    case Direction.LEFT:
      console.info('转向左边')
      break;
    case Direction.RIGHT:
      console.info('转向右边')
      break;
    case Direction.TOP:
      console.info('转向上边')
      break;
    case Direction.BOTTOM:
      console.info('转向下边')
      break;
    default:
      const myDirection:never=direction
  }
}
```





## 泛型

### 泛型接口

```typescript
interface IFoo<T>{
  initialValue:T,
  valueList:T[],
  handleValue:(value:T)=>void
}

const foo:IFoo<number>={
  initialValue:0,
  valueList:[0,1,2],
  handleValue:function(value:number){
    console.info(value)
  }
}

interface IFoo<T=number>{
  initialValue:T,
  valueList:T[],
  handleValue:(value:T)=>void
}
```



### 泛型类

```typescript
class Point<T>{
  x:T
  y:T
  
  constructor(x:T,y:T){
    this.x=x
    this.y=y
  }
}

const p1=new Point(10,20)
const p2=new Point<number>(10,20)
const p3:Point<number>=new Point(10,20)
```



### 泛型约束

- 有时候我们希望传入的类型有某些共性，但是这些共性可能不是在同一种类型中

  - 比如string和array都是有length的，或者某些对象也是会有length属性的
  - 那么只要是拥有length的属性都可以作为我们的参数类型

  ```typescript
  interface ILength{
    length:number
  }
  
  function getLength<T extends ILength>(args:T){
    return args.length
  }
  
  console.info(getLength('abc'))
  console.info(getLength(['abc','cba']))
  console.info(getLength({length:100,name:'why'}))
  ```



### 命名空间

```typescript
export namespace Time{
  export function format(time:string){
    return '2022-01-01'
  }
}
export namespace Price{
  export function format(price:number){
    return '222.22'
  }
}
```



## 声明

当一个模块引用的是另一个模块的数据时，没有引入需要声明

### 声明变量、函数、类

```typescript
// 
let wName='why'
let mAge=19
let mHeight=18

function wFoo(){
  console.info('wfoo')
}

function wBar(){
  console.info('wBar')
}

function Person(name,age){
  this.name=name
  this.age=age
}
```

```typescript
declare let wName:string
declare let wAge:number
declare let wHeight:number

declare function wFoo():void
declare function wBar():void

declare class Person{
  name:string
  age:number
  
  constructor(name:string,age:number)
}
```



### 声明模块

- 我们也可以声明模块，比如lodash模块默认不能使用的情况，可以自己来声明这个模块

```typescript
declare module 'lodash'{
  export function join(args:any[]):any
}
```



