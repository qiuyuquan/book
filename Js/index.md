## 一、node基础
#### 1. node简单介绍
  node就是运行在服务端的js代码，是一个事件驱动I/O服务端js环境，基于google的v8引擎，
  node异步编程的直接体现就是回调
  事件驱动， 非阻塞I/O 模型观察者模式和事件循环机制来实现这种异步I/O
  node.js采用单线程异步非阻塞模式，也就是说每一个计算独占cpu，遇到I/O请求不阻塞后面的计算，当I/O完成后，以事件的方式通知，继续执行计算2。
  遇到I/O请求不阻塞后面的计算，当I/O完成后，已事件的方式通知。

#### 2. 基本模块
  **http**
  要开发服务器，从处理tcp连接，到解析http是不可能，node提供了对应http模块处理，我们只需要操作http操作提供了request，response。

  ```
  <!-- 引用http -->
  var http = require('http')

  <!-- 创建服务器 -->
  http.createServer(function (request, response) {
    response.end('hello world')
  })

  <!-- 服务器监听端口 -->
  http.listen(8000)
  ```

  **fs**
  fs是内置的文件系统模块，负责读写文件，提供同步和异步的方法。

  ```
  var fs = require('fs');

  <!-- 异步读取文件，通过回调函数的实现监听 -->
  fs.readFile('gworld.txt', function(err, data) {
    if (err) {
      return console.log(err)
    }
  
    console.log(data)
  })

  <!-- 同步读取文件 -->
  var data = fs.readFile('gworld.txt')
  console.log(data)
  ```

  **stream**
  文件流，一种抽象的数据结构。可以用来读写文件，比如从文件读取数据时，可以打开一个数据流，然后从文件流中不断地读取数据，在写入文件时，只需要流写入文件中即可完成数据读取。

  ```
  var fs = require('fs')

  <!-- 打开一个文件的流 -->
  var rs = fs.createReadStream('gworld.txt', 'utf-8')

  rs.on('data', function(chunk) {
    console.log(chunk)
  })

  rs.on('end', function(chunk) {
    console.log(chunk)
  })
  
  rs.on('error', function(chunk) {
    console.log(chunk)
  })
  ```

#### 3. node 异步
  node三大特点
    1. 单线程
    2. 非阻塞I/O
    3. 事件驱动
  
  **单线程**
  > 多线程：将cpu分成几个线程，每个线程同步运行
  > 单线程：负责解析和执行js代码线程的只有一个，每个计算都独占cpu，顺序执行，每次只执行一个任务。

  ![多线程](https://segmentfault.com/img/bVbaNf0?w=489&h=466)

  ![单线程图](https://segmentfault.com/img/bVbaNgD?w=481&h=561)

  **非阻塞I/O** 


  **事件驱动**
  何为事件驱动，事件驱动是发布-订阅（观察者）的一种模式，通过事件或状态的变化来进行应用程序的流程控制

  主要执行过程是，当IO线程执行完毕后，将事件压入消息队列中，然后node通过循环来检测队列中的事件状态变化，如果检测到有状态变化的事件，就执行该事件对应的处理代码。

#### 4. node内部组成
  ![](https://segmentfault.com/img/bVbaNhj?w=865&h=374)
  
  如上图，node的组成分成了3个部分，有点类似于webapp的形式
  1. node标准库，使用js编写，这部分使我们可已直接调用API
  2. node bindings，这一部分是js与底层c/c++构建的关键
  3. 最后是node的核心，由c/c++编写
    v8：提供了js在非浏览器运行的环境
    libuv：为node提供了跨平台，线程池，事件池，异步I/O能力
    c-ares：提供了异步处理dns的能力
    http_parser、OpenSSL、zlib 等：提供包括 http 解析、SSL、数据压缩等其他的能力

  [相关文章](https://www.cnblogs.com/wxmdevelop/p/10234556.html)


#### 4. 消息队列与事件循环
  > 消息队列：消息队列是一个先进先出的队列，当一个事件状态发生变化时，就将一个消息压入队列中。
  > 事件循环：事件循环是指主线程循环从消息队列中取出任务、执行的过程。

  NodeJS的事件循环模型一般要注意下面几点
  1. 因为是单线程的，所以当顺序执行js文件中的代码的时候，事件循环是被暂停的
  2. 当JS文件执行完以后，事件循环开始运行，并从消息队列中取出消息，开始执行回调函数
  3. 因为是单线程的，所以当回调函数被执行的时候，事件循环是被暂停的
  4. 当涉及到I/O的时候，nodejs会开一个独立的线程来进行异步I/O操作，操作结果以后将消息压入消息队列

  当node执行的时候，会生成一个主循环来监听事件，当检测到事件时触发回调函数
  ![](https://segmentfault.com/img/bVxLvF)

#### 参考文章
  [nodejs 异步I/O和事件驱动](https://segmentfault.com/a/1190000005173218)
  [JavaScript：彻底理解同步、异步和事件循环(Event Loop)](https://segmentfault.com/a/1190000004322358)

