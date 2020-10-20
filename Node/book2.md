## http系统模块
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

## fs文件模块
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

## stream（流）
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

## node.js模块系统
为了让Node.js的文件可以相互调用，Node.js提供了一个简单的模块系统Commonjs。
Node.js提供了exports和require两个对象，exports是模块公开的接口，require用于从外部获取一个模块

```
<!-- 创建模块 -->
var hello = require('hello')
hello.world()

<!-- 导出模块 -->
function getName() {}
modules.exports = getName
```

commonjs与es6模块输出的区别
1. CommonJS模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
2. CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。
3. CommonJS 是动态语法可以写在判断里，es6 module静态语法只能写在顶层
 