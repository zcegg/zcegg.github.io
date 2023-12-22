## 定义变量
在 TypeScript 中，我们可以使用 let、const、var 等关键字来定义变量。变量的类型可以通过类型注解或者类型推断来指定。

**01 使用 let 和 const**

```javascript
// 使用 let 定义可变变量
let myNumber: number = 42;
let myString: string = "Hello, TypeScript";
let myBoolean: boolean = true;

// 使用 const 定义不可变变量（常量）
const PI: number = 3.14;
const MY_NAME: string = "ha ha";
```

**02 使用 var**

在现代 TypeScript 中，通常更推荐使用 let 和 const 而不是 var，因为 var 存在一些作用域上的问题。如果需要在较老的 TypeScript 或 JavaScript 代码中使用则也可以按着语法来书定

```javascript
// 使用 var（不推荐）
var x: number = 10;
var y: string = "Hello";

// 注意：在较新版本的 TypeScript 和现代 JavaScript 中，let 和 const 更安全、更可控。
```

**03 类型注解**

在上面的示例中我们采用了完整的语法来显示指定变量的数据类型，而这个显示指定的方式就是类型注解。在类型注解中，变量名后面 : 的后跟着的就是当前变量的类型。这样书写有助于提高代码的可读性，并且可以让 TypeScript 能更好的进行类型检查。

```javascript
// 使用类型注解
let myNumber: number = 42;
let myString: string = "Hello, TypeScript";
let myBoolean: boolean = true;
```

**04 类型推断**

在 TypeScript 中，也可以根据变量的初始值来推断其类型，无需显式指定类型。当 TS 能够依据上下文推断出变量类型时，通常不需要显式指定类型。但，这样做的同时也会丢失一定性的清晰度和可读性，所以手动指定类型也是一个好的实践。

```javascript
// TypeScript 可以根据初始值推断变量类型
let myNumber = 42; // 推断为 number 类型
let myString = "Hello, TypeScript"; // 推断为 string 类型
let myBoolean = true; // 推断为 boolean 类型
```

## 常见数据类型

我们常说 TypeScript 是一个 JavaScript 超集，也是因为 JS 中的数据类型在 TS 中都能找到。TS 也提供了一系列数据类型，用于定义变量、函数参数和返回值等。

TypeScript 有了这些常见数据类型后，为他们提供了强大的静态类型检查，这样在开发过程中就可以捕获潜在的错误并提高代码的可维护性。我们可以根据需要来使用这些类型，同时也可以结合它们去创造一些更复杂的类型

**01 基本数据类型**
- `number`：表示数字类型，包括整数和浮点数
- `string`：表示字符串类型
- `boolean`：表示布尔类型，值为 `true` 和 `false`
- `null` 和 `undefined` ：分别表示 `null` 和 `undefined`
- `symbol`：表示唯一不的，不可变的值

```javascript
let num: number = 42;
let str: string = "Hello";
let flag: boolean = true;
let nul: null = null;
let und: undefined = undefined;
let sym: symbol = Symbol("key");
```

**02 复合数据类型**
- `array`：表示数组类型
- `tuple`：表示元组类型，是一个固定长度和固定类型的数组
- `enum`：表示枚举类型

```javascript
let arr: number[] = [1, 2, 3];
let tuple: [string, number] = ["syy", 25];
enum Color { Red, Green, Blue };
let myColor: Color = Color.Red;
```

**03 对象类型**
- `object`：表示对象类型
- `interface`：表示接口类型，用于定义对象的结构

```javascript
let obj: object = { key: "value" };

interface Person {
  name: string;
  age: number;
}

let person: Person = { name: "John", age: 25 };
```

**04 特殊类型**
- `any`：表示任意类型，通常用于与 JavaScript 代码集成
- `void`：表示某个函数没有返回值时的类型
- `never`：表示永远不会有返回值的类型，或者抛出异常的类型

```javascript
let anyVar: any = "Hello";

function sayHello(): void {
  console.log("Hello");
}

function throwError(message: string): never {
  throw new Error(message);
}
```

**05 联合类型和交叉类型**
- `union`：表示联合类型，一个值的类型可以是多个类型中的一个
- `intersection`：交叉类型，一个值同时具有多个类型的特性

```javascript
let unionVar: number | string = 42;
let intersectionVar: { name: string } & { age: number } = { name: "John", age: 25 };
```

## Array类型