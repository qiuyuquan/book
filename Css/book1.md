### 2.1. css盒模型
盒模型分为标准盒模型（box-sizing: content-box;），IE盒模型（box-sizing: border-box;）
IE盒模型的width和height包含了padding和border，而标准盒模型则没有

### 2.2. 什么是BFC
简介
1. BFC简称块级格式化上下文，是一个独立的渲染区域，一种边距重叠解决方案

BFC布局规则
1. 内部的Box会在垂直方向，一个接一个地放置。
2. Box垂直方向的距离由margin决定，属于同一个BFC的两个相邻Box的margin会发生重叠
3. BFC的区域不会与float box重叠，计算BFC的高度时，浮动元素也参与计算
4. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。

BFC创建
1. float值不为none
2. position值不为static或relative
3. display值为inline-block，table-cell，table-caption时创建
4. overflow值部位visibile创建

### 2.3. 实现1px

### 2.4. em、vw、rem区别
<strong>em</strong> 参考物是父元素的font-size，具有继承的特点，比如父元素字体大小为16px，1em = 16px

<strong>vw</strong> 是一种视窗单位，有视窗viewport大小来决定，单位1，比如1vw = viewport width / 100，兼容性较差

<strong>rem</strong> 是根据根元素字体大小来动态变化的，1rem = 根元素html的font-size值，可参考[使用Flexible实现手淘H5页面的终端适配](https://github.com/amfe/article/issues/17)

  优点:
    1. 兼容性好，不用写多个css代码
    2. 还可以用@media进行优化，自由度较大

  缺点:
    1. 需要引入一段js代码，
    2. 单位需要改成rem

### 2.5. css modules和scoped的原理
#### scoped原理
  1. 每个Vue文件都对应一个id，id是根据文件路径和内容hash生成
  2. 编译template标签时为每个标签添加上当前组件的id，如<div class="test" data-v-1822asd><div>
  3. 编译style标签时，会根据id通过属性选择器和组合选择器输出样式，如.test[data-v-1822asd] { color: red }

#### css modules
  通过给样式名加hash字符串后缀的方式，实现特定作用域中样式存在全局唯一

  注意点
    1. 处理动画animation的关键帧keyframes，动画名需先写 如 animation: deni .5s
    2. 需要css-loader配置生效