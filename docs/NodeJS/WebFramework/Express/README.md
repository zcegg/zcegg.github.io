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

## 处理静态网页

### static 中间件使用

`express.static` 中间件用于提供静态文件，如图片、CSS、JavaScript 文件等。它通过简单的配置，能够高效地为应用提供静态文件服务，同时还提供了多种选项用于定制行为。例如 缓存控制、路径处理等。

**语法**

```javascript
express.static(root, [options])
```
- root：必需参数，用于指定提供静态资源的目录。
- options：可选参数，用于改变中间件的行为。
  - maxAge：设置 http 缓存头的最大过期时间（以毫秒为单位）
  - index：指定索引文件，默认值为 `inex.html`。
  - redirect：当路径为目录时，是否自动重定向到以斜线结尾的路径，默认为 `true`
  - setHeaders：函数，用于设置响应的头部

**示例**

下面是一个基础的 Express 应用实例，它设置了一个静态文件目录，并启动了一个 HTTP 服务器。

```javascript
const express = require('express')
const app = express()

// 设置静态文件目录为 'public'
app.use(express.static('public'))

// 可选：使用自定义设置
app.use(express.static('public'), {
  maxAge: '1d', // 缓存期限为一天
  index: false
})

const PORT = 3000
app.listen(PORT, () => {
  console.log(`服务运行在 http://localhost:${PORT}`)
})

```

**说明**

1. **路径解析**：默认情况下，`express.static` 中间件不处理 URL 编码，这意味着对于编码的 URL 路径，我们可能需要额外的处理逻辑
2. **安全性**：为了安全起见，建议明确指定提供静态资源的目录，避免使用相对路径或者父目录路径，因为这可能导会导致敏感文件的泄露
3. **性能优化**：对于生产环境，应该考虑设置适当的 `maxAge` 来利用浏览器缓存，减少重复资源的下载
4. **内容类型**：`express.static` 自动依据扩展名确定 `Content-Type`。确保静态资源的文件名正确，以便正确地设置 `MIME` 类型
5. **虚似路径前缀**：如果需要，可以为静态资源指定虚拟路径前缀，这个前缀实际上并不对应文件系统中的路径。

## 处理动态网页

在 Express 框架下处理动态网页通常涉及使用模板引擎来动态生成 HTML 内容。 Express 支持多种模板引擎，如 EJS、Pug（之前称为 Jdge）、Handlebars 等。使用模板引擎可以在服务器端动态插入数据到 HTML 文件中，然后发送这个生成的 HTML 响应给客户端。

使用模板引擎在 Express 中处理动态网页是一种常见且强大的做法。它允许我们将数据和 HTML 模板结合起来，生成动态网页内容。选择合适的模板引擎，并理解它的基本语法和使用方式，对于创建动态网页至关重要。通过这种方式，我们可以构建更加丰富和交互性强的 Web 应用。

### 安装模板引擎
> 这里以 EJS 为例，使用 npm 安装 

```bash
npm install ejs
```

### 设置引擎
> 在你的 Express 应用中设置模板引擎

```javascript
app.set('view engine', 'ejs')
```

### 创建视图文件

创建一个 EJS 视图文件。例如，创建一个 `index.ejs` 文件

### 渲染视图

使用 `res.render` 函数渲染视图。这个函数接受视图的名称和一个数据对象，数据对象的属性可以在视图中使用

### 示例代码

假设我们有一个简单的 Express 应用，我们想用 EJS 模板引擎渲染一个动态网页。

**01 app.js**

```javascript
const express = require('express')
const path = require('path')

// 创建实例

const app = express()

// 设置动态资源的目录，默认会查找根目录下的 views
app.set('views engine', path.join(__dirname, 'views'))

// 告诉 Express 采用是哪种模板引擎
app.set('view engine', 'ejs')

// 监听不同类型下的不同路径的请求，返回渲染后的动态界面
app.get('/', (req, res) => {
  res.render('index', { title: 'Jude 的网站' })
})

// 设置端口号
const PORT = 3307
app.listen(PORT, () => {
  console.log('服务开启了')
})
```

**02 views/index.ejs**

```html
<!DOCTYPE html>
<html>
<head>
    <title><%= title %></title>
</head>
<body>
    <h1><%= title %></h1>
    <p>EJS渲染的动态网页</p>
</body>
</html>
```
**注意**

- 模板引擎配置：`app.set('view engine', 'ejs')` 告诉 Express 应用使用 EJS 作为模板引擎
- 视图目录：默认情况下， Express 会在项目的 `views` 目录下查找模板文件。
- 数据传递：在 `res.render()` 方法中，我们可以传递一个对象，它的属性在模板中作为变量使用。在上面的代码中，`title` 属性在 `index.ejs` 文件中被使用
- 模板语法：EJS使用 `<%= %>` 来输出变量的值，其它模板引擎可能有不同的语法

## 路由处理方式

在 Express 中，我们可以通过多种方式来处理路由，包括使用应用实例方法、`Router` 类、路由参数、中间件、以及链式路由处理。这些方式提供了强大的灵活性和控制力，使得我们可以依据不同的需要灵活地定义和管理我们的应用中由。这对于构建结构清晰、易于维护的 Express 应用至关重要。

### 应用实例方法

Express 的应用实例（通过 `express()`创建）提供了一系列方法来定义路由，对应于 HTTP 的各种请求方法（如 GET、POST、PUT、DELETE等）。

**语法和示例**

```javascript
const express = require('express')
const app = express()

// 处理 GET 请求
app.get('/path', (req, res) => {
  res.send('GET 请求')
})

// 处理 POST 请求
app.post('/post', (req, res) => {
  res.send('POST 请求')
})

const PORT = 3307
app.listen(PORT, () => {
  console.log('服务开启了')
})
```

### Router 类

使用 `express.Router` 类可以创建模块化的路由处理器。这种方式使得路由更加灵活，易于维护，特别适用于大型应用。

**语法示例**

```javascript
const express = require('express')
const router = express.Router()

// 定义路由
router.get('/', (req, res) => {
  res.send('Home Page')
})

router.get('/about', (req, res) => {
  res.send('About Page')
})

// 使用路由
const app = express()
app.use('/', router)

const PORT = 3307
app.listen(PORT)
```

### 路由参数

路由参数用于捕获 URL 中的动态值 。它们通常用于依据 URL 中的某个部分来获取数据。

**语法示例**

```javascript
app.get('/user/:userId', (req, res) => {
  const userId = req.params.userId
  res.send(`User ID is ${userId}`)
})
```

### 路由中间件

在 Express 中，我们可以为路由指定一个或者多个中间件函数，这些函数可以执行任何代码、修改请求和响应对象、结束请求-响应循环，或调用堆栈中的下一个中间件。

**语法和示例**

```javascript
// 中间件函数
const logMiddleware = (req, res, next) => {
  console.log('请求捕获到了')
  next() // 调用下一个中间件/路由处理器
}

// 应用中间件于特定路由
app.get('/somepath', logMiddleware, (req, res) => {
  res.send('处理somepath 的响应')
})
```

### 链式路由

Express 允许我们以链式的方式对同一个路径使用多个处理器

**语法和示例**

```javascript
app.route('/book')
  .get((req, res) => {
    res.send('查询 book')
  })
  .post((req, res) => {
    res.send('添加book')
  })
  .put((req, res) => {
    res.send('更新 book')
  })
```

## 路由模块化

在实际的 Express 应用，路由处理通常遵循模块化和可维护的原则。这意味着优先考虑使用 `express.Router` 来创建可组合的路由处理器。这种方式有助于将路由相关的处理逻辑组织在一起，使得代码更加清晰和易于管理。

### `express.Router` 模块化

将相关的路由处理逻辑组织到单独的模块中，每个模块使用 `express.Router`实例

假设我们有一个博客应用，那么我们可以将与用户相关的路由放在一个模块中，将与文章相关的路由放在另一个模块中。

**01 用户路由模块（`users.js`）**
```javascript
const express = require('express')
const router = express.router()

router.get('/', (req, res) => {
  res.send('User list')
})

router.post('/', (req, res) => {
  res.send('创建一个新的用户')
})

// 当前模块中就包含了所有与 user 相关的路由处理
module.exports = router

```

**02 文章路由模块（`posts.js`）**

```javascript
const express = require('express')
const router = express.Router()

router.get('/', (req, res) => {
  res.send('List of blog posts')
})

router.post('/', (req, res) => {
  res.send('Create a new blog post')
})

module.exports = router
```

**03 主应用文件（`app.js`）**

```javascript
const express = require('express')
const router = express.Router()

// 引入不同的路由处理模块
const usersRouter = require('./users')
const postsRouter = require('./posts')

// 采用中间件的方式让不同路由生效
app.use('/users', usersRouter)
app.use('/posts', postsRouter)

// 设置端口信息
const PORT = 3307
app.listen(PORT, () => {
  console.log(`当前服务运行在http://localhost:${PORT}`)
})
```

### RESTful API 原则

在设计 API 时，遵循 RESTful 设计原则可以让我们的 API 更加直观和易于理解。这意味着使用 HTTP 方法（如 GET POST PUT DELETE）和路径来表达操作意图。

### 中间件预处理

使用中间件来处理跨多个路由的通用逻辑，如验证、日志记录等。

### 错误处理

为路由提供统一的错误处理逻辑，以便于管理异常和提供一致的错误响应