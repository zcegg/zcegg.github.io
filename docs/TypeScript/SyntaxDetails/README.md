## 联合类型语法

在 TypeScript 中，联合类型（Unio Types）允许一个值具有多种可能的类型。使用联合类型时，可以使用 `|` 符号将多个类型联合在一起。

**01 基本语法**

```typescript
// 使用联合类型
let myVariable: string | number;
myVariable = "Hello"; // 合法
myVariable = 42;      // 合法
// myVariable = true;  // Error: 不能将类型 "boolean" 分配给类型 "string | number"
```

**02 类型守卫**

通过类型守卫，可以在运行时检查联合类型的具体类型。这个其实就是我们前文提到过的类型缩小、缩窄。可以理解为只要我们使用了联合类型，那么在使用具体类型值前最后先明确一下它的具体类型。

```typescript
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

```typescript
type Point = { x: number } | { y: number };

let myPoint: Point = { x: 10 }; // 合法
myPoint = { y: 20 };             // 合法
// myPoint = { x: 10, y: 20 };   // Error: 不能将类型 "{ x: number; y: number; }" 分配给类型 "Point"
```

**04 联合类型与`null`和`undefined`**

联合类型经常与 `null` 和 `undefined` 一起使用，表示一个值可以是某种类型或者是 `null` 或 `undefined`

```typescript
let myValue: string | null | undefined;
myValue = "Hello";   // 合法
myValue = null;      // 合法
myValue = undefined; // 合法
```

**05 可辩识的联合类型**

通过给联合类型的每个成员添加一个其同的属性，可以创建可辩识的联合类型，从而在使用 `switch` 语句时更加安全

其实就是我们可能会设置 A | B | C 这样的联合类型，但是 A B C 本身又是一个非简单的数据类型，如果我们想在使用时再次区分 A B C ，那么就可以给他们设置具体的标识，这样就能区分他们，让类型变得更加安全

在下面的示例中，`Shape` 类型是一个可辩识联合类型，通过 `kind` 属性来区分不同的形状

```typescript
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

## 交叉类型

在 TypeScript 中，交叉类型是一种高级类型，它允许我们将多个类型合并为一个类型。这意味着我们可以组合多个类型（类、接口或其它类型）来创建一个包含所有类型属性的单一类型。

**01 基本事法**

交叉类型使用 `&` 符号来组合多个类型

```typescript
type FirstType = { name: string; };
type SecondType = { age: number; };

// 此后 CombinedType 就是 { name: string, age: number }
type CombinedType = FirstType & SecondType;
```

**02 属性合并**

- 如果两个类型具有相同的属性，但属性类型不同，则结果类昔中该属性的类型就是这些类型的交叉类型
- 如果属性类型不兼容，则交叉之后就是 `never` 类型

```typescript
type Type1 = { prop: string; };
type Type2 = { prop: number; };

// 两个类型交叉出现了相同的 key 属性，且它们的值类型也不相同
// 交叉之后的结果就是保留这个 key 属性，让它们的值类型进行交叉
// 但是不可能有一个类型，既是 number 也是 string，所以返回的就是 never类七里
type CombinedType = Type1 & Type2; // { prop: never; }
```

**03 接口与类**

交叉类型可以用来合并接口或类型的类型

```typescript
// 定义一个接口，包含一个 run 属性，它是一个函数
interface Runnable {
    run(): void;
}

interface Jumpable {
    jump(): void;
}

// 使用交叉类型合并两个接口。因为 key 不同，所以就是两者都会有
type Action = Runnable & Jumpable;

// 接口是可以继承和实现的
class Hero implements Action {
    run() { /*...*/ }
    jump() { /*...*/ }
}
```

**04 交叉类型与泛型**

交叉类型可以与泛型一起使用，提供更灵活的类型组合

```typescript
type Container<T> = { value: T };
type Timestamped<T> = { timestamp: Date } & T;

// 这里要想明白，先走 Container<T>
// 再走 Timestamped<Container<T>>
// 其实就和函数的调用很像
type TimestampedContainer<T> = Timestamped<Container<T>>;
```

**05 使用细节**

1. 交叉类型可能导致类型过于复杂，使用时需要注意保持类型的清晰和简洁
2. 在组合类时，只有类的类型信息会被组合，构造函数和方法不会被组合

## 字面量类型

在 TypeScript 中，字面量类型是一种使用具体的值来定义类型的方式，这些类型代表了单一的、确切的值，可以用于创建更精确的类型定义。使用字面量类型，我们可以限制变量或参数必须拥有特定的值。

**01 字符串字面量类型**

字符串字面量类型允许我们指定变量或参数必须是一个确切的字符串值

```typescript

// 联合类型的几个值都是具体的 string 类型值
type Direction = "north" | "south" | "east" | "west";

function move(direction: Direction) {
    // ...
}

// 使用 Directiion 来约束 形参，只能是这四个值当中的一个
move("north"); // 正确
move("up");    // 错误：类型 '"up"' 不能赋值给类型 'Direction'
```

**02 数字字面量**

限制类型的值必须是某个固定的数值字面量

```typescript
type Answer = 42;

let myAnswer: Answer = 42;    // 正确
let wrongAnswer: Answer = 43; // 错误：类型 '43' 不能赋值给类型 '42'
```

**03 布尔字面量类型**

布尔字面量类型限制变量必须是 `true` 或 `false`

```typescript
type TrueType = true;
let isTrue: TrueType = true;  // 正确
let isFalse: TrueType = false; // 错误：类型 'false' 不能赋值给类型 'true'
```

**04 使用场景**

『字面量类型通常都是与联合类型结合使用，用于创建一组固定的值』

```typescript
type HTTPMethod = "GET" | "POST" | "PUT" | "DELETE";

function sendRequest(method: HTTPMethod, url: string) {
    // ...
}
```

**05 可辩识联合**

字面量类型是实现可辩识联合（一种模式，用于结合联合类型和字面量类型）的关键部分。

在下面的示例中，我们定义了两个接口，然后通过类型别名将他们组合在一起，这样就生成了一个更复杂的类型。但是两个接口中的属性并不完全相同，所以将来我们在具体使用组合而成的复杂类型时，为了保证类型安全就需要具体化当前值的类型。此时就可以在接口中定义一个字面量类型。这样进行类型缩小或者守卫时就可以直接判断使用。从而也创建了一个可辩识的联合类型。

```typescript
interface Circle {
    kind: "circle";
    radius: number;
}

interface Square {
    kind: "square";
    sideLength: number;
}

type Shape = Circle | Square;

function getArea(shape: Shape): number {
    switch (shape.kind) {
        case "circle":
            return Math.PI * shape.radius ** 2;
        case "square":
            return shape.sideLength ** 2;
    }
}
```

## 类型别名

在 TypeScript 中，类型别名(`type`) 是一种声明任意类型的方法，包括原始类型、对象类型、联合类型、交叉类型等。类型别名的语法非常灵活，允许你定义复杂的类型结构。

**01 基本语法**

类型别名用于为一个类型指定一个名字

```typescript
type Point = {
    x: number;
    y: number;
};

const point: Point = { x: 10, y: 20 };
```

**02 联合类型**

类型别名可以用于创建联合类型，表示一个值可以是几种类型之一

```typescript
// 定义联合类型的类型别名
type ID = string | number;

let userId: ID = "user123";
userId = 123;  // 也可以是一个 number
```

**03 交叉类型**

类型别名也可以用来组合多个类型，创建一个包含所有类型属性的单一类型

```typescript
// 定义一个 Draggable
type Draggable = {
    drag(): void;
};

// 定义一个 Resizable
type Resizable = {
    resize(): void;
};

// 将两者的交叉类型再次联合生成一个新的类型
type UIWidget = Draggable & Resizable;

// 约束 textBox
const textBox: UIWidget = {
    drag() {},
    resize() {}
};
```

**04 泛型**

类型别名可以使用泛型，提供更高的复用性和适用性

```typescript

// 定义了 泛型 T ，内部使用 T 
type Container<T> = { value: T };

// 使用时传入具体的类型 number 和 string 
const numberContainer: Container<number> = { value: 123 };
const stringContainer: Container<string> = { value: "Hello" };
```

**05 映射类型**

类型别名非常适合用于创建映射类型，基于旧类型来创建新类型。但是，你可能对于映射类型本身感到非常因扰。所以，我们后面会单独说明映射类型

```typescript
type Readonly<T> = {
  // readonly 用于限制只读
  // keyof T 会得到 T 中的所有的 key 组成的[]
  // P in keyof T 则会循环拿到所有的 key
  readonly [P in keyof T]: T[P];
};

// Reaonly 就相当于是一个工具类型，接收 Point 类型，返回一个新的类型
// 新类型的 key 都是只读的
type ReadonlyPoint = Readonly<Point>;
```

**06 条件类型**

类型别我可以用来定义条件类型，这种类型依据条件表达式的结果产生不同的类型，上面的映射类型与这里的条件类型，在后续我们会继续说明。这里主要有个印象

```typescript
// T extends string 就是一个条件语句，返回 true 或 false
// T 只要是 string 的子类型就返回 true，否则 false
type Check<T> = T extends string ? "String" : "Other";

type Type1 = Check<string>;  // "String"
type Type2 = Check<number>;  // "Other"
```

**07 使用类型别名定义函数类型**

类型别名可以用来定义函数的类型

```typescript
// 将一个 (message: string) => void 类型通过 type 来定义别名
type Greeter = (message: string) => void;

const greet: Greeter = message => console.log(message);
```

**08 元组和数组类型**

类型别名也可以用来定义元组和数组类型

```typescript
// 下面定义数组的方式并不推荐，可以 string[]
type StringArray = Array<string>;
type NumberArray = number[];

type Response = [string, number];  // 元组类型
```

**09 不支持声明合并**

不同于接口，类型别名不支持声明合并。如果你尝试声明两个相同名称的类型别名， TypeScript 会报错

## 接口使用

在 TypeScript 中，接口（Interface）用于定义对象的结构，包括属性的名称和类型。接口可以用于描述对象的形状、函数的参数和返回值等。

**01 基本语法**

在下面的代码中，`Point` 是一个接口，表示具有 `x` 和 `y` 属性的点类型。`myPoint` 是一个符合 `Point` 接口定义的对象

```typescript
// 定义接口：采用 interface 关键字
interface Point {
  x: number;
  y: number;
}

// 使用接口：约束类型
let myPoint: Point = { x: 10, y: 20 };
```

**02 可选属性**

接口中的属性可以标记为可选，使用 `?` 符号，我们在之前已经见到过了

```typescript
interface Person {
  name: string;
  age?: number; // 可选属性
}

// 可以不传递 age 属性
let person1: Person = { name: "Alice" };
// 可以传递 age 属性
let person2: Person = { name: "Bob", age: 30 };
```

**03 只读属性**

接口中的属性可以标记为只读，使用 `readonly` 关键字

```typescript
// 问号在属性的后面
// readonly 在属性的前面
interface Config {
  readonly apiKey: string;
  endpoint: string;
}

let config: Config = { apiKey: "abc123", endpoint: "/api" };
// config.apiKey = "newKey"; // Error: 无法分配到 "apiKey" ，因为它是只读属性。
```

**04 接口与函数类型**

接口也可以用于描述函数类型，在函数类型中我们会再次见到。下面的语法我们也称之为叫调用签名

```typescript

// 定义一个接口，描述函数时采用 ():返回值的 语法结构
interface GreetFunction {
  (name: string): string;
}

let greet: GreetFunction = function (name) {
  return `Hello, ${name}!`;
};
```

**05 索引属性**

接口可以描述对象的可索引属性，支持字符串和数字索引（只支持这两种）

```typescript
interface StringArray {
  // 索引的类型是 number ，对应的值类型是 string
  [index: number]: string;
}

// 用 StringArray 来约束变量 myArray 
// 赋值的时候是[]，它的 index 是number 。值是 string
let myArray: StringArray = ["one", "two", "three"];

// 取出的值是 string 类型，合法
let firstItem: string = myArray[0];
```


**06 继承接口**

接口可以通过 `extends` 关键字继承其它接口

```typescript
// 定义一个接口 Shape
interface Shape {
  color: string;
}

// 使用 关键字 extends 让 Square 来继承 Shape接口
interface Square extends Shape {
  sideLength: number;
}

// 赋值的时候发现，必须设置 color 属性，否则语法报错
let square: Square = { color: "red", sideLength: 10 };
```

**07 实现接口**

接口可以被类实现，从而确保类拥有接口定义的属性和方法。关于 TS 中的类，在后面还会详细说明

```typescript
// 定义一个接口 Clock，包含 currentTime key ，及 setTime 函数
interface Clock {
  currentTime: Date;
  setTime(d: Date): void;
}

// 定义一个类叫 Wallclock ，通过关键字实现 implements 来实现 Clock 接口
class WallClock implements Clock {
  currentTime: Date = new Date();

  setTime(d: Date): void {
    this.currentTime = d;
  }
}
```

**08 混合类型**

接口可以用来定义混合类型，例如一个对象可以同时作为函数和对象使用。

```typescript

// 使用 interface 关键字定义一个接口
// 看似一个对象类型，但是可以是函数
interface Counter {
  // 调用签名：表示实现了 Counter 类型的变量可调用
  (start: number): string;
  // 约束有一个 interval 属性
  interval: number;
  // 有一个 reset 属性，是函数
  reset(): void;
}

// 定义一个函数
function getCounter(): Counter {
  // 使用 as Counter 将 counter 强制断言为 Counter 类型
  // 然后 设置 interval reset 属性
  // 此时 counter 就是一个对象，也是一个函数，因为可调用
  let counter = (function (start: number) {}) as Counter;
  counter.interval = 123;
  counter.reset = function () {};
  return counter;
}
```

**09 声明合并**

TypeScript 的接口支持声明合并。如果在同一个作用域内多次声明同一个接口，TypeScript 会将它们合并为单个接口。

```typescript
// 定义接口 Box
interface Box {
    height: number;
}

// 再次使用 Box：合法
interface Box {
    width: number;
}

// 结果是两者会合并。
let box: Box = { height: 5, width: 6 };
```

## 类型与接口的比较

### 一、主要区别

**01 用途和表达力**

- 类型别名更灵活，可以用来表示几乎任何类型，包括复杂的联合或交叉类型
- 接口更适合定义对象结构，特别是当需要创建一系列具有某些共同特征的对象时

**02 扩展与合并**

- 接口支持扩展和声明合并，使得它们更适合于定义大型的、复杂的类型系统，例如在类库或 API 中。
- 类型别名不支持扩展和声明合并，但它们在组合现有类型时更灵活

### 二、使用建议

1. 当我们需要一个联合类型、元组类型、或者其它 TS 接口不能表示的复杂类型时，使用类型别名即可
2. 当我们定义的类型主要用于对象，且可能需要继承或实现的时候则使用接口
3. 如果类型需要在多个地方重复使用，并且有可能需要将多个声明合并为一个，则接口更适合

## 类型断言

在 TypeScript 中，类型断言是一种告诉编译器 『我知道我在做什么』的方式。它允许我们将一个变量强行指定为更具体的类型。类型断言不会进行任何特殊的数据检查或重构，它只会影响 TypeScript 的类型检查，常见的断言方式有两种

**01 尖括号语法**

在下面的代码， `someValue` 的类型是 `any`，但是我们知道它实际上是一个字符串。我们使用类型断言 `<string>someValue` 来告诉 TypeScript，`someValue`  应该被视为 `string` 类型。

```typescript
// 定义了一个 someValue 变量，类型是 any 
// 在 ts 看来，这个类型就已经不安全了，可以对它进行任何的操作
let someValue: any = "this is a string";
// 之后定义一个变量 strLength，希望它是一个 number 类型
// 具体的值来自于 xxx.length，可以通过 someValue.length 来返回
// 由于someValue 是一个any 类型。它可以直接使用
// 这里采用 <string>someValue 的语法是为了告诉TS类型检查系统按 string 处理 someValue
let strLength: number = (<string>someValue).length;
```

**02 `as` 语法**

上述的 尘括号 语法在 JSX 中会产生理解上的困扰，所以我们还可以使用 as 语法。

```typescript
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
```

**03 使用场景**

1. 与 `any` 类型交互：当从 javascript 迁移到 TypeScript，或者处理复杂的库和数据结构时，我们可能需要 `any` 类型。此时可以通过类型断言来明确指定更具体的类型
2. 类型缩窄：在一些复杂类的条件语句中，我们可能需要缩小某个变量的类型来保证语法的安全，此时也可以通过 as 来实现。
3. 类型守卫：虽然类型断言在某些情况下是必须的，但在可能的情况下，使用类型守卫（typeof、instanceof检查）是更好的选择，因为它可以提供真正的运行时检查。

**04 注意事项**

1. 类型断言不是类型转换：它不会改变变量的实际类型，如果我们错误地断言了类型，可能会导致运行时错误（只是解决了我开发阶段的语法飘红）
2. 避免过度使用：频繁使用类型推断可能是类型设计不够健全的信号。如果开发中我们经常需要使用类型断言，那就需要重新审视我们的类型设计了

## 非空断言

在 TypeScript 中，非空断言操作符（`!`）是一种特殊的类型断言，用于告诉编译器在某个特定的位置，表达式一定不是 `null` 或 `undefined`。非空断言提供了一种方式来断言某个值在运行时一定存在，即使 TypeScript 的类型检查无法推断出这一点。

非空断言在处理严格的空检查时非常有用，但它应该被谨慎使用。理想情况下，我们应该通过类型守卫（`typeof`、`if`、`instanceof`）或其它的类型检查方法来确保值的存在，则不是依赖非空断言，就像我们不能频繁使用类型断言一样。

**01 基本语法**

非空断言操作符紧跟在一个表达式后面，没有空格。形式为 `expression!`

在下面的代码中， `value` 是一个可能为 null 的字会串类型，虽然我们是显示注解的，但实际开发中不停的调用某个函数或者回调函数，参数再传递的过程中也会被推断为 undefined 或 null。此时使用 `!` 之后，就相当于告诉 TypeScript `value` 在这里不会是 `null` 。那么它就会被当做是 `string` 类型。

```typescript
let value: string | null = "hello world";
let strLength: number = value!.length;
```

**02 使用场景**

1. 与可选链结合：当我们使用可选链 (`?.`) 但确定表达式的值存在时，可以通过非空断言来避免额外的 `undefined` 检查
2. 在严格的空检查下断言存在：在 TypeScript 的严格模式下，即使我们知道某个值不会是`null` 或 `undefined` ，编译器也可能无法推断出来。此时我们就可以使用非空断言。

```typescript
let obj: { a?: { b: string } } = { a: { b: "hello" } };

// 因为 a 是一个可选属性，所以 obj.a 时 可能拿到的是 undefined 
// 结合当前的上下文，我们明确的知道 obj 中的 a 是有值的
// 所以在 obj.a.b 的语法中我们就可以使用非空断言的操作
let b: string = obj.a!.b;  // 非空断言，确认 a 存在
```

**03 注意事项**

1. 风险：非空断言操作符会绕过编译器的空检查，如果错误地使用，可能会导致运行时错误。因为我们实际上是告诉编译器『相信我，我知道我在干什么』，即使那个值是 `null` 或 `undefined` 我们不在意
2. 谨慎：应当尽量避免使用非空断言，除非我们非常确定该值在运行时一定不是 `null` 或 `undefined`

## 类断守卫

类型守卫（Type Guards）是一种技术，它用于确保在特定的作用域内变量属于特定义的类型。类型守卫可以在编译时提供类型保障，并在运行时进行实际的类型检查。

**01 `typeof` 守卫**

`typeof` 守卫用于基本类型检查（如 `string`、`number`、`boolean`、`symbol`）

```typescript
function padLeft(value: string | number) {
  // value 是一个不明确的类型，在使用的时候为了保证类型安全
  // 我们可以通过 typeof 来完成
  if (typeof value === "string") {
      return `String: ${value}`;
  } else {
      return `Number: ${value}`;
  }
}
```

**02 `instanceof` 守卫**

该守卫一般用于类的实例检查

```typescript
class Bird {
    fly() {}
}

class Fish {
    swim() {}
}

function getSmallPet(pet: Bird | Fish) {
  // 同样 pet 并不是一个明确的类型
  // 我们可以通过守卫来明确它的类型
  if (pet instanceof Bird) {
      pet.fly();
  } else {
      pet.swim();
  }
}
```

**03 自定义守卫**

```typescript
function isFish(pet: Bird | Fish): pet is Fish {
  // 01 pet 是一个联合类型
  // 02 使用 as 将它断言为 Fish
  // 03 访问 Fish 身上的 swim 属性，如果真的是 Fish 则存在
  // 04 返回一个布尔值，来表明 pet 是不是 Fish，如果返回false 则肯定 Bird
  return (pet as Fish).swim !== undefined;
}

// 在调用之后，TypeScript 知道在对应的分支中 `pet` 是 `Fish` 类型
if (isFish(pet)) {
    pet.swim();
} else {
    pet.fly();
}
```

**04 字面量类型守卫**

```typescript
type Shape = Circle | Square;

interface Circle {
    kind: "circle";
    radius: number;
}

interface Square {
    kind: "square";
    sideLength: number;
}

// 本质上就是通过 可辩识的标记来明确复杂类型中是不是某个具体的类型
function getArea(shape: Shape) {
    if (shape.kind === "circle") {
        return Math.PI * shape.radius ** 2;
    } else {
        return shape.sideLength ** 2;
    }
}
```

**05 `in` 操作符**

```typescript

function move(pet: Bird | Fish) {
  if ("fly" in pet) {
      return pet.fly();
  }
  // 如果不是 Bird 肯定是 Fish 类型
  // 直接调用 swim 即可
  // 如果两者都不是则不可能成功赋值给 pet（除非我们断言）
  return pet.swim();
}
```

## 空值合并

这里刻意将空值合并语法单独拿出来是因为它也能实现类型守卫，但本质是一个新的语法。

在 TS 中，空值合并操作符（`??`）是一种逻辑操作符，用于在左侧的操作数为 `null` 或 `undefined` 时返回右侧的操作符。这个操作符在处理可能为 `null` 或 `undefined` 的值时非常有用，尤其是在赋值或设置默认值的场景中。

**01 基本语法**

```typescript
// 如果 value 为 undefined 或 null 则会返回 defaultValue
const result = value ?? defaultValue;
```

**02 与 `||` 的比较**

1. `||` 如果要返回左侧操作数，它就和须是一个 truthy 值（即非 `false` `0` `""` `null` `undefined` `NaN`），否则返回右侧
2. `??` 仅在左侧操作数是 `null` 或 `undefined` 时返回右侧操作数，否则一直返回左侧
3. 这意味着 `??` 在处理包含 `0` `false` 和空字符串等 falsy值时更加精确，因为它不会将他们视为 `null`或`undefined` 从而返回默认值。

**03 注意事项**

- 空值合并操作符不能与 `&&` 或 `||` 操作符直接组合使用，除非我们使用括号明确指定优先级
- 确保在使用空值合并操作符时左侧的变量类型包括 `null` 或 `undefined`，否则 typescript 编译器可能会直接报错。简单说就是有 `null` 和 `undefined` 存在的情况，「`??`」 才有意义



