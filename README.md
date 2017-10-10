# WebSocket
# 为什么需要WebSocket?
```
因为HTTP协议有一个缺陷：通信只能由客户端发起。
HTTP协议做不到服务器主动向客户端推送信息。
这种单向请求的特点，注定了如果服务器又连续的状态变化，客户端要获知就非常麻烦。我们只能使用“轮询”：每隔一段时间，就发出一个询问，了解服务器有没有新的信息。
轮询的效率低，非常浪费资源。
```
# 简介
```
所有浏览器都已经支持了。
他最大的特点就是，服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，是真正的双向平等对话，属于服务器推送技术的一种。
其他特点包括：
·建立在TCP协议之上，服务器端的实现比较容易。
·与HTTP协议有着良好的兼容性。默认端口也是80和443，并且握手阶段采用HTTP协议，因此握手时不容易屏蔽，能通过各种HTTP代理服务器。
·数据格式比较轻量，性能开销小，通信高效。
·可以发送文本，也可以发送二进制数据。
·没有同源限制，客户端可以与任意服务器通信。
·协议标识符是ws(如果加密，则为wss)，服务器网址就是URL。（ws://example.com:90/some/path）
```
# 客户端的简单示例
```
var ws = new WebSocket("wss://echo.websocket.org");
ws.onopen=function(evt){
    console.log("connection open ...");
    ws.send("Hello WebSockets");
};

ws.onmessage=function(evt){
    console.log("Received Message:"+evt.data);
    ws.close();
};

ws.onclose=function(){
    console.log("Connection closed");
}
```
# 客户端的API
```
# WebSocket 构造函数
```
WebSocket 对象作为一个构造函数，用于新建WebSocket实例。
```
```
