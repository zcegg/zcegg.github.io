## 直接操作MySQL

在 Node.js 中连接 MySQL 数据库，我们通常会使用 `mysql` 这个流行的第三方模块，并使用该模块提供的 API 来与 MySQL 数据库进行交互。但这里想说的是另外一个叫 `mysql2` 的库，至于两者之间的渊源，这里不做讲解。我们只关注它是 `mysql` 的平替，且异步编程支持的更友好即可。两者的使用逻辑是一致的，同时如果我们安装的是 `mysql8` 之后的版本，那么更应该直接使用 `mysql2`。

**01 安装模块**

```bash
npm install mysql2
```

**02 导入mysql模块**

```javascript
const mysql = require('mysql')
```

**03 创建数据库连接**

使用 `mysql.createConnection()` 方法创建一个到 MySQL 数据库的连接。我们需要提供包括数据库服务器地址、数据库用户名、密码和数据名称在内的数据库连接信息。

```javascript
const connection = mysql.createConnection({
  host: 'localhost'， // 数据库服务器地址
  user: 'username'， // 数据库用户名
  password: 'password'， // 数据库密码
  database: 'database'， // 要连接的数据库
})
```

**04 连接到数据库**

使用 `connection.connect()` 方法建立连接

```javascript
connection.connect((err) => {
  if(err){
    console.error('错误信息是：' + err.stack)
    return
  }
  console.log('连接的id是 ' + connection.threadId)
})
```

**05 执行查询语句**

```javascript
connection.query('SELECT * FROM stu;', (err, results, fields) => {
  if(err) throw err
  console.log(results)
})
```

**06 关闭连接**

```javascript
connection.end();
```

**07 完整示例**

假设我们现在有一个 `demo` 数据库，在 `demo` 里有一个 stu 表，其中有一些数据。包含 `stu_id`，`stu_name`， `stu_age` 三个字段，我们在 Node.js 中连接到 mysql，然后进行查询。

```javascript
// 导入 mysql 模块

const mysql = require('mysql2')

// 创建数据库连接
const connection = mysql.createConnection({
  host: 'localhost', // 数据库服务器地址
  user: 'root', // 数据库用户名
  password: 'root123456', // 数据库密码
  database: 'demo' // 要连接的数据库
})

// 连接到数据库
connection.connect((err) => {
  if (err) {
    console.error('Error connecting： ' + err.stack)
    return
  }
  console.log('连接的 id ' + connection.threadId)
})

// 执行查询
connection.query(`SELECT * FROM stu`, (err, results, fields) => {
  if (err) throw err
  console.log(results)
})

// 关闭连接
connection.end()
```

## 数据库 ORM

ORM(Object-Relational Mapping) 是数据库对象关系映射的简称，一种常见的编程技术，用于在关系数据库和业务实例体对象之间进行转换。简而言之，ORM 允许我们用面向对象的方式来操作数据库中的数据，而不用直接通过 SQL 语句来完成。

**01 为什么需要ORM**

- 简化数据操作：ORM 让开发者可以使用熟悉的面向对象的方式来处理数据库操作，面不再直接编写复杂的 SQL 代码。
- 减少代码重复：ORM 通过提供通用的数据访问模式来减少重复的数据访问代码
- 提高开发效率：由于减少了对 SQL 的直接操作，ORM 可以加快开发过程，使开发者能够专注于业务逻辑
- 数据库抽象：ORM 提供了数据库抽象，使得更换数据库系统时，不需要重写大量代码
- 数据型和业务模型分离：ORM 允许更清晰地分离数据模型和业务模型，提高代码的可维护性

**02 Node生态下常见ORM库**

- Sequelize：是一个基于 Promise 的 node.js ORM，支持Postgres、MySQL、MariaDB、SQLite 和 Microsoft SQL Server。
- TypeORM：支持使用 TypeScript 和 JavaScript（最新版的 ECMAScript）开发，支持 MySQL MariaDB PostgreSQL SQLite等多种数据库
- Prisma：是一个较新的 ORM， 提供了强大的类型安全和数据库工具，支持 Postgres、MySQL、MariaDB、SQLite
- Mongoose：专为 MongoDB 设计的对象数据模型（ODM）库，它提供了一种基于模式的解决方案来建模应用程序数据。
- 其它

## Sequelize 库

Sequelize 是一个强大的 Node.js ORM 库，用于异步操作各种数据库。

### 连接池

连接池（Connection Pool）是一种创建和管理数据库连接的技术。它维护一组已经建立的数据库连接，并在需要时将这些连接提供给应用程序。当应用程序完成数据库操作并释放连接后，该连接不会关闭，而是返回到连接池中，以便再次使用。

Sequelize 通过内部的连接池来管理数据库连接，这样做既提高了性能和资源利用率，又保持了稳定性和可靠性。通过正确配置连接池，我们可以确应用在高并发情况下依然能高效稳定运行。

**01 Sequelize和连接池的关系**

1. `Sequelize 的连接管理`
   1. Sequelize 内部使用连接池来管理数据库连接
   2. 当创建一个新的Sequelize 实例时，它会依据配置创建一个连接池
   3. 这个连接池负责维护数据库连接，并在城要的时提供已建立的连接
2. `配置连接池`
   1. 在 Sequelize 中，我们可以通过其构造函数的选项来配置连接池的参数，如最大连接数、最小连接数、连接的最大空闲时间等。
   2. 这些配置项允许开发者依据应用需求和数据库服务器的能力调整连接池的行为

**02 连接池作用和好处**

1. `提高性能`
   1. 连接池可以重用现有的数据库连接，而不是每次查询时都建立新连接，从而显著提高应用性能
   2. 连接和断开数据库连接是耗时操作，使用连接池可以减少这些操作
2. `资源管理`
   1. 连接池帮助管理数据库连接资源，防止数据库因为过多的连接请求而超载
   2. 通过限制最大连接数，连接池确保数据库不会因为过多的并发连接而崩溃
3. `稳定可靠`
   1. 连接池可以自动管理无效连接的检测和重建，提高应用程序的稳定性和可靠性

```javascript
const Sequelize = require('sequelize')

const sequelize = new Sequelize('database', 'username', 'password', {
  host: 'localhost',
  dialect: 'mysql',
  pool: {
    max: 10,  // 连接池中最大连接数量
    min: 0, // 连接池中最小连接数量
    acquire: 30000, // 一个连接在被释放之前最长可被闲置的时行（毫秒）
    idle: 10000 // 一个连接在被从连接池中取出之间，最长可被闲置时间
  }
})
```

### 使用流程

**01 安装 Sequelize 和数据库驱动**

首先，我们需要安装 `Sequelize` 以及对应数据库的驱动，例如：对于 MySQL，我们可以使用 `mysql2` 驱动。

```bash
npm install --save sequelize
npm install --save mysql2
```

**02 配置数据库连接**

在 Node.js 项目中，首先引入 Sequelize 并配置数据库连接信息。

```javascript
const Sequelize = require('sequelize')

// 创建一个 Sequelize 的实例并配置数据库连接
const sequelize = new Sequelize('database', 'username', 'password', {
  host: 'localhost',
  dialect: 'mysql' // 依据数据库类型选择 'mysql' | 'mariadb' | 'postgres' | 'mssql'
  // 其它配置项
})
```

**03 测试数据库连接**

我们可以通过调用 `.authenticate()` 方法来测试数据库连接是否成功

```javascript
sequelize.authenticate()
  .then(() => {
    console.log('连接成功')
  })
  .catch((err) => {
    console.log('连接失败--->', err)
  })
```

**04 定义模型**

在 Sequelize 中，我们可以定义模型来表示数据库中的表

```javascript
const User = sequelize.define('user', {
  // 定义列
  fistrName: {
    type: Sequelize.STRING,
    allowNull: false
  },
  lastName: {
    type: Sequelize.STRING,
    // allowNull 默认为 true
  }
  // 其它配置
})

// 同步数据模型至数据库
User.sync()
```

**05 使用模型进行操作**

一旦模型定义好并同步到数据库，我们就可以使用它来进行 CURD 操作

```javascript
// 创建新用户
User.create({firstName: 'John', lastName: 'Doe'})
  .then(user => console.log(user.get({plain: true})))
  .catch(err => console.log(err))
```

**06 注意**

- 在产生环境中，不建议直接在代码中硬编码数据库凭据。考虑使用环境变量或配置文件来管理敏感信息
- Sequelize的 `sync()` 方法可以自动创建或更新数据库表，但在生产环境中，更推荐使用迁移工具来管理数据库结构变化
- 依据需求，可能需要配置更多的选项，比如连接池设置、日志记录、时区设置等
- Sequelize 支持 Promise 和 async、awiait 方便进行异步操作

## 定义模型（新建表）

在 Sequelize 中，定义数据模型是通过定义模型类来完成的。这个模型类表示数据库中的表，模型的每个实例对应表中的一行 Sequelize 使用 JS 类和对象映射数据库的表结构。

**01 创建 sequelize 实例**

首先我们需要导入 Sequelize 并创建一个 Sequelize 实例，通过这个实例可以连接数据库

```javascript
const Sequelize = require('sequelize')

const sequelize = new Sequelize('database', 'username', 'password', {
  host: 'localhost',
  dialect: 'mysql'
})
```

**02 定义模型**

使用 `sequelize.define` 方法定义模型，该方法接受三个参数：模型名称、模型属性、模型选项。

```javascript
const User = sequelize.define('user', {
  // 定义属性
  firstName: {
    type: Sequelize.STRING,
    allowNull: false
  },
  lastName: {
    type: Sequelize.STRING
  },
  age: {
    type: Sequelize.TINYINT,
    defaulteValue: 21
  }
}, {
  // 模型选项
  timestamps: false // 禁用时间戳
})
```

**03 同步至数据库**

使用 `sync` 方法将模型同步至数据库，这将创建表（如果它不存在）

```javascript
User.sync().then(() => {
  console.log('User 表创建成功')
})
.catch((err) => {
  console.log('创建失败--->', err)
})
```

**04 实例**

假设我们当前要创建一个 user 表，包含 id name age gender 几个字段，使用 Sequelize 来创建模型，并完成相应约束的定义。

```javascript
const Sequelize = require('sequelize')

// 创建 Sequelize 实例连接数据库

const sequelize = new Sequelize('demo', 'root', 'root123456', {
  host: 'localhost', // 数据库服务器地址
  port: 3306, // 数据库端口号
  dialect: 'mysql', // 告诉 sequelize 当前要操作的数据库是 mysql
  pool: {
    max: 5, // 最多连接数
    min: 0, // 最少连接数
    idle: 10000, // 当前连接多久没有操作则断开
    acquire: 30000 // 多久没有获取到连接就断开
  }
})

// 定义一个 user 模型（创建 user表）
const User = sequelize.define('user', {
  id: {
    type: Sequelize.INTEGER, // 当前类型
    primaryKey: true, // 是否设置为 主键
    autoIncrement: true
  },
  name: {
    type: Sequelize.STRING, // VARCHAR(255)
    allowNull: false, // 不允许为 Null
    unique: true // 设置 unique
  },
  age: {
    type: Sequelize.TINYINT, // 设置类型
    defaultValue: 66
  },
  gender: {
    type: Sequelize.ENUM(['男', '女', '其它']),
    defaultValue: '其它'
  }
}, {
  freezeTableName: true, // 告诉 sequelize 不需要自动将表名变成复数
  // tableName: 'student', // 自定义表名
  timestamps: false, // 不需要自动创建 createAt/updateAt 这两个字段
  indexs: [
    {
      name: 'idx_age', // 索引名称
      fields: ['age'] // 索引字段名称
    }
  ]
})

// 同步模型到数据库
User.sync().then(() => {
  console.log('user表新建成功')
})
  .catch((err) => {
    console.log('创建失败-->', err)
  })
```

**05 注意**

- 数据类型：确保我们的模型属性选择了适当的数据类型
- 默认值和验证：可以在模型定义中添加默认值和验证规则
- 模型同步：`sync` 方法在开发阶段使用，但是在生产环境中可能需要更复杂的数据库迁移策略
- 模型选项：我们可以在模型定义中设置多种选项，如是否自动添加时间戳（`createdAt` 和 `updatedAt`）、是否使用软件删除等。

## 插入数据

在 sequelize 中，一旦定义了模型，就可以使用该模型的方法来插入数据到数据库中。最常用的方法是 `create` ，它用于在模型对应的表中创建一条记录。

**01 语法**

```javascript
Model.create({
  // 对应字段及其值 
})
```

**02 示例**
> 假设我们定义了一个 `user` 模型

```javascript
const Sequelize = require('sequelize')

const sequelize = new Sequelize('demo', 'root', 'rootxxx', {
  host: 'localhost',
  port: 3306
})

const User = sequelize.define('user', {
  name: {
    type: Sequelize.STRING,
    allowNull: false
  },
  email: {
    type: Sequelize.STRING,
    allowNull: false
  }
})

sequelize.sync().then(() => {
  console.log('User表创建成功')
  // 模型创建成功之后插入记录
  User.create({
    name: 'syy',
    email: 'syy@163.com'
  }).then(() => {
    console.log('插入成功')
  })
  .catch((err) => {
    console.log('插入失败', err)
  })
})
.catch((err) => {
  console.log('创建失败', err)
})
```

**03 注意**

- 数据验证：当我们使用 `create` 方法时，Sequelize 会自动执行定义在型上的任何验证
- 异步处理：由于 `create` 是异步操作，需要使用 `.then()` 和 `.catch` 或 `async/await` 来处理响应或捕捉错误
- 数据库同步：确保在尝试插入数据值之前，使用 `sequelize.sync()` 方法同步模型到数据库。在生产环境中，通常不直接使用 `sync` ，而是使用迁移策略
- 安全性：在处理用户输入时要格外小心，以避免诸如 SQL 注入等安全问题。

**04 实例**

```javascript
const Sequelize = require('sequelize')

const sequelize = new Sequelize('demo', 'root', 'root123456', {
  host: 'localhost',
  port: 3306,
  dialect: 'mysql',
  pool: {
    max: 5,
    min: 0,
    idle: 10000,
    acquire: 30000
  }
})

// 创建模型
let User = sequelize.define('user', {
  id: {
    type: Sequelize.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  name: {
    type: Sequelize.STRING,
    allowNull: false,
    unique: true
  },
  age: {
    type: Sequelize.TINYINT,
    defaultValue: 22
  },
  gender: {
    type: Sequelize.ENUM(['男', '女', '其它']),
    defaultValue: '其它'
  }
}, {
  freezeTableName: false,
  timestamps: false,
  indexs: [
    {
      name: 'idx_age',
      fields: ['age']
    }
  ]
})

// 同步数据库并插入数据
sequelize.sync().then(() => {
  console.log('模型创建成功')
  // 创建数据插入
  User.create({
    name: 'yyshi',
    age: 18,
    gender: '男'
  }).then(() => {
    console.log('数据插入成功')
  })
    .catch((err) => {
      console.log('数据插入失败--->', err)
    })
})
  .catch((err) => {
    console.log('模型创建失败')
  })
```

## CRUD 总结

在 Sequelize 中执行 CURD （创建、读取、更新、删除）操作涉及到几个关键方法，每个都对应于数据库操作的一个方面。

### 创建（Create）
> 使用 `create` 方法在数据库中创建新记录

```javascript
const User = sequelize.define('user', {
  name: Sequelize.STRING,
  email: Sequelize.STRING
})

User.create({
  name: 'syy',
  email: 'syy@163.com'
}).then(() => {
  console.log('插入成功'， user.get())
})
.catch((err) => {
  console.log('插入失败', err)
})
```

### 读取（Read）

> 使用 `findAll` `findOne` 或 `findByPK` 等方法读取数据库记录

- 查找所有记录

```javascript
User.findAll().then((users) => {
  console.log('所有用户--->', users.map(user=> user.get()))
})
.catch((err) => {
  console.log('Error', err)
})
```

- 查找单个记录

```javascript
User.findOne({where: {name: 'syy'}})
  .then((user) => {
    console.log('User', user.get())
  })
  .catch((err) => {
    console.log('Error', err)
  })
```

- 按主键查询

```javascript
User.findByPk(1).then((user) => {
  if(user){
    console.log('User', user.get())
  }else{
    console.log('user 不存在')
  }
})
.catch((err) => {
  console.log('Error', error)
})
```

### 更新（Update）

> 使用 `update` 方法更新数据库中的记录

```javascript
User.update({name: 'abc'}, {where: {id: 1}}).then(() => {
  console.log('更新成功')
})
.catch((err) => {
  console.log('Error', err)
})
```

### 删除（Delete）

> 使用 `destory` 方法删除数据库中的记录

```javascript
User.destory({where:{id: 1}}).then(() => {
  console.log('删除成功')
})
.catch((err) => {
  console.log('Error', err)
})
```