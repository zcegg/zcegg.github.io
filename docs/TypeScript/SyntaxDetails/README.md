## 联合类型语法

在 TypeScript 中，联合类型（Unio Types）允许一个值具有多种可能的类型。使用联合类型时，可以使用 `|` 符号将多个类型联合在一起。

**01 基本语法**

```javascript
// 使用联合类型
let myVariable: string | number;
myVariable = "Hello"; // 合法
myVariable = 42;      // 合法
// myVariable = true;  // Error: 不能将类型 "boolean" 分配给类型 "string | number"
```

**02 类型守卫**

通过类型守卫，可以在运行时检查联合类型的具体类型。这个其实就是我们前文提到过的类型缩小、缩窄。可以理解为只要我们使用了联合类型，那么在使用具体类型值前最后先明确一下它的具体类型。

```javascript
function printValue(value: string | number): void {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else {
    console.log(value.toFixed(2));
  }
}

printValue("Hello"); // 输出: HELLO
printValue(42);      // 输出: 42.00
```

**03 交叉类型与联合类型结合**

我们可以将联合类型与交叉类型结合使用，创建出更复杂的类型

在下面的代码中，`Point` 类型是一个联合类型，表示一个点可以具有 `x` 属性或者 `y` 属性。

```javascript
type Point = { x: number } | { y: number };

let myPoint: Point = { x: 10 }; // 合法
myPoint = { y: 20 };             // 合法
// myPoint = { x: 10, y: 20 };   // Error: 不能将类型 "{ x: number; y: number; }" 分配给类型 "Point"
```

**04 联合类型与`null`和`undefined`**

联合类型经常与 `null` 和 `undefined` 一起使用，表示一个值可以是某种类型或者是 `null` 或 `undefined`

```javascript
let myValue: string | null | undefined;
myValue = "Hello";   // 合法
myValue = null;      // 合法
myValue = undefined; // 合法
```

**05 可辩识的联合类型**

通过给联合类型的每个成员添加一个其同的属性，可以创建可辩识的联合类型，从而在使用 `switch` 语句时更加安全

其实就是我们可能会设置 A | B | C 这样的联合类型，但是 A B C 本身又是一个非简单的数据类型，如果我们想在使用时再次区分 A B C ，那么就可以给他们设置具体的标识，这样就能区分他们，让类型变得更加安全

在下面的示例中，`Shape` 类型是一个可辩识联合类型，通过 `kind` 属性来区分不同的形状

```javascript
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "square"; sideLength: number };

function getArea(shape: Shape): number {
  // 这样我们就可以保证类型的安全，不同的类型进入到不同的操作
  // 也相当于完成了类型守卫
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.sideLength ** 2;
  }
}
```

## 类型别名

在 TypeScript 中，类型别名（Type Aliases）用于给一个类型起一个新名字，使代码更具可读性和可维性

**01 基本用法**

```javascript
// 使用类型别名：给 {x:number;y:number} 这个类型起了个别名
type Point = {
  x: number;
  y: number;
};

// 直接使用 Point 来代表我们想要约束的类型即可
let myPoint: Point = { x: 10, y: 20 };
```

**02 泛型类型别名**

我们当前可能还是不理解泛型，所以先有个印象即可。类型别名是可以与泛型一起使用的，从而创建更通用的类型

```javascript
// 泛型类型别名
// T 就是一个泛型，相当于函数的形参，函数没调用时，我们不知道形参是什么
// 但是我们可以在函数体内部去使用我们的形参，它就是一个变量
type Pair<T> = {
  first: T;
  second: T;
};

// 我们现在使用了 Pair 这个类型，就相当于我们调用了 Pair 这个函数
// 定义 Pair 类型时，我们设定了一个形参 T，此时我们可以考虑传参了
// 在下面的代码中，我们就传入了一个具体的 string 类型。那么随之而来的就是整个函数体内部使用 T 的地方都会被 string 代替
let myPair: Pair<string> = { first: "hello", second: "world" };
```

**03 联合类型和交叉类型的类型别名**

类型别名可以用于联合类型和交叉类型

```javascript
// 联合类型的类型别名
type Result = "success" | "failure";

// 交叉类型的类型别名
type Coordinates = { x: number } & { y: number };

let myResult: Result = "success";
let myCoordinates: Coordinates = { x: 10, y: 20 };
```

**04 可辩识联合类型的类型别名**

类型别名可以用于可辩识联合类型（Discriminated Unions）

```javascript
// 可辨识联合类型的类型别名
// 可辩识指的就是 A | B 时 A 与 B 都有自己的 kind
type Shape = { kind: "circle"; radius: number }
  | { kind: "square"; sideLength: number };

function getArea(shape: Shape): number {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.sideLength ** 2;
  }
}
```

**05 接口 VS 类型别名**

接口类型我们后续会详细介绍，这里先对两者进行比较，等理解了接口类型后再回来理解也 OK 。

类型别名和接口都可以用于定义对象类型，但有一些区别。主要在于类型别名支持联合类型和交叉类型，而接口是不支持的。

```javascript
// 类型别名支持联合类型和交叉类型
type UnionType = number | string;
type IntersectionType = { x: number } & { y: number };

// 接口不支持联合类型和交叉类型
// 不要给对象类型的 key 的 value 设置一个联合类型或交叉类型
interface MyInterface {
  // Error: Interface 'MyInterface' cannot simultaneously extend types 'number' and 'string'.
  // value: number | string;
  x: number;
  y: number;
}
```

**06 typeof类型别名**

使用 `typeof` 可以创建一个类型别名，表示某个变量的类型。这在 TS 的类型编程中还是很常用的。希望你有个记忆

```javascript
// 我们定义了一个变量，直接赋值，在 TS 中会推断出它的类型
const myVariable = { x: 10, y: 20 };

// 我们使用 type xxx 的操作想要声明一个类型
// 这个类型直接来自于 typeof 变量名  的结果
type MyType = typeof myVariable;

// 直接使用上述的类型别名即可
let myValue: MyType = { x: 30, y: 40 }; // 合法
```