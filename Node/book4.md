## express
优点：易上手、高性能、扩展性强
* 易上手：express对web开发的相关模块进行了适度的封装，屏蔽了大量复杂繁琐的细节，让开发者只需要关注业务逻辑开发，极大的降低了入门和学习成本。
* 高性能：express仅在web相关的node模块进行了封装和扩展，避免了过度封装的性能消耗
* 扩展性强：基于中间件的开发模式，使得express得到扩展，和模块拆分简单、灵活、扩展性强

express 的核心概念主要包括：路由、中间件、模板引擎

**路由**：客户端（包括 Web 前端、移动端等等）向服务器发起请求时包括两个元素：路径（URI）以及 HTTP 请求方法（包括 GET、POST 等等）。路径和请求方法合起来一般被称为 API 端点（Endpoint）。而服务器根据客户端访问的端点选择相应处理逻辑的机制就叫做路由。

```
const express = require('express');

const hostname = 'localhost';
const port = 3000;

const app = express();

<!-- 路由 -->
app.get('/', (req, res) => {
  res.send('Hello World');
});

app.listen(port, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

**中间件**：是指将具体的业务逻辑和底层逻辑解耦的组件。类似于装饰器的模式。
express的中间件流程
![](https://user-gold-cdn.xitu.io/2019/12/18/16f17d3c90298090?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

客户端向服务器发送请求，依次经过每个中间件，最后才到路由，进行相对应的逻辑处理。

```
<!-- 自定义中间件的样子 -->
function someMiddleware(req, res, next) {
  // 自定义逻辑
  console.log('经过中间件!')
  next();
}
```

express的中间件又区分了全局中间件和路由中间件

**全局中间件**：通过app.use(someMiddleware)注册，注册之后每次请求都会执行此中间件
```
app.use(someMiddleware)
```
常见的全局中间件body-parser，处理程序之前，在中间件中对传入的请求体进行解析
```
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));

app.post('/setRole', (req, res) => {
  console.log(req.body.name);
})
```
如果不使用中间件解析，req.body == undefined

**路由中间件**：通过在路由定义时注册中间件，每次访问此路由时都会执行
```
app.get('/login', someMiddleware, (req, res) => {})
```

## koa2
koa2是有express原班人马打造的，基于Node.js平台的下一代web框架。
koa2利用ES7的async/await特性，极大的解决了我们在做nodejs开发的时候异步给我们带来的烦恼。

```
  const koa = require('koa')
  const app = new koa()

  app.use(async (ctx, next) => {
    await next()
    ctx.response.type = 'text/html'
    ctx.response.body = 'hello koa2'
  })

  app.listen(3000)
  console.log('app started at port 3000')
``` 
注意点
  1. 每收到一个http请求，koa会调用app.use注册的async函数，并传入ctx，next参数。await next()是用来调用下一个async函数的。
  2. ctx（context上下文），每个请求都会创建一个新的context，koa把node的request，response封装到context的对象中，所以我们可以直接使用ctx.require，ctx.response，ctx.app = 应用程序实例引用

**koa中间件**
koa的中间件的模型也叫洋葱模型
![](https://static.tuture.co/c/67e4c19/17255546ba9ee525.jpeg)

express的中间件，请求到中间件再到响应，很简单，如下图。
![](https://static.tuture.co/c/67e4c19/17255546abea03b1.jpeg)

koa的中间件分为两个阶段next前，next后
![](https://static.tuture.co/c/67e4c19/17255546d2d5409a.jpeg)

```
async function middleware(ctx, next) {
  <!-- 第一阶段 -->
  await next()
  <!-- 第二阶段 -->
}
```

自定义日志记录中间件
```
export function logger() {
  return async (ctx, next) => {
    const start = Date.now();
    await next();
    const ms = Date.now() - start;
    console.log(`${ctx.method} ${ctx.url} ${ctx.status} - ${ms}ms`);
  };
}
```

## 参考文章
  [一杯茶的时间，上手 Koa2 + MySQL 开发](https://tuture.co/2020/05/22/fac8401/)