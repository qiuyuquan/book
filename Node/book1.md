## node简单介绍
  **简介**
  1. node诞生于2009年
  2. Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境
  3. Node.js 使用了一个事件驱动、非阻塞式 I/O 的模型，使其轻量又高效

  **node的特点**  
  1. [单线程](/Node/book3.md)
  2. [非阻塞I/O](/Node/book3.md)
  3. [事件驱动](/Node/book3.md)

## node内部组成
  ![](https://image-static.segmentfault.com/255/540/2555405192-5afeac8190278_articlex)
  
  如上图，node的组成分成了3个部分，有点类似于webapp的形式
  1. node标准库，使用js编写，这部分使我们可已直接调用API
  2. node bindings，这一部分是js与底层c/c++构建的关键
  3. 最后是node的核心，由c/c++编写  
    v8：提供了js在非浏览器运行的环境  
    libuv：为node提供了跨平台，线程池，事件池，异步I/O能力  
    c-ares：提供了异步处理dns的能力  
    http_parser、OpenSSL、zlib 等：提供包括 http 解析、SSL、数据压缩等其他的能力

## 相关文章
  [nodejs真的是单线程吗？](https://www.cnblogs.com/wxmdevelop/p/10234556.html)
