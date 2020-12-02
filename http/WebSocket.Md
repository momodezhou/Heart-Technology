#WebSocket

##特点-webSocket是客户端实现的，一个支持长连接的协议。

1 WebSocket基于tcp，与http兼容好--可以用http的套路来开发

2 WebSocket使得客户端和服务端可以主动，向对方推送信息，特备是服务端接收到某个信号后，可以主动向已经建立WebSocket的客户端推送信息


##实现WebSocket

1 客户端请求建立WebSocket连接

- 请求的协议不是http,而是ws
- 请求头多了一个属性,其实是声明是WebSocket协议`Connection: Upgrade`
- Sec-WebSocket-Key是链接的标识
- Sec-WebSocket-Version指明WebSocket协议版本
```
GET ws://localhost:11234/ws/png HTTP/1.1
Host: localhost
Upgrade: websocket
Connection: Upgrade
Origin: http://localhost:11234
Sec-WebSocket-Key: client-random-string
Sec-WebSocket-Version: 13
```

2 服务端回应，成功建立WebSocket连接

- http_code=101是指协议成功由http变更为WebSocket协议

```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: server-random-string
```

3 WebSocket协议特点

- WebSocket协议可以发送内容，但是消息格式只有两种，一种是文本，一种是二进制数据
- WebSocket协议只有以下客户端支持：Chrome、 Firefox IE >= 10、 Sarafi >= 6、 Android >= 4.4、 iOS >= 8
- 服务需要自行处理WebSocket协议回应，大部分语言一般都有成熟的库。
