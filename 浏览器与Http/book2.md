# 浏览器缓存
**强缓存** **协商缓存**

## 缓存过程分析
从浏览器发起 HTTP 请求到获得请求结果，可以分为以下几个步骤：

1.  浏览器第一次发起 HTTP 请求，在浏览器缓存中没有发现缓存结果和缓存标识
2.  因此向服务器发起 HTTP 请求，获得该请求的结果以及缓存规则（也就是 Last-Modified 或者是 ETag）
3.  浏览器把响应内容存入 Disk Cache，把响应内容的引用存入 Memory Cache
4.  把响应内容存入 Service Worker 的 Cache Storage（如果 Service Worker 的脚本调用了 cache.put()）

下一次请求相同资源的时候：

1.  调用 Service Worker 的 fetch 事件响应
2.  查看 memory cache
3.  查看 disk cache，这里细分为：
    *   有强缓存且未失效，则是用强缓存，不请求服务器，返回的状态码都是 200
    *   有强缓存且已失效，使用协商缓存判断，是返回 304 或者是 200

## 缓存类型
#### 强缓存
强缓存：不会向服务器发送请求，直接从缓存中读取资源。code=200，并且size显示form disk cache或from memory cache就是强缓存。强缓存可以通过设置两种http header实现：Expires和cache-control

**Expires（http/1.0时使用）**
缓存过期时间，Expires=max-age+请求时间，需要和last-modified结合使用。

**cache-control（http/1.1使用）**
缓存控制：有种指令实现缓存。常见的有max-age最大缓存时间；no-cache资源被缓存，但立即失效，下次会发起请求验证资源是否过期；no-store是否使用缓存需要经过协商缓存来验证决定；

常用指令
* public：客户端和代理服务器都可以缓存，响应可以被中间任何一个节点缓存
* private：这个是 Cache-Control 的默认取值，只有客户端可以缓存，中间节点不允许缓存
* no-cache：表示不进行强缓存验证，而是用协商缓存来验证
* no-store：所有内容都不会被缓存，既不使用强缓存，也不使用协商缓存
* max-age：表示多久时间之后过期
* s-max-age：作用同 max-age，但是表示代理缓存，且优先级更高
* max-stale：能容忍的最大过期时间
* min-fresh：能容忍的最小新鲜度

cache-control优先级高于Expires;

#### 协商缓存
协商缓存就是强缓存失效后，浏览器携带缓存标识向服务器发起请求，有服务器根绝缓存标识决定是否使用缓存的过程。
协商缓存可以通过设置两种 HTTP Header 实现：Last-Modified 和 ETag

**last-modified**
1. 浏览器第一次向服务器请求该资源
2. 服务器在返回这个资源的同时，会在响应头中添加 Last-Modified 的字段，值为最后的修改时间
3. 当浏览器接收后缓存文件和这个 header
4. 当下一次浏览器再次发送请求请求该资源的时候，检测到有 Last-Modified 这个 header，就会在请求头中添加 If-Modified-Since 这个字段，值为 Last-Modified
服务器接收到请求之后，根据 If-Modified-Since 与服务器的这一资源的最后修改时间做对比
5. 如果结果相同，则返回 304 状态码和一个空的响应题，告诉浏览器使用缓存
6. 反之，返回 200 状态码和请求的资源，同时在响应头中更新 Last-Modified

弊端：如果本地打开缓存文件，但没有修改，也是会造成last-modified修改。

**ETag**
ETag是否服务器响应请求时，返回当前资源文件的唯一一个标识，只要资源又变化就会重新生成。

1. 服务器在响应请求的时候，会在响应头添加一个 ETag 的字段，值为这个文件当前的唯一标识。
2. 浏览器在接收到文件并缓存文件和 请求的响应头。
3. 在下一次请求这个资源的时候，浏览器会在请求头上加一个 If-None-Match 的字段，值为 ETag。
4. 服务器用请求头上的值和本地资源的值进行对比，如果命中则返回 304 告知浏览器使用本地缓存，否则返回 200 和最新的资源文件。

**last-modified和ETag对比**
精确度上，ETag要优与last-modified，性能上，Etab要弱与last-modified，毕竟last-modified只需要记录时间，Etag需要在服务器上计算一个hash值出来，但是服务器优先校验Etag。

## 缓存位置
从优先级上说，缓存位置分为4种
* Service Worker
* Memory Cache
* Disk Cache
* Push Cache

## service work
service work 是运行在浏览器背后的独立线程，脱离浏览器窗体，所以无法直接访问dom，功能上主要用来实现：离线缓存、消息推送、网络代理。

一般有有几个步骤
1. 注册server work
2. 监听到install事件，缓存需要的文件，在下次访问时就可以通过拦截请求来判断是否存在缓存。
3. 当server work没有命中缓存时，先去fetch获取数据，根据浏览器缓存去优先级查找数据，无所是从哪里取来的钱，都显示从server work中获取到的

workbox资源缓存策略
1. stable-while-revalidate: 使用缓存内容尽快响应，如果未缓存，则使用网络请求，然后再更新缓存。
2. cache-first: 缓存优先，如果请求命中缓存，则使用缓存，不请求网络，如果没有命中，则请求网络，再缓存，减少服务器压力。
3. network-firt: 网络优先，先从网络中获取，获取成功就把数据放入缓存，否则直接使用缓存数据。
4. netwrork only: 只使用网络请求数据。
5. cache onle: 只使用缓存数据。

## memory cache
内存缓存，存储的主要是当前网页上已经下载到的资源，比如样式，脚本，图片等；

特点是：
* 读取效率高，会随着进程的释放而释放
* 几乎所有的请求资源都能进入memory cache

## disk cache
硬盘缓存，特点是持续时间长，存储量大；

disk cache对比memory cache
* 比较大的js、css文件会存储在disk cache，反之会在memory cache
* 当前系统内存使用率比较高的时候，会优先存入磁盘

## 用户行为对浏览器缓存的影响
* 打开网页，地址栏输入地址： 查找 disk cache 中是否有匹配。如有则使用；如没有则发送网络请求。
* 普通刷新 (F5)：因为 TAB 并没有关闭，因此 memory cache 是可用的，会被优先使用(如果匹配的话)。其次才是 disk cache。
* 强制刷新 (Ctrl + F5)：浏览器不使用缓存，因此发送的请求头部均带有 Cache-control: no-cache(为了兼容，还带了 Pragma: no-cache),服务器直接返回 200 和最新内容。

[深入理解浏览器的缓存机制](https://www.jianshu.com/p/54cc04190252)
