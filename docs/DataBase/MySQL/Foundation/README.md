## 数据库介绍

数据库是一个用于存储、检索、管理和处理数据的系统。它们是信息管理的关键组件，在许多应用程序和组织中发挥着核心作用。数据库允许用户高效效地组织、存储和查询大量信息。

### 基本概念

1. **数据的组织和存储**：数据库通过表格式的形式存储数据，表格中的每一行代表一个数据项，每一列代表该数据项的一个属性。
2. **数据管理系统（DBMS）**：数据库通常通过一个数据库管理系统进行管理。DBMS 是一种软件，它提供了创建、管理和操作数据库的工具和功能
3. **数据完整性和安全性**：数据库设计时会考虑数据的完整性（确保数据的准确性和一致性）和安全性（保护数据免受未授权访问和损坏）

### 发展历史

1. **早期文件系统**：在数据库出现之前，数据存储在文件系统中，每个应用程序都需要单独的文件处理程序来读写数据，这种方式效率低下，且数据共享和集成困难。
2. **层次式和网状数据库**：随着计算机技术的发展，出现了层次式和网状数据库系统。它们提供了比文件系统更复杂的数据组织方式，但数据的物理结构和应用程序紧密耦合，灵活性有限。
3. **关系型数据库**：这种模型用表格的形式来组织了数据，并通过 SQL（结构化查询语言）进行操作，大大提高了灵活性和易用性。
4. **对象数据库和NoSQL**：随着面向对象编程的普及，对象数据库（ObjectStroe和 Versant）出现。同时，为了应对大数据和高扩展性的需求，NoSQL 数据库（如 MongDB, Cassandra）也开始兴起
5. **云数据和大数据**：云计算的兴起带来了基于云的数据库服务（如 Amazon RDS, Google Cloud SQL，腾讯云，阿里云等），同时大数据技术（如 Hadoop， Spark）也影响了数据库技术的发展。

### 数据库类型
1. **关系型数据库**：这是最常见的数据库类型，如 MySQL PostgreSQL SQLite 和 Oracle。它们使用表来存储数据，这些表通过关系（例如，外键）相互连接。
2. **非关系型数据库**：又称为 NoSQL 数据库，如 MongoDB\Cassandra 和 Redis 。这类数据库不使用传统的表格模型，而是采用文档，键值对、图或列存储数据。
3. 对象数据库：这种数据库将数据作为对象存储，与面向对象编程语言中的对象类型

### MySQL数据库

MySQL 是一个开源的关系型数据库管理系统（RDBMS），使用结构化查询语言（SQL）进行数据管理。它是由瑞典的 MySQL AB 公司开发的，后来被 Sun Microsystems 收购，Sun 随后又被甲骨文公司（ Oracle Corporation）收购。作为世界上最流行的开源数据库之一，MySQL 在小型到大型的应用程序中都得到了广泛的使用，尤其是在与Web应用结合时。

## MySql 安装和连接

当前文档基于 MySQL8.x 进行编写，它的安装其实非常简单，只需几个关键步骤

### 下载 MySQL

1. **访问官方网站**：前往 MySQL 的官方网站 [下载MySQL](https://dev.mysql.com/downloads/mysql/ "MySQL 官网")
2. **选择版本**：基于我们的操作系统，选择对应系统的版本
3. **选择安装包类型**：依据我们的操作系统选择合适的安装包类型（如 windows 上的 MSI 安装程序，Linux 上的 RPM 包， mac 上的 DMG）
4. **下载**：下载相应版本的安装文件，对于个人使用，我们可以选择 `Community` 版本，这是一个免费开源的版本。
![下载MySQL](../img/mysql_01.png)

### 安装 MySQL

1. windows 安装
   1. 运行下载的 MSI 文件
   2. 按照安装向导步骤操作，选择所需的配置选项，如安装路径、配置类型（开发者、服务器、客户端）等。
   3. 设置 root 用户密码，并记住它，因为后续需要用到
   4. 配置 MySQL 服务，包括是否在系统启动时自动运行（可选）

2. Linux 安装
   1. 对于使用 RPM 包的 Linux 发行版（如 Red Hat、Fedora、CentOS），使用 `rpm` 命令安装。
   2. 对于 Debian或 Ubuntu，可能需要使用 `dpkg` 命令或通过添加 MySQL 仓库后使用 `apt-get` 安装。
   3. 安装过程中需要设置 root 用户名密码
   4. 启动 MySQL 服务，并确保它在启动时自动运行

3. macOS 安装
   1. 运行下载的 DMG 文件
   2. 遵循安装向导进行安装
   3. 设置 root 用户密码，并启动 MySQL 服务

### 命令行连接

在命令行（或终端）中连接到 MySQL 数据库，通常使用 MySQL客户端工具。但是在实际生产环境中，我们一般采用 GUI 可视化工具。

**01 基本命令**

```shell
  mysql -u [username] -p[password] -h [hostname] -P [port]
```

**说明**

1. `-u [username]` mysql 的用户名
2. `-p [password]` mysql 的密码，注意 -p 和密码之间没有空格。如果省略密码，系统在执行命令后会提示我们输入
3. `-h [hostname]` mysql 服务器的主机名，如果 mysql 服务器运行在本地机器上，可以使用 localhost ，或省略该选项
4. `-p [port]` mysql 服务器监听的端口号，如果是默认端口（3306），可以省略该选项

**示例**

假设我们的用户名是 `root` ，密码是 `123456`，mysql 服务器运行在本地机器上，使用默认端口，则连接命令为：

```bash
mysql -u root -p 123456
mysql -u root -p 然后依据提示输入密码
```

**02 注意事项**

1. 确保在运行 mysql 客户端之前， MySQL 服务已经启动
2. 在生产环境中，为了安全起见，避免在命令行中直接包含密码，最好在提示时输入

### GUI连接

MySQL 数据库的图形用户界面（GUI）工具提供了一种直观和友好的方式来管理数据库，执行查询，以及进行数据库设计和优化。

**01 MySQL Workbench**

它是 MySQL 官方提供的一个强大的可视化工具，它提供了数据库设计，sql 开发，数据库管理和服务器配置等功能。通过它的 EER(增强实体-关系)图功能，用户可以非常直观地设计和修入数据库结构

**02 Navicat for MySQL**

它是一个商业的数据库管理工具，提供跨平台支持（windows macos linux）。它具有丰富的功能，如数据库设计、数据同步、备份、导入、导出等功能，适用于专业开发人员和新手。推荐购买正版使用

## 初识 SQL 语句

### 认识 SQL 语句

SQL(Structured Query Language) 是一种专门用于管理和操作关系型数据库的编程语言，它允许用户创建、修改、管理和检索数据库中存储的数据。 SQL 的设计目录是提供一种简单有效的方式来读写复杂的数据结构。

1. **标准化**：SQL 是一种标准化的语言，被广泛应用于各种数据库系统中，如 MySQL, SQL Server，Oracle 等。
2. **声明性**：与过程性编译语言不同，SQL 是声明性的。用户只需要指定要完成的任务（例如检索和更新数据），而不是需要提供具体的步骤或方法来完成这些任务。
3. **多功能性**：SQL 可以用来执行各种数据操作，包括数据查询、数据更新（插入、修改、删除）数据库和表的创建和修改，以及数据访问控制。

### SQL 语句分类

**01 数据定义语句（DDL）**
- 用途：用于定义和修改数据库(表)结构
- 关键字：`CREATE` `ALTER` `DROP`
- 示例：`CREATE TABLE users (id INT, name VARCHAR(50))`
- 说明：DDL 用于创建新的数据库、表、视图等，或者修改现有数据库结构的布局。`CREATE` 用于创建新结构， `ALTER` 用于修改现有结构，`DROP` 用于删除结构。

**02 数据操作语句（DML）**
- 用途：用于在数据库中插入、更新、删除数据
- 关键字：`INSERT` `UPDATE` `DELETE`
- 示例：`INSERT INTO users (id, name) values(1, 'syy')`
- 说明：DML 用于管理数据库中的表的数据，`INSERT` 用于添加新的记录， `UPDATE` 用于更新现有记录，`DELETE` 用于删除记录

**03 数据查询语句（DQL）**
- 用途：用于查询数据库中表的数据
- 关键键：`SELECT`
- 示例：`SELECT name, age FROM users WHERE age>18`
- 说明：此类 SQL 用于从数据库的表中查询数据，可以使用各种条件来过滤数据，选择特定列，以及对结果进行排序和分组

**04 数据控制句语（DCL）**
- 用途：用于管理数据库的权限和安全
- 关键字：`GRANT` `REVOKE`
- 示例：`GRANT SELECT ON database.tabale TO user`
- 说明：DCL 涉及到数据库的安全性和用户权限，`GRANT` 用于授权，而 `REVOKE` 用于撤权



## 数据库操作（DDL）

上文提到过 DDL 语句的用途就是用来控制数据库、数据表的增、删、改操作。这里就归纳一些生产过程中可能会用到的数据库操.

### 创建数据库

在创建数据库的时候，如果数据已经存在，标准的 `CREATE DATABASE` 语句会导致语法错误。为了避免这种情况，我们可以使用 `IF NOT EXISTS` 选项，这样的话，如果数据库已经存在，SQL 命令就不会执行任何操作，也不会显示报错。

```shell
# 创建一个数据库，如果它不存在的话
CREATE DATABASE IF NOT EXISTS database_name
```

**说明**
- `IF NOT EXISTS` 这是一个条件判断，用于检查指定的数据库名称是否已存在，如果数据库不存在，那么就会创建一个新的数据库；如果已经存在，那么 SQL 命令不会执行任何操作，也不会引发错误。
- `database_name` 是我们要创建的数据库名称。名称应该遵循数据库系统的命名规则，例如：不包含特殊字符、不与保留字冲突等

**注意**
- `命名规则`：选择的数据库名称应该符合特定的规则和限制，例如：避免使用空格、特殊字符或关键字。
- `字符集和排序规则`：在创建数据库时，我们可以指定字符集和排序规则。

```shell
CREATE DATABASE IF NOT EXISTS mydatabase
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci
```

这里指定了字符集为 `utf8mb4` 和对应的排序规则 `utf8mb4_unicode_ci`

### 删除数据库

删除数据库是一个重要操作，一旦执行，所有存储在数据库中的数据将被永久删除。

```shell
DROP DATABASE IF EXISTS database_name
DROP DATABASE IF EXISTS mydatabase
```

**注意**
- `IF EXISTS` 这个条件用于检查指定的数据库是否存在，如果数据库存在，则执行删除操作；如果不存在，则不执行任何操作，避免了错误。
- `操作前备份`：在执行删除操作之前，请确保已备份所有重要数据。

### 修改数据库

修改数据库通常涉及更改其配置或参数。这类操作依赖于具体的数据库系统和设置，但是修改数据的操作在实际生产时，并不常见，因为在设计之初这些信息都应制定完善。

**示例**

在 MySQL 中，修改数据库的字符集和排序规则

```shell
ALTER DATABASE database_name
CHARACTER SET = utf8mb4
COLLATE=utf8mb4_unicode_ci
```

**注意**
- 修改数据库属性可能会影响存储在其中的数据，应该谨慎操作
- 一些修改可能需要数据库重启才能生效

### 使用某个数据库

在执行针对特定数据库的操作之间，需要先选择（使用）这个数据库

```bash
  USE database_name
```

**注意**
- 在多用户、多数据库的环境中，确保选择了正确的数据库，避免在错误的数据库上执行操作

### 查看数据库

查看数据库服务器上所有数据库的列表

```bash
  SHOW DATABASE;
```

**注意**
- 这个命令列出所有数据库，但我们可能只能够访问其中一部分，这取决于我们的用户权限

我们还可以获取特定数据库的详细信息，如表结构、大小等。

**示例**

```bash
  SHOW CREATE DATABASE datatabase_name
  SHOW TABLES FROM database_name
  DESCRIBE database_name.table_name
```

**注意**
- 获取数据库信息的方法和可用命令会依据具体的数据库系统而有所不同

## 数据表操作（DDL）

### 创建数据表

创建新表就是在数据库中定义新数据结构的过程

**语法**

```bash
  CREATE TABLE table_name(
    column1 datatype constraint,
    column2 datatype constraint,
    ......
  )
```

**示例**

```bash
  CREATE TABLE students (
    id INT AUTO_INCREMNT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    enrollement_data DATE
  )
```

**注意**
- 每个列(column) 必须有一个数据类型
- 可以为列添加约束，如 `NOT NULL` (非空)、`PRIMARY_KEY`(主键)等。

### 删除数据表

删除表将移除表及其所有数据

**语法**

```bash
  DROP TABLE IF EXISTS table_name;
```

**示例**

```bash
  DROP TABLE IF EXISTS students;
```

**注意**
- `IF EXISTS` 防止在表不存在时出现语法错误
- 此操作不可逆，所有数据将被永久删除

### 修改表结构

修改表结构可以添加、删除或更改列

**添加列**

```bash
  # ADD 新字段 字段类型
  ALTER TABLE table_name ADD column_name datatype;
  # 示例
  ALTER TABLE students ADD birth_date DATE;
```

**删除列**

```bash
  # 关键字 DROP COLUMN 字段名
  ALTER TABLE table_name DROP COLUMN column_name
  # 示例
  ALTER TABLE sudents DROP COLUMN brith_date
```

**更改列**

```bash
  # MODIFY COLUMN 字段名 字段类型
  ALTER TABLE table_name MODIFY COLUMN column_name new_type;
  # 示例
  ALTER TABLE students MODIFY COLUMN name VARCHAR(255)
```

**更改表名**

```bash
  # 关键字 RENAME TO 新表名
  ALTER TABLE table_name RENAME TO new_table_name;
  # 示例
  ALTER TABLE students RENAME TO student_info;
```

**注意**
- 修改表结构可能会影响现有数据
- 在进行结构更改之前，请确保已备份表
- 修改表名是一个立即执行的操作，一旦更改，原始的表名将不再生效
- 在不同的数据库系统中，修改表名可能使用 `RENAME TABLE table_name TO new_table_name`

### 查询表信息

查询数据库中现有表的信息，如表结构

**语法**

```bash
  SHOW TABLES; # 查询所有的 table 列表
  DESCRIBE table_name; # 查询表的创建语句
```

**示例**

```bash
  SHOW TABLES;
  DESCRIBE sutdents;
```

**注意**
- 这些命令对于了解数据库中的表和表结构非常有用。
- `DESCRIBE` 提供了关于表中列的详细信息，如数据类型、是否允许空值等。

## 数据类型

MySQL 支持多种数据类型，允许我们依据数据的性质和要求选择最合适的类型。了解这些类型及其语法细节对于设计有效的数据库表结构至关重要。

### 整型类型

整型数据用于存储整数值。MySQL 提供了多种整型数据类型，以适应不同大小的数值范围。

**01 TINYINT**
- 存储范围：-128 到 127 （有符号），0 到 255 （无符号）
- 用途：用于存储小一些的整数值
- 示例：`age TINYINT UNSIGNED`

**02 SMALLINT**
- 存储范围：-32768到32767（有符号），0到65535（无符号）
- 用途：用于存储稍大一点的整数值
- 示例：`year SMALLINT`

**03 MEDIUMINT**
- 存储范围：-8388608 至 8388607 （有符号）， 0 至 16777215（无符号）
- 用途：用于存储中等大小的数值
- 示例：`population MEDIUMINT`

**04 INT 或 INTEGER**
- 存储范围：-2147483648至2147483647（有符号），0至4294967295(无符号)
- 用途：常用的整数类型，适用于大多数需求
- 示例：`user_id INT`

**05 BIGINT**
- 存储范围：-9223372036854775808 到 9223372036854775807（有符号），0 到 18446744073709551615（无符号）
- 用途：用于存储非常大的整数类型值
- 示例：`national_debt BIGINT`

**语法细节**
- 有符号与无符号：可以通过在类型后添加 `UNSIGNED` 关键字来定义无符号整数。无符号整数只能是正数或零，而有符号整数可以是正数、负数或零。
- 自动增长：可以通过 `AUTO_INCREMENT` 关键字设置字段为自增长，常用于主键，例如 `id INT AUTO_INCRMENT PRIMARY KEY`

**使用示例**

```bash
  CREATE TABLE users(
    id INT AUTO_INCREMENT PRIMARY KEY,
    age TINYINT UNSIGNED,
    year_of_exprience SMALLINT,
    score MEDIUMINT,
    total_points BIGINT
  )
```

- `id` ：是一个自增长的主键
- `age`：是一个无符号的 `TINYINT` 类型，适用于存储年龄
- `year_of_experience`：使用 `SMALLINT` 存储工作年数
- `score`：使用 `MEDIUMINT` 存储可能的中等数值
- `total_points`：使用 `BIGINT` 存储可能的很大的数值

### 日期类型

日期和时间类型用于存储日期和时间信息。这些类型提供了灵活的方式来存储和操作日期和时间数据。

**01 DATE**
- 存储格式：`YYYY-MM-DD`（年-月-日）
- 用途：仅用于存储日期，不包括时间
- 示例：`birth_date DATE`

**02 TIME**
- 存储格式：`HH:MM:SS`（小时：分钟：秒）
- 用途：仅用于存储时间，不包括日期
- 示例：`start_time TIME`

**03 DATETIME**
- 存储格式：`YYYY-MM-DD HH:MM:SS`（年-月-日 小时：分钟：秒）
- 用途：用于存储日期和时间
- 示例：`appointment DATETIME`

**04 TIMESTAMP**
- 存储格式：`YYYY-MM-DD HH:MM:SS`，通常用于记录时间戳
- 特点：`TIMESTAMP` 值在存储时会转换为 UTC 时间，在检索时转换回当前时区的时间。
- 用途：常用于记录数据的创建和更新时间

**05 YEAR**
- 存储格式：`YYYY`
- 用途：仅用于存储年份
- 示例：`graduation_year YEAR`

**语法细节**
- 在使用 `DATETIME` 和 `TIMESTAMP` 时，通常 `TIMESTAMP` 用于记录数据的修改创建时间，因为它会自动设置为当前的时间戳
- `TIMESTAMP` 类型也适用于存储那些需要时区转换的时间值
- `DATE` 和 `TIME` 类型更专注于存储单独的日期和时间值

**示例代码**

```bash
  CREATE TABLE events (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    event_date DATE,
    start_time TIME,
    end_time TIME,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  )
```
  
- `event_data` 用于存储事件的日期
- `start_time` 和 `end_time` 用于存储事件的开始和结束时间
- `created_at` 是一个 `TIMESTAMP` 类型，自动设置为记录创建的时间

### 布尔类型

在 MySQL 中，没有专门的布尔(Boolean) 数据类型，但是可以使用 `TINYINT` 类型来表示布尔值，标准的做法是使用 `TINYINT(1)`，其中 `0` 表示 `FALSE`，非零值（通常是 `1`）表示 `TRUE`

**示例**

在下面这的代码中 `isActive` 字段用于表示用户是否处理活跃状态。它被定义为 `TINYINT(1)`，以模拟布尔值。

```bash
  CREATE TABLE users(
    id INT AUTO_INCRMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    isActive TINYINT(1) NOT NULL
  )
```

**插入布尔值数据**

```bash
  INSERT INTO users (id, name, isActive) VALUES ('syy', 1)
  INSERT INTO users (id, name, isActive) VALUES ('tu', 0)
```

在这些插入操作中，`1` 用于表示 `true` （即 syy 是活跃用户）， 而 `0` 用于表示 `false` （即 tu 不是活跃用户）。

**查询布尔类型字段**

在查询布尔类型字段时，我们可以直接使用 `0` 和 `1` 来进行比较

```bash
  # 这个查询将返回所有 isActive 字段值为 1（即真）的记录
  SELECT * FROM users WHERE isActive = 1;
```

**注意事项**
- 在 MySQL 中，布尔值实质上是 `TINYINT` 类型。虽然我们可以使用 `BOOLEAN` 或 `BOOL` 作为列的类型，但它们只是 `TINYINT(1)` 的别名。
- 在处理布尔值时，建议使用 `0` 和 `1` 来保持清晰和一致性。

### 浮点类型

浮点型数据用于存储包含小数点的数值，有两种主要的浮点型数据类型：`FLOAT`和`DOUBLE`。

**01 FLOAT**
- 用途：用于存储单精度浮点数
- 范围：大约 `-3.402823466E+38` 到 `-1.175494351E-38`、`0` 以及 `1.175494351E-38` 到` 3.402823466E+38`。
- 语法：`FLOAT(M, D)`, 其中 `M` 是总位数，`D` 是小数点后的位数

**02 DOUBLE**
- 用途：用于存储双精度浮点数，提供比 `FLOAT` 更大的精度
- 范围：大约 `-1.7976931348623157E+308` 到 `-2.2250738585072014E-308`、`0` 以及 `2.2250738585072014E-308` 到 `1.7976931348623157E+308`。
- 语法：`DOUBLE(M, D)`, 其中 `M` 是总位数， `D` 是小数点后的位数

**示例**

```baash
  CREATE TABLE measurements(
    id INT AUTO_INCREMENT PRIMRY KEY,
    temperature FLOAT(5, 2),
    depth DOUBLE(8, 3)
  )
```

- `temperature` 列被定为 `FLOAT` 类型，总共有5位数字，其中小数点后有2位
- `depth` 列被定义为 `DOUBLE` 类型，总共有8位数字，小数点后有 3 位

**注意**

- `FLOAT` 和 `DOUBLE` 类型都可能导致精度问题，由于它们是近似值类型，可能无法精确表示某些数值。
- 对于需要高精度的计算（如财务计算），应考虑使用 `DECIMAL` 类型，因为它提供了精确的浮点运算。
- 在定义 `FLOAT` 和 `DOUBLE` 时，可以选择是否指定 `M` 和 `D`。如果不指定，数据库系统将选择默认的精度值。

### 枚举类型

枚举类型(ENUM) 用于表示一组预定义的字符串值中的一个。 ENUM 类型非常适合表示有限数量的固定选基，比如状态、类型、标志等。

**语法**

```bash
  ENUM(value1, value2, value3, .....)
```

- ENUM 类型的列只能是一个预定义值列表中的值
- 值在内部由整数索引表示，从 1 开始，但在查询和显示时使用字符串值
- 如果插入不在枚举列表中的值，将产生错误或插入特殊的错误值

**使用枚举类型**

假设我们要创建一个表来存储用户信息，其中包括用户的状态，状态有限制为 `active` `inactive` 和 `pending`

```bash
  CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARE KEY,
    name VARCHAR(100),
    status ENUM('active', 'inactive', 'pending')
  )
```

**插入枚举类型**

```bash
  INSERT INTO users (name, status) VALUES ('syy', 'active');
  INSERT INTO users (name, status) VALUES ('abc', 'pending');
```

**注意**
- 有限的选项：枚举类型适用于有限选项的场景。如果选项很多或经常变化，使用枚举可能不是最佳选择。
- 插入无效值：如果尝试插入不在枚举列表中的值， MySQL 默认会插入一个空值，如果列被定义为 `NOT NULL`，则会插入索引为 0 的特殊错误值，并显示警告
- 性能：在某些情况下，使用枚举可以提高性能，因为它们在内部由整数表示，但这可能会增加维护的复杂性。
- 可读性 VS 灵活性：枚举类型增强了数据完整性和可读性，但牺牲了灵活性。在枚举列表中添加或删除项需要修改表结构

### 定点类型

在 MySQL 中的定点类型用于存储精确的小数值，主要用于需要精确数值表示的场景，如财务计算。定义类型主要包括 `DECIMAL` 和 `NUMERIC`，这两者在 MySQL 中同义。

**语法**

```bash
  DECIMAL(M, D)
  NUMERIC(M, D)
```

- `M` 总的数字个数（精度），包括小数点两侧的数字
- `D` 小数点后的数字个数（标度），如果省略则默认为 0 
- 示例：`price DECIMAL(10, 2)` 表示 `price` 是一个最多有 10 位数字的定点数，其中最多有 2 位小数。

**使用定点类型**

```bash
  CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    price DECIMAL(10, 2)
    weight DECIMAL(5, 3)
  )
```

- `price` 列被定义为 `DECIMAL(10, 2)` ，意味着价格最多可以是10位数字，其中包括小数点后的 2 位
- `weight` 列被定义为 `DECIMAL(5, 3)` ， 表示重量最多是 5 位数字，其中包括小数点后的 3 位

**插入定义类型数据**

```bash
  INSERT INTO products (name, price, weight) VALUES ('abc', 199.99, 0.255)
```

**注意**
- **精度：** `DECIMAL` 类型非常适合需要精确计算的应用，如金融和会度，因为它避免了浮点数类型的舍入误差
- **存储空间**：虽然 `DECIMAL` 类型提供了精确的数值表示，但它需要更多的存储空间相比于浮点类型（如 `FLOAT` 和 `DOUBLE`）。
- **性能**：在某些情况下，使用 `DECIMAL` 可能导致比使用浮点数慢的计算和处理束度
- **范围和溢出**：尽管 `DECIMAL` 提供了精确的存储，但仍然有范围限制。超出范围的值将会引起错误。

### 文本类型

在 MySQL 中，文本类型主要用于存储字符串数据。这些数据类型包括  `TEXT` `TINYTEXT` `MEDIUMTEXT` `LONGTEXT`。

**TEXT**
- 用于存储长文本字符串，最大长度为 65535 字节
- 适合存储不确定大小的文本，如文章、描述等。
- 示例：`description TEXT`

**TINYTEXT**
- 最大长度为 255 字节
- 示例：`remarks TINYTEXT`

**MEDIUMTEXT**
- 最大长度为 16777215 字节
- 示例：`comments MEDIUMTEXT`

**LONGTEXT**
- 最大长度为 4,294,967,295 字节
- 适用于非常长的文本，如博客内容、程序代码等
- 示例：`content LONGTEXT`

**示例**

```bash
  CREATE TABLE articles (
    id INT AUTO_INCRMENT PRIMARY KEY,
    title VARCHAR(200),
    abstract TEXT,
    body LONGTEXT
  )
```

- `title` 列被定义为最多 200 个字符的  `VARCHAR` 类型
- `abstract` 列使用 `TEXT` 类型来存储文章摘要
- `body` 列使用 `LONGTEXT` 类型来存储文章的主体内容

**注意**
- `存储和性能`：选择合适的文本类型对于优化存储空间和查询性能很重要。例如，对于较短的字符串， `VARCHAR` 比 `TEXT` 类型更有效率。
- `字符集`：文本数据的存储大小也取决于所使用的字符集，例如 UTF8编码的字符可能占多个字节
- `索引和搜索``：TEXT` 类型的列可以进行索引，但索引长度可能受限。在进行全文搜索时，应考虑限制

### 字符类型

字符类型主要用于存储字符串数据，主要的字符串类型包括 CHAR 和 VARCHAR 。这两种类型用于存储文本字符串，但它们在存储方式和用途上有所不同。

**CHAR**
- `用途`：用于存储固定长度的字符串。如果存储的字符串长度小于指定的长度，MySQL 会用空格填充剩余部分
- `语法`：`CHAR(M)` 其中 M 表示字符的最大数量，范围是 1 至 255
- `适用场景`：适合存储长度几乎总是相同的数据，如性别、国家代码、邮政编编

**示例**

```bash
  CREATE TABLE table(
    code CHAR(5)
  )
```

**VARCHAR**
- 用途：用于存储可变长度的字符串，只占用必要的空间加上一个额外的长度字节（字符串长度小于 256时）或两个长度字节（字符串长度 256或更大）
- 语法：`VARCHAR(M)`, 其中 M 表示字符的最大数量，范围是 1 到 65525
- 适用场景：适合存储长可变的数据，如名称、地址、描述等

**示例**

```bash
  CREATE TABLE my_table (
    name VARCHAR(100)
    email VARCHAR(255)
  )
```

**注意**
- **存储空间**： `CHAR` 类型总是使用固定的长度，无论存储的数据有多长，这可能导致空间的浪费。而 `VARCHAR` 类型只占用必要的空间。
- **性能考虑**： `CHAR` 类型在某些情况下的性能可能略优于 `VARCHAR` ，尤其是所有值都接近于最大长度时，这是因为 `CHAR` 类型的固定长度使得 MySQL 更容易计算记录位置
- **尾随空格**：在 `CHAR` 类型中，尾随空格被保留，而在 `VARCHAR` 类型中，尾随空格会被删除。
- **字符集**：字符类型的实际存储空间还是取决于所使用的字符串，如 UTF-8 字符可能占用多一个字节的空间

选择 `CHAR` 还是 `VARCHAR` 类型以决于具体的应用场景。对于经常变化长度的字段，推荐使用 `VARCHAR` ，对于长度基本固定的字段，可以考虑使用 `CHAR` 

### 集合类型

MySQL 中的集合类型 `SET`，它允许在单个列中存储多个值（从预定义的值列表中选择）。`SET` 类型类似于枚举（`ENUM`），但是一个 `SET` 列表可以包含零个、一个或者多个列表中的值。

**语法**

```bash
  SET('value1','value2','value2',......)
```
- 列定义为 `SET` 类型，并指定一个值列表
- 每个值作为一个字符串提供，多个值间用逗号分隔
- 存储在 `SET` 类型列中的值是值列表中字符串的组合
- 可以存储任意数量的值，包括 0 个

**使用集合类型**

假设我们要创建一个表来存储用户的兴趣，用户可以有多种兴趣

```bash
  CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    interests SET('sports', 'music', 'travel', 'books', 'tech')
  )
```

**插入集合类型数据**

```bash
  # 设置两个爱好
  INSERT INTO users (name, interests) VALUES ('abc', 'music, books');
  # 设置一个爱好
  INSERT INTO users (name, interests) VALUES ('cba', 'tech');
  # 没有设置爱好
  INSERT INTO users (name, intersets) VALUES ('kk', '');
```

**注意事项**
- **值的限制**： `SET` 类型最多可以包含 64 个不同的选项
- **查找和匹配**：查询 `SET` 类型时，需要考虑如何匹配其中的值，因为它们存储为逗号分隔的字符串。
- **灵活和限制**：虽然 `SET` 类型提供了在单个列中存储多个值的灵活类型，但它也限制了这些值必须是预定义的。如果有大量的组合或者组合经常变化，使用 `SET` 可能不是最佳选择。
- **内部表示**： `SET` 类型的值在内部表示为整数，每个项对应一个位，这意味着 `SET` 类型的操作可以非常快速，但对于人类来说可能不是很直观

`SET` 类型适用于在有限预定义选项集中选择多个值的场景，如标签、属性、分类等

## 数据完整性

MySQL 中的数据完整性是指确保数据的准确性、一致性和可靠性的一系列措施和规则。数据完整性是数据库管理的一个关键方面，涉及到数据库的结构、规则以及操作过程，以保障存储在数据库中的数据符合特定标准和期望。

### 完整性说明

**1 实体完整性**
- 定义：确保每个表有一个唯一的标识符，通中是主键
- 作用：主键约束保证了表中每行的唯一性，防止重复记录的产生
- 实现方式：通过定主义主键( Primary Key ) 来实现

**2 域完整性**
- 定义：确保列中的数据符合预期的类型、格式和范围
- 作用：域完整性规则确保数据的有效性和适用性
- 实现方式：通过数据类型定义、NOT NULL 约束，CHECK 约束（在某些数据库系统中）和默认值来实现

**3 引用完整性**
- 定义：确保表之间的关系正确，即外键的值必须在参照表的主键中存在
- 作用：维护表之间的一致性和逻辑关系，防止孤立的记录
- 实现方式：通过外键（Foreign Key）约束来实现

**4 用户定义的完整性**
- 定义：确保数据符合业务规则和逻辑
- 作用：保证数据对于特定业务场景的有效性
- 实现方式：通常通过应用程序逻辑、存储过程、触发器和自定义函数来实现

**5 事务完整性**
- 定义：确保数据库事务的 ACID 属性（原子性、一致性、隔离性、持久性）
- 作用：保证即使在系统故障的情况下，数据库的状态也保持一致
- 实现方式：通过数据库管理系统的事务控制机制来实现

**示例**

```bash
  CREATE TABLE students (
    id INT AUTO_INCRMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    age TINYIINT CHECK (age >= 18),
  )
```

```bash
  CREATE TABLE enrollments (
    student_id INT,
    course_id INT,
    FOREING KEY (student_id) REFERENCES students(id)
  )
```

### 实体完整性

实体完整性主要是通过主键（Primary Key）约束来实现的。主键是一种特殊类型的数据库约束，旨在保证表中每条记录的唯一性。每个表可以有一个主键，主键列中的每个值必须是唯一的，且不允许为空（NULL）

**语法**

```bash
  CREATE TABLE table_name(
    column1 datatype PRIMARY KEY,
    column2 datatype,
    ......
  )
```

或者，主键由多个列组成则

```bash
  CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    ....
    PRIMARY KEY(column1, column2, ......)
  )
```

**示例**

```bash
  # 单列主键
  CREATE TABLE students (
    student_id INT AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    age TINYINT UNSINGEN,
    PRIMARY KEY (student_id)
  )

  # 多列主键
  CREATE TABLE course_enrollments (
    student_id INT,
    course_id INT，
    enrollment_date DATE,
    PRIMARY KEY(student_id, course_id)
  )
```

**注意事项**
- 唯一性：主键列的每个值必须是唯一的，如果尝试插入重复的主键值，数据库将拒绝这个操作
- 非空性：主键列不能包含 NULL 值
- 自动增长：通常，主键列会与 `AUTO_INCREMENT` 属性一起使用，特别是在单列主键的情况下，这样每当插入新记录时， MySQL 会自动为主键列生一个唯一的值
- 主键选择：在选择主键时，应考虑业务逻辑和查询性能。理想的主键应该是稳定的（即不会随时间改变）和尽可能紧凑的。

### 域完整性

在 MySQL 中，域完整性是通过限制表中列的数据类型和可能的值来实现的，它确保数据符合特定的格式、范围或集合。

**数据类型**
- 作用：指定列可以存储的数据类型，例如 `INT` `VARCHAR` `DATE`等
- 示例：

```bash
  # 通过数据类型来约束某个字段类型的完整性
  CREATE TABLE students (
    student_id INT,
    name VARCHAR(100),
    birth_date DATE
  )
```

**NOT NULL约束**
- 作用：确保列中的数据值不能为 NULL
- 示例：

```bash
  CREATE TABLE students (
    student_id INT NOT NULL,
    name VARCHAR(100) NOT NULL
  )
```

**默认值**
- 作用：如果在插入时没有指定列的值，则会自动使用默认值
- 示例：

```bash
  CREATE TABLE students (
    student_id INT NOT NULL,
    name VARCHAR(100) NOT NULL,
    enrolled BOOLEAN DEFAULT FALSE
  )
```

**CHECK 约束**
- 版本：在 MySQL8.0.16 及之后的版本中可用
- 作用：确保列中的数据符合特定的条件
- 示例：

```bash
  CREATE TABLE students (
    student_id INT,
    age INT,
    CHECK (age >= 18)
  )
```

**注意**
- **数据类型选择**: 选择适当的数据类型对于确保数据的准确性和优化存储非常重要
- **使用 NOT NULL**:适当使用`NOT NULL`约束可以防止数据完整性问题
- **默认值**:默认值应该谨慎选择，确保它们符合业务逻辑
- **CHECK**:在早期版本中 MySQL, `CHECK` 约束被解析但不被强制执行，从对应的版后得到了完全支持

### 引用完整性

引用完整性主要通过外键（Foreign Key）约束来维护，外键是一个表中的列，它引用另一个表的主键列，外键确保参照关系的一致性和数据的完整性。防止在关联表之间出现不一致的数据。

**语法**

```bash
  FOREIGN KEY (column_name) REFERENCES parent_table(parent_column_name)
```
- `column_name` 是当前表中的列，通常是所引用的外部数据标识符
- `parent_table` 是包含被引用主键的表
- `parent_column_name` 是被引用的列，这通常是另一个表的主键

**示例**

假设我们有两个表，一个是 students 表，另一个是 enrollments 表。在 enrollments 表中，我们希望引用 students 表中的 student_id

```bash
  CREATE TABLE students (
    student_id INT NOT NULL PRIMARY KEY,
    name VARCHAR(100) NOT NULL
  )

  CREATE TABLE enrollments(
    enrollment_id INT AUTO_INCRMENT PRIMARY KEY,
    student_id INT,
    course_name VARCHAR(100) NOT NULL,
    FOREING KEY (student_id) REFERENCES students(students_id)
  )
```

**注意**
- 数据一致性：外键约束确保只能插入存在于父表中的值，维护数据的一致性
- 删除和更新规则：可以指定当父表中的数据被删除或更改时，子表中的相应行如何响应。通过 `ON DELETE` 和 `ON UPDATE` 子句实现，常见的动作有 `CASCADE` `SET NULL`, `NO ACTION` 等。
- 性能考虑：外键约束可能会影呼插入和更新操作的性能，因为每次操作都需要检查引用的完整性。
- 唯一性：引用的表必须有对应的主键或唯一性，外键列引用的列在其所在的表中必须是唯一的，通常是主键名具有唯一约束的列。

### 表约束总结

创建新表时经常使用的约束包括主键约束、外键约束、唯一约束、非空约束和默认值。这些约束用于确保数据的完整性和一致性。

**01 主键约束（PRIMARY KEY）**

```bash
  # 用于唯一标识表中的每条记录
  # student_id 被设置为主键，它将唯一标识 students 表中的每条记录
  CREATE TABLE students (
    student_id INT AUTO_INCREMENT,
    name VARCHAR(100),
    PRIMARY KEY(student_id)
  );
```

**02 外键约束（FOREIGN KEY）**

```bash
  # 用于定义两个表之间的关系，确保引用的完整性
  # enrollments 表中的 student_id 和 course_id 是外键
  CREATE TABLE enrollments (
    enrollment_id INT AUTO_INCRMENT,
    student_id INT,
    course_id INT,
    PRIMARY KEY (enrollment_id),
    FOREIGN KEY (student_id) REFERENCES students (student_id),
    FOREING KEY (course_id) REFERENCES course(course_id)
  )
```

**03 唯一约束（UNIQUE）**

```bash
  # username 和 email 列都有唯一约束，确保不会有重复的用户名和电子邮箱
  CREATE TABLE users (
    user_id INT AUTO_INCRMENT,
    username VARCHAR(50),
    email VARCHAR(100),
    PRIMARY KEY (user_id),
    UNIQUE(username),
    UNIQUE(email)
  )
```

**04 非空约束（NOT NULL）**

```bash
  # name 和 price 列都不允许空值
  CREATE TABLE products(
    product_id INT AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    PRIMARY KEY (product_id)
  )
```

**05 默认值（DEFAUTL）**

```bash
  CREATE TABLE articles(
    article_id INT AUTO_INCREMENT,
    title VARCHAR(200) NOT NULL,
    content TEXT,
    status VARCHAR(50) DEFAULT 'draft',
    PRIMARY KEY(article_id)
  )
```

这些约束在数据库设计中非常重要，因为通过他们帮助维护数据的一致性、准确性和可靠性。正确使用这些约束可以大大提高数据库的整体质量和性能。