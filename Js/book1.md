## 3.1. 什么是闭包，闭包的作用是什么
  闭包的定义：当函数可以记住并访问其定义时所在词法作用域，即使该函数在其词法作用域之外执行，就产生了闭包。
  （内部函数可以记住外部函数的词法作用域）

  闭包的原因：由于js引擎的垃圾回收机制，执行代码时维护一个调用栈，当函数执行完毕后，垃圾收回机制会处理这个调用栈（调用栈内包含了函数的词法作用域），要销毁调用栈时，如果发现还其他调用栈中还存在引用，则不销毁此函数的词法作用域。
  
  闭包的作用：
  1. 私有化变量
  2. 构建块级作用域
  3. 模块化


## 3.2. 执行上下文
##### 什么是执行上下文
  执行上下文就是js代码被解析和执行时所在环境的抽象，为我们的可执行代码提供了执行前的准备，如变量对象的定义，作用域链扩展，提供调用者的对象引用信息

##### 执行上下文的类型
  1. 全局执行上下文，这是默认，最基础的上下文，不在任何函数中的代码都在全局执行上下文中，只做两件事 
    （1）创建一个全局对象，浏览器中就是window对象。
    （2）将this指针指向这个对象，一个程序只能存在一个全局执行上下文
  
  2. 函数执行上下文，每次调用函数，都会为该函数创建一个新的执行上下文，只有被调用时才会被创建，可存在任意数量的函数执行上下文

  3. eval函数执行上下文，

##### 执行上下文的生命周期
  1. 创建阶段：
    （1）创建变量对象，初始化函数参数arguments，提升函数和变量声明
    （2）创建作用域，作用域链是在变量对象之前创建的，包含变量对象，用于解析变量，当被要求解析变量时，js从代码嵌套内层开始，如果没有找到，就忘上一级查找，直到查找到该变量
    （3）确定this指向
  
  2. 执行阶段：执行变量赋值，代码执行

  3. 执行上下文出栈，等待虚拟机回收执行上下文

##### 参考文章
  [深入理解 JavaScript 执行上下文和执行栈](https://blog.fundebug.com/2019/03/20/understand-javascript-context-and-stack/)

  [面试官：说说执行上下文吧](https://juejin.im/post/6844904158957404167#heading-17)

## 3.3. js 常用设计模式
##### 设计原则
  单一开放原则：一个函数只做一件事，如果功能过于复杂，最好让一段代码只负责一部分逻辑。

  开放-封闭原则：对扩展开放，对修改封闭

  最少知识原则：应当尽可能减少与其他实体发生相互作用，减少对象间的联系

##### 单例模式
  定义：保证一个类仅有一个实例，并提供一个访问它的全局访问点

  优点：唯一实例，节约内存开销
  缺点：不易扩展，不适用于容易变化的实例

##### 迭代器模式
  定义：提供一种方法能够顺序访问一个聚合对象中各个元素（forEach），而又不需要暴露该对象的内部表示

  ```
  [1, 2, 3].forEach(res => console.log(res))
  ```

##### 代理模式
  定义：为一个对象或方法提供一个代用品或占位符，以便控制对它的访问（中介）

  ```
  <!-- 发送消息 -->
  function sendMessage(msg) {
    console.log(msg)
  }

  <!-- 对发送消息方法进行代理 -->
  function proxySendMessage(msg) {
    msg = '代理发送消息体，' + msg
    sendMessage(msg)
  }

  proxySendMessage('我发送了消息')
  ```

##### 适配器模式
  定义：解决两个函数间接口不兼容的问题，对不兼容的模式进行适配

  ```
  <!-- 限制data为数组 -->
  function renderData(data) {
    data.map(res => ())
  }

  <!-- 对数据进行适配，转换对象为数组返回 -->
  function arrayAdapter(data) {
    if (Object.prototype.toString.call(data) != '[object Array]') {
      return data
    }

    var temp = []
    for (var item in data) {
      temp.push(item)
    }
    return temp
  }

  var data = {
    0: 'A',
    1: 'B'
  }

  renderData(arrayAdapter(data))
  ```

##### 装饰器模式
  定义：为对象装饰添加一些功能，而不会影响旧属功能
  
  ```
  <!-- 不传递参数时@就相当于 Aanimal = decorator(Animal) -->
  function decorator(target) {
    target.type = '人类'
  }

  @decorator
  class Animal {}

  <!-- 传递参数时@就相当于 Animal = setType(type)(Animal) -->
  function setType(type) {
    return function(target) {
      target.type = type
    }
  }

  @setType('人类')
  class Animal {}
  ```

##### 发布订阅模式
  定义：对象间的一种一对多的关系，让多个观察者对象同时监听某个主题对象，当一个对象发送改变时，所有依赖于它的对象都将得到通知

  优势：
    1. 支持简单的广播通信，当对象的状态发送改变时，会自动通知已经订阅的对象
    2. 发布者与订阅者耦合性降低，对象之间解耦
  
  ```
  class EventEmitter {
      constructor() {
          this.eventMap = {};
      }

      on( type, fn ) {
          if ( !this.eventMap[type] ) {
              this.eventMap[type] = []
          } 
          this.eventMap[type].push(fn)
      }

      emit( type, ...params ) {
          this.eventMap[type].forEach(fn => {
              fn(...params);
          });
      }

      off( type, fn ) {
          let list = this.eventMap[type];
          let atIndex = list.indexOf(fn);
          
          if (atIndex !== -1) {
              list.splice(atIndex, 1);
          }
      }
  }
  ```

##### 参考文章
  [7 种 Javascript 常用设计模式学习笔记](https://www.ctolib.com/topics-136631.html#articleHeader7)

## 3.4. 原型与原型链
  ```
  function person() {}

  var man = new person()

  person.prototype.constructor == person

  man.__proto__.constructor == person  

  ```

## 3.5. new 实现
  思路
    1. 创建一个空的简单的js对象
    2. 将函数的prototype复制给对象的__proto__属性，继承原型上的方法
    3. 调用函数，并将步骤1创建的对象对位函数的this上下文，获取私有属性
    4. 如果函数没有返回值或者返回值不是对象，则返回创建的对象，如果返回值是对象，则返回对象

  ```
    function newOperator(ctor) {
      if (typeof ctor !== "function") {
        throw('ctor must be a function')
      }

      var args = Array.prototype.slice.call(arguments, 1)
      
      <!-- 创建一个新对象 -->
      var obj = {}

      <!-- 将函数的prototype赋值给__proto__ -->
      obj.__proto__ = ctor.prototype

      <!-- 调用函数 -->
      var result = ctor.apply(obj, args)

      <!-- 判断函数返回值 -->
      if ((typeof result === 'object' && result != null) || typeof result === 'function') {
        return result
      } else {
        return obj
      }
    }
  ```

### 3.4. js 继承，继承有哪些方式


### 3.6. typeof、instance、constructor 判断类型的区别

### 3.7. 类型转换

### 3.8. 深拷贝与浅拷贝

### 3.9. 数组去重的几种方式

### 3.10. 什么是函数柯里化，如何实现

### 3.11. 节流与防抖，使用场景

### 3.12. js 的数据类型

### 3.13. call、bind、apply 的区别

### 3.14. 堆和栈的区别

### 3.15. js 的事件循环  https://juejin.im/post/6886992599006380045

### 3.16. 如何实现模块化，AMD、CMD、COMMON 的区别

### 3.17. JS 垃圾回收机制，有几种回放方式

### 3.18. xss 与 csrf

### 3.19. 什么是 JSONP，什么是 CORS，什么是跨域？

### 3.20. 什么是内存泄漏，哪些操作会造成内存泄漏

### 3.21. 介绍下观察者模式和订阅-发布模式的区别，各自适用于什么场景

### 3.22. null 和 undefined 的区别

### 3.23. Base64 的原理？编码后比编码前是大了还是小了

### 3.24. javascript 创建对象的几种方式？

### 3.25. 移动端的点击事件的有延迟，时间是多久，为什么会有？ 怎么解决这个延时？

### 3.26. 什么是 requestAnimationFrame ？

### 3.27. 即时通讯的实现，短轮询、长轮询、SSE 和 WebSocket 间的区别？

### 3.28. js 事件流，事件委托和事件冒泡

### 3.29. jsbridge

### 3.30. 前端路由实现。history 什么坑，怎么解决

### 3.31. 为什么 0.1 0.2 != 0.3？如何解决这个问题？

### 3.32. 函数柯里化的实现

### 3.33. 什么是 Virtual DOM？为什么 Virtual DOM 比原生 DOM 快？

### 3.34. URL 和 URI 的区别？

### 3.35. js 如何实现数组去重？

### 3.36. 即时通讯的实现，短轮询、长轮询、SSE 和 WebSocket 间的区别？

### 3.37. 线程和进程

### 3.38. 了解v8引擎吗，一段js代码如何执行的

### 3.39. 如何计算白屏时间和首屏渲染时间的，如何进行数据上报的，上报到监控系统展示是怎样的一个过程

### 3.40. new 原理实现

### 3.41. rem vw 区别

### 3.42. 发布订阅和观察者的区别

### 3.43.  JSONP 实现原理
