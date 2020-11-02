## 浏览器跨域
同源策略是一种约定，是浏览器最核心也是最基本功能，如果缺少同源策略，浏览器容易受到xss、csrf攻击。同源指的是URL中protocol协议，host域名，post端口一样。
如果不一致的话，则会导致
1. ajax 请求不能发送
2. 无法获取BOM元素进行操作
3. 无法读取cookie，localStorageor和indeg db;

#### 为什么要设置跨域
首先，跨域只存在于浏览器，浏览器的形态多种多样，所以要对其有限制，其次设置跨域是为了保证用户信息安全。

同源策略又分为：**ajax同源策略**和**dom同源策略**
ajax同源策略：主要是使不同源的页面不能获取cookie且不能获取请求，这样一定程度上防止了csrf攻击；
dom同源策略：限制了不同源的页面不能获取dom，这样可以防止恶意网站在自己的网站中利用iframe嵌入，依次来达到窃取用户信息；
  
#### 实现跨域的几种方式
**JSONP**
利用script标签没有跨域限制的漏铜，网页可以得到从其他来源动态产生的JSON数据，JSONP请求需要服务器支持才可以。

执行过程：
1. 前端定义一个解析函数（var jsonpCallback=function(res){}）。
2. 通过parmas把函数名传递给后台（&cd=jsonpCallback），后台获取到前端声明的执行函数，并以带上参数且调用执行函数的方式传递给前端。
3. 前端在script标签返回资源时就会执行相对应的jsonpCallback。

优点：
1. ajax和jsonp都是客户端向服务器发送请求，从而获取数据的方式，但ajax属于同源策略，而jsonp属于非同源策略。
2. 兼容性好，能解决主流浏览器跨域访问的问题

缺点：
1. 仅支持get请求
2. 不安全，可能会收到xss攻击

[JSONP原理及实现](https://www.jianshu.com/p/88bb82718517)

**cors**
原理：跨域资源共享（cors）是一种机制，w3c的标准，它允许浏览器向跨源服务器，发出XMLHttpRequest或Fetch请求，并且整个cors通信过程都是浏览器自动完成的。

启用：需要浏览器支持这个功能，服务器需要设置Access-Control-Allow-Origin 就可以开启 CORS。

过程：
1. 浏览器根据同源策略对前端页面和后台交互地址做匹配，若同源，则直接发送数据请求，若不同源，则发送跨域请求。
2. 服务器收到浏览器的跨域请求后，根据自身配置返回对应的文件头，若未配置任何跨域，则返回的文件头中不包含Accss_Control-All-Cache，若配置过，则返回Acss-Control-Allow-origin+配置规则里的域名。
3. 浏览器收到后，根据响应头里的acces-control做匹配，若相等则说明该域名下可以开源

另外cors分为两种**简单请求**和**复杂请求**
简单请求：只能使用get，post，head房，与普通的xhr没有太大的区别，但是http头中要求包含一个域（origin）的信息，包含协议名、地址以及一个可选的端口。使用post方法时，content-type只能使用application/x-www-form-urlencoded、multipart/form-data、text-plain编码，并且不能使用自定义的http headers;

复杂请求：需要先发送一个预检请求，服务端需要返回“预回应”响应，实际上是对服务端的一种权限请求，只有当预检成功后，实际请求才会开始执行。

[CORS原理及实现][https://www.jianshu.com/p/b2bdf55e1bf5]

**postmessage**
postmessage是http5的一种api，window属性之一。
它可用于解决一下方面的问题
* 页面与其他窗口的数据传递
* 多窗口之间传递信息
* 页面与嵌套iframe消息传递

postmessage方法允许来自不同源的脚本采用异步的方式进行通信，可以实现跨文本档，多窗口，跨域消息传递等。
