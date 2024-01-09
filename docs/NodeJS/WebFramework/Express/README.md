## 初识 Express

总结来说，Express.js 是 Node.js 开发者的利器，它极大地丰富和简化了 Node.js 应用的开发过程。通过提供简单易用的接口和广泛的社区支持。它使得开发高效、可维护的 Web 应用成为可能。

### 什么是 Express.js框

Express.js 是一个基于 Node.js 平台的极简而灵活的 Web 应用框架，它提供了一系列强大的功能来帮助创建各种 Web 应用和 API。它也是 Node.js 最流行的 Web 框架之一，被广泛的应用于开发单页、多页或混合 Web 应用。

### 为什么需要 Express

- 简化 web 开发： Node.js 的 HTTP 模块功能强大，但编写大型应用的时候如果 url 、参数、路由、动态渲染的处理都采用原生完成，则会显得特别笨重。Express.js 提供了更高层次的抽象，使得创建复杂的 Web 应用更加简单。
- 中间件支持：Express.js 提供了强大的中间件功能，它们使得请求处理管理道化，让请求的解析处理、响应头的设置等任务变得更容易实现
- 路由控制：express 允许定义路由来响应客户端不同的请求（例如不同的 URL、HTTP方法等）
- 社区支持：作为一个流行的框架，它有着庞大的社区，提供了大量的插件和中间件，方便扩展和解决各种需求。
- 提高效率：使用 Express.js 可以快速开发高质量的 Web 应用，提高了开发效率和项目的可维护性。

### Express 能做什么

**路由处理**
- 基本路由：对不同的 HTTP 请求方法和 URL 实现响应，例如 GET POST 请求。
- 路由参数：处理动态 URL 路径的参数
- 链式路由处理器：在路由上应用多个回调函数，提高代码的模块化

**中间件**
- 使用中间件：利用中间件处理请求和响应，例如静态文件服务，请求体解析，Cookie解析等。
- 自定义中间件：依据应用需求编写自定义中间件

**模板引擎**
- 视图渲染：集成模板引擎（如 EJS、pug）来动态渲染 HTML 页面
- 模板布局和数据传递：在视图中使用布局，并向模板传递数据

**错误处理**
- 定制错误处理：通过中间件处理和响应错误

**API构建**
- RESTful API：创建遵循 REST 原则的 Web API，方便与其它应用或服务集成。

**文件上传和处理**
- 处理多媒体和文件上传：使用如 `multer` 这样的中间件处理文件上传

**数据集成**
- 数据库集成：与各种数据库（如 MongoDB、MySQL）集成，便于数据存储和检索。

## 基本使用

Express 是一个灵活的 Node.js Web 应用框架，用于构建各种 Web 应用和API。它简化了路由、中间件的使用，使得开发过程更快捷、高效。

我们可以自主安装 Express 然后配置使用，也可以通过脚手架来初始化项目。

### 手动安装 Express

**01 安装 Express**

首先我们需要在 Node.js 项目中安装 Express，可以通过 npm 完成

```bash
npm init -y # 初始化一个 Node 项目
npm install express --save # 安装 express 并将它添加至依赖中
```

**02 基本服务器设置**

1. 创建 Express 应用

导入 Express 模块并创建一个应用实现。

```javascript
const express = require('express')
const app = express()
```

2. 定义路由

设置应用响应某个路径（如根路径 `/`） 的 GET 请求方式

```javascript
app.get('/', (req, res) => {
  res.send('Hello World!');
});
```

3. 启动服务器

让应用监听指定端口上的 HTTP 请求

```javascript
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`服务在 ${PORT}上运行`)
})
```

4. 使用中间件

Express 的强大功能之一是使用中间件来处理请求和响应，这里包括框架自带的中间件、自定义中间件、第三方中间件，后续会单独说明

```javascript
// 使用内置的  express.json() 和 express.urlencoded 来解析 JSON 和 URL 编码的请求体

app.use(express.json())
app.use(express.urlencoded({extended: true}))
```

```javascript
// 创建并使用自定义中间件来处理请求
app.use((req, res, next) => {
  console.log('中间件')
  next() // 调用 next 继续到下一个中间件、路由处理器
})
```

5. 结合模板引擎

Express 允许我们结合使用模板引擎来生成动态 HTML

```javascript
// 1 设置视图引擎，例如 EJS
app.set('view engine', 'ejs')
// 2 渲染视图：在路由处理器中渲染 EJS 文件
app.get('/', (req, res) => {
  res.render('index', {title: 'Express'})
})
```

6. 处理静态文件

Express 可以方便地托管静态文修的，例如样式表、脚本和图片。将静态资源放在项目的 `public` 目录下，Express 会自动提供对它们的访问。

```javascript
app.use(express.static('public'))
```
7. 组合使用

```javascript
const express = require('express')

const app = new express()

// 使用中间件
app.use(express.json()) // 告诉 exprss 它现在能处理 json 格式的请求体
app.use(express.urlencoded({ extended: true })) // 告诉 express 可以使用第三方插件处理 post 请求体

// 处理静态文件：public 目录下的静态资源可以直接访问
// app.use(express.static('public'))

// 路由
app.get('/', (req, res, next) => {
  // 这里本身也是一个中间件
  console.log(req.query, '<----query')
  res.end('express基本使用')
})

// 监听端口
const PORT = process.env.port || 3001

app.listen(PORT, () => {
  console.log(`服务运行在 http://localhost:${PORT}`)
})

```

### Express 脚手架使用

我们除了使用 npm 来安装 express.js 之外，还可以你使用官方脚手架工具：`express-genderator`。这个工具可以帮助我们快速搭建一个基本的 Express 应用结构。节省了手动创建文件和目录的时间。`express-generator` 提供了一些预设的目录结构和文件，这对于快速开始一个新项目是非常有用的。

**01 安装 `express-generator`**

通过npm 执行全局的安装

```bash
npm install -g express-generator
```

**02 使用 `express-generator`**

创建一个新的 Express 应用

```bash
express myapp
```

执行上述的命令之后会在当前目录下创建一个名为 `myApp` 的新目录。其中包含 Express 应用的初始结构。

- bin：包含可执行文件，用于启动应用
- pubulic：用于存放静态文修的，如样式表、脚本和图片
- rutes：用于存放路由文件
- views：用于存放模板文件（如果使用了模板引擎）
- app.js：应用的主文件

## 静态网页资源处理

### satic 中间件使用

`express.static` 中间件用于提供静态文件，如图片、CSS、JavaScript 文件等。它通过简单的配置，能够高效地为应用提供静态文件服务，同时还提供了多种选项用于定制行为。例如 缓存控制、路径处理等。

**语法**

```javascript
express.static(root, [options])
```
- root：必需参数，用于指定提供静态资源的目录。
- options：可选参数，用于改变中间件的行为。
  - maxAge：设置 http 缓存头的最大过期时间（以毫秒为单位）
  - index：指定索引文件，默认值为 `inex.html`
  - redirect：
  - setHeaders：