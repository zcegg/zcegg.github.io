## 函数类型声明

声明函数类型涉及为函数的参数和返回值指定类型注解，这种声明方式不仅增强了代码的可读性和维护性，还提供了类型检查。

**01 基本函数类型声明**

```javascript
// 下面的语法就表示 add 有两个参数，类型均为 number，返回值也是 number 类型
function add(x: number, y: number): number {
    return x + y;
}
```

**02 函数表达式类型声明**

函数表达式的类型注解类似于普通的函数声明，我们可以注解变量的类型为一个函数类型，然后将一个符合该类型的函数赋值给该变量

```javascript
// 与上面不同的在于，我们使用 const 声明了一个变量 add 
// 然后注解 add 的类型是(x:number, y:number) => number
// 之后再将一个函数赋值给 add
const add: (x: number, y: number) => number = function(x, y) {
    return x + y;
};
```

**03 箭头函数的类型声明**

```javascript
// 箭头函数的声明和函数表达式类似
const add: (x: number, y: number) => number = (x, y) => x + y;
```

## 函数的参数

了解函数参数的方方面面是非常必要的，因为它为函数提供了必要的类型安全性。

**01 基本参数类型**

在 TS 中，我们可以为函数的每个参数指定类型

```javascript
// 当前函数有两个参数
// 分别为 name 与 age 指定明确的类型
function greet(name: string, age: number): string {
  return `Hello, my name is ${name} and I am ${age} years old.`;
}
```

**02 可选参数**

通过在参数名后面添加 `?` ，可以指定参数为可选，这意味着调用函数时可以不传递这个参数

```javascript
// age 为可选参数，类型为 number 类型
// 将来在调用 greet 的时候， age 就是可选参数
// 可选参数要放在必传参数的后面
function greet(name: string, age?: number): string {
  if (age !== undefined) {
      return `Hello, my name is ${name} and I am ${age} years old.`;
  }
  return `Hello, my name is ${name}.`;
}
```

**03 默认参数**

TS 支持带有默认值的参数，如果调用函数时没有提供该参数，将使用默认值

```javascript
// greeting 类型为 string，同时设置默认值为 hello
// 调用 greet 函数时，如果没有传入具体的值，则会使用该默认值
function greet(name: string, greeting: string = "Hello"): string {
  return `${greeting}, my name is ${name}.`;
}
```

**04 剩余参数**

使用 `...` 符号定义剩余参数，这允许我们将不定数量的参数作为一个数组传递

```javascript
// buildName 有一个 firstName 是必填写的参数
// 余下的参数都是非必填的，我们放在一个 string[] 中
function buildName(firstName: string, ...restOfName: string[]): string {
    return firstName + " " + restOfName.join(" ");
}
```

**05 函数重载中的参数**

关于函数重载，我们单独说明，这里只是说一下它的参数。在函数重载中，我们可以为同一函数的不同版本指定不同类型的参数类型

```javascript
// 定义函数重载一
function makeDate(timestamp: number): Date;
// 定义函数重载二
function makeDate(m: number, d: number, y: number): Date;

// 重载的具体实现
function makeDate(mOrTimestamp: number, d?: number, y?: number): Date {
  // 类型守卫，将 d 与 y 变成更具体的类型
  if (d !== undefined && y !== undefined) {
      return new Date(y, mOrTimestamp, d);
  } else {
      return new Date(mOrTimestamp);
  }
}
```

**06 接口定义复杂参数类型**

对于复杂的参数类型，我们可以使用接口来定义

```javascript
interface Point {
  x: number;
  y: number;
}

// 其实就是一个正常的参数类型注解，只不这个类型我们可以是接口或类型别名
function drawPoint(point: Point) {
  // ...
}
```

**07 类型别名定义参数类型**

与接口类似，类型别名也可以用来定义参数的类型，特别是在需要多次重复使用相同类型。

```javascript
// 定义一个类型别名
type Point = { x: number, y: number };

// 普通的类型注解，使用类型别名来约束 point 
function drawPoint(point: Point) {
  // ...
}
```

**08 箭头函数中的参数类型**

箭头函数的参数类型注解方式与普通函数相同，因为都是函数参数

```javascript
const add: (x: number, y: number) => number = (x, y) => x + y;
```

## 函数调用签名

在 TypeScript 中，函数调用签名是用于描述函数类型的一种方式，它指定了函数的参数类型和返回值类型，函数调用签名非常适合用于定义回调用函数的类型、定义接口中的函数类型，或者在类型别名中指定函数类型。

**01 函数调用签名的基本结构**
