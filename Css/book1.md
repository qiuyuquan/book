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

### 2.4. vw与rem区别
  
