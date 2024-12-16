---
title: IM通信技术：短轮询、长轮询、SSE、WebSocket
published: 2024-12-16
description: 'IM通信技术：短轮询、长轮询、SSE、WebSocket'
image: ''
tags: [IM]
category: 'IM'
draft: false 
---

# IM通信技术：短轮询、长轮询、SSE、WebSocket

### 前言

即时通讯（IM）技术在现代互联网应用中扮演着至关重要的角色，无论是社交聊天、企业协作，还是实时通知，都离不开高效的通信方式。从早期的短轮询、长轮询到后来的 SSE 和 WebSocket，IM 技术经历了多次演进，每一种技术都在特定的历史阶段解决了对应的问题。虽然 WebSocket 被视为当下的主流选择，但短轮询和长轮询在某些场景中仍然具有不可替代的优势。本文将深入解析这几种通信技术的特点、适用场景以及技术选型思路，帮助开发者更好地理解和应用 IM 通信技术。

### 即时通信

即时通讯系统（IM/消息推送）的核心在于确保“即时通信”的能力，具体可以从以下两个维度理解：

1. 客户端能主动向服务器发送消息或请求。
2. 当服务器的数据或状态发生变化时，能够立即将变化通知到客户端。

### 常用技术

- 客户端轮询：传统意义上的短轮询（Short Polling）
- 服务器端轮询：长轮询（Long Polling）
- 单向服务器推送：Server-Sent Events（SSE）
- 全双工通信：WebSocket

### 短轮询（Short Polling）

短轮询（Short Polling）是一种简单的客户端与服务器之间进行数据更新检查的技术，其基本原理较为直接，下面是其实现过程：

#### 实现原理：

1. 客户端发起请求： 客户端定期向服务器发送HTTP请求（通常是GET请求），请求检查是否有新的数据或事件。
2. 服务器响应请求： 服务器接收到请求后，立即处理并返回响应。如果服务器有新的数据或事件，返回这些数据；如果没有更新数据，则返回一个空响应或者一个标识说明没有新数据。
3. 客户端处理响应： 客户端根据服务器返回的结果来处理数据。如果有新数据，客户端进行相应的操作；如果没有新数据，客户端一般不会做额外处理。
4. 周期性请求： 客户端会在固定的时间间隔后（比如每隔几秒）再次发送请求，继续检查是否有新的数据。

#### 优点：

- 实现简单，适合不要求极高实时性的场景。
- 客户端和服务器之间的连接生命周期短，减少了长时间保持连接的复杂性。

#### 缺点：

- 延迟较高：因为客户端是定时请求服务器，无法实现真正的实时性，数据更新的响应时间取决于请求的频率。
- 资源浪费：即使服务器没有数据更新，客户端仍然会定期发起请求，可能造成带宽和计算资源的浪费。
- 负载较重：如果请求的频率较高，可能会增加服务器的负载。

### 长轮询（Long Polling）

长轮询（Long Polling）是一种模拟实时通信的技术，常用于客户端和服务器之间的消息推送。

#### 实现原理：

1. 客户端发起请求： 客户端向服务器发送一个HTTP请求（通常是GET请求）。
2. 服务器等待数据： 服务器接收到请求后，如果没有新的数据或者事件发生，服务器并不会立即返回响应，而是保持该连接处于“挂起”状态，直到有新的数据或事件需要推送给客户端为止。
3. 服务器返回响应： 一旦服务器检测到新的数据或事件（例如新消息、状态更新等），它会将该数据作为响应返回给客户端。
4. 客户端处理响应： 客户端接收到响应后，处理数据并可以立即再次发起请求。这样就形成了一个“请求-等待-响应-再次请求”的循环。
5. 重复请求： 客户端每次收到响应后，立即发送下一个请求，继续等待新的数据更新。这个过程不断循环，模拟了实时通信的效果。

#### 优点：

- 实现相对简单，不需要复杂的协议或额外的基础设施。
- 在没有频繁事件的情况下，服务器负载较轻。

#### 缺点：

- 相较于WebSocket等技术，长轮询的延迟较高，因为每次请求和响应之间存在等待时间。
- 每次请求都需要建立新的HTTP连接，可能会造成较高的网络开销，尤其是在客户端和服务器之间的通信频繁时。

### Server-Sent Events (SSE)

Server-Sent Events (SSE) 是一种基于HTTP协议的单向通信技术，允许服务器向浏览器推送实时更新。

特点： 单项通信，基于HTTP/1.1长连接，自动重连，事件推送

#### 工作原理：

**客户端发起连接**：

- 客户端通过创建一个 `EventSource` 对象发起请求，指向服务器的SSE端点（通常是一个URL）。

```js
const eventSource = new EventSource('http://example.com/events');
```

**服务器保持连接并推送数据**：

- 服务器保持HTTP连接打开，并通过流式传输数据，数据通常采用`text/event-stream` MIME类型，逐条地发送数据。
- 服务器通过每个消息块来标识事件的开始，事件可以是纯文本格式或者包含字段如`event`, `data`, `id`和`retry`。

**客户端接收事件**：

- 客户端通过`EventSource`对象的事件监听器接收数据，当服务器推送新数据时，客户端会触发对应的事件。

```js
eventSource.onmessage = function(event) {
  console.log('New message:', event.data);
};
```

**自动重连机制**：

- 如果连接中断，浏览器会自动重连服务器（通常在3秒钟后）。服务器可以在响应头中指定重连时间间隔：

```js
retry: 5000  // 5秒后重连
```

#### 优点：

1. 实时性：SSE 能够提供实时的数据推送功能，客户端能够立即接收到来自服务器的更新。
2. 基于HTTP：使用标准的HTTP协议，避免了WebSocket等技术需要额外的协议支持，兼容性较好。
3. 自动重连：SSE 自带自动重连机制，能够处理网络断开和恢复。

#### 缺点：

1. 单向通信：SSE 只支持服务器推送数据，客户端不能直接通过同一连接发送数据。
2. 浏览器支持：尽管大多数现代浏览器支持SSE，但仍有一些浏览器（如IE）不原生支持SSE。
3. 不适用于高频实时推送：SSE 可能不适合需要超低延迟或者极高频率更新的应用场景，如在线游戏或高频交易。

#### 适用场景：

- 实时通知：例如推送消息、社交媒体通知、股票价格更新等。
- 动态数据流：例如实时天气、实时数据监控、直播更新等。
- 新闻更新：新闻网站可以通过SSE向客户端推送最新的新闻。

### **WebSocket** 

WebSocket 是一种网络通信协议，旨在提供浏览器和服务器之间的全双工（双向）通信，支持实时、低延迟的交互。与传统的HTTP请求-响应模型不同，WebSocket能够在客户端和服务器之间建立持久的连接，从而实现双向数据流动。

#### 工作原理：

1. 握手过程：

   - 客户端首先通过HTTP发起请求，请求包含`Upgrade`头部，告知服务器希望建立WebSocket连接。
   - 服务器收到请求后，如果支持WebSocket协议，会返回一个含有`Upgrade`头部的HTTP响应，确认建立连接。

   示例：

   - 客户端发送请求：

     ```
     http复制代码GET /chat HTTP/1.1
     Host: example.com
     Upgrade: websocket
     Connection: Upgrade
     Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
     Sec-WebSocket-Version: 13
     ```

   - 服务器返回响应：

     ```
     http复制代码HTTP/1.1 101 Switching Protocols
     Upgrade: websocket
     Connection: Upgrade
     Sec-WebSocket-Accept: x3JJHMbDL1EzLkh9R8H2bZxv8kRYc7qgWzvM1wt+fZk=
     ```

2. 数据传输：

   - 一旦连接建立，客户端和服务器可以通过WebSocket进行数据传输。数据传输是通过**消息帧**进行的，消息帧中包含数据（可以是文本、二进制数据等），并通过WebSocket连接实时传输。

3. 关闭连接：

   - 连接结束时，客户端或服务器可以发送关闭帧来主动关闭连接，另一方接收到关闭帧后，连接被正式关闭。

#### 优点：

1. 低延迟：建立连接后，数据可以几乎立即传输，无需每次重新建立连接。
2. 双向通信：支持客户端和服务器之间双向通信，可以更灵活地进行交互。
3. 节省资源：避免了频繁的HTTP请求和响应，减少了连接建立和关闭的开销。
4. 高效性：适合高频实时应用，如即时通讯、实时数据流、在线游戏等。

#### 缺点：

1. 浏览器支持：虽然大多数现代浏览器都支持WebSocket，但在某些较老的浏览器或网络环境中（如代理服务器可能会干扰WebSocket连接）可能不兼容。
2. 连接管理：由于WebSocket是持久连接，需要管理连接的建立、维护、重连等。大量并发连接时，服务器需要处理更多的资源和连接状态。
3. 安全性：WebSocket虽然支持加密（wss://），但不当的权限管理和连接控制可能带来安全风险，如恶意的跨站点脚本攻击（XSS）或跨站点请求伪造（CSRF）等问题。

#### 适用场景：

- 即时通讯：如聊天应用、社交媒体消息推送。
- 实时数据流：例如股票、体育赛事数据更新、天气预报等。
- 在线游戏：支持多人实时交互的在线游戏。
- 实时协作应用：例如多人同时编辑文档、白板等。

待续。。。。

后面再认真看看WebSocket相关技术细节和实践