---
title: flex布局
date: 2022-02-12 21:46:33
tags: ['CSS','布局']
---

## flex容器属性

### 改变主轴方向`flex-direction`

#### `flex-direction: row`默认

![image-20220212214719022](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214719022.png)

#### `flex-direction: row-reverse`

![image-20220212214727609](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214727609.png)

#### `flex-direction: column`

![image-20220212214732803](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214732803.png)

#### `flex-direction: column-reverse`

![image-20220212214739075](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214739075.png)



### 换行`flex-wrap`

#### `flex-wrap: nowrap`默认

![image-20220212214747177](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214747177.png)

#### `flex-wrap: wrap`

![img](https://cdn.nlark.com/yuque/0/2022/png/1356933/1644628808182-d39788af-f26b-49ad-a537-bd77f9bf9f37.png)



#### `flex-wrap: wrap-reverse`

![image-20220212214752405](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214752405.png)



### 缩写`flex-flow: [flex-direction] [flex-wrap]`

#### `flex-flow: column wrap`

![image-20220212214757643](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214757643.png)

## 

### 主轴对齐`justify-content`

#### `justify-content: flex-start`默认

![img](https://cdn.nlark.com/yuque/0/2022/png/1356933/1644630167409-b9bbad0e-aa81-4afe-90e2-8c7a72171ba2.png)

#### `justify-content: flex-end`

![image-20220212214802306](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214802306.png)

#### `justify-content: space-around` 平均分配  每个方块的margin-left+margin-right+width相等

![image-20220212214807733](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214807733.png)



#### `justify-content: space-between` 两边没有空隙 中间空隙平均分配

![image-20220212214812645](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214812645.png)



#### `justify-content: space-evenly`所有空隙平均分配

![image-20220212214819677](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214819677.png)



### 交叉轴整体对齐`align-content`

<mark> 必须要有折行属性才能生效 </mark> 

#### `align-content: stretch`默认  

- 如果交叉轴上的宽度未设置则自动拉伸填满交叉轴
- ![image-20220212214825843](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214825843.png)

- 若交叉轴上的宽度已经设置则效果和flex-start一样
- ![image-20220212214830873](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214830873.png)

#### `align-content: flex-start`

![image-20220212214835653](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214835653.png)



#### `align-content: flex-end`

![image-20220212214840206](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214840206.png)



#### `align-content: center`

![image-20220212214844914](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214844914.png)

#### 其他属性 `space-around``space-between``space-evenly`和主轴属性类似



### 交叉轴每一行对齐 `align-items`

#### `align-items: stretch`默认

#### `align-items: flex-start`

![image-20220212214851355](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214851355.png)

#### `align-items: flex-end`

![image-20220212214856140](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214856140.png)

#### `align-items: center`

![image-20220212214900901](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214900901.png)

#### `align-items: baseline` 内容以小写x为基线对齐

![image-20220212214905631](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214905631.png)



### 内联与块的上下左右居中布局

#### 内联上下左右居中

![image-20220212214909804](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214909804.png)

#### 块级上下左右居中

![image-20220212214915118](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214915118.png)



### 不定项居中布局

![image-20220212214919862](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214919862.png)



### 均分列布局

![image-20220212214924984](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214924984.png)

### 子项分组布局

#### 复杂模式 使用div嵌套

![image-20220212214930509](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214930509.png)

#### 简单方式 `margin-right: auto`

![image-20220212214936331](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214936331.png)

## flex子项属性



### 扩展比例`flex-grow`

#### 一个子元素时

- 默认值为0
- 比例值大于等于1，沾满剩余所有空间

- 比例值为0.5，占剩余空间的一半

![image-20220212214944988](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214944988.png)

![image-20220212214950098](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214950098.png)



#### 多个子元素时

- 只有一个有flex-grow时

![image-20220212214954991](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214954991.png)

- 两个都有flex-grow时

![image-20220212214959906](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212214959906.png)

- 当多个元素加起来小于1时，还有剩余空间

![image-20220212215004740](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212215004740.png)



### 收缩比例`flex-shrink`

- 默认值为1，溢出部分完全收缩，小数按比例收缩

![image-20220212215009431](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212215009431.png)

#### 默认情况当有两个及以上需要收缩时

- `main`width: 400

- `box1`width: 200
- `box2`width: 300

- 则，默认情况下收缩后 所占比例按照宽度计算
- `box1`收缩后所占尺寸：200-2/5*(200+300-400)=160

- `box2`收缩后所占尺寸：300-3/5*(200+300-400)=240

![image-20220212215014237](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212215014237.png)

#### 不同比例收缩时

- `main`width: 400
- `box1`width: 200

- `box2`width: 300
- 则，比例情况下收缩后 所占比例按照宽度计算

- `box1`收缩后所占尺寸：200-4/7*(200+300-400)=142
- `box2`收缩后所占尺寸：300-3/7*(200+300-400)=257

![image-20220212215019585](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212215019585.png)

### 指定flex元素在主轴上的初始大小`flex-basis`

- 当主轴方向是水平时，覆盖水平宽度
- 当主轴方向是垂直时，覆盖垂直高度

- **可选值**：0% auto 200px 100%  0  

![image-20220212215024413](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212215024413.png)

#### `flex-basis: auto`默认值

![image-20220212215029405](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212215029405.png)

#### `flex-basis: 0`表示占据最小宽度，会竖起来

![image-20220212215034262](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212215034262.png)



### flex缩写

#### flex: 1

![image-20220212215039444](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212215039444.png)

#### flex: 0

![image-20220212215044185](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212215044185.png)

#### flex: auto

![image-20220212215048190](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212215048190.png)



### 改变某个子项的排序位置`order`

- `order: 0`当前位置保持不变
- `order: -1`向前排

- `order: 1`向后拍

![image-20220212215052972](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212215052972.png)



### 控制单独某一个元素交叉轴的布局`align-self`

![image-20220212215057472](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212215057472.png)



## 等高布局 内容填充两边也等高

![image-20220212215102252](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212215102252.png)



## 两列或三列布局  两边固定宽度 中间自适应

![image-20220212215107507](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212215107507.png)



## Sticky Footer布局 内容空页脚在最底部 内容满也在最底部

![image-20220212215116299](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212215116299.png)



## 溢出项布局

![image-20220212215123298](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212215123298.png)

![image-20220212215128574](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/image-20220212215128574.png)
