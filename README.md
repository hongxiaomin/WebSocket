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

# WebSocket 构造函数
```
WebSocket 对象作为一个构造函数，用于新建WebSocket实例。
var ws = new WebSocket('ws://localhost:8080');
执行上面语句之后，客户端就会与服务器进行连接。
```
# WebSocket.readyState
```
readyState属性返回实例对象的当前状态，共有四种
·CONNECTING:值为0，表示正在连接。
·OPEN: 值为1，表示连接成功，可以通信了。
·CLOSING: 值为2，表示连接正在关闭。
·CLOSED:值为3，表示链接已经关闭，或者打开连接失败。
```
# WebSocket.onopen
   ```
   实例对象的onopen属性，用于指定连接成功后的回掉函数。
   ws.onopen=function(){
       ws.send('Hello Server');
   }

   如果要指定多个回掉函数，可以使用addEventListener方法
   ws.addEventListener('open',function(event){
       ws.send('Hello Server');
   })
   ```
# WebSocket.onclose
```
实例对象的onclose属性，用于指定连接关闭后的回掉函数。
ws.onclose=function(event){
    var code=event.code;
    var reason=event.reason;
    var wasClean=event.wasClean;
}

如果要指定多个回掉函数，可以使用addEventListener方法
ws.addEventListener('close',function(event){
   var code=event.code;
   var reason=event.reason;
   var wasClean=event.wasClean;
})
```
# WebSocket.onmessage
```
实例对象的onmessage属性，用于指定收到服务器数据后的回掉函数。
ws.onmessage=function(event){
    var data=event.data
}

如果要指定多个回掉函数，可以使用addEventListener方法
ws.addEventListener('message',function(event){
   var data=event.data
})
注意，服务器数据可能是文本，也可能是二进制数据（blob对象或Arraybuffer对象）
ws.onmessage=function(event){
    if(typeof event.data===String){
        console.log("Received data string");
    }
    if(event.data instanceof ArrayBuffer){
        var buffer=event.data;
        console.log("Received arraybuffer");
    }
}
除了动态判断收到的数据类型，也可以使用binaryType属性，显示指定收到的二进制数据类型。
收到的是blob数据
ws.binaryType="blob";
ws.onmessage=function(e){
    console.log(e.data.size);
}
收到的是ArrayBuffer数据
ws.binaryType="arraybuffer";
ws.onmessage=function(e){
    console.log(e.data.byteLength);
}
```
# WebSocket.send()
```
实例对象的send()方法用于向服务器发送数据。
发送文本的例子
ws.send("your message");

发送Blob对象的例子
var file=document.querySelector('input[type="file"]').files[0];
ws.send(file);

发送ArrayBuffer对象的例子
var img=canvas_context.getImageData(0,0,400,320);
var binary=new Unit8Array(img.data.length);
for(var i=0;i<img.data.length;i++){
    binary[i]=img.data[i];
}
ws.send(binary.buffer)
}
```
# WebSocket.bufferedAmount
```
实例对象的bufferedAmount属性，表示还有多少字节的二进制数据没有发送出去。他可以用来判断发送是否结束。
var data=new ArrayBuffer(10000000);
socket.send(data);
if(socket.bufferedAmount===0){
//发送完毕
}else{
//发送还没结束
}
```
# WebSocket.onerror
```
实例对象的onerror属性，用于指定报错时的回掉函数。

```