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

## 数据类型

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
在 TypeScript 中，有多种定义数组类型的语法，具体选择取决于我们的需求和使用场景。

**01 基本数组类型**

```javascript
// 类型名称[] 的语法来表示这是一个什么样类型的数组
// 使用元素类型后面跟上 [] 表示数组
let numbers: number[] = [1, 2, 3];  // 只能存放number 的
let strings: string[] = ["a", "b", "c"];
let mixedArray: (number | string)[] = [1, "two", 3, "four"]; // 可以存放 number 和 string 类型的数组
```

**02 泛型数组**

使用泛型的方式来定义数组，可以更灵活地指定数组元素的类型，但是如果当前你觉得生理排斥，只需要知道有这个操作即可。暂时无须焦虑

```javascript
// 使用 Array<ElementType> 泛型语法
// 实际开发中这种定义方式可能不太常见，因为和 JSX 语法有些类似，容易混淆
// 但是泛型在 TS 开发中是必然会使用的
let numbers: Array<number> = [1, 2, 3];
let strings: Array<string> = ["a", "b", "c"];
let mixedArray: Array<number | string> = [1, "two", 3, "four"];
```

**03 readonly 创建只读数组**

```javascript
// 可以将 ReadonlyArray 理解为是一个内部函数，调用该函数就会生成一个数组类型，而这个类型中的值都是只读的
// 使用 readonly 创建只读数组
let readonlyNumbers: ReadonlyArray<number> = [1, 2, 3];
let readonlyStrings: ReadonlyArray<string> = ["a", "b", "c"];
```

**04 `as const` 创建只读数组**

这个语法类型断言中会再次提及，目前放在这里是因为它属于 Array 操作的一部分。其实也好理解，就相当于我们在 JS 中使用 const 定义了一个变量，存放的是基本数据类型。原则上来讲肯定是不会再允许修改它的值。

```javascript
// 使用 as const 创建只读数组
let readonlyNumbers = [1, 2, 3] as const;
let readonlyStrings = ["a", "b", "c"] as const;
```

**05 类数组**

有时候，我们会遇到类数组，例如函数中的 `arguments` 对象，我们可以使用下面的语法来描述

```javascript
// arrayLike 的类型是 { length: number; [index: number]: string }
// 这就相当于约束了 arrayLike 要有 length 属性，且是一个 number 类型
// 还有一些不确定的 key 我们用 index 来表示，同时限定了 index 的类型是 number。而它对应的值是 string
// 如果当前你也觉得无聊同样可以忘记它，在后面的接口中你还会再次看到类似的解释
function toArray(arrayLike: { length: number; [index: number]: string }): string[] {
  return Array.from(arrayLike);
}
```

**06 类型推断**

TS 可以根据数组的初始值自动推断出数组的类型

```javascript
// TypeScript 可以根据初始值推断数组的类型
// 我们并没有显式的注解 inferredArray 的类型，只是给它赋了值
let inferredArray = [1, 2, 3]; // 推断为 number[]
```

**07 数组的方法**

如果我们使用了数组方法，例如 map、filter、forEach等，TS 会依据方法的返回值类型来推断新数组的类型

这在 TS 中其实是一个很常见和常用的现象，回调函数的参数类型推断，所以希望你能在生理上接受它

```javascript
// 这里是通过显式注解的方式设置了 numbers 的类型
let numbers: number[] = [1, 2, 3];

// 然后通过 numbers 来调用 map 方法，整个过程我们并不有显式的设置任何类型
// 但是 map 接收一个回调函数，我们称之为 cb ，这个 cb 接收一个参数 num ，然后直接返回了 num * num
// 在这个过程中，TS 会自动推断出  num 就是一个 number 类型，从而 squaredNumbers 就是一个 number[]
// 这种推断的好处就是将来我们调用 map 的方法可能不是一个 number[]类型，可以是任意的类型，最终的结果都会推断
let squaredNumbers = numbers.map(num => num * num); // 推断为 number[]
```

**08 Tuple 数组**

Tuple 就是一个固定长度和类型的数组，相对于数组来说每个元素的类型都可以不同

```javascript
let tuple: [number, string, boolean] = [1, "two", true];
```

## Tuple 类型

在 TypeScript 中，元组是一种有序且固定长度的数组类型。元组中的每个元素都可以有一个特定的类型。

**01 定义元组**

```javascript
// 这种方式我们在介绍 TS 数据类型的时候就已经介绍过
// 要求 [] 中有三个元素，且每个位置上的元素都限制了类型
// 定义一个包含 number、string 和 boolean 类型的元组
let myTuple: [number, string, boolean] = [1, "hello", true];
```

**02 元组访问**

```javascript
// 访问元组元素，直接通过下标来获取就可以
let firstElement: number = myTuple[0]; // 1
let secondElement: string = myTuple[1]; // "hello"
let thirdElement: boolean = myTuple[2]; // true
```

**03 越界元素**

元组允许在索引范围外添加元素，但是这些越界元素的类型会被限制为元组中定义的联合类型

```javascript
// 合法，但越界元素会被限制为 number | string | boolean 类型
// 意就是讲：我们想要添加的元素只能是 number string boolean 中的某一种
myTuple.push(4); 
let fourthElement: number | string | boolean = myTuple[3]; // number | string | boolean
```

**04 赋值和初始化**

其实就是类型推断，我们可以通过赋值或初始化的方式创建元组

```javascript
// 赋值方式，利用类型推断来获取元组中元素的类型
myTuple = [2, "world", false];

// 初始化方式：通过显式注解来说明元组中元素的类型
let anotherTuple: [number, string] = [42, "typescript"];
```

**05 解构元组**

可以使用解构语法来提取元组中的元素，其实它就是一个长度和类型固定的数组。因此这个操作和数组是一样的

```javascript
let [num, str, bool] = myTuple;
console.log(num, str, bool); // 2, "world", false
```

**06 元组中的可选元素**

在元组中可以包含可选元素，类似于函数中的可选参数，接口中的可选属性等，道理都是相通的

```javascript
// 下面的语法就设置了元组中的第二个元素是 string 类型，且它是可选的
let optionalTuple: [number, string?] = [1]; // 合法，第二个元素是可选的
```

**07 元组中的剩余元素**

使用剩余元素语法，将剩余的元素放入一个数组

```javascript
// 明确的限制了元组中前两个位置的类型，后面所有的通过剩余运算符放在一个数组中
let restTuple: [number, string, ...boolean[]] = [1, "hello", true, false, true];
```

## Object 类型
> 当前只是对该类型做一些介绍和说明，更多的细节会在后面详细展开

在 TypeScript 中，对象类型是一种表示具有特定属性和属性类型的数据结构的方式

**01 基础语法**

```javascript
// 定义一个包含特定属性的对象类型
// 约不了 myObj 包含key1 key2 key3 三个特定的属性，且这三个属性都有自己的类型
let myObj: { key1: number; key2: string; key3: boolean } = {
  key1: 42,
  key2: "hello",
  key3: true,
};
```

**02 可选属性**

通过在属性名后面添加 `?`，可以将当前属性定义为可选的，这个在元组中我们也提到过。

```javascript
// 可选属性
let optionalObj: { key1: number; key2?: string } = { key1: 42 };
```

**03 只读属性**

通过在属性名前面添加 `readonly` ，可以将该属性设置为只读的

```javascript
// 只读属性
// 此时 key1 属性就是只读的，我们无法再去能它的值进行修改
let readOnlyObj: { readonly key1: number; key2: string } = { key1: 42, key2: "hello" };
// readOnlyObj.key1 = 100; // Error: Cannot assign to 'key1' because it is a read-only property.
```

**04 索引签名**

使用索引签名可以定义具有动态属性的对象类型，例如后端接口给你返回了一个 json，但是你常用的就几个属性，还有很多 key 其实你不在意。这个时候你又不想使和 any 类型。此时就可以考虑使用索引签名来约束

```javascript
// 索引签名
// indexedObj 有多个 key ，这些 key 的类型是string，他们的值类型是 number
let indexedObj: { [key: string]: number } = {
  key1: 42,
  key2: 100,
};
```

**05 接口**

接口的在 TS 中也是一个非常重要的存在，后面会单独说明，这是只是因为它的结构和对象一样，使用接口可以更清晰的定义对象类型，并且可以重复使用这些类型的定义

```javascript
// 使用接口定义对象类型
// 定义接口的语法就是 interface 接口名 {}
interface MyObject {
  key1: number;
  key2: string;
  key3: boolean;
}

// 使用 MyObject 这个接口的格式来约束 obj 的类型
let obj: MyObject = {
  key1: 42,
  key2: "hello",
  key3: true,
};
```

**06 类型别名**

使用类型别名可以创建复杂的对象类型，并提高代码的可读性。关于类型别名的语法我们会在后面单独说明。这里只是想要表达，在对象的使用过程中可以结合类型别名

```javascript
// 类型别名
type Point = { x: number; y: number };
let point: Point = { x: 10, y: 20 };
```

## Symbol 类型

在 TypeScript 中，`Symbol` 类型表示唯一的、不可为的值。`Symbol` 是 ECMAScript6 引入的一种新的原始数据类型，用于创建唯一标识符。

**01 基本语法**

```javascript
// 创建唯一的 Symbol，类型就是 symbol
let mySymbol: symbol = Symbol("mySymbol");

// 将创建的 Symbol 作为对象属性的键
let obj = {
  [mySymbol]: "Hello, Symbol!",
};

// 访问 Symbol 属性
console.log(obj[mySymbol]); // "Hello, Symbol!"
```

**02 唯一性**

每个通过 `Symbol` 函数创建的 `Symbol` 都是唯一的，即使传递相同的描述字符串也不会相等。这倒不是 TS 做了什么，而是 symbol 类型本身就具有这样的特性

```javascript
// 创建 symbol 对象时，刻意传入了两个相同的字符串值
let symbol1 = Symbol("mySymbol");
let symbol2 = Symbol("mySymbol");

// 通过比较发现两个 symbol 并不相同
// 这也就是它的唯一性
console.log(symbol1 === symbol2); // false
```

**03 Symbol做为对象键**

```javascript
// 和基本语法一致，创建一个唯一的 symbol 对象
const sym = Symbol();

// 定义一个对象，使用唯一的 sym 来做为 key
let obj = {
  [sym]: "value",
};

// 通过唯一的 key 来访问对象
console.log(obj[sym]); // "value"
```

**04 keyof和字符串字面量**

这个操作其实是想提前说明 `keyof`，这在类型体操里还是很常用的，可以将一个对象的 key 都提取出来组成一个可遍历的对象。结合 keyof 可以方便的创建具有唯一 Symbol 键的对象。

```javascript
const sym1 = Symbol("sym1");
const sym2 = Symbol("sym2");

// 使用 keyof 将对象中的 sym1 sym2 提取出来
// 组成一个可遍历的对象
type MySymbolKeys = keyof {
  [sym1]: number;
  [sym2]: string;
};

// 使用 key in xxx 的操作可以遍历出所有的 key
// 再使用这些 key 来做为另外一个对象的 key 使用
let obj: { [K in MySymbolKeys]: any } = {
  [sym1]: 42,
  [sym2]: "hello",
};
```

## Null 类型

> 在 TypeScript 中，`null` 类型表示一个变量的值可以是 `null` 。

在 TypeScript 的类型系统中，有两种方式来表示 `null` 类型。一种是采用联合类型，另一种是设置 `strictNullChecks` 编译选项。

**01 使用联合类型**

```javascript
// 使用联合类型表示可能为 null 的值
// 下面的 nullableValue 的类型就是 `string|null`。表示可能是字符串类型或 `null`
let nullableValue: string | null = "Hello";
nullableValue = null; // 合法
```

**02 strictNullChecks编译选项**

在 TS 中，可以通过启用 `strictNullChecks` 编译选项来更加严格地检查 `null` 类型。这个需要操作 `tsconfig.json` 文件，当前我们可能还未涉及，不要焦虑。先有个印象

首先，确保 `tsconfig.json` 文件中的 `"strictNullChecks"` 被设置为了 true

```json
{
  "compilerOptions": {
    "strictNullChecks": true
  }
}
```

此后我们就可以直接将变量声明为特定类型，而不是联合类型。但是在启用了 `strictNullChecks` 选项的情况下，如果我们将某个值设定为具体的类型之后一，再将 `null` 赋值给它，就会导致编译报错

这样的严格检查可以帮助我们捕获潜在的空值错误，提高代码的健壮性。在实际开发中，我们推荐启用 `strictNullChecks`选项。

```javascript
// 使用 strictNullChecks 检查 null 类型
let nonNullableValue: string = "Hello";
// nonNullableValue = null; // Error: 不能将类型 "null" 分配给类型 "string"
```

## Undefined 类型

> undefined 类型的操作和 null 其实非常类似。

在 TypeScript 中，`undefined` 类型表示一个变量的值可以是 `undefined` 。 和 `null` 类型类似，`undefined` 可以通过联合类型或者启用 `strictNullChecks` 编译选项来表示

**01 使用联合类型**

```javascript
// 使用联合类型表示可能为 undefined 的值
// 有些时候函数参数如果没有默认值也会推断出 undefined 类型
let undefinedValue: number | undefined = 42;
undefinedValue = undefined; // 合法
```

**02 strictNullChecks选项**

在实际开发中，我们同样推荐开启  `strictNullChecks` 编译选项来减少潜在空值和未定义值的错误

```json
{
  "compilerOptions": {
    "strictNullChecks": true
  }
}
```

```javascript
// 使用 strictNullChecks 检查 undefined 类型
let nonUndefinedValue: string = "Hello";
// nonUndefinedValue = undefined; // Error: 不能将类型 "undefined" 分配给类型 "string"
```

## Any 类型

> 你可能会感觉它很好用，但是我们推荐尽量少用

在 TypeScript 中，`any` 类型表示一个变量的类型可以是任意值，其实就是取消了静态类型检查。使用 `any` 类型可以使代码更灵活，但同时也失去了 TypeScript 提供的类型检查优势。所以在实际开发中，我们应尽量避免滥用 `any` 类型。通过使用更具体的类型来提高代码的健壮性。

**01 基本语法**

```javascript
// 使用 any 表示变量的类型可以是任意值
// 然后我们可以在程序中的任意位置，任何时刻去修改变量的类型和值
let myVariable: any = "Hello";
myVariable = 42; // 合法
myVariable = { key: "value" }; // 合法
```

**02 在数组中使用 any**

```javascript
// 使用 any 表示数组中的元素可以是任意类型
// 此后 myAraay 中可以放置任意类型的元素，这就违背了数组应该是固定类型集合这一说法
let myArray: any[] = [1, "two", true];
```

**03 在函数中使用 any**

```javascript
// 使用 any 表示函数参数和返回值可以是任意类型
// 此后函数可以接受任意类型的参数，返回任意类型的结果，这的确是一个存在的通用需求
// 除了 any ，我们还可以通过重载和泛型来实现
function dynamicFunction(arg: any): any {
  return arg;
}
```

**04 在对象中使用 any**

```javascript
// 使用 any 表示对象中的属性值可以是任意类型
let myObject: { key: any } = { key: "value" };
```

**05 显式标注和隐式推断**

尽管 `any` 提供了灵活性，但在大多数情况下，最后避免使用 `any` 类型，因为它破坏了 TS 的类型检查。语法上来说 any 做为一种类型，同样可以显式标注或隐式推断

```javascript
// 显式注解
let variable1: any = "Hello";

// 隐式推断：如果我们没有标注类型，可能会推断为具体的类型或 any 类型
let variable2 = "World";
```

## Void 类型

在 TypeScript 中，`void` 类型表示函数没有返回值，它通常用于标记函数的返回类型，以表明该函数不返回任何值。

**01 基本语法**

```javascript
// 使用 void 表示函数没有返回值
// 下面的函数没有显式的 reutrn 语句，因此返回值类型被推断为 void
function logMessage(message: string): void {
  console.log(message);
}
```

**02 变量类型为 void**

这种行为并不多见，但从语法上来讲是通过的。

```javascript
// 将变量的类型声明为 void
// 可以将 undefined 分配给 void 类型，这也从一方面证明了，如果一个函数没有写 return 或者写了 return undefined 时。它的返回值类型还可以是 void
let myVariable: void;
myVariable = undefined; // 合法
// myVariable = 42; // Error: 不能将类型 "number" 分配给类型 "void"
```

**03 函数没有返回值**

当函数确实没有返回值时，可以将其返回类型标注为 `void`

```javascript
// 函数没有返回值
function showMessage(): void {
  console.log("Hello, World!");
}
```

**04 函数返回undefined**

虽然 `void` 表示函数没有返回值，但在实际情况中，函数如果确实不显式返回任何值。其返回值也就是 `undefined`

```javascript
// 函数返回 undefined
function doSomething(): undefined {
  // 没有显式的 return 语句
}
```

**05 undefined 与 void**

`undefined` 类型和 `void` 类型有些相似，但它们并不完全等同。 `void` 主要用于函数的返回类型，而 `undefined` 用于标识可能存在的 `undefined` 值

```javascript
let myVariable: void;
let myUndefined: undefined;

myVariable = undefined; // 合法
// myUndefined = null; // Error: 不能将类型 "null" 分配给类型 "undefined"
```

## Never 类型

在 TypeScript 中，`never` 类型表示函数永远不会返回值，或者变量永远不会拥有特定的值。`never`类型通常会用于表示抛出异常或无限循环等永远不结束的操作。

**01 函数永远不会返回值**

在下面的代码中， `throwError` 函数抛出一个错误，导致函数永远不会返回值，因此它的返回值类型就是 `never`。同样 `infiiteLoop` 函数进入无限循环，也永远不会返回。所以它的返回值类型也是 `never`。

```javascript
// 函数永远不会返回值
function throwError(message: string): never {
  throw new Error(message);
}

// 推断为 never 类型
function infiniteLoop(): never {
  while (true) {
    console.log("Looping...");
  }
}
```

**02 switch语句中的穷举检查**

`never` 类型通常用于确保 switch 语句中的 `穷举检查`，也就是用来确保覆盖了所有可能的情况。

在下面的代码中，通过 `default` 分支，可以确保对所有可能的 `Direction` 类型进行了处理。如果新增了一个方向，TS 就会报错。因为 `exhaustiveCheck` 的类型被推断为 `never`，表示该分支永远不会执行。

```javascript
type Direction = "up" | "down" | "left" | "right";

function move(direction: Direction): void {
  switch (direction) {
    case "up":
      // move up
      break;
    case "down":
      // move down
      break;
    case "left":
      // move left
      break;
    case "right":
      // move right
      break;
    default:
      // 如果有新的方向添加，TypeScript 会报错
      // 直白的说就是代码如果走到了这里，就说明我们遗漏了什么。
      const exhaustiveCheck: never = direction;
  }
}
```

**03 类型断言中的 never**

在某些情况下，我们可以使用类型断言将一值标记为 `never` 类型

下面的代码中，`myVariable` 被标记为了 `never` 类型，因为它被赋值为一个永远不会返回值的函数调用。

总体而言， `never` 类型用于表示永远不会发生的操作或函数。如果发生了就说明有些地方出现的意外，我们需要检查它所在的代码。

```javascript
let myVariable: never;
myVariable = throwError("This is an error"); // 合法
```

## Unknow 类型

在 TypeScript 中，`unknow` 类型表示一个值的类型是未知的，与 `any` 类型不同， `unknow` 类型更加安全，因为在使用 `unknow` 类型的值时，必须先进行类型检查或类型断言。

**01 基本语法**

```javascript
// 使用 unknown 表示值的类型是未知的
// 你可能会疑问，我们为什么可以将 number 赋值给 unknow 类型，这就是之前提到的，它表示未知的类型，在这个层面上它和 any 的表现是一样的。不同点在于对具体值的使用上
let myVariable: unknown = 42;
```

**02 类型检查**

使用 `typeof` 或其它的类型检查操作符来确定 `unknow` 类型的值的具体类型，这个操作你可能也称之为叫做 `类型缩小`。核心的作用就是将 unknow 具体为某个类型后再做操作，这样就会获得更加安全的类型，使用类型检测功能。

```javascript
if (typeof myVariable === "number") {
  let myNumber: number = myVariable; // 合法
} else {
  // 在这个分支中，TypeScript 知道 myVariable 不是 number 类型
}
```

## 枚举类型

枚举（Enum）是一种特殊的类型，它为一组相关的值定义了一个友好的名称。枚举在编写清晰且易于理解的代码时非常有用。

**01 数字枚举**

数字枚举在 TS 中是最常见的枚举类型，其成员会被赋予从 0 开始的递增数字

```javascript
// 在下面的代码中 Direction.Up 的值为 0, Direction.Down 为1
enum Direction {
  Up,
  Down,
  Left,
  Right
}

let dir: Direction = Direction.Up;
```

**02 字符串枚举**

字符串枚举要求每个成员的值必须是字符串字面量，或者是另一个字符串枚举成员的引用。

```javascript
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT"
}

let dir: Direction = Direction.Left;
```

**03 异构枚举**

虽然不常用，但是 TS 支持将数字和字符串混合在一个枚举中

```javascript
enum BooleanLikeEnum {
  No = 0,
  Yes = "YES"
}
```

**04 计算的和常量成员**

枚举成员可以是常量（在编译时计算）或计算出的（在运行时计算）

```javascript
enum FileAccess {
  // 常量成员
  None,
  Read    = 1 << 1,
  Write   = 1 << 2,
  ReadWrite  = Read | Write,
  // 计算的成员
  G = "123".length
}
```

**05 反向映射**

数字枚举具有反向映射，即从枚举值到枚举名字的映射

```javascript
enum Enum {
  A
}
let a = Enum.A;
let nameOfA = Enum[a]; // "A"
```

**05 const 枚举**

为了避免额外的生成代码和额外的间接引用，可以直接使用 `const` 枚举

`const` 枚举只能使用常量枚举表达式，并且在编译时会被删除

```javascript
const enum Enum {
  A = 1,
  B = A * 2
}
```

**06 枚举类型**

枚举类型可以用于类型安全场景，确保只能使用枚举的值

```javascript
// 使用 Direction 类型来约束 direction 参数的类型
function walk(direction: Direction) {
    // ...
}

walk(Direction.North); // 正确
// walk('North'); // 错误：不能将类型“"North"”分配给类型“Direction”。
```
