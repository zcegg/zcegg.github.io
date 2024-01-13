## NodeJS中连接MySQL

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

## 其它操作
