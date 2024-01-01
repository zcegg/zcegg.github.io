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

**01 基本函数调用签名**

一个基本的函数调用签名包括参数列表和返回值类型

```javascript
// (...args:any[]) => returnVal
// 将来哪个变量被 GreetFunction 约束，则说明它是可调用的
type GreetFunction = (name: string) => string;
```

**02 接口中的调用签名**

我们可以在接口中定义函数调用签名，这在定义类似于API的结构时非常有用。

```javascript
// 定义了一个接口 AddFunction
// 它定义了一个调用签名，注意这与上面的语法格式不太相同
// (...args:any[]):returnValue
interface AddFunction {
  (a: number, b: number): number;
}

const add: AddFunction = (a, b) => a + b;
```

**03 调用签名中的参数**

在函数中可以使用的可选参数、默认参数、剩余参数语法都可以在调用签名中直接使用。

```javascript
// 可选参数和默认参数
type GreetFunction = (name: string, greeting?: string) => string;

const greet: GreetFunction = (name, greeting = "Hello") => `${greeting}, ${name}`;

// 类型别名语法：剩余参数
type BuildNameFunction = (firstName: string, ...restOfName: string[]) => string;

// 接口语法：剩余参数
interface BuildNameFunction {
  (firstName: string, ...restOfName: string[]): string;
}

const buildName: BuildNameFunction = (firstName, ...restOfName) => {
  return firstName + " " + restOfName.join(" ");
};
```

**04 泛型函数的调用签名**

泛型也可以用于函数调用签名中，从而提供更高的灵活性

```javascript
// 明确的调用签名是 (...args:any[]) => returnVal
// 配合泛型就是 <T>(arg:T)=> T
type IdentityFunction = <T>(arg: T) => T;

const identity: IdentityFunction = <T>(arg: T) => arg;
```

**05 高级语法**

函数调用签名适用于定义复杂的函数类型，例如在定义回调函数或将函数作为参数传递时

```javascript

// 01 定义一个类型别名叫 CallbackFunction ，通过调用签名来声明
// 02 调用签名的参数语法和之前一样，联合类型，可选，剩余，默认值等
type CallbackFunction = (err: Error | null, response: string) => void;

// 此后我们将 CallbackFunction 做为一个类型来约束某个回调函数
function fetchData(callback: CallbackFunction) {
    // ...
}
```

## 函数构造签名

在 TypeScript 中，函数构造签名用于描述一个`对象或类的构造函数`的类型。它指定了构造函数的参数类型和返回类型（通常是类的实例类型）。这种签名在定义接口或类型别名时特别有用，尤其是当我们在处理类和更高级的抽象时。

**01 构造签名基本语法**

在接口中，我们可以定义一个构造签名，这表明该接口可以被用作构造函数

在下面的代码中 `ClockConstructor` 接口定义了一个构造签名，它接收两个参数 `hour` `minute`，并返回一个 `ClockInterface` 类型的对象

```javascript
// 调用签名可以通过 type 或 interface 来实现
// 构造签名的语法主要在于多了一个 new ，表示可以当做构造函数用
interface ClockConstructor {
    new (hour: number, minute: number): ClockInterface;
}

interface ClockInterface {
    tick(): void;
}

function createClock(ctor: ClockConstructor, hour: number, minute: number): ClockInterface {
    return new ctor(hour, minute);
}
```

**02 类实现构造签名**

构造签名本质上就是说明某个类型可以被执行 new 操作，所以直接通过类型来定义 constructor 函数是一样的效果

下面的代码中 `DigitalClock` 和 `AnalogClock` 类都实现了 `ClockInterface` 接口，并且它们的构造函数符合 `ClockConstructor`的构造签名

```javascript
class DigitalClock implements ClockInterface {
    constructor(h: number, m: number) { /* ... */ }
    tick() { console.log("beep beep"); }
}

class AnalogClock implements ClockInterface {
    constructor(h: number, m: number) { /* ... */ }
    tick() { console.log("tick tock"); }
}

let digital = createClock(DigitalClock, 12, 17);
let analog = createClock(AnalogClock, 7, 32);
```

**03 构造签名和类型别名**

同样，我们也可以使用类型别名来定义构造签名。格式上的区别仍然是 ():void 和 ()=>void

```javascript
type ClockConstructor = new (hour: number, minute: number) => ClockInterface;
```

**04 泛型构造签名**

构造签名也可以包含泛型参数，这使得签名更加灵活

```javascript
interface Constructor<T> {
  new (...args: any[]): T;
}

class SomeClass {
  constructor(public name: string) { /* ... */ }
}

function createInstance<T>(C: Constructor<T>, ...args: any[]): T {
  return new C(...args);
}

let instance = createInstance(SomeClass, "Test");
```

## 可选参数

函数的可选参数允许我们在调用函数时有选择的省略这些参数，这个操作提供了更大的灵活性，同时保持了类型安全。

**01 定义可选参数**

在 TS 中，我们可以通过在参数名称后面添加一个 `?` 来定义一个可选参数

```javascript
// greeting 是一个可选参数，如果在调用 greet 函数时不提供 greeting 参数，它的值将是 undefined
function greet(name: string, greeting?: string) {
  return `${greeting || "Hello"}, ${name}`;
}
```

**02 可选参数位置**

可选参数必须位于必须参数之后，将一个可选参数置于必须参数之前会导致编译错误

```javascript
// 错误：可选参数不能位于必需参数之前
function greet(greeting?: string, name: string) {
  // ...
}
```

**03 使用默认参数**

作为一种替代可选参数的方式，我们可以为函数参数提供一个默认值。如果调用时未提供该参数，将使用其默认值

使用默认参数时，该参数将自动变为可选，但与明确标记为可选的参数不同，如果未提供值，它将使用默认值而非 `undefined`

```javascript
function greet(name: string, greeting: string = "Hello") {
  return `${greeting}, ${name}`;
}
```

**04 可选参数与类型推断**

当使用可选参数时，TS 会自动将参数类型推断为 `原类型|undefined`。例如，如果一个参数被定义为 `string | undefined`，那么我们可以将它标记为可选

```javascript
function greet(name: string, greeting?: string) {
  // greeting 的类型是 'string | undefined'
}
```

**05 可选参数与剩余参数结合**

可选参数可以与剩余参数结合使用，但同样的，所有可选参数必须位于剩余参数之前。

```javascript
function buildName(firstName: string, lastName?: string, ...titles: string[]) {
  // ...
}
```

**06 可选回调函数**

对于回调函数，我们也可以将其作为可选参数

```javascript
function loadData(callback?: () => void) {
    // ...
  callback?.();
}
```

## 参数默认值

函数参数可以有默认值，这意味着当调用函数时，如果没有提供某个参数，那么将使用该参数的默认值。这增加了函数的灵活性，同时保待了类型安全。

**01 定义默认参数**

```javascript
// 使用默认值之后 greeting 就自动转为可选参数
// 如果调用时不传入，则使用默认值而非 undefined
function greet(name: string, greeting: string = "Hello") {
  return `${greeting}, ${name}`;
}
```

**02 默认参数的类型推断**

默认参数不需要显式的定义类型，类型将从默认值自动推断出来

```javascript
// value 参数具有默认值 empty ，无须显式的注解为 any 类型。它可以自动推断
function createArray(length: number, value: any = "empty") {
  // ...
}
```

**03 默认参数后的必须参数**

在 JS 和 TS 中，可以在有默认值的参数后面放置没有默认值的参数，但是这样的参数只能通过明确传递  `undefined` 来使用它的默认值，所以实际开发中，我们建议还是将有默认值参数放在必填参数的后面

```javascript
// greeting 是一个有默认值的参数，语法上允许我们在后面放置必填参数
function greet(greeting: string = "Hello", name: string) {
  return `${greeting}, ${name}`;
}

greet(undefined, "Alice"); // 正确
greet("Hi", "Alice");      // 正确
// 这样操作就相当于将 greeting 赋值为 Alice 而name 没有赋值
greet("Alice");         // 错误：第二个参数没有提供
```

**04 默认值与可选参数**

虽然默认值在调用时是可选的，但在 TS 中，它们与明确标记为可选的参数（使用 `?`） 是不同的。默认参数在没有提供时使用默认值，而可选参数如果没有设置则使用 `undefined`

```javascript
// lastName 有默认值参数，默认传为可选
// 有默认值参数无须注解类型，默认会进行推断
// 如果调用时没有传递会使用默认值，即使我们传递了 undefined
function buildName(firstName: string, lastName = "Smith") {
  // ...
}

buildName("Alice");        // 正确，lastName 将是 "Smith"
buildName("Alice", undefined); // 正确，lastName 仍然是 "Smith"
buildName("Alice", null);     // 错误，null 不是 string 类型
```

**05 函数签名与默认参数**

在类型别名或接口中定义函数签名时，不能在签名中指定参数的默认值。相反，所有参数都被认为是必须需的。

```javascript
// 使用类型别名，创建了一个函数调用签名，来实现一个函数类型
// 定义调用签名时，它的参数不能指定默认值，都是必须的
// 但是在实现调用签名的时候可以设置默认值
type GreetFunction = (greeting: string, name: string) => string;

// 实现时可以提供默认参数值
const greet: GreetFunction = (greeting = "Hello", name) => `${greeting}, ${name}`;
```

## 剩余参数

函数的剩余参数（Rest Parameters）允许我们将一个不定量的参数作为一个数组。这在处理多个参数，特别是参数数量不确定时非常有用。

**01 剩余参数基本语法**

剩余参数通过在参数名称前添加 `...` 来表示，这表明这个参数将收集所有剩余的传递给函数的参数

```javascript

// restOfName 是一个类型为 string[] 的剩余参数，我们可以传递任意数量的字符串参数给 buildName 函数
function buildName(firstName: string, ...restOfName: string[]): string {
  return firstName + " " + restOfName.join(" ");
}
```

**02 剩余参数与其它参数**

剩余参数可以与其他参数一起使用，但是必须放在参数列表的最后。其实目前来看，参数总计就四种：必填参数、可选参数、默认值参数、剩余参数。推荐在使用的时候就按着这种排序进行设置

```javascript
function printInfo(message: string, ...tags: string[]) {
  console.log(message, tags);
}
```

**03 函数重载与剩余参数**

在使用函数重载时，剩余参数应在每个重载签名中保持一致

```javascript
function foo(...args: string[]): void;
function foo(...args: number[]): void;

// 实现时剩余参数的类型要兼容所有的重载的定义，否则就无法做到类型兼容
function foo(...args: any[]) {
  // 实现部分
}
```

**04 泛型和剩余参数**

剩余参数也可以与泛型一起使用

```javascript
// 调用 mergeArrays 时可以传入具体的 T 类型。
// 这里的语法就是要求调用时传入任意个由 T 类型组成的 array 类型
// 因为是剩余参数，所以任意个 array 又组成了一个新的 array
// 所以 arrays 就是 [[], [], []]
function mergeArrays<T>(...arrays: T[][]): T[] {
  return arrays.flat();
}

// 我们调用 mergeArrays 函数并传入三个数字类型的数组，函数返回一个新的数组，它包含所有传入数组的元素。由于我们传递的是 number 类型的数组，所以T会被推断为 number 类型
const array1 = [1, 2, 3];
const array2 = [4, 5, 6];
const array3 = [7, 8, 9];

const mergedArray = mergeArrays(array1, array2, array3);
console.log(mergedArray); // 输出: [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## 函数重载

函数重载是一种机制，这种机制让我们可以多次声明同一个函数名，但是他们具有不同的参数类型或参数数量。这样我们的函数就可以依据不同的参数类型或数量来执行不同的操作。

**01 重载签名**

首先，我们需要声明重载的签名，这些签名定义了函数的不同调用方式。注意，这些签名只是声明，不包含具体的实现

```javascript
// add 函数有两个重载，一个接受两个number 类型的参数，另一个接受两个string 类型的参数
function add(a: number, b: number): number;
function add(a: string, b: string): string;
```

**02 实现签名**

有了重载签名之后，我们就可以实现他们。此时我们需要提供一个兼容所有重载签名的实现。

实现签名通常具有更通用的参数类型，`以涵盖所有重载的情况`。重载签名则定义了函数对外暴露的具体接口，它们是可见的部分

```javascript

// 定义重载签名
function add(a: number, b: number): number;
function add(a: string, b: string): string;

// 实现重载签名
// 在实现的时候，一定要兼容所有的重载，这就要求我们想办法做到类型兼容
// add 方法我们也称为叫重载函数
function add(a: number | string, b: number | string): number | string {
  // 这里做类型守卫，让值在使用的时候是某一种更具体的类型
  if (typeof a === "number" && typeof b === "number") {
      return a + b;
  } else if (typeof a === "string" && typeof b === "string") {
      return a.concat(b);
  }
  throw new Error("Invalid arguments");
}
```

**03 调用重载函数**

调用重载函数时，TS 会依据传递的参数类型选择合适的重载签名

```javascript
// 推断出两个 number 类型
let result1 = add(10, 20); // 使用第一个重载，返回 number
// 推断出两个 string 类型
let result2 = add("Hello, ", "world!"); // 使用第二个重载，返回 string
```

**04 参数个数的重载**

重载不仅可以依据参数的类型不同，也可以基于参数个数的不同

```javascript
// 下面代码中定义了三个重载签名，他们分别接收不同的参数个数
// 将来再实现重载的时候可以通过参数个数来区别
function greet(name: string): string;
function greet(name: string, greeting: string): string;
function greet(name: string, greeting?: string): string {
  return `${greeting || "Hello"}, ${name}`;
}
```

**05 函数重载的应用**

有了函数重载的机制之后，同一个函数名可以有多个不同的实现，每个实现依据不同的参数类型或参数个数执行不同的操作。这在创建具有多种用途的函数时特别有用，尤其是在类型检查和自动补全等方面有额外的好处。

假设我们有一个 `formatDate` 函数，它可以接受不同类型的参数：一个 `Date` 对象，或 年、月、日这三个单独的数字。

```javascript
// 01 定义函数重载签名，接收不同的类型 与参数个数
function formatDate(date: Date): string;
function formatDate(year: number, month: number, day: number): string;

// 02 实现重载签名：重载函数
// 实现时一定要兼容所有的重载签名，特别是类型
// 如果参数个数不同，只是类型不同，那么使用联合类型即可解决兼容问题
// 如果参数个数和类型都不相同，则需要使用联合类型 与 可选参数来解决兼容问题
function formatDate(dateOrYear: Date | number, month?: number, day?: number): string {
  // 通过类型守卫来明确具体的类型
  if (dateOrYear instanceof Date) {
    return dateOrYear.toISOString().substring(0, 10);
  } else {
    return `${dateOrYear}-${month!}-${day!}`;
  }
}

// 03 使用重载函数
const date1 = formatDate(new Date()); // 传入 Date 对象
const date2 = formatDate(2023, 4, 5); // 传入年、月、日

console.log(date1); // 输出类似 "2023-04-05"
console.log(date2); // 输出 "2023-4-5"
```

**06 泛型与函数重载**

有些时候使用泛型来代替函数重载可以让代码更简洁，同时保留了灵活性和类型安全性。

假设我们有一个函数 `wrapInArray`，它的功能就是将一个单一的元素或者多个元素包装成数组。

```javascript
// 函数重载版本
function wrapInArray(element: number): number[];
function wrapInArray(element: string): string[];
function wrapInArray(element: any): any[] {
  return [element];
}
```

在泛型版本中，我们定义一个泛型函数 `wrapInArray<T>`，其中`T`是一个类型变量。这个函数接受一个类型为 `T` 的参数。并返回一个类型为 `T[]` 的数组。之后我们就可以用不同的类型调用这个泛型函数

```javascript
// 泛型版本
function wrapInArray<T>(element: T): T[] {
  return [element];
}
```

在这些调用中， TS 依据传入的参数类型自动推断出泛型类型 `T` 的具体类型。

```javascript
let numberArray = wrapInArray(5); // 类型为 number[]
let stringArray = wrapInArray("hello"); // 类型为 string[]
let booleanArray = wrapInArray(true); // 类型为 boolean[]
```

## 函数中的 `this`

在 TS 中，正确处理函数中的 `this` 关键字是非常重要的，尤其是在类方法和回调函数中。 `this` 的行为在 TS 中与 JavaScript 类似，但 TS 提供了额外的类型检查和注解功能。

**01 类和对象方法中的 this**

在类的方法中，`this` 自动指向类的实例

```javascript
class MyClass {
  // 下面的 this 自动指向类的实例
  myMethod() {
    console.log(this); // 'this' 指向 MyClass 的一个实例
  }
}
```

**02 箭头函数中的 this**

箭头函数不绑定 `this`， 它们捕获定义时所在的上下文的 `this` 值

```javascript
  class MyClass{
    myProperty = 'value'

    myMethod = () => {
      // this 捕获 MyClass 实例
      console.log(this.myProperty)
    }
  }
```

**03 this 参数**

TS 允许我们在函数定义时的参数列表中显式声明 `this` 类型，以指定函数预期的 `this` 类型。

```javascript
// 这个 this 并不会被当做真的参数在调用时被传递，专门用来指定 this 的类型
function myFunction(this: MyClass, param: number) {
  console.log(this.myProperty); // 'this' 被指定为 MyClass 类型
}
```

**04 回调函数中的 this**

在回调函数中，`this` 的值可能会丢失上下文。为此，我们可以使用箭头函数或 `bind` 方法来绑定 `this`

```javascript
class MyClass{
  registerCallback(callback: ()=> void){
    callback()
  }
  myMethod(){
    this.registerCallback(() => {
      // 使用箭头函数来保持 「this」上下文
      console.log(this)
    })
  }
}
```

**05 this 类型守卫**

TS 允许在方法中使用 `this` 作为类型守卫

```javascript
class MyClass {
  isReady: boolean = false;

  // 允许在参数列表中的第一个位置给 this 指定类型
  run(this: MyClass) {
    // 守卫就可以理解为在进入xxx逻辑之前，检查一下类型
    if (this.isReady) {
        // ...
    }
  }
}
```

**06 this 和 重载**

TS 不允许在重载签名中改变 this 的类型，`this`类型必须在实现签名中统一声明

```javascript
  class MyClass{
    doSomething(this: MyClass, arg:number):void
    doSomething(this: MyClass, arg:string):void

    // 重载函数、重载实现
    doSomething(arg: number|string):void{
      // 具体的逻辑
    }
  }
```

**07 使用 `bind` `call` `apply`**

我们可以使用 `bind` `call` `apply`方法来显式地设置函数的 `this`上下文

```javascript
function myFunction() {
  console.log(this);
}

const myContext = { value: "A" };

// 返回一个绑定 this 之后的函数
const boundFunction = myFunction.bind(myContext);

boundFunction(); // 'this' 现在指向 myContext
```


