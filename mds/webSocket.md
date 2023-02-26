# WebSocket

## 介绍 
WebSocket 是一种网络通信协议，由HTML5提供的一种在单个TCP连接上进行全双工通讯的协议。

Http协议是一种无状态的，无连接的，单向的应用层协议。只能由客户端发起，服务端回应。效率低，浪费资源。

websocket：

![avatar](https://pylist.com/static/upload/3e7c20f6f302376163dbdec90d38043d.jpg)

首先建立连接，然后数据交互。

## websocket协议 
来自客户端的握手*例子：
```json
GET ws://localhost/chat HTTP/1.1
Host: localhost
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: ..........
...

```

来自服务端的握手*例子：
```json
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: ..........
...
```

|  头名称   | 说明  |
|  ----  | ----  |
| Connection: Upgrade  | 标识该GTTP请求是一个协议升级请求 |
| Sec-WebSocket-Key  | 客户端采用base64编码的24位随机字符序列， 响应一个对应加密的Sec-WebSocket-Accept头信息作为应答 |

## 客户端（浏览器）实现
创建WebSocket对象：
`var ws = new WebSocket(url)`
url格式： ws://ip地址:端口/资源名称

以React为例：
```javascript
import useWebSocket from 'react-use-websocket';

const WS_URL = 'ws://127.0.0.1:8000';

export default function App() {
  useWebSocket(WS_URL, {
    onOpen: () => {
      console.log('WebSocket connection established.');
    }
  });

  return (
    <div>Hello WebSockets!</div>
  );
}
```

### WebSocket事件

| 事件 | 事件处理程序 | 描述 |
| ---- | ---- | ---- |
| open | websocket对象.onopen | 连接时触发 |
| message | websocket对象.onmessage | 客户端接收服务端数据时 |
| error | websocket对象.onerror | 错误时 |
| close | websocket对象.onclose | 连接关闭时 |

### WebSocket方法
我们主要关注send()， 用于向服务端发送数据。

## 服务端实现
```javascript
ws.on('open', function () {})
```

在Node.js中，使用最广泛的WebSocket模块是ws.
