# websocket 协议

## 什么是websocket
websocket是html5的一个新协议，它允许服务端向客户端传递信息，实现浏览器和客户端的双互通信，支持持久连接

## websocket的特点
* 建立在tpc协议上，服务端实现也比较容易
* 与http协议有这良好的兼容性，默认端也是也是80和443
* 数据格式比较轻量，性能开销小
* 可以发送文本也可以发送二进制数据
* 没有同源限制，客户端可以与任意服务器通信
* 协议标识符是ws(如果加密则为wsss)

相对于http协议，http的问题
* 一条连接上只可以发送一个请求
* 请求只能又客户端发起
* 请求和响应首部为经过压缩就直接传输，首部信息越多，延迟就越大
* 发送允长的首部，每次互相发送相同的首部造成浪费

## websocket与http的特点
websocket借助http协议进行握手，三次握手成功后，就会变成tcp通道。
浏览器只需要完成一次握手（不是指建立tpc连接的那个三次握手，是指在建立tpc连接后，传输一次握手数据），两者之间就可以直接创建持久性的连接，并进行双向数据传输。

## 如何创建一个webscoket连接
1. 首先，客户端发送http请求，请求头中包含connect:upgrade，upgard:websocket,sec-websocket-key等字段
2. 服务器根据请求头中connecnt: upgrade，知道这个连接需要进行协议升级。
3. upgard: websocket，告诉服务器，这个连接具体升级成websocket协议。
4. sec-webscoket-key，随机字符串base64，传输给服务器的key，服务器接受后，用sha1算法生成一个结果，当作sec-websocket-accpet的值返回给客户端，客户端收到后按照对应的算法计算出最终base64，和sec-websocket-key进行对比，如果相同表示验证通过。建立连接。

## 参考文章
[WebSocket能干些啥？](https://www.toutiao.com/i6893045753846071815/?tt_from=weixin&utm_campaign=client_share&wxshare_count=1&timestamp=1604926333&app=news_article&utm_source=weixin&utm_medium=toutiao_ios&use_new_style=1&req_id=202011092052130102040551593904F129&group_id=6893045753846071815)
