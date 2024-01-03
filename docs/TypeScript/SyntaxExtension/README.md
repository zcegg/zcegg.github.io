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