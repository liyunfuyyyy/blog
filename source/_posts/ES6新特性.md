---
title: ES6新特性
date: 2022-03-12 18:48:14
categories: 知识点
tags: ['ES6','前瞻']
---

## let&const



## 解构赋值



## 数组新特性

- `Array.of()` ：将一组值转化为数组，返回一个新数组，并且不考虑参数的数量或类型
- `copyWithin()` ：把指定位置的成员复制到其他位置，返回`原数组`
- `find()` ：返回第一个符合条件的值
- `findIndex()` ：返回第一个符合条件的索引
- `keys()` ： 对键名的1遍历，返回一个遍历器对象，可用`for-of` 循环
- `values()` ：与`keys()` 用法一样，不过是对键值的遍历
- `entries()` ：与`keys()` 用法一样，不过是对 键值对的遍历
- `Array.from()` ： 从一个类数组或可迭代对象中新建一个新的数组实例
- `fill()` ： 使用定制的元素填充数组，返回`原数组`
- `includes()` ：判断是否包含某一个元素，返回布尔值，对NaN有效，但不能定位，第二个参数开始寻找位置
- `flatMap()` ：方法首先使用映射函数映射每个元素，然后将结果压缩成一个新数组
- `flat()` ： 方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回，默认值为1(应用：数组扁平化(当输入`Infinity` 自动解到最底层))

```js
 let arr = [1, 2, 3, 4, 5]

 //Array.of()
 let arr1 = Array.of(1, 2, 3);
 console.log(arr1) // [1, 2, 3]
 
 //copyWithin(): 三个参数 (target, start = 0, end = this.length)
 // target: 目标的位置
 // start: 开始位置，可以省略，可以是负数。
 // end: 结束位置，可以省略，可以是负数，实际位置是end-1。
 console.log(arr.copyWithin(0, 3, 5)) // [4, 5, 3, 4, 5]
 
 //find()
 console.log(arr.find((item) => item > 3 )) // 4
 
 //findIndex()
 console.log(arr.findIndex((item) => item > 3 )) // 3
 
 // keys()
 for (let index of arr.keys()) {
     console.log(index); // 一次返回 0 1 2 3 4
 }
 
 // values()
 for (let index of arr.values()) {
     console.log(index); // 一次返回 1 2 3 4 5
 }
 
 // entries()
 for (let index of arr.entries()) {
     console.log(index); // 一次返回 [0, 1] [1, 2] [2, 3] [3, 4] [4, 5]
 }
 
  let arr = [1, 2, 3, 4, 5]
 
 // Array.from(): 遍历的可以是伪数组，如 String、Set结构，Node节点
 let arr1 = Array.from([1, 3, 5], (item) => {
     return item * 2;
 })
 console.log(arr1) // [2, 6, 10] 
 
 // fill(): 三个参数 (target, start = 0, end = this.length)
 // target: 目标的位置
 // start: 开始位置，可以省略，可以是负数。
 // end: 结束位置，可以省略，可以是负数，实际位置是end-1。
 console.log(arr.fill(7)) // [7, 7, 7, 7, 7]
 console.log(arr.fill(7, 1, 3)) // [1, 7, 7, 4, 5]
 
 let arr = [1, 2, 3, 4]
 
 //includes()
 console.log(arr.includes(3)) // true
 console.log([1, 2, NaN].includes(NaN)); // true

 let arr = [1, 2, 3, 4]
 
 // flatMap()
 console.log(arr.map((x) => [x * 2])); // [ [ 2 ], [ 4 ], [ 6 ], [ 8 ] ]
 console.log(arr.flatMap((x) => [x * 2])); // [ 2, 4, 6, 8 ]
 console.log(arr.flatMap((x) => [[x * 2]])); // [ [ 2 ], [ 4 ], [ 6 ], [ 8 ] ]
 
 const arr1 = [0, 1, 2, [3, 4]];
 const arr2 = [0, 1, 2, [[[3, 4]]]];

 console.log(arr1.flat()); // [ 0, 1, 2, 3, 4 ]
 console.log(arr2.flat(2)); // [ 0, 1, 2, [ 3, 4 ] ]
 console.log(arr2.flat(Infinity)); // [ 0, 1, 2, 3, 4 ]
```

## 字符串新特性

- `Unicode`：`大括号包含`表示Unicode字符
- `codePointAt()` ： 返回字符对应码点，与`fromCharCode()` 对应
- `String.fromCharCode()` ：将对应的码点返回为字符，与`codePointAt()` 对应
- `String.raw()` ：返回把字符串所有变量替换且对斜杠进行转义的结果
- `startsWith()` ： 返回布尔值，表示参数字符串是否存在元字符串的头部
- `endsWith` ：返回布尔值，表示参数字符串是否存在源字符串的头部
- `repart()` ：返回一个新字符串，表示将原字符串重复n次
- `includes()` ：返回布尔值，表示是否找到了参数字符串
- `trimStart()` ：方法从字符串的开头删除空格，`trimLeft()` 是此方法的别名
- `trimEnd()` ：方法从字符串的末端删除空格，`trimRight()` 是此方法的别名
- `padStart()` ： 用于头部补全
- `padEnd()` ： 用于尾部补全
- `JSON.stringify()` : 可返回不符合UTF-8标准的字符串
- `replace()` ：仅`替换一个` 字符串中某模式的首个实例
- `replaceAll()` ： 返回一个新字符串，该字符串中用一个替换项替换了原字符串所有匹配了模式的部分
- `模式可以是一个字符串或一个正则表达式，而替换项可以是一个字符串或一个应用于每个匹配项的函数`

```js
 //Unicode
 console.log("a", "\u0061"); // a a
 console.log("d", "\u{4E25}"); // d 严
 
 let str = 'Domesy'
 
 //codePointAt()
 console.log(str.codePointAt(0)) // 68
 
 //String.fromCharCode()
 console.log(String.fromCharCode(68)) // D
 
 //String.raw()
 console.log(String.raw`Hi\n${1 + 2}`); // Hi\n3
 console.log(`Hi\n${1 + 2}`); // Hi 3
 
 let str = 'Domesy'
 
 //startsWith()
 console.log(str.startsWith("D")) // true
 console.log(str.startsWith("s")) // false

 //endsWith()
 console.log(str.endsWith("y")) // true
 console.log(str.endsWith("s")) // false
 
 //repeat(): 所传的参数会自动向上取整，如果是字符串会转化为数字
 console.log(str.repeat(2)) // DomesyDomesy
 console.log(str.repeat(2.9)) // DomesyDomesy
 
 // 遍历：for-of
  for(let code of str){
    console.log(code) // 一次返回 D o m e s y
  }
  
  //includes()
  console.log(str.includes("s")) // true
  console.log(str.includes("a")) // false
  
  // trimStart()
  const string = "   Hello world!   ";
  console.log(string.trimStart()); // "Hello world!   "
  console.log(string.trimLeft()); // "Hello world!   "
  
  // trimEnd()
  const string = "   Hello world!   ";
  console.log(string.trimEnd()); // "   Hello world!"
  console.log(string.trimRight()); // "   Hello world!"

 let str = 'Domesy'
 
 //padStart(): 会以空格的形式补位吗，这里用0代替，第二个参数会定义一个模板形式，会以模板进行替换
 console.log("1".padStart(2, "0")); // 01
 console.log("8-27".padStart(10, "YYYY-0M-0D")); //  YYYY-08-27
  
 // padEnd()：与padStart()用法相同
 console.log("1".padEnd(2, "0")); // 10

 //JSON.stringify() 升级
 console.log(JSON.stringify("\uD83D\uDE0E")); // 😎
 console.log(JSON.stringify("\u{D800}")); // \ud800

 let str = "Hi！，这是ES6~ES12的新特性，目前为ES12"
 
 console.log(str.replace("ES", "SY")); // Hi！，这是SY6~ES12的新特性，目前为ES12
 console.log(str.replace(/ES/g, "Sy")); // Hi！，这是Sy6~Sy12的新特性，目前为Sy12
 
 console.log(str.replaceAll("ES", "Sy")); // Hi！，这是Sy6~Sy12的新特性，目前为Sy12
 console.log(str.replaceAll(/ES/g, "Sy")); // Hi！，这是Sy6~Sy12的新特性，目前为Sy12


```

