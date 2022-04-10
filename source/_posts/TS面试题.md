---
title: TS面试题
date: 2022-04-10 20:15:36
tags: 面试
categories: 知识点
---



### 第一题

```typescript
type User = {
  id: number;
  kind: string;
};

function makeCustomer<T extends User>(u: T): T {
  // Error（TS 编译器版本：v4.4.2）
  // Type '{ id: number; kind: string; }' is not assignable to type 'T'.
  // '{ id: number; kind: string; }' is assignable to the constraint of type 'T', 
  // but 'T' could be instantiated with a different subtype of constraint 'User'.
  return {
    id: u.id,
    kind: 'customer'
  }
}
复制代码
```

以上代码为什么会提示错误，应该如何解决上述问题？

### 第二题

本道题我们希望参数 `a` 和 `b` 的类型都是一致的，即 `a` 和 `b` 同时为 `number` 或 `string` 类型。当它们的类型不一致的值，TS 类型检查器能自动提示对应的错误信息。

```typescript
function f(a: string | number, b: string | number) {
  if (typeof a === 'string') {
    return a + ':' + b; // no error but b can be number!
  } else {
    return a + b; // error as b can be number | string
  }
}

f(2, 3); // Ok
f(1, 'a'); // Error
f('a', 2); // Error
f('a', 'b') // Ok
复制代码
```

### 第三题

在 [掌握 TS 这些工具类型，让你开发事半功倍](https://link.juejin.cn?target=https%3A%2F%2Fmp.weixin.qq.com%2Fs%3F__biz%3DMzI2MjcxNTQ0Nw%3D%3D%26mid%3D2247484142%26idx%3D1%26sn%3D946ba90d10e2625513f09e60a462b3a7%26chksm%3Dea47a3b6dd302aa05af716d0bd70d8d7c682c9f4527a9a0c03cd107635711c394ab155127f9e%26scene%3D21%23wechat_redirect) 这篇文章中，阿宝哥介绍了 TS 内置的工具类型：`Partial<T>`，它的作用是将某个类型里的属性全部变为可选项 `?`。

```typescript
interface Todo {
  title: string;
  description: string;
}

function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
  return { ...todo, ...fieldsToUpdate };
}

// lib.es5.d.ts
type Partial<T> = {
  [P in keyof T]?: T[P];
};
复制代码
```

那么如何定义一个 `SetOptional` 工具类型，支持把给定的 keys 对应的属性变成可选的？对应的使用示例如下所示：

```typescript
type Foo = {
	a: number;
	b?: string;
	c: boolean;
}

// 测试用例
type SomeOptional = SetOptional<Foo, 'a' | 'b'>;

// type SomeOptional = {
// 	a?: number; // 该属性已变成可选的
// 	b?: string; // 保持不变
// 	c: boolean; 
// }
复制代码
```

在实现 `SetOptional` 工具类型之后，如果你感兴趣，可以继续实现 `SetRequired` 工具类型，利用它可以把指定的 keys 对应的属性变成必填的。对应的使用示例如下所示：

```typescript
type Foo = {
	a?: number;
	b: string;
	c?: boolean;
}

// 测试用例
type SomeRequired = SetRequired<Foo, 'b' | 'c'>;
// type SomeRequired = {
// 	a?: number;
// 	b: string; // 保持不变
// 	c: boolean; // 该属性已变成必填
// }
复制代码
```

### 第四题

`Pick<T, K extends keyof T>` 的作用是将某个类型中的子属性挑出来，变成包含这个类型部分属性的子类型。

```typescript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = Pick<Todo, "title" | "completed">;

const todo: TodoPreview = {
  title: "Clean room",
  completed: false
};
复制代码
```

那么如何定义一个 `ConditionalPick` 工具类型，支持根据指定的 `Condition` 条件来生成新的类型，对应的使用示例如下：

```typescript
interface Example {
	a: string;
	b: string | number;
	c: () => void;
	d: {};
}

// 测试用例：
type StringKeysOnly = ConditionalPick<Example, string>;
//=> {a: string}
复制代码
```

### 第五题

定义一个工具类型 `AppendArgument`，为已有的函数类型增加指定类型的参数，新增的参数名是 `x`，将作为新函数类型的第一个参数。具体的使用示例如下所示：

```typescript
type Fn = (a: number, b: string) => number
type AppendArgument<F, A> = // 你的实现代码

type FinalFn = AppendArgument<Fn, boolean> 
// (x: boolean, a: number, b: string) => number
复制代码
```

### 第六题

定义一个 NativeFlat 工具类型，支持把数组类型拍平（扁平化）。具体的使用示例如下所示：

```typescript
type NaiveFlat<T extends any[]> = // 你的实现代码

// 测试用例：
type NaiveResult = NaiveFlat<[['a'], ['b', 'c'], ['d']]>
// NaiveResult的结果： "a" | "b" | "c" | "d"
复制代码
```

在完成 `NaiveFlat` 工具类型之后，在继续实现 `DeepFlat` 工具类型，以支持多维数组类型：

```typescript
type DeepFlat<T extends any[]> = unknown // 你的实现代码

// 测试用例
type Deep = [['a'], ['b', 'c'], [['d']], [[[['e']]]]];
type DeepTestResult = DeepFlat<Deep>  
// DeepTestResult: "a" | "b" | "c" | "d" | "e"
复制代码
```

### 第七题

使用类型别名定义一个 `EmptyObject` 类型，使得该类型只允许空对象赋值：

```typescript
type EmptyObject = {} 

// 测试用例
const shouldPass: EmptyObject = {}; // 可以正常赋值
const shouldFail: EmptyObject = { // 将出现编译错误
  prop: "TS"
}
复制代码
```

在通过 `EmptyObject` 类型的测试用例检测后，我们来更改以下 `takeSomeTypeOnly` 函数的类型定义，让它的参数只允许严格SomeType类型的值。具体的使用示例如下所示：

```typescript
type SomeType =  {
  prop: string
}

// 更改以下函数的类型定义，让它的参数只允许严格SomeType类型的值
function takeSomeTypeOnly(x: SomeType) { return x }

// 测试用例：
const x = { prop: 'a' };
takeSomeTypeOnly(x) // 可以正常调用

const y = { prop: 'a', addditionalProp: 'x' };
takeSomeTypeOnly(y) // 将出现编译错误
复制代码
```

### 第八题

定义 `NonEmptyArray` 工具类型，用于确保数据非空数组。

```typescript
type NonEmptyArray<T> = // 你的实现代码

const a: NonEmptyArray<string> = [] // 将出现编译错误
const b: NonEmptyArray<string> = ['Hello TS'] // 非空数据，正常使用
复制代码
```

> 提示：该题目有多种解法，感兴趣小伙伴可以自行尝试一下。

### 第九题

定义一个 `JoinStrArray` 工具类型，用于根据指定的 `Separator` 分隔符，对字符串数组类型进行拼接。具体的使用示例如下所示：

```typescript
type JoinStrArray<Arr extends string[], Separator extends string, Result extends string = ""> = // 你的实现代码

// 测试用例
type Names = ["Sem", "Lolo", "Kaquko"]
type NamesComma = JoinStrArray<Names, ","> // "Sem,Lolo,Kaquko"
type NamesSpace = JoinStrArray<Names, " "> // "Sem Lolo Kaquko"
type NamesStars = JoinStrArray<Names, "⭐️"> // "Sem⭐️Lolo⭐️Kaquko"
复制代码
```

### 第十题

实现一个 `Trim` 工具类型，用于对字符串字面量类型进行去空格处理。具体的使用示例如下所示：

```typescript
type Trim<V extends string> = // 你的实现代码

// 测试用例
Trim<' semlinker '>
//=> 'semlinker'
复制代码
```

> 提示：可以考虑先定义 TrimLeft 和 TrimRight 工具类型，再组合成 Trim 工具类型。

### 第十一题

实现一个 `IsEqual` 工具类型，用于比较两个类型是否相等。具体的使用示例如下所示：

```typescript
type IsEqual<A, B> = // 你的实现代码

// 测试用例
type E0 = IsEqual<1, 2>; // false
type E1 = IsEqual<{ a: 1 }, { a: 1 }> // true
type E2 = IsEqual<[1], []>; // false
复制代码
```

### 第十二题

实现一个 `Head` 工具类型，用于获取数组类型的第一个类型。具体的使用示例如下所示：

```typescript
type Head<T extends Array<any>> = // 你的实现代码

// 测试用例
type H0 = Head<[]> // never
type H1 = Head<[1]> // 1
type H2 = Head<[3, 2]> // 3
复制代码
```

> 提示：该题目有多种解法，感兴趣小伙伴可以自行尝试一下。

### 第十三题

实现一个 `Tail` 工具类型，用于获取数组类型除了第一个类型外，剩余的类型。具体的使用示例如下所示：

```typescript
type Tail<T extends Array<any>> =  // 你的实现代码

// 测试用例
type T0 = Tail<[]> // []
type T1 = Tail<[1, 2]> // [2]
type T2 = Tail<[1, 2, 3, 4, 5]> // [2, 3, 4, 5]
复制代码
```

> 提示：该题目有多种解法，感兴趣小伙伴可以自行尝试一下。

### 第十四题

实现一个 `Unshift` 工具类型，用于把指定类型 `E` 作为第一个元素添加到 `T` 数组类型中。具体的使用示例如下所示：

```typescript
type Unshift<T extends any[], E> =  // 你的实现代码

// 测试用例
type Arr0 = Unshift<[], 1>; // [1]
type Arr1 = Unshift<[1, 2, 3], 0>; // [0, 1, 2, 3]
复制代码
```

> 提示：该题目有多种解法，感兴趣小伙伴可以自行尝试一下。

### 第十五题

实现一个 `Shift` 工具类型，用于移除 `T` 数组类型中的第一个类型。具体的使用示例如下所示：

```typescript
type Shift<T extends any[]> = // 你的实现代码

// 测试用例
type S0 = Shift<[1, 2, 3]> // [2, 3]
type S1 = Shift<[string,number,boolean]> // [number,boolean]
复制代码
```

### 第十六题

实现一个 `Push` 工具类型，用于把指定类型 `E` 作为最后一个元素添加到 `T` 数组类型中。具体的使用示例如下所示：

```typescript
type Push<T extends any[], V> = // 你的实现代码

// 测试用例
type Arr0 = Push<[], 1> // [1]
type Arr1 = Push<[1, 2, 3], 4> // [1, 2, 3, 4]
复制代码
```

### 第十七题

实现一个 `Includes` 工具类型，用于判断指定的类型 `E` 是否包含在 `T` 数组类型中。具体的使用示例如下所示：

```typescript
type Includes<T extends Array<any>, E> = // 你的实现代码

type I0 = Includes<[], 1> // false
type I1 = Includes<[2, 2, 3, 1], 2> // true
type I2 = Includes<[2, 3, 3, 1], 1> // true
复制代码
```

> 提示：该题目有多种解法，感兴趣小伙伴可以自行尝试一下。

### 第十八题

实现一个 `UnionToIntersection` 工具类型，用于把联合类型转换为交叉类型。具体的使用示例如下所示：

```typescript
type UnionToIntersection<U> = // 你的实现代码

// 测试用例
type U0 = UnionToIntersection<string | number> // never
type U1 = UnionToIntersection<{ name: string } | { age: number }> // { name: string; } & { age: number; }
复制代码
```

### 第十九题

实现一个 `OptionalKeys` 工具类型，用来获取对象类型中声明的可选属性。具体的使用示例如下所示：

```typescript
type Person = {
  id: string;
  name: string;
  age: number;
  from?: string;
  speak?: string;
};

type OptionalKeys<T> = // 你的实现代码
type PersonOptionalKeys = OptionalKeys<Person> // "from" | "speak"
复制代码
```

> 提示：该题目有多种解法，感兴趣小伙伴可以自行尝试一下。

### 第二十题

实现一个 `Curry` 工具类型，用来实现函数类型的柯里化处理。具体的使用示例如下所示：

```typescript
type Curry<
  F extends (...args: any[]) => any,
  P extends any[] = Parameters<F>, 
  R = ReturnType<F> 
> = // 你的实现代码

type F0 = Curry<() => Date>; // () => Date
type F1 = Curry<(a: number) => Date>; // (arg: number) => Date
type F2 = Curry<(a: number, b: string) => Date>; //  (arg_0: number) => (b: string) => Date
复制代码
```

### 第二十一题

实现一个 `Merge` 工具类型，用于把两个类型合并成一个新的类型。第二种类型（SecondType）的 `Keys` 将会覆盖第一种类型（FirstType）的 `Keys`。具体的使用示例如下所示：

```typescript
type Foo = {
	a: number;
	b: string;
};

type Bar = {
	b: number;
};

type Merge<FirstType, SecondType> = // 你的实现代码

const ab: Merge<Foo, Bar> = { a: 1, b: 2 };
复制代码
```

### 第二十二题

实现一个 `RequireAtLeastOne` 工具类型，它将创建至少含有一个给定 `Keys` 的类型，其余的 `Keys` 保持原样。具体的使用示例如下所示：

```typescript
type Responder = {
	text?: () => string;
	json?: () => string;
	secure?: boolean;
};

type RequireAtLeastOne<
	ObjectType,
	KeysType extends keyof ObjectType = keyof ObjectType,
> = // 你的实现代码

// 表示当前类型至少包含 'text' 或 'json' 键
const responder: RequireAtLeastOne<Responder, 'text' | 'json'> = {
	json: () => '{"message": "ok"}',
	secure: true
};
复制代码
```

### 第二十三题

实现一个 `RemoveIndexSignature` 工具类型，用于移除已有类型中的索引签名。具体的使用示例如下所示：

```typescript
interface Foo {
  [key: string]: any;
  [key: number]: any;
  bar(): void;
}

type RemoveIndexSignature<T> = // 你的实现代码

type FooWithOnlyBar = RemoveIndexSignature<Foo>; //{ bar: () => void; }
复制代码
```

### 第二十四题

实现一个 `Mutable` 工具类型，用于移除对象类型上所有属性或部分属性的 `readonly` 修饰符。具体的使用示例如下所示：

```typescript
type Foo = {
  readonly a: number;
  readonly b: string;
  readonly c: boolean;
};

type Mutable<T, Keys extends keyof T = keyof T> = // 你的实现代码

const mutableFoo: Mutable<Foo, 'a'> = { a: 1, b: '2', c: true };

mutableFoo.a = 3; // OK
mutableFoo.b = '6'; // Cannot assign to 'b' because it is a read-only property.
复制代码
```

### 第二十五题

实现一个 `IsUnion` 工具类型，判断指定的类型是否为联合类型。具体的使用示例如下所示：

```typescript
type IsUnion<T, U = T> = // 你的实现代码

type I0 = IsUnion<string|number> // true
type I1 = IsUnion<string|never> // false
type I2 =IsUnion<string|unknown> // false
复制代码
```

### 第二十六题

实现一个 `IsNever` 工具类型，判断指定的类型是否为 `never` 类型。具体的使用示例如下所示：

```typescript
type I0 = IsNever<never> // true
type I1 = IsNever<never | string> // false
type I2 = IsNever<null> // false
复制代码
```

### 第二十七题

实现一个 `Reverse` 工具类型，用于对元组类型中元素的位置颠倒，并返回该数组。元组的第一个元素会变成最后一个，最后一个元素变成第一个。

```typescript
type Reverse<
  T extends Array<any>,
  R extends Array<any> = []
> = // 你的实现代码

type R0 = Reverse<[]> // []
type R1 = Reverse<[1, 2, 3]> // [3, 2, 1]
复制代码
```

### 第二十八题

实现一个 `Split` 工具类型，根据给定的分隔符（Delimiter）对包含分隔符的字符串进行切割。可用于定义 `String.prototype.split` 方法的返回值类型。具体的使用示例如下所示：

```typescript
type Item = 'semlinker,lolo,kakuqo';

type Split<
	S extends string, 
	Delimiter extends string,
> = // 你的实现代码

type ElementType = Split<Item, ','>; // ["semlinker", "lolo", "kakuqo"]
复制代码
```

### 第二十九题

实现一个 `ToPath` 工具类型，用于把属性访问（`.` 或 `[]`）路径转换为元组的形式。具体的使用示例如下所示：

```typescript
type ToPath<S extends string> = // 你的实现代码

ToPath<'foo.bar.baz'> //=> ['foo', 'bar', 'baz']
ToPath<'foo[0].bar.baz'> //=> ['foo', '0', 'bar', 'baz']
复制代码
```

### 第三十题

完善 `Chainable` 类型的定义，使得 TS 能成功推断出 `result` 变量的类型。调用 `option` 方法之后会不断扩展当前对象的类型，使得调用 `get` 方法后能获取正确的类型。

```typescript
declare const config: Chainable

type Chainable = {
  option(key: string, value: any): any
  get(): any
}

const result = config
  .option('age', 7)
  .option('name', 'lolo')
  .option('address', { value: 'XiaMen' })
  .get()

type ResultType = typeof result  
// 期望 ResultType 的类型是：
// {
//   age: number
//   name: string
//   address: {
//     value: string
//   }
// }
复制代码
```





##  ts中的访问修饰符

- public，任何地方
- private，只能在类的内部访问
- protected，能在类的内部访问和子类中访问
- readonly，属性设置为只读

##  const和readonly的区别

1. const用于变量，readonly用于属性
2. const在运行时检查，readonly在编译时检查
3. 使用const变量保存的数组，可以使用push，pop等方法。但是如果使用`ReadonlyArray<number>`声明的数组不能使用push，pop等方法。

##  枚举和常量枚举（const枚举）的区别

1. 枚举会被编译时会编译成一个对象，可以被当作对象使用
2. const枚举会在ts编译期间被删除，避免额外的性能开销

```ts
// 普通枚举
enum Witcher {
  Ciri = 'Queen',
  Geralt = 'Geralt of Rivia'
}
function getGeraltMessage(arg: {[key: string]: string}): string {
  return arg.Geralt
}
getGeraltMessage(Witcher) // Geralt of Rivia
复制代码
// const枚举
const enum Witcher {
  Ciri = 'Queen',
  Geralt = 'Geralt of Rivia'
}
const witchers: Witcher[] = [Witcher.Ciri, Witcher.Geralt]
// 编译后
// const witchers = ['Queen', 'Geralt of Rivia'
复制代码
```

##  ts中interface可以给Function/Array/Class做声明吗？

```ts
// 函数类型
interface SearchFunc {
  (source: string, subString: string): boolean;
}
let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
}
复制代码
// Array
interface StringArray {
  [index: number]: string;
}

let myArray: StringArray;
myArray = ["Bob", "Fred"];
复制代码
// Class, constructor存在于类的静态部分，所以不会检查
interface ClockInterface {
    currentTime: Date;
    setTime(d: Date);
}

class Clock implements ClockInterface {
    currentTime: Date;
    setTime(d: Date) {
        this.currentTime = d;
    }
    constructor(h: number, m: number) { }
}
复制代码
```

## ts中的this和js中的this有什么差异？

不了解

##  ts中如何枚举联合类型的key?

```ts
type Name = { name: string }
type Age = { age: number }
type Union = Name | Age

type UnionKey<P> = P extends infer P ? keyof P : never

type T = UnionKey<Union>
复制代码
```

##  ts中 ?.、??、!.、_、** 等符号的含义？

- ?.  可选链
- ??  ?? 类似与短路或，??避免了一些意外情况0，NaN以及"",false被视为false值。只有undefind,null被视为false值。
- !.  在变量名后添加!，可以断言排除undefined和null类型
- _ , 声明该函数将被传递一个参数，但您并不关心它
- ** 求幂
- !:，待会分配这个变量，ts不要担心

```ts
// ??
let x = foo ?? bar();
// 等价于
let x = foo !== null && foo !== undefined ? foo : bar();

// !.
let a: string | null | undefined
a.length // error
a!.length // ok
复制代码
```

##  什么是抗变、双变、协变和逆变？

- Covariant 协变，TS对象兼容性是协变，父类 <= 子类，是可以的。子类 <= 父类，错误。
- Contravariant 逆变，禁用`strictFunctionTypes`编译，函数参数类型是逆变的，父类 <= 子类，是错误。子类 <= 父类，是可以的。
- Bivariant 双向协变，函数参数的类型默认是双向协变的。父类 <= 子类，是可以的。子类 <= 父类，是可以的。

##  ts中同名的interface或者同名的interface和class可以合并吗？

1. interface会合并
2. class不可以合并

##  如何使ts项目引入并识别编译为js的npm库包？

1. `npm install @types/xxxx`
2. 自己添加描述文件

##  ts如何自动生成库包的声明文件？

可以配置`tsconfig.json`文件中的`declaration`和`outDir`

1. declaration: true, 将会自动生成声明文件
2. outDir: '', 指定目录

##  什么是泛型

泛型用来来创建可重用的组件，一个组件可以支持多种类型的数据。这样用户就可以以自己的数据类型来使用组件。**简单的说，“泛型就是把类型当成参数”。**

##  -?，-readonly 是什么含义

用于删除修饰符

```ts
type A = {
    a: string;
    b: number;
}

type B = {
    [K in keyof A]?: A[K]
}

type C = {
    [K in keyof B]-?: B[K]
}

type D = {
    readonly [K in keyof A]: A[K]
}

type E = {
    -readonly [K in keyof A]: A[K]
}
复制代码
```

##  TS是基于结构类型兼容

typescript的类型兼容是基于结构的，不是基于名义的。下面的代码在ts中是完全可以的，但在java等基于名义的语言则会抛错。

```ts
interface Named { name: string }
class Person {
  name: string
}
let p: Named
// ok
p = new Person()
复制代码
```

##  const断言

const断言，typescript会为变量添加一个自身的字面量类型

1. 对象字面量的属性，获得readonly的属性，成为只读属性
2. 数组字面量成为readonly tuple只读元组
3. 字面量类型不能被扩展（比如从hello类型到string类型）

```ts
// type '"hello"'
let x = "hello" as const
// type 'readonly [10, 20]'
let y = [10, 20] as const
// type '{ readonly text: "hello" }'
let z = { text: "hello" } as const
复制代码
```

##  type 和 interface 的区别

1. 类型别名可以为任何类型引入名称。例如基本类型，联合类型等
2. 类型别名不支持继承
3. 类型别名不会创建一个真正的名字
4. 类型别名无法被实现(implements)，而接口可以被派生类实现
5. 类型别名重名时编译器会抛出错误，接口重名时会产生合并

##  implements 与 extends 的区别

- extends, 子类会继承父类的所有属性和方法。
- implements，使用implements关键字的类将需要实现需要实现的类的所有属性和方法。

##  枚举和 object 的区别

1. 枚举可以通过枚举的名称，获取枚举的值。也可以通过枚举的值获取枚举的名称。
2. object只能通过key获取value
3. 数字枚举在不指定初始值的情况下，枚举值会从0开始递增。
4. 虽然在运行时，枚举是一个真实存在的对象。但是使用keyof时的行为却和普通对象不一致。必须使用keyof typeof才可以获取枚举所有属性名。

##  never, void 的区别

- never，never表示永远不存在的类型。比如一个函数总是抛出错误，而没有返回值。或者一个函数内部有死循环，永远不会有返回值。函数的返回值就是never类型。
- void, 没有显示的返回值的函数返回值为void类型。如果一个变量为void类型，只能赋予undefined或者null。

## unknown, any的区别

unknown类型和any类型类似。与any类型不同的是。unknown类型可以接受任意类型赋值，但是unknown类型赋值给其他类型前，必须被断言

##  如何在 window 扩展类型

```ts
declare global {
  interface Window {
    myCustomFn: () => void;
  }
}
复制代码
```

## 复杂的类型推导题目

### implement UnionToIntersection

```ts
type A = UnionToIntersection<{a: string} | {b: string} | {c: string}> 
// {a: string} & {b: string} & {c: string}

// 实现UnionToIntersection<T>
type UnionToIntersection<U> = 
  (U extends any ? (k: U) => void : never) extends ((k: infer I) => void) ? I : never
// https://stackoverflow.com/questions/50374908/transform-union-type-to-intersection-type
// https://jkchao.github.io/typescript-book-chinese/tips/infer.html#%E4%B8%80%E4%BA%9B%E7%94%A8%E4%BE%8B
复制代码
```

###  implement ToNumber

```ts
type A = ToNumber<'1'> // 1
type B = ToNumber<'40'> // 40
type C = ToNumber<'0'> // 0

// 实现ToNumber
type ToNumber<T extends string, R extends any[] = []> =
    T extends `${R['length']}` ? R['length'] : ToNumber<T, [1, ...R]>;
复制代码
```

###  implement Add<A, B>

```ts
type A = Add<1, 2> // 3
type B = Add<0, 0> // 0

// 实现ADD
type NumberToArray<T, R extends any[]> = T extends R['length'] ? R : NumberToArray<T, [1, ...R]>
type Add<T, R> = [...NumberToArray<T, []>, ...NumberToArray<R, []>]['length']
复制代码
```

###  implement SmallerThan<A, B>

```ts
type A = SmallerThan<0, 1> // true
type B = SmallerThan<1, 0> // false
type C = SmallerThan<10, 9> // false

// 实现SmallerThan
type SmallerThan<N extends number, M extends number, L extends any[] = [], R extends any[] = []> = 
    N extends L['length'] ? 
        M extends R['length'] ? false : true
        :
        M extends R['length'] ? false : SmallerThan<N, M, [1, ...L], [1, ...R]>;
复制代码
```

###  implement LargerThan<A, B>

```ts
type A = LargerThan<0, 1> // false
type B = LargerThan<1, 0> // true
type C = LargerThan<10, 9> // true

// 实现LargerThan
type LargerThan<N extends number, M extends number, L extends any[] = [], R extends any[] = []> =
    N extends L['length'] ?
        false : M extends R['length'] ?
            true : LargerThan<N, M, [1, ...L], [1, ...R]>;
复制代码
```

###  implement IsAny

```ts
type A = IsAny<string> // false
type B = IsAny<any> // true
type C = IsAny<unknown> // false
type D = IsAny<never> // false

// 实现IsAny
type IsAny<T> = true extends (T extends never ? true : false) ?
                  false extends (T extends never ? true : false) ?
                    true
                    :
                    false
                  :
                  false;

// 更简单的实现
type IsAny<T> = 0 extends (T & 1) ? true : false;
复制代码
```

###  implement Filter<T, A>

```ts
type A = Filter<[1,'BFE', 2, true, 'dev'], number> // [1, 2]
type B = Filter<[1,'BFE', 2, true, 'dev'], string> // ['BFE', 'dev']
type C = Filter<[1,'BFE', 2, any, 'dev'], string> // ['BFE', any, 'dev']

// 实现Filter
type Filter<T extends any[], A, N extends any[] = []> =
    T extends [infer P, ...infer Q] ?
        0 extends (P & 1) ? Filter<Q, A, [...N, P]> : 
        P extends A ? Filter<Q, A, [...N, P]> : Filter<Q, A, N>
        : N;
复制代码
```

###  implement TupleToString

```ts
type A = TupleToString<['a']> // 'a'
type B = TupleToString<['B', 'F', 'E']> // 'BFE'
type C = TupleToString<[]> // ''

// 实现TupleToString
type TupleToString<T extends any[], S extends string = '', A extends any[] = []> =
    A['length'] extends T['length'] ? S : TupleToString<T, `${S}${T[A['length']]}`, [1, ...A]>
复制代码
```

###  implement RepeatString<T, C>

```ts
type A = RepeatString<'a', 3> // 'aaa'
type B = RepeatString<'a', 0> // ''

// 实现RepeatString
type RepeatString<T extends string, C extends number, S extends string = '', A extends any[] = []> =
    A['length'] extends C ? S : RepeatString<T, C, `${T}${S}`, [1, ...A]>
复制代码
```

###  implement Push<T, I>

```ts
type A = Push<[1,2,3], 4> // [1,2,3,4]
type B = Push<[1], 2> // [1, 2]
type C = Push<[], string> // [string]

// 实现Push
type Push<T extends any[], I> = T extends [...infer P] ? [...P, I] : [I]
复制代码
```

###  implement Flat

```ts
type A = Flat<[1,2,3]> // [1,2,3]
type B = Flat<[1,[2,3], [4,[5,[6]]]]> // [1,2,3,4,5,6]
type C = Flat<[]> // []

// 实现Flat
type Flat<T extends any[]> =
    T extends [infer P, ...infer Q] ?
        P extends any[] ? [...Flat<P>, ...Flat<Q>] : [P, ...Flat<Q>]
        : [];
复制代码
```

###  implement Shift

```ts
type A = Shift<[1,2,3]> // [2,3]
type B = Shift<[1]> // []
type C = Shift<[]> // []

// 实现Shift
type Shift<T extends any[]> = T extends [infer P, ...infer Q] ? [...Q] : [];
复制代码
```

###  implement Repeat<T, C>

```ts
type A = Repeat<number, 3> // [number, number, number]
type B = Repeat<string, 2> // [string, string]
type C = Repeat<1, 1> // [1, 1]
type D = Repeat<0, 0> // []

// 实现Repeat
type Repeat<T, C, R extends any[] = []> = 
    R['length'] extends C ? R : Repeat<T, C, [...R, T]>
复制代码
```

###  implement ReverseTuple

```ts
type A = ReverseTuple<[string, number, boolean]> // [boolean, number, string]
type B = ReverseTuple<[1,2,3]> // [3,2,1]
type C = ReverseTuple<[]> // []

// 实现ReverseTuple
type ReverseTuple<T extends any[], A extends any[] = []> =
    T extends [...infer Q, infer P] ? 
        A['length'] extends T['length'] ? A : ReverseTuple<Q, [...A, P]>
        : A;
复制代码
```

###  implement UnwrapPromise

```ts
type A = UnwrapPromise<Promise<string>> // string
type B = UnwrapPromise<Promise<null>> // null
type C = UnwrapPromise<null> // Error

// 实现UnwrapPromise
type UnwrapPromise<T> = T extends Promise<infer P> ? P : Error;
复制代码
```

###  implement LengthOfString

```ts
type A = LengthOfString<'BFE.dev'> // 7
type B = LengthOfString<''> // 0

// 实现LengthOfString
type LengthOfString<T extends string, A extends any[] = []> =
    T extends `${infer P}${infer Q}` ? LengthOfString<Q, [1, ...A]> : A['length']
复制代码
```

###  implement StringToTuple

```ts
type A = StringToTuple<'BFE.dev'> // ['B', 'F', 'E', '.', 'd', 'e','v']
type B = StringToTuple<''> // []

// 实现
type StringToTuple<T extends string, A extends any[] = []> =
    T extends `${infer K}${infer P}` ? StringToTuple<P, [...A, K]> : A;
复制代码
```

###  implement LengthOfTuple

```ts
type A = LengthOfTuple<['B', 'F', 'E']> // 3
type B = LengthOfTuple<[]> // 0

// 实现
type LengthOfTuple<T extends any[], R extends any[] = []> =
    R['length'] extends T['length'] ? R['length'] : LengthOfTuple<T, [...R, 1]>
复制代码
```

###  implement LastItem

```ts
type A = LastItem<[string, number, boolean]> // boolean
type B = LastItem<['B', 'F', 'E']> // 'E'
type C = LastItem<[]> // never

// 实现LastItem
type LastItem<T> = T extends [...infer P, infer Q] ? Q : never;
复制代码
```

###  implement FirstItem

```ts
type A = FirstItem<[string, number, boolean]> // string
type B = FirstItem<['B', 'F', 'E']> // 'B'

// 实现FirstItem
type FirstItem<T> = T extends [infer P, ...infer Q] ? P : never;
复制代码
```

###  implement FirstChar

```ts
type A = FirstChar<'BFE'> // 'B'
type B = FirstChar<'dev'> // 'd'
type C = FirstChar<''> // never

// 实现FirstChar
type FirstChar<T> = T extends `${infer P}${infer Q}` ? P : never;
复制代码
```

###  implement Pick<T, K>

```ts
type Foo = {
  a: string
  b: number
  c: boolean
}

type A = MyPick<Foo, 'a' | 'b'> // {a: string, b: number}
type B = MyPick<Foo, 'c'> // {c: boolean}
type C = MyPick<Foo, 'd'> // Error

// 实现MyPick<T, K>
type MyPick<T, K extends keyof T> = {
    [Key in K]: T[Key]
}
复制代码
```

###  implement Readonly

```ts
type Foo = {
  a: string
}

const a:Foo = {
  a: 'BFE.dev',
}
a.a = 'bigfrontend.dev'
// OK

const b:MyReadonly<Foo> = {
  a: 'BFE.dev'
}
b.a = 'bigfrontend.dev'
// Error

// 实现MyReadonly
type MyReadonly<T> = {
    readonly [K in keyof T]: T[K]
}
复制代码
```

###  implement Record<K, V>

```ts
type Key = 'a' | 'b' | 'c'

const a: Record<Key, string> = {
  a: 'BFE.dev',
  b: 'BFE.dev',
  c: 'BFE.dev'
}
a.a = 'bigfrontend.dev' // OK
a.b = 123 // Error
a.d = 'BFE.dev' // Error

type Foo = MyRecord<{a: string}, string> // Error

// 实现MyRecord
type MyRecord<K extends number | string | symbol, V> = {
    [Key in K]: V
}
复制代码
```

###  implement Exclude

```ts
type Foo = 'a' | 'b' | 'c'

type A = MyExclude<Foo, 'a'> // 'b' | 'c'
type B = MyExclude<Foo, 'c'> // 'a' | 'b
type C = MyExclude<Foo, 'c' | 'd'>  // 'a' | 'b'
type D = MyExclude<Foo, 'a' | 'b' | 'c'>  // never

// 实现 MyExclude<T, K>
type MyExclude<T, K> = T extends K ? never : T;
复制代码
```

###  implement Extract<T, U>

```ts
type Foo = 'a' | 'b' | 'c'

type A = MyExtract<Foo, 'a'> // 'a'
type B = MyExtract<Foo, 'a' | 'b'> // 'a' | 'b'
type C = MyExtract<Foo, 'b' | 'c' | 'd' | 'e'>  // 'b' | 'c'
type D = MyExtract<Foo, never>  // never

// 实现MyExtract<T, U>
type MyExtract<T, U> = T extends U ? T : never
复制代码
```

###  implement Omit<T, K>

```ts
type Foo = {
  a: string
  b: number
  c: boolean
}

type A = MyOmit<Foo, 'a' | 'b'> // {c: boolean}
type B = MyOmit<Foo, 'c'> // {a: string, b: number}
type C = MyOmit<Foo, 'c' | 'd'> // {a: string, b: number}

// 实现MyOmit
type MyOmit<T, K extends number | string | symbol> = {
    [Key in Exclude<keyof T, K>]: T[Key]
}

type MyOmit<T, K extends number | string | symbol> = Pick<T, Exclude<keyof T, K>>
复制代码
```

###  implement NonNullable

```ts
type Foo = 'a' | 'b' | null | undefined

type A = MyNonNullable<Foo> // 'a' | 'b'

// 实现NonNullable
type MyNonNullable<T> = T extends null | undefined ? never : T;
复制代码
```

###  implement Parameters

```ts
type Foo = (a: string, b: number, c: boolean) => string

type A = MyParameters<Foo> // [a:string, b: number, c:boolean]
type B = A[0] // string
type C = MyParameters<{a: string}> // Error

// 实现MyParameters<T>
type MyParameters<T extends (...params: any[]) => any> =
    T extends (...params: [...infer P]) => any ? P : never
复制代码
```

###  implement ConstructorParameters

```ts
class Foo {
  constructor (a: string, b: number, c: boolean) {}
}

type C = MyConstructorParameters<typeof Foo> 
// [a: string, b: number, c: boolean]

// 实现MyConstructorParameters<T>
type MyConstructorParameters<T extends new (...params: any[]) => any> =
    T extends new (...params: [...infer P]) => any ? P : never
复制代码
```

###  implement ReturnType

```ts
type Foo = () => {a: string}

type A = MyReturnType<Foo> // {a: string}

// 实现MyReturnType<T>
type MyReturnType<T extends (...params: any[]) => any> =
    T extends (...params: any[]) => infer P ? P : never;
复制代码
```

###  implement InstanceType

```ts
class Foo {}
type A = MyInstanceType<typeof Foo> // Foo
type B = MyInstanceType<() => string> // Error

// 实现MyInstanceType<T>
type MyInstanceType<T extends new (...params: any[]) => any> =
    T extends new (...params: any[]) => infer P ? P : never;
复制代码
```

###  implement ThisParameterType

```ts
function Foo(this: {a: string}) {}
function Bar() {}

type A = MyThisParameterType<typeof Foo> // {a: string}
type B = MyThisParameterType<typeof Bar> // unknown

// 实现MyThisParameterType<T>
type MyThisParameterType<T extends (this: any, ...params: any[]) => any> =
    T extends (this: infer P, ...params: any[]) => any ? P : unknown;
复制代码
```

###  implement TupleToUnion

```ts
type Foo = [string, number, boolean]

type Bar = TupleToUnion<Foo> // string | number | boolean

// 实现TupleToUnion<T>
type TupleToUnion<T extends any[], R = T[0]> =
    T extends [infer P, ...infer Q] ? TupleToUnion<Q, R | P> : R;

// 其他回答
type TupleToUnion<T extends any[]> = T[number]
复制代码
```

###  implement Partial

```ts
type Foo = {
  a: string
  b: number
  c: boolean
}

// below are all valid

const a: MyPartial<Foo> = {}

const b: MyPartial<Foo> = {
  a: 'BFE.dev'
}

const c: MyPartial<Foo> = {
  b: 123
}

const d: MyPartial<Foo> = {
  b: 123,
  c: true
}

const e: MyPartial<Foo> = {
  a: 'BFE.dev',
  b: 123,
  c: true
}

// 实现MyPartial<T>
type MyPartial<T> = {
    [K in keyof T]?: T[K]
}
复制代码
```

###  Required

```ts
// all properties are optional
type Foo = {
  a?: string
  b?: number
  c?: boolean
}


const a: MyRequired<Foo> = {}
// Error

const b: MyRequired<Foo> = {
  a: 'BFE.dev'
}
// Error

const c: MyRequired<Foo> = {
  b: 123
}
// Error

const d: MyRequired<Foo> = {
  b: 123,
  c: true
}
// Error

const e: MyRequired<Foo> = {
  a: 'BFE.dev',
  b: 123,
  c: true
}
// valid

// 实现MyRequired<T>
type MyRequired<T> = {
    [K in keyof T]-?: T[K]
}
复制代码
```

###  implement LastChar

```ts
type A = LastChar<'BFE'> // 'E'
type B = LastChar<'dev'> // 'v'
type C = LastChar<''> // never

// 实现FirstChar<T>
type LastChar<T extends string, A extends string[] = []> =
    T extends `${infer P}${infer Q}` ?  LastChar<Q, [...A, P]> :
        A extends [...infer L, infer R] ? R : never
;
复制代码
```

###  implement IsNever

```ts
// https://stackoverflow.com/questions/53984650/typescript-never-type-inconsistently-matched-in-conditional-type
// https://www.typescriptlang.org/docs/handbook/advanced-types.html#v
type A = IsNever<never> // true
type B = IsNever<string> // false
type C = IsNever<undefined> // false

// 实现IsNever<T>
type IsNever<T> = [T] extends [never] ? true : false;
复制代码
```

###  implement KeysToUnion

```ts
type A = KeyToUnion<{
  a: string;
  b: number;
  c: symbol;
}>
// 'a' | 'b' | 'c'

// 实现KeyToUnion
type KeyToUnion<T> = {
  [K in keyof T]: K;
}[keyof T]
复制代码
```

###  implement ValuesToUnion

```ts
type A = ValuesToUnion<{
  a: string;
  b: number;
  c: symbol;
}>
// string | number | symbol

// ValuesToUnion
type ValuesToUnion<T> = T[keyof T]
复制代码
```

### FindIndex<T, E>

> [bigfrontend.dev/zh/typescri…](https://link.juejin.cn?target=https%3A%2F%2Fbigfrontend.dev%2Fzh%2Ftypescript%2FSearch)

```ts
type IsAny<T> = 0 extends (T & 1) ? true : false;
type IsNever<T> = [T] extends [never] ? true : false;

type TwoAny<A, B> = IsAny<A> extends IsAny<B> ? IsAny<A> : false;
type TwoNever<A, B> = IsNever<A> extends IsNever<B> ? IsNever<A> : false;

type SingleAny<A, B> = IsAny<A> extends true ? true : IsAny<B>
type SingleNever<A, B> = IsNever<A> extends true ? true : IsNever<B>


type FindIndex<T extends any[], E, A extends any[] = []> =
    T extends [infer P, ...infer Q] ?
        TwoAny<P, E> extends true ? 
            A['length']
            :
            TwoNever<P, E> extends true ?
                A['length']
                :
                SingleAny<P, E> extends true ?
                    FindIndex<Q, E, [1, ...A]>
                    :
                    SingleNever<P, E> extends true ?
                        FindIndex<Q, E, [1, ...A]>
                        :
                        P extends E ? A['length'] : FindIndex<Q, E, [1, ...A]>
        : 
        never
复制代码
```

### implement Trim

```ts
type A = Trim<'    BFE.dev'> // 'BFE'
type B = Trim<' BFE. dev  '> // 'BFE. dev'
type C = Trim<'  BFE .   dev  '> // 'BFE .   dev'

type StringToTuple<T extends string, A extends any[] = []> =
    T extends `${infer K}${infer P}` ? StringToTuple<P, [...A, K]> : A;

type TupleToString<T extends any[], S extends string = '', A extends any[] = []> =
    A['length'] extends T['length'] ? S : TupleToString<T, `${S}${T[A['length']]}`, [1, ...A]>

type Trim<T extends string, A extends any[] = StringToTuple<T>> =
    A extends [infer P, ...infer Q] ?
        P extends ' ' ?
            Trim<T, Q>
            :
            A extends [...infer M, infer N] ? 
                N extends ' ' ?
                    Trim<T, M>
                    :
                    TupleToString<A>
                :
                ''
        :
        '';
复制代码
```
