## 开启 web 服务

使用 Node.js 的 `http` 模块可以开启一个 Web 服务，我们需要编写一个脚本来创建服务器实例并监听传入的请求。

**01 基本步骤**
1. **导入HTTP 模块**：首先，需要包含 Node.js 的 http 模块，这个模块允许我们通过超文本传输协议（HTTP）来传输数据
2. **创建服务器**：使用 `http.createServer()` 方法来创建一个 HTTP 服务器
3. **定义请求处理器**：在 `createServer()` 内部，定义一个函数来处理传入的请求。每当客户端向服务器发送请求时，都会调用这个函数
4. **开始监听端口**：使用 `listen` 方法将服务器绑定到特定的端口。服务器将在此端口上监听传入的连接。

**02 示例**

```javascript
// 从 node.js 中导入 http 模块
const http = require('http')

// 创建一个 HTTP 服务器
const server = http.createServer((req, res) => {
  // 当收到请求时发送一个 HTTP 响应
  res.writeHead(200, { 'Content-Type': 'text/plain;charset=utf-8' });
  res.end('http模块开启了服务');
})

// 服务器监听指定的 3001 端口
server.listen(3001, () => {
  console.log('服务器运行在 http://localhost:3001/')
})
```
- 当对服务器发出请求时（例如，通过在web 浏览器中访问 `http://localhost:3000`），它响应 `res.end()` 里写入的内容
- `res.writeHead(200, {'Content-Type':xxx})` 用于设置 http 状态码，并将内容类型设置为了文本类型，同时告诉客户端采用哪种编码
- `res.end` 操作是向客户端发送带有文本内容的响应
- 服务器设置监听 `3001` 端口

## 请求与路由处理

当我们开启了一个 web 服务之后，就会接收到客户端的各种 request 请求。此后服务端就应该分析请求的 URL 和 HTTP 方法（如 GET、POST等）。依据这些信息，我们可以决定如何响应请求。

**01 步骤说明**
1. **分析请求 URL 和方法**：通过请求对象（`req`）的 `url` 和 `method` 属性获取这些信息。
2. **基于 URL 和方法进行路由**：使用条件语句（如 `if` 或 `switch`）来判断请求的URL和方法，然后根据不同的情况执行不同的逻辑
3. **发送响应**：使用响应对象（`res`）来发送适当的响应

**02 示例**

```javascript
// 引入相应的模块
const http = require('http')
const url = require('url')

// 开启服务
const server = http.createServer((req, res) => {
  // 解析请求URL和方法
  const parsedUrl = url.parse(req.url, true)

  // 将 url 模块处理好的pathname 取出
  const path = parsedUrl.pathname

  // 去除 pathname 前后的 ///// 等
  // /login->login   //user/list--->user/list
  // 显然，当前示例只考虑非嵌套的路由
  const trimmedPath = path.replace(/^\/+|\/+$/g, '')

  // 将请求类型得了统一转为大写，方便后续逻辑判断使用
  const method = req.method.toUpperCase()

  // 路由处理
  if (trimmedPath === 'hello' && method === 'GET') {
    // 处理 /hello 路由过来的  GET 请求
    res.writeHead(200, { 'Content-Type': 'text/plain;charset=utf-8' })
    res.end('你好')
  } else if (trimmedPath === 'data' && method === 'POST') {
    // 处理 /data 路由过来的 POST 请求
    res.writeHead(200, { 'Content-Type': 'application/json' })
    res.end(JSON.stringify({ message: '数据已接收' }))
  } else {
    // 处理其它情况
    res.writeHead(404, { 'Content-Type': 'text/plain;charset=utf-8' })
    res.end('未找到对应的页面')
  }
})

// 监听端口
server.listen(3001, () => {
  console.log('服务运行在：http://localhost:30001/')
})
```

**测试**
1. GET请求：在浏览器中访问 `http://localhost:3001/hello` ，正常时返回 `你好`
2. POST请求：可以使用工具如 Postman 或 cURL 发送 post 请求到 `http://locahost:3001/data`.
3. 其它请求：访问任何其它 URL（如：`http://localhost:3001/unknow`）将返回 "未找到页面"

```bash
# curl 发送 post 请求
curl -X POST http://localhost:3000/data
```

## 请求参数处理

在使用 Node.js 的 `http` 模块创建的 web 服务中处理客户端请求的参数时，主要涉及解析查询字符串和处理 POST 请求体。这里的关键在于正确解析和提取请求中的数据。

**01 查询字符串参数**
> 用于 GET 请求

1. **解析 URL**：使用 Node.js 的 `url` 模块来解析 URL，并获取查询字符串
2. **提取查询参数**：从解析后的 URL 对象中提取查询字符串参数

**示例**

```javascript
// 导入必需模块
const http = require('http')
const url = require('url')

// 开启服务
const server = http.createServer((req, res) => {
  // 解析请求，包括查询字符串
  const parsedUrl = url.parse(req.url, true)
  // // 获取查询字符串参数，此时格式已被解析为 {}
  const query = parsedUrl.query 

  // 响应包含查询参数的消息
  res.writeHead(200, {'Content-Type': 'text/plain;charset=utf-8'})
  res.end(`查询参数：${JSON.stringify(query)}`)
})

// 监听端口
server.listen(3001, () => {
  console.log('服务器运行在 http://localhost:3000/')
})
```

**02 POST请求体**

1. **收集请求体数据**：监听 `data` 事件来收集传入的数据片段，并将这些片段组合在一起。
2. **解析请求体**：监听 `end` 事件，在请求数据完全接收后进行解析。

**示例**

```javascript
// 引入必需模块
const http = require('http')
const url = require('url')
// 内置模块，将 buffer 转为字符串
const StringDecoder = require('string_decoder').StringDecoder;

// 开启服务
const server = http.createServer((req, res) => {
  // 将请求类型得了统一转为大写，方便后续逻辑判断使用
  const method = req.method.toUpperCase() 
  // 只处理 post 请求
  if(method === 'POST'){
    // 创建一个新的字符串解码器，它是一个可写流
    const decoder = new StringDecoder('utf-8')
    // 准备一个 buffer 收集器
    let buffer = ''

    // 监听客户端请求，数据流的到来
    req.on('data', (data) => {
      // 将内容写入收集器中
      buffer += decoder.write(data)
    })

    // 监听客户端请求的，数据流结束
    req.on('end', () => {
      buffer += decoder.end()

      // 收集完成之后，响应收到的数据
      res.writeHead(200, {'Content-Type':'application/json;charset=utf-8'})
      res.end(`收到数据：${buffer}`)
    })
  }else{
    // 非 POST 请求
    res.writeHead(405)
    res.end()
  }
})

// 监听端口
server.listen(3000, () => {
  console.log('服务器运行在 http://localhost:3001/');
})
```

**测试**

使用工具如 Postman 或 cURL 发送 POST 请求到 `http://localhost:3001`。

```javascript
curl -X POST http://localhost:3001 -d "param1=value&param2=value2"
```

**注意**
- 这些示例只展示了基础的参数处理。在实际应用中，我们可能需要更复杂的逻辑，例如验证和错误处理。
- 对于 POST 请求，示例中没有实现特定的数据格式解析（如 JSON 或 URL编码表单）。依据实际需求，我们可能需要特定格式的数据。例如，如果我们知道 POST 数据是 JSON 格式的，我们需要解析这个 JSON 字符串以使用数据。
- 实际生产时，我们通常会使用框架（如 Express.js ）来简化请求处理和路由

## 响应不同类型

使用 node.js 的 http 模块开启的 web 服务中，服务端可以返回多种类型的数据。包括纯文本、HTML、JSON、XML等。依据客户端的需要，我们可以选择合适的格式来返回数据。

**01 返回纯文本**

对于纯文本响应，设置 `Content-Type` 为 `text/plain`

```javascript
const http = require('http')

const server = http.createServer((req, res) => {
  res.writeHead(200, {'Content-Type':'text/plain;charset=utf-8'})
  res.end('你好，这返回的是纯文本')
})

server.listen(3001,() => {
  console.log('服务已开启')
})
```

**02 返回 HTML 内容**

当返回 HTML 内容时，将 `Content-Type` 设置为 `text/html`

```javascript
const http = require('http')

const server = http.createServer((req, res) => {
  res.wirteHead(200, {'Content-Type': 'text/html'})
  res.end('<html><body><h1>Hello, HTML</h1></body></html>')
})

server.listen(3000)
```

**03 返回 JSON**

对于 JSON 响应，设置 `Content-Type` 为 `application/json` 并确保发送的是有效的 JSON 字符串。

```javascript
const http = require('http')

const server = http.createServer((req, res) => {
  const data = {
    id: 1,
    name: 'Example'
  }
  res.writeHead(200, {'Content-Type': 'application/json'})
  res.end(JSON.stringify(data))
})

server.listen(3000)
```

**04 返回 XML 数据**

返回 XML时，设置 `Content-Type` 为 `application/xml` 或 `text/xml`

```javascript
const http = require('http')

const server = http.createServer((req, res) => {
  const xmlData = `<note><body>hello, XML!</body></note>`

  res.writeHead(200, {'Content-Type': 'application/xml'})
  res.end(xmlData)
})

server.listen(3000)
```

**语法细节**
- 使用 `res.writeHead` 方法来设置 HTTP 响应的状态码和头部。`Content-Type` 是响应头部的重要部分，它告诉客户端响应体的内容类型。
- 使用 `res.end` 方法来发送响应体。这个方法接受一个字符串或者 Buffer 作为参数，用来表示实际发送的数据
- 在返回 JSON 数据时，使用 `JSON.stringify` 方法将 JS 对象转换成 JSON 字符串

**注意**
1. 确保响应的 `Content-Type` 和实际返回的数据类型相匹配
2. 在实际的应用开发中，依据不同的URL路径或请求类型（GET、POST）返回不同类型的数据是很常见的方法，我们可以使用条件语句来实现这种逻辑
3. 对于更复杂的应用，我们应该采用 web 框架来简化路由和响应处理

## 动态渲染

当前示例采用 EJS（Embedded JavaScript templating）作为模板引擎来完成渲染，实际生产中，我们还是推荐使用成熟的 web 框架来完成。

**1 安装EJS**

首先，我们需要确保已经安装了 `ejs`。如果没有，可以通过 npm 安装

```bash
npm install ejs
```

**2 目录结构**

假设我们当前的目录结构如下，其中，`views` 文件夹存放 EJS 模板文件，`server.js` 是启动服务器的主文件。

```bash
- project
  - views
    - layout.ejs
    - index.ejs
  - server.js
```

**示例代码**
1. server.js 文件

```javascript
const http = require('http')
const ejs = require('ejs')
const fs = require('fs')
const path = require('path')

const server = http.createServer((req, res) => {
  // 读取 index.ejs 文件
  const indexPath = path.join(__dirname, 'views', 'index.ejs')
  const indexContent = fs.readFileSync(indexPath, 'utf-8')

  // 将数据传递给模板并渲染
  const renderHtml = ejs.render(indexContent, {title: 'Hello EJS'})

  // 响应数据
  res.writeHead(200, {'Content-Type': 'text/html'})
  res.end(renderHtml)
})

server.listen(3000, () => {
  console.log('Server is running on http://localhost:3000');
})
```

2. views/layout.js

这是一个基本布局文件，我们可以在这里定义 HTML 结构的共通部分。

```html
<!DOCTYPE html>
<html>
  <head>
      <title><%= title %></title>
  </head>
  <body>
      <%- body %>
  </body>
</html> 
```

3. views/index.ejs

这是具体页面的模板文件

```html
<% include layout.ejs %>
<% 
  // 把 layout.ejs 的 body 替换成下面的内容
  body = `
    <h1>Welcome to EJS</h1>
    <p>This is a dynamic page rendered with EJS.</p>
  `;
%>
```

**说明**

- 在 `server.js`中，我们使用 `ejs.render` 方法渲染 `index.ejs`模板，并传递一些数据（例如标题）给模板。
- 在 `layout.ejs` 中，我们定义了网页的基本结构。这个文件可以被多个页面共用，以保持网站布局的一致性。
- 在 `index.ejs` 中，我们通过 `<% include layout.ejs %>` 包含基本布局，并定义了此特定页面的内容
- 使用 `fs.readFileSync` 读取模板文件。在实际生产环境中，可能会用到更高级的方法来处理文件读取，比如异步读取或使用 Web框架提供的特性。

**注意**

- 在大型项目中，通常会结合使用 Web 框架（如 Express.js）和模板引擎（如 EJS）。Express.js 等框架提供了更方便的方法来渲染模板
- 在实际应用中，为了更好地处理路由和中间件，通常会使用 Express.js 或其它类似的框架。


