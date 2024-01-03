## 条件类型

TS 中，条件类型是一种高级类型，它允许我们依据条件表达式在类型之间进行选择。条件类型的语法类似于 JS 中的三元条件表达式 (`condition ? trueType : falseType`)，使得类型定义更加灵活和动态。

**01 基本语法**

条件类型的基本语法如下，下面代码中 `T extends U` 是一个类型检查，用于检查 `T` 是否可以赋值给 `U`。如果条件为真则类型为 `X`，否则类型为 `Y`

```javascript
// 如果 T 兼容 U 则返回 X，否则返回 Y
type ConditionalType = T extends U ? X : Y;
```

**02 基本条件类型**

```javascript
  type IsNumber<T> = T extends number ? "YES" : "NO"
  type Result1 = IsNumber<number>; // "yes"
  type Result2 = IsNumber<string>; // "no"
```

**03 条件类型与泛型结合**

```javascript

// 联合类型遇到 extends 之后会进行类型分发
type Filter<T, U> = T extends U ? T : never;

// 只保留 number 类型: number
// 这里会分别执行 string extends u   number extends u 等
type NumbersOnly = Filter<string | number | boolean, number>;
```

**04 分布式条件类型**

在泛型中使用条件类型时，如果传入的是一个联合类型，那么条件类型分被分布应用到联合类型的每个成员上。

```javascript
type NonNullable<T> = T extends null | undefined ? never : T;

// 结果类型是 string | number
type StringOrNumber = NonNullable<string | number | undefined>;

```

**05 条件类型推断 `infer`**

`infer` 关键字用于在条件类型中声明一个类型变量，可以在条件的真分支中引用。

```javascript
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

```javascript
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

```javascript
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

```javascript
// app.ts
import { add, subtract } from "./mathUtils";

console.log(add(5, 3)); // 输出: 8
console.log(subtract(5, 3)); // 输出: 2
```

也可以使用星号 `*` 导入模块中的所有导出

```javascript
// app.ts
import * as MathUtils from "./mathUtils";

console.log(MathUtils.add(5, 3)); // 输出: 8
console.log(MathUtils.subtract(5, 3)); // 输出: 2
```

**03 默认导出 `Default Export`**

每个模块可以有一个 `default` 导出，默认导出使用 `default` 关键字标记

```javascript
// calculator.ts
// 每个模块可以有一个默认的 default 导出，可以是 类、函数等
export default class Calculator {
  add(x: number, y: number): number {
      return x + y;
  }
}
```

使用默认导出时，不需要花括号

```javascript
// app.ts
// 默认导出，直接使用名称即可
import Calculator from "./calculator";

const calc = new Calculator();
console.log(calc.add(5, 3)); // 输出: 8
```

**04 重新导出 （Re-export）**

可以使用 `export` 从其它模块重新导出功能，常用于创建单一的入口点

```javascript
// index.ts
// 从 mathUtils 中导入 add 和 subtract 然后再导出
export { add, subtract } from "./mathUtils";

// 重新做为默认导出
export { default as Calculator } from "./calculator";
```

然后可以从 `index.ts` 统一导入所有的需要的功能

```javascript
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

```javascript
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

```javascript
// 命名空间.成员属性名
let result = MathUtilities.add(5, 3);
console.log(result); // 输出: 8
```

**03 命名空间嵌套**

命名空间可以嵌套在其它命名空间中

```javascript

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

```javascript
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

## 内置声明文件

声明文件（通常以 `.d.ts`）结尾，用于提供 JS 库的类型信息，这样，TS 代码就可以知道如何正确地调用库中的函数和方法，即使实际的函数和方法是用 JS 编写的。

**01 基本类型声明**

我们可以为 JS 中存在的变量、函数、类等声明类型，使用对应的关键字即可。下面的代码中就使用 `declare` 来声明了一个函数类型

```javascript
// someLibrary.d.ts
declare function myFunction(a: number, b: number): number;

// 使用
// myFunction(1, 2); // 正确
// myFunction("1", "2"); // 错误：不能将类型“string”分配给类型“number”
```

**02 声明变量**

使用 `declare var` 来描述一个全局变量的类型

```javascript
// globals.d.ts
declare var myGlobalVar: number;

// 使用：此时在任意的文件中都能使用该变量
console.log(myGlobalVar); // 可以使用 myGlobalVar
```

**03 声明类**

声明文件也可以声明类及其属性和方法

```javascript
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

```javascript
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

```javascript
// node_modules/someLibrary/index.d.ts
declare module "someLibrary" {
    export function someFunction(a: number): number;
}

// 使用
import { someFunction } from "someLibrary";
someFunction(5);
```

**06 命名空间的使用**

可以使用命名空间来组织类型声明

```javascript
// utils.d.ts
declare namespace Utils {
  function utilityFunction(a: number): number;
}

// 使用
Utils.utilityFunction(10);
```

**07 全局扩展**

在声明文件中，我们还可以扩展全局现有的类型，比如扩展内置对象的原型

```javascript
// extensions.d.ts
declare global {
  // 内置类型 String，有一个 toNumber 的方法
  interface String {
    toNumber(): number;
  }
}

// 使用
// 这样就可以直接调用该方法了（只是在提示上有）
"123".toNumber(); // 假设 String 原型上确实有这个方法
```

**08 注意**

1. 声明文件中的代码并不是真正可执行代码，只是类型声明，不会被编译到 JS 
2. 在编写声明文件时，应该尽量精确地描述类型，以便 TS 提供尽可能准确的类型检查。

## 第三方库声明文件

## 自定义声明文件

## 配置文件解析

## 装饰器