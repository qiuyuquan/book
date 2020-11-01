## 浏览器缓存
浏览器缓存一共分为四种，按照优先级排名
* service work
* memory cache
* disk cache
* push cache

### 浏览器缓存机制
1. 浏览器每次发起请求，都会先在浏览器缓冲中查找该请求的结果以及缓存标识。
2. 浏览器每次拿到返回的请求结果都会将该结果和缓存标识存入浏览器中。

### service work
service work 是运行在浏览器背后的独立线程，一般可以用来做缓存，传输协议必须要是https，因为server work拦截请求，需要https协议来保证安全，server work与其他浏览器缓存不一样，它可以让我们自由控制缓存那些文件，如何匹配，读取等。

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

### http的缓存机制
http的缓存主要区分为强缓存和协商缓存

#### 强缓存
强缓存：不会向服务器发送请求，直接从缓存中读取资源。code=200，并且size显示form disk cache或from memory cache就是强缓存。强缓存可以通过设置两种http header实现：Expires和cache-control

1. Expires
缓存过期时间，Expires=max-age+请求时间，需要和last-modified结合使用。

2. cache-control
缓存控制：有种指令实现缓存。常见的有max-age最大缓存时间；no-cache资源被缓存，但立即失效，下次会发起请求验证资源是否过期；no-store是否使用缓存需要经过协商缓存来验证决定；

3. Expires和cache-control对比
Expires是http1.0的产物，cache-control是http1.1的产物，同时存在的话，cache-control优先级高于Expires;

#### 协商缓存
协商缓存就是强缓存失效后，浏览器携带缓存标识向服务器发起请求，有服务器根绝缓存标识决定是否使用缓存的过程。
协商缓存可以通过设置两种 HTTP Header 实现：Last-Modified 和 ETag

1. last-modified
浏览器第一次访问资源的时候，服务器放回资源的同时，在response header 添加last-modified的header，值是这个资源在服务器上的最后修改时间，浏览器接收后缓存文件和header。

弊端：如果本地打开缓存文件，但没有修改，也是会造成last-modified修改。

2. ETag
ETag是否服务器响应请求时，返回当前资源文件的唯一一个标识，只要资源又变化就会重新生成。

3. last-modified和ETag对比
精确度上，ETag要优与last-modified，性能上，Etab要弱与last-modified，毕竟last-modified只需要记录时间，Etag需要在服务器上计算一个hash值出来，但是服务器优先校验Etag。

[深入理解浏览器的缓存机制](https://www.jianshu.com/p/54cc04190252)
