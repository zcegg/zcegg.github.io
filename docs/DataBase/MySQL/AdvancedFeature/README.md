## 聚合函数

聚合函数用于对一组值执行计算，并返回单个值。常见的聚合函数包括 `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()` 等。这些函数通常在使用 `GROUP BY` 子句进行分组的查询中使用。

### `COUNT`

> 用于计算行数

```bash
SELECT COUNT(column_name) FROM table_name
```

**示例**

```bash
# 计算students表中的学生数量
SELECT COUNT(*) FROM students;
```

### `SUM`
> 用于计算数值列的总和

```bash
SELECT SUM(column_name) FROM table_name;
```

**示例**

```bash
# 计算 orders 表中所有订单的总金额
SELECT SUM(amount) FROM orders;
```

### `AVG`

```bash
SELECT AVG(column_name) FROM table_name;
```

**示例**
```bash
# 计算students表中学生的平均年龄
SELECT AVG(age) FROM students;
```

### `MIN` 和 `MAX`
> `MIN` 用于找出列中的最小值，`MAX` 用于找出列中的最大值

**语法**
```bash
SELECT MIN(column_name), MAX(column_name) FROM table_name;
```

**示例**

```bash
# 找出 students 表中学生年龄的最小值和最大值
SELECT MIN(age), MAX(age) FROM students;
```

### `GROUP_CONCAT`
> 该函数用于将来自多个行的列值连接成一个字符串，在使用 GROUP BY 子句进行分组时，这个函数特别有用

**语法**
```bash
SELECT GROUP_CONCAT(column_name SEPARTOR 'separator')
FROM table_name
GROUP BY another_column
```
- column_name：要连接的列
- SEPARATOR 'SEPARATOR'：指定连接时使用分隔符，默认为逗号

**示例**

假设 `students` 表中有学生的ID和所选的课程，可以使用 `GROUP_CONCAT()` 来列出每个学生选择的所有课程。

```bash
CREATE TABLE student_courses(
  student_id INT,
  course_name VARCHAR(100)
)

INSERT INTO student_courses (student_id, course_name) VALUES 
(1, 'Math'),
(1, 'Science'),
(2, 'Math'),
(2, 'History')

# 输出每个学生所选的课程列表，课程之间用逗号和空格分隔
SELECT student_id, GROUP_CONCAT(course_name SEPARATOR ', ')
FROM student_course
GROUP BY student_id
```

**注意**
- NULL 值的处理：聚合函数在计算时会忽略 NULL 值。
- 与 GROUP BY 的结合使用：当与 `GROUP BY` 子句结合使用时，聚合函数会对每个分组应用。

## 外键约束

外键 （Foreign Key）约束用于在两个表之间建立关联，以保持引用的完整性和数据的一致性。外键约束确保一个表中的列值必须出现在另一个表的列中。

**基本语法**

```bash
CREATE TABLE child_table(
  column1 datatype,
  column2 datatype,
  ...
  FOREIGN KEY(column1) REFERENCES parents_table(parent_column)
);
```
- `child_table` 是包含外键的表
- `coulmn1` 是 `child_table` 中作为外键的列
- `parent_table` 是被引用的表
- `parent_column` 是 `parent_table` 中被引用的列，通常是主键

**示例**

假设有两个表：一个是 `students` 表，另一个 `enrollments` 表。在 `enrollents` 表中，我们希望引用 `students` 表中的 `student_id`

在下面的代码中，enrollments表中的student_id列是一个外键，它引用了students表中的student_id列

```bash
CREATE TABLE students(
  student_id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL
);

CREATE TABLE enrollments(
  enrollment_id INT AUTO_INCREMENT PRIMARY KEY,
  student_id INT,
  course_name VARHCAR(100) NOT NULL,
  FOREIGN KEY (student_id) REFERENCES students (stuent_id)
)
```

**注意**
- 引用的完整性：外键约束要求在 `child_table` 中的每个 `column1` 值都必须在 `parent_table` 中存在。
- 删除和更新的级联：可以定义当父表（`parent_table`） 中的记录被删除或更新时，子表（`child_table`）中的相关记录如何响应，通过 `ON DELETE` 和 `ON UPDATE` 子句实现。
- 性能考虑：外键约束可能影响插入和更新操作的性能，因为每次操作都需要检查引用的完整性
- 外键和索引：在作为外键的列上通常会自动创建索引

## 多表查询

多表查询，又被称为连接（Join）查询，是数据库操作中的一种常用技术，它允许我们在一个查询中结合多个表的数据。在关系型数据库系统中，多表查询是必不可少的。因为可以实现从一些相关的表中提取和合并信息。

### 多表查询的类型
- **内连接（`INNER JOIN`）**：只返回两个表中相互匹配的行，如果一行在一个表中有匹配项，但是在另一个表中没有，则这行不会出现在查询结果中。
- **左连接（`LEFT JOIN`）**：返回左表中的所有行，即使在右表中没有匹配项。如果右表中没有匹配项，则结果中对应的字段会显示 NULL
- **右连接（`RIGHT JOIN`）**：与左连接相板，返回右表中的所有行，即使在左表中不有匹配项。
- **全外连接（`FULL OUTER JOIN`）**：结合左连接全右连接的效果，返回左表和右表中的所有行。如果一侧没有匹配项，则结果中对应的字段会是 NULL

### 多表查询基础

**语法**

```bash
SELECT column_name(s)
FROM table1
JOIN table2
ON table1.column_name = table2.column_name;
```

**示例**

使用内连接查询，将 students 和 enrollments 结合查询
```bash
#查询所有学生及其选课信息，只包括那些在两个表中都有记录的学生
SELECT students.name, enrollments.course_name
FROM students
JOIN enrollments
ON students.student_id = enrollment.student_id
```

**示例**

使用左边接查询，查询出所有学生的姓名，即使他们没有选课

```bash
#查询所有学生，对于那些没有选课的学生，course_name将为NULL
SELECT students.name, enrollments.course_name
FROM students
LEFT JOIN enrollments
ON students.student_id = enrollments.student_id
```

## 左连接

左连接查询（LEFT JOIN）在 SQL 中用于从两个表中获取数据，即使其中一个表里没有匹配的行，在左连接查询中，左表（位于`LEFT JOIN`关键字之前的表）的所有行都会被包含在结果集中，如果右表中没有匹配行。则相应的结果集中的列会包含 NULL 值。

### 语法

```bash
SELECT column_name(s)
FORM table1
LEFT JOIN table2
ON table1.comlumn_name = table2.column_name
```
- `table1`：左表，其数据行将全部出现在查询结果中
- `table2`：右表，仅当前某行与左表中的行匹配时，该行者会出现在结果中
- `ON table1.column1_name = table2.column_nam`e：连接条件，指定了如何匹配两个表中的行。

### 示例

假设有两个表，一个是 `employees` 表，包含员工信息；另一个是 `departments` 表，包含部门信息。每个员工被分配到一个部门，但有些员工可能没有分配部门。

```bash
CREATE TABLE employees(
  employee_id INT,
  employee_name VARCH(50),
  department_id INT
)

CREATE TALE departments(
  department_id INT,
  department_name VARCHAR(50)
)

# 插入数据
INSERT INTO employees(employee_id, employee_name, department_id) VALUES 
(1, 'Alice', 1),
(2, 'Bob',NULL),
(3, 'Charlie', 2);

INSERT INTO departments(department_id, department_name) VALUES(1, 'HR'), (2, 'Tech'), (3, 'Finance')

# 左连接查询
SELECT employees.employee_name, departments.department_name
FROM employees
LEFET JOIN departments
ON employees.department_id = departments.department_id
```

### 注意
- 左连接可能会返回重复的行，特别是如果右表中有多行与左表中的单行匹配时
- 使用左连接时，应当明确知道为什么选择它而不是内连接，如果不需要包括那些在右表中没有匹配的左表行，通常使用内连接
- 如果只要取出左表中有，但是右表中没有的数据，则使用 WHERE 子句进行筛选即可。

## 右连接

在 MySQL 中，右连接查询（Right JOIN）类似于左连接查询，但是它从右表（位于 `RIGHT JOIN`关键字之后的表）开始匹配左表，如果左表中没有匹配项，则相应的结果集中的列会包含 NULL 值。右连接确保右表中的所有行都会出现在结果集中，不管它们是否在左表中有匹配行。

### 语法

```bash
SELECT column_name(s)
FROM table1
RIGHT JOIN table2
ON table1.column_name = table2.column_name
```
- `table1` 和 `table2` ：参与连接的两个表
- `ON table1.column_name = table2.column_name`：定义如何匹配两个表中的行

### 示例

假设我们有两个表 `employees` 和 `departments`。每个员工属于一个部门，但是某些部分可能暂时没有员工。

查询出所有员工及其所属部门的名称。如果有员工没有分配部门，他们的部门名称将为NULL

```bash
CREATE TABLE employees(
  employee_id INT,
  employee_name VARCAHR(50),
  department_id INT
)

CREATE TABLE departments(
  department_id INT,
  department_name VARCHAR(50)
)

# 插入数据
INSERT INTO employees (employee_id, employee_name, department_id) VALUES
(1, 'Alice', 1),
(2, 'Bob', 2)

INSERT INTO departments (department_id, department_name) VALUES(1, 'HR'),(2, 'Tech'),(3, 'Finance')

# 执行右连接查询
SELECT departments.department_id, employees.employees_name
FROM departments
RIGHT JOIN employees
ON departments.department_d = employees.department_id

```

### 注意
- 数据覆盖：右连接确保右表（`employees`）中的所有行都会出现在查询结果中，而左表（`departments`）中未匹配的行将以 NULL 填充。
- 使用场景：右连接在实际开中并不多见，因为我们完全可以通过调整查询的表顺序和使用连接方式将它用左连接实现

## 内连接

内连接查询（Inner join）是 SQL 中用于结合两个表的一种常用方法。它返回两个表中的满足连接条件的行。如果一个表中的行在另一个表中没有匹配，则这些行不会现现在查询结果中。

### 语法

```bash
SELECT column_name(s)
FROM table1
INNER JOIN table2
ON table1.column_name = table2.column_name
```
- `table1` 和 `table2` ：参与连接的两个表
- `ON table1.column_name = tabl2.column_name`：指定连接的条件，通常是两个表中的共同字段

### 示例

假设我们有两个表 `employess` 和 `departments` 。每个员工属于一个部门，我们希望查询员工及其所属部门的信息

```bash
CREATE TABLE employees (
  employee_id INT,
  employee_name VARCHAR(50),
  department_id INT
);

CREATE TABLE departments (
  department_id INT,
  department_name VARCHAR(50)
);

# 插入示例数据
INSERT INTO employees (employee_id, employee_name, department_id) VALUES
(1, 'Alice', 1),
(2, 'Bob', 2);

INSERT INTO departments (department_id, department_name) VALUES
(1, 'HR'),
(2, 'Tech');

# 查询
SELECT employees.employee_name, departments.dempartment_name
FROM employees
INNER JOIN departments
ON employees.department_id = departments.department_id
```

### 注意
- 匹配行：内连接瓜返回两个表中都有匹配的行。如果左表或右表中的行在另一个表中不有匹配项，则这些行不会出现在结果集中。
- 性能优化：合理使用索引可以优化内连接查询的性能
- 多表连接：可以通过多次使用 `INNER JOIN` 来连接多个表

## 全外连接

在 MySQL 中，全外连接（Full Outer Join）并不直接支持，但可以通过组合左连接和右连接的结果来模拟，全外连接的目的是从两个表中获取所有的行，如果某一侧的行在另一侧没匹配，则相关的列会被设置为 NULL

### 实现
1. 首先进行左连接，获取左表所有行
2. 然后进行右连接，获取右表所有行
3. 使用 `UNIO` 或 `UNIO ALL` 来合并这两个查询的结果

### 示例

假设我们有两个表：`employees` 和 `departments` 。我们想要获取所有员工和所有部门的信息，即使某些员工没有部门，或某些部门没有员工。

```bash
CREATE TABLE employees (
    employee_id INT,
    employee_name VARCHAR(50),
    department_id INT
);

CREATE TABLE departments (
    department_id INT,
    department_name VARCHAR(50)
);

-- 插入示例数据
INSERT INTO employees (employee_id, employee_name, department_id) VALUES
(1, 'Alice', 1),
(2, 'Bob', 2);

INSERT INTO departments (department_id, department_name) VALUES
(1, 'HR'),
(2, 'Tech'),
(3, 'Finance');

# 模拟全外连接查询
SELECT e.employee_name, d.deparment_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.department_id
UNIO ALL 
SELECT e.employees_name, d.department_name
FROM employees e
RIRHGT JOIN departments d ON e.department_id = d.department_id
WHERE e.employee_id IS NULL
```

### 注意
- 结果去重：在使用 `UNION`时，默认会去除重复行，如果想包含所有重复行，可以使用 `UNION ALL`
- 性能考虑：这种方法实际上涉及两次查询，对于大型数据集可能效率低
- WHERE 子句：在右连接查询中使用 `WHERE e.employee_id IS NULL` 来确保不重复左连接查询中已获取的行。

## 多表操作

### 表与表关系

表与表之间的关系是通过关系型数据的核心概念来定义的。这些关系帮助在不同的表之间建立联系，并保持数据的组织和完整性。

**1 一对一关系**

在一对一关系中，一个表中的每个行记录只与另一个表中的一行记录相关联。这种关系通常通过在一个表中设置外链来引用另一个表的唯一（通常是主键）字段来实现

**示例**

假设我们有两个表 `users` 和 `user_profiles`。每个用户在 `users` 表中都有一个唯一记录，且在 `user_profiles` 表中有一个详细的记录。

```bash
CREATE TABLE users(
  user_id INT PRIMARY KEY,
  user_name VARCHAR(50) NOT ULL
)

CREATE TABLE user_profiles(
  user_id INT PRIMARY KEY,
  user_profile TEXT,
  FOREIGN KEY(user_id) REFERENCES users(user_id)
)
```

**2 一对多关系**

一对多关系是最常见的关系类型。在这种关系中，一个表里的一行记录可以与另一个表中的多行记录相关联

**示例**

假如我们有两个表 `authors` 和 `books`。 一个作者可以写多本书，但每本书只能有一个作者。

```bash
CREATE TABLE authors(
  author_id INT PRIMARY KEY,
  author_name VARCHAR(100)
)

CREATE TABLE books(
  book_id INT PRIMARY KEY,
  book_title VARCHAR(100),
  author_id INT,
  FOREIGN KEY(author_id) REFERENCES authors(author_id)
)
```

**3 多对多关系**

在多对多关系中，一个表中的一行记录可以与另一个表中的多行记录相关联，反之也是，这种关系常用通过第三个表（称之为关联表或连接表）来实现，其中包含两个表的外键。

**示例**

假如我们有两个表 `students` 和 `courses` 。一个学生可以注册多个课程，一个课程也可以由多个学生注册。

```bash
# 学生表
CREATE TABLE students (
  student_id INT PRIMARY KEY,
  student_name VARCHAR(100)
);

# 课程表
CREATE TABLE courses (
  course_id INT PRIMARY KEY,
  course_name VARCHAR(100)
);

# 关系表
CREATE TABLE student_courses(
  student_id INT,
  course_id INT,
  FOREIGN KEY(student_id) REFERENCES students(student_id),
  FOREIGN KEY(course_id) REFERENCES courses(course_id)
  PRIMARY KEY(sutudent_id, course_id)
)

```

### 多对多建表

在数据库中建立多对多关系通常涉及三个表：两个主表和一个中间表，后者用于存储两个主表之间的关联信息。在多对多关系中，每个主表中的一行可以与另一个主表中的多行相关联。中间表至少包含两个字段，每个字段是两个主表的外键。

**多对多关系的建表**

假设我们有两个实例：`students` 和 `courses`。一个学生可以注册多门课程，同时一门课程可以由多个学生注册。因此， `students` 和 `courses` 之间是多对多关系。

**第一步：创建主表**

首先创建两个主表 `students` 和 `courses`

```bash
CREATE TABLE students (
  student_id INT PRIMARY KEY,
  student_name VARCHAR(100)
)

CREATE TABLE courses (
  course_id INT PRIMARY KEY,
  course_name VARCHAR(100)
)
```

**第二步：创建中间表**

然后创建一个中间表 `student_courses` ，包含两个外键字段 `student_id`和 `course_id`。这两个字段共同作为中间表的复合主键。

```bash
CREATE TABLE student_cousrses(
  student_id INT,
  course_id INT,
  FOREIGN KEY(student_id) REFERENCES students(student_id),
  FOREIGN KEY(course_id) REFERENCES courses(course_id),
  PRIMARY KEY(student_id, course_id)
)
```

**说明**

- `students` 表和 `courses` 表是主表，分别存储学生和课程的信息
- `student_courses` 是中间表，用于存储学生和课程之间的关系
- 在 `student_courses` 表中， `student_id` 和 `course_id` 字段分别引用 `students` 表和 `courses` 表的主键
- 通过将 `student_id` 和 `course_id` 组合作为主键，`students_courses` 表确保了每个学生与课程之间的唯一关联

## 多表查询

在多对多关系的情况下，进行表查询通常涉及联接（Join）操作，以便从相关联的表中检索数据。考虑到之前提到的 `students` 、`courses`、`students_courses` 的例子，我们可以通过连接这些表来查询相关数据。

### 语法

```bash
SELECT column_name(s)
FROM table1
JOIN table2
ON table1.common_column = table2.common_colum
JOIN table3
ON table2.anthor_common_colum = table3.anthor_common_column
```

- 这里 `table1` `table2` 和 `table3` 是需要联接的表
- `common_column` 和 `anthor_common_column` 用于联接表的共同字段

### 示例

假设我们想要查询哪些学生注册了特定的课程，或者一个学生注册了哪些课程。

```bash
# 查询显示了名为'Alice'的学生注册的所有课程
SELECT students.student_name, courses.course_name
FROM students
JOIN student_courses ON students.student_id = student_courses.student_id
JOIN courses ON student_courses.course_id = courses.course_id
WHERE students.student_name = 'Alice'
```

```bash
# 查询注册了'Math'课程的所有学生。
SELECT student.student_name, courses.course_name
FROM courses
JOIN student_courses ON courses.course_id = student_courses.course_id
JOIN students ON students.student_id = student_courses.student_id
WHERE courses.course_name= 'Math'
```

### 语法细节
- **JOIN** ：我们使用了 `INNER JOIN（通常简称为 JOIN）` 来连接表。这种类型的连接只返回在两个表中都有匹配的行。
- **ON**：`on` 子句用于指定连接的条件
- **WHERE**：`WHERE` 子句用于进一步过滤结果，比如指定特定学生或课程。

### 注意事项
- 保证连接条件的正确性是非常重要的，错误的连接条件可能导致查询结果不正确
- 在执行复杂的连接查询时，尤其是涉及大型数据集时，查询性能可能成为一个考虑因素。适当的索引可以帮助提高查询效率。