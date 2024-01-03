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

## 内置声明文件

## 第三方库声明文件

## 自定义声明文件

## 配置文件解析

## 装饰器