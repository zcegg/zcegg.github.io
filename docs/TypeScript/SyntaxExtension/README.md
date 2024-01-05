## 条件类型

TS 中，条件类型是一种高级类型，它允许我们依据条件表达式在类型之间进行选择。条件类型的语法类似于 JS 中的三元条件表达式 (`condition ? trueType : falseType`)，使得类型定义更加灵活和动态。

**01 基本语法**

条件类型的基本语法如下，下面代码中 `T extends U` 是一个类型检查，用于检查 `T` 是否可以赋值给 `U`。如果条件为真则类型为 `X`，否则类型为 `Y`

```typescript
// 如果 T 兼容 U 则返回 X，否则返回 Y
type ConditionalType = T extends U ? X : Y;
```

**02 基本条件类型**

```typescript
  type IsNumber<T> = T extends number ? "YES" : "NO"
  type Result1 = IsNumber<number>; // "yes"
  type Result2 = IsNumber<string>; // "no"
```

**03 条件类型与泛型结合**

```typescript

// 联合类型遇到 extends 之后会进行类型分发
type Filter<T, U> = T extends U ? T : never;

// 只保留 number 类型: number
// 这里会分别执行 string extends u   number extends u 等
type NumbersOnly = Filter<string | number | boolean, number>;
```

**04 分布式条件类型**

在泛型中使用条件类型时，如果传入的是一个联合类型，那么条件类型分被分布应用到联合类型的每个成员上。

```typescript
type NonNullable<T> = T extends null | undefined ? never : T;

// 结果类型是 string | number
type StringOrNumber = NonNullable<string | number | undefined>;

```

**05 条件类型推断 `infer`**

`infer` 关键字用于在条件类型中声明一个类型变量，可以在条件的真分支中引用。

```typescript
// (...args: any[]) => infer R 
// 上面的语法中 R 是一个类型变量， infer R 让 TS 推断 R 的类型
// 而 R 就是当前函数的返回值类型，在我们调用时传入 T 的类型之后
// R 就会推断出 string 类型
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : any;

// ExampleType 的类型是 string
type ExampleType = ReturnType<() => string>;

```

**06 条件类型约束**

条件类型可以被用于构建复杂的类型约束和关系

```typescript
type TypeName<T> =
  T extends string ? "string" :
  T extends number ? "number" :
  T extends boolean ? "boolean" :
  T extends undefined ? "undefined" :
  T extends Function ? "function" :
  "object";

type T0 = TypeName<string>;  // "string"
type T1 = TypeName<string[]>;  // "object"
```

## TS 模块化

模块化是一种代码组织方式，它允许我们将代码分割成可重用的单元。模块化在自己的作用域中运行，而不是在全局作用域中，这意味着在模块中声明的变量、函数、类等在模块外部是不可见，除非我们明确地导出它们。

**01 导出 `Export`**

可以使用 `export` 关键字导出变量、函数、类、接口等，也可以使用命名导出的方式导出多个成员

```typescript
// 导出 函数 add
export function add(x: number, y: number): number {
  return x + y;
}

// 导出函数 subtract
export function subtract(x: number, y: number): number {
  return x - y;
}
```

**02 导入 `Import`**

使用 `import` 关键字从其它模块中导入功能

```typescript
// app.ts
import { add, subtract } from "./mathUtils";

console.log(add(5, 3)); // 输出: 8
console.log(subtract(5, 3)); // 输出: 2
```

也可以使用星号 `*` 导入模块中的所有导出

```typescript
// app.ts
import * as MathUtils from "./mathUtils";

console.log(MathUtils.add(5, 3)); // 输出: 8
console.log(MathUtils.subtract(5, 3)); // 输出: 2
```

**03 默认导出 `Default Export`**

每个模块可以有一个 `default` 导出，默认导出使用 `default` 关键字标记

```typescript
// calculator.ts
// 每个模块可以有一个默认的 default 导出，可以是 类、函数等
export default class Calculator {
  add(x: number, y: number): number {
      return x + y;
  }
}
```

使用默认导出时，不需要花括号

```typescript
// app.ts
// 默认导出，直接使用名称即可
import Calculator from "./calculator";

const calc = new Calculator();
console.log(calc.add(5, 3)); // 输出: 8
```

**04 重新导出 （Re-export）**

可以使用 `export` 从其它模块重新导出功能，常用于创建单一的入口点

```typescript
// index.ts
// 从 mathUtils 中导入 add 和 subtract 然后再导出
export { add, subtract } from "./mathUtils";

// 重新做为默认导出
export { default as Calculator } from "./calculator";
```

然后可以从 `index.ts` 统一导入所有的需要的功能

```typescript
// app.ts
import { add, Calculator } from "./index";

console.log(add(5, 3)); // 输出: 8
const calc = new Calculator();
```

## TS 命名空间

命名空间（之前称为『内部模块』）是一种将相关的代码组织在一起的方式。命名空间可以包含类、接口、函数和变量，用于避免全局命名空间的污染。

但是在 TS 中更推荐使用模块，在一些特定情况下（如组织大量相关的代码，且不希望将它们放入模块），命名空间仍然很有

使用时，我们应该避免过度使用命名空间，尤其是在模块系统（如 ES6模块）可以u被使用的情况下。使用命名空间时，要避免潜在的作用域问题

**01 声明命名空间**

使用 `namespace` 关键字来声明一个命名空间，命名空间可以包含子命名空间、类、接口、函数、和变量

```typescript
namespace MathUtilities {
  export function add(x: number, y: number): number {
    return x + y;
  }

  export function subtract(x: number, y: number): number {
    return x - y;
  }
}
```

**02 使用命名空间成员**

要在同一个文件内使用命名空间中的成员，直接使用命名空间的名称加点符号（`.`）访问。

```typescript
// 命名空间.成员属性名
let result = MathUtilities.add(5, 3);
console.log(result); // 输出: 8
```

**03 命名空间嵌套**

命名空间可以嵌套在其它命名空间中

```typescript

// namespace 定义命名空间
// 自定义命名空间名称
namespace Geometry {
  // 内部可以嵌套命名空间
  export namespace Area {
    // 导出指定的成员
    export function square(sideLength: number): number {
        return sideLength * sideLength;
    }
  }
}

// 访问的时候采用 命名空间A.命名空间B.成员名称
let area = Geometry.Area.square(5);
console.log(area); // 输出: 25
```

**04 分割命名空间**

大型命名空间可以分割成多个文件，使用 `/// <reference path="...." />` 指令来引用其它文件中的命名空间

```typescript
// MathUtilities.ts
namespace MathUtilities {
  export function add(x: number, y: number): number {
    return x + y;
  }
}

// MathUtilities-Subtract.ts
// 使用下在的语法可以引入 MathUtilities.ts 中的命名空间
/// <reference path="MathUtilities.ts" />
namespace MathUtilities {
  export function subtract(x: number, y: number): number {
    return x - y;
  }
}
```

## 声明文件

声明文件（通常以 `.d.ts`）结尾，用于提供 JS 库的类型信息，这使得 TS 能够理解这些代码的结构，从而提供类型检查和智能提示。

**01 基本类型声明**

声明文件通常包含 `declare` 关键字的使用，它用于声明 变量、函数、类或者任何其它类型的结构，而不实际定义它们的实现。

```typescript
// example.d.ts
declare function exampleFunction(a: number, b: number): number;
declare class ExampleClass {
    constructor(message: string);
    showMessage(): void;
}
```

**02 声明全局变量**

使用 `declare var` 来描述一个全局变量的类型

```typescript
// globals.d.ts
declare var myGlobalVar: number;

// 使用：此时在任意的文件中都能使用该变量
console.log(myGlobalVar); // 可以使用 myGlobalVar
```

**03 声明类**

声明文件也可以声明类及其属性和方法

```typescript
// MyClass.d.ts
declare class MyClass {
  constructor(message: string);
  greet(): string;
}
// 使用
let myClassInstance = new MyClass("Hello, world!");
console.log(myClassInstance.greet()); // 正确
```

**04 声明接口**

接口在声明文件中非常有用，尤其是描述对象字面量的结构

```typescript
// interface.d.ts
declare interface MyInterface {
    myProperty: string;
    myMethod(arg: number): void;
}

// 使用
let obj: MyInterface = {
  myProperty: "Hello",
  myMethod(arg: number) {
      // ...
  }
};
```

**05 声明模块**

当我们需要提供对外部模块（如 npm包）的类型声明时，可以使用 `declare module`。

```typescript
// node_modules/someLibrary/index.d.ts
declare module "someLibrary" {
  export function someFunction(a: number): number;
}

// 使用
import { someFunction } from "someLibrary";
someFunction(5);
```

例如 JQuery 声明文件，假设我们正在使用 JQuery， TS 需要知道 Jquery 的 `$` 变量的类型。

```typescript
// jquery.d.ts
// 声明一个全局的变量  $，类型为 JQueryStatic
declare var $: JQueryStatic;

// 使用
$("#myId").fadeIn();
```

例如 node.js 的内置模块，如 `fs` ，TS 有官方的类型声明文件

```typescript
// 使用
import * as fs from "fs";

// 此后我们就可以全局使用 fs，及它的结构提示。
fs.readFileSync("path/to/file", "utf8");
```

TS 提供了许多内置的类型声明，如 `Promise` `Array` `NodeList` 等

```typescript
let promise: Promise<string>;
promise = new Promise((resolve, reject) => {
  resolve("Hello, TypeScript!");
});

let numbers: Array<number> = [1, 2, 3];
let nodeList: NodeList = document.querySelectorAll("div");
```

**06 注意**

1. 声明语文件不会被编译成 JS ，它们只用于类型检查
2. 当使用第三方JS库的时候，我们通常需要安装相应的类型声明文件（`@types/node`）
3. 我们应该只在不有现成的类型声明文件时编写自定义声明文件

## 自定义声明文件

自定义声明文件允许我们为已有的 JS 代码提供类型定义。这样， TS 就可以理解这些 JS 代码的结构。这在我们使用没有提供自己的类型定义的第三方库时特别有用。

**01 声明文件结构**

自定义声明文件通常具有 `.d.ts` 扩展名，在这个文件中，我们可以使用 `declare` 关键字来告诉 TS 一些全局变量的类型

```typescript
// someLibrary.d.ts
// 声明一个全局的函数
declare function myFunction(a: number, b: number): number;
// 声明一个全局的变量
declare const myVariable: number;
```

**02 为函数提供类型定义**

如果我们的 JS 库有一个全局函数，我们可以这样声明它

```typescript
// calculator.d.ts
declare function add(a: number, b: number): number;

// 使用
const sum = add(1, 2); // 正确
```

**03 为类提供类型定义**

对于库中的类，我们可以声明它的构造函数和方法

```typescript
// greeter.d.ts
declare class Greeter {
  constructor(greeting: string);
  greet(): string;
}

// 使用时
let greeter = new Greeter("Hello");
console.log(greeter.greet()); // 输出: "Hello"
```

**04 为模块提供类型定义**

如果一个库是一个模块，我们可以这样声明它的导出

```typescript
// node_modules/my-lib/index.d.ts
declare module "my-lib" {
  export function myLibFunction(a: number): number;
}

// 使用时
import { myLibFunction } from "my-lib";
myLibFunction(10);

```

**05 声明命名空间**

有时，库使用了全局命名空间，我们可以这样声明它

```typescript
// utils.d.ts
declare namespace Utils {
  function calculateDistance(x: number, y: number): number;
}

// 使用时
const distance = Utils.calculateDistance(10, 5);
```

**06 扩展全局对象**

有时候我们需要扩展全局对象，比如 `window`

```typescript
// global.d.ts
declare global {
  interface Window {
    myGlobalFunction(): void;
  }
}

// 使用时
// 只是在语法提示上有，不是说原型上就真的有
window.myGlobalFunction()
```

**07 注意事项**

1. 声明文件中不包含实际的代码逻辑，只用于类型声明
2. 为确保类型安全，应该尽可能准确地反映 JS 代码的实际结构
3. 在 TS 项目中，确保声明文件被正确的引入，以便 TS 能够找到这些类型声明

## 配置文件解析

在 TS 项目中， `tsconfig.json` 文件是一个重要的配置文件，用于指定 TypeScript 编译器的编译选项和项目设置。

**01 `compilerOptions`**

该配置项的值是一个对象，包含用于控制编译器行为的各种选项

- target：指定 ECMAScript 目标版本（如：es5 es6 es2017等）
- module：指定模块代码生成方式（如 commonjs amd es2015等）
- strict：启用所有严格类型检查选项
- outDir：指定输出目录，编译后的文件将放置在这里
- sourceMap：是否生成源代码映射文件（.map文件），用于调试

```typescript
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "strict": true,
    "outDir": "./dist",
    "sourceMap": true
  }
}
```

**02 `include` 和 `exclude`**

这两个选项用于指定编译器应包含或排除哪些文件

1. include：指定一个文件模式列表，编译器将包含这些模式匹配的文件
2. exclude：指定一个文件模式列表，编译器将排除这些械式匹配的文件

```typescript
{
  "include": [
    "src/**/*"
  ],
  "exclude": [
    "node_modules",
    "**/*.spec.ts"
  ]
}
```

**03 `files`**

如果我们只想编译项目中的特定文件，可以使用 `files` 属性明确指定这些文件

```typescript
{
  "files": [
    "core.ts",
    "main.ts"
  ]
}
```

**04 `extends` **

该属性用于指定另一个要继承的 `tsconfig.json` 文件的路径

```typescript
{
  "extends": "./base-config.json"
}
```

**05 `lib`**

使用 `lib` 指定编译过程中将包含在编译环境中的库文件列表，例如，如果我们的代码运行在浏览器中，我们可能需要 DOM 类型的定义。(运行时需要依赖的库文件的类型定义文件列表)

```typescript
{
  "compilerOptions": {
      "lib": ["dom", "es6"]
  }
}
```

**06 `noEmit`**

这个选项用于在不生成输了文件的情况下运行编译器，通常用于仅检查类型错误

```typescript
{
  "compilerOptions": {
    "noEmit": true
  }
}
```

**07 `jsx`**

如果我们在项目中使用 JSX(如 React) ，需要设置 `jsx` 选项来指定 JSX 代码的编译方式

```typescript
{
  "compilerOptions": {
    "jsx": "react"
  }
}
```

**08 `noImplicitAny`**

设置为 `true` 时，任何隐式 `any` 类型都会导致编译错误，这有助于确保所有变量的类型都是显式声明，提高代码的类型安全。

```typescript
{
    "compilerOptions": {
        "noImplicitAny": true
    }
}
```

**09 `strictNullChecks`**

当设置为 `true` 时，将对 `null` 和 `undefined` 进行严格检查，从而避免许多常见的运行时错误

```typescript
{
  "compilerOptions": {
    "strictNullChecks": true
  }
}
```

**09 `esModuleInterop`**

启用该选项后，可以允许默认导入与非 ECMAScript 模块之间的互操作性，在这处理某些第三方库时非常有用。

```typescript
{
  "compilerOptions": {
    "esModuleInterop": true
  }
}
```

**10 `skipLibCheck`**

设置为 `true` 时，编译器将跳过所有声明文件（.d.ts） 的类型检查，某种程度上可以加快编译速度

**11 `allowJS`**

允许编译器编译 JS 文件，在将 JS 项目迁移至 TS 时能用到

**12 `noUnusedLocals` 和`noUnusedParameters`**

这两个选项用于检查未使用的局部变量和函数参数，有助于保持代码的整洁和可维护性。

**13 `baseUrl` 和 `paths`**

用于配置模块解析方式，特别是在有复杂的项目结构时

```typescript
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
        "*": ["node_modules/*", "src/types/*"]
    }
  }
}
```

## 装饰器