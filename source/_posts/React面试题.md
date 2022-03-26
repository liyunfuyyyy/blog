---
title: React面试题
date: 2022-03-10 18:29:44
categories: 面试
tags: ['React','Redux','Router']
---

#### 1. React组件如何通讯

- props
- context
- redux
- 自定义事件

#### 2. JSX本质是什么

#### 3. context是什么，有何用途

#### 4. SCU的用途

- 性能优化
- 必须配合不可变值使用

#### 5. 描述redux单项数据流

#### 6. setState是同步还是异步（场景题）

![image-20220310194203671](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220310194203671.png)

- React默认父组件有更新，子组件无条件更新

### 异步action

- 需要使用`redux-thunk` 中间件
- 异步返回一个函数，如让网络请求等

![image-20220310211346082](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220310211346082.png)![image-20220310211427340](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220310211427340.png)

![image-20220310212120841](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220310212120841.png)

- view触发一个新的action
- action到dispatch中
- 如果dispatch有中间件可能再次触发action
- dispatch触发action到reducer
- reducer根据action生成新的state
- state去更新view
