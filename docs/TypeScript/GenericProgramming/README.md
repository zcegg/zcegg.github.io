## 需要泛型

TS 中的泛型是一种强大的工具，允许我们在定义函数、类或接口时不预先指定具体的类型，而是在使用时再指定类型。

**01 类型安全**

- 可重用代码：使用泛型我们可以创建多种类型的组件（如函数、类或接口），而不需要重复编写代码。
- 类型检查：使用泛型时，TS 能够进行更准确的类型检查，这样就可以在编译时捕获更多的错误，减少运行时的问题。
- 类型推断：TS 能够根据传递给泛型的实际类型自动推断出具体的类型，这使得代码更加简洁和易于维护。


**02 灵活性和表达式**
- 泛型约束：我们可以通过泛型约束限制泛型可以使用的类型，这提供了额外的灵活性，允许我们指定泛型必须遵循的某些特性或接口
- 复杂数据类型处理：泛型对于处理复杂的数据结构（树、图、键表）非常有用，因为它们通常需要能够处理多种类型的数据

**03 更好的组织代码**

- 清晰的意图：使用泛型可以更清楚地表达我们的代码意图，例如：一个函数接收任意类型的数组并返回该类型的元素，使用泛型就很好编写
- 避免类型断言：在没有泛型的情况下，我们可能需要不停的使用类型断言来处理多种类型的数据。泛型避免了这种需要，并提供了一种更优雅的方式来处理不同类型的数据。

```typescript
// 定义 identity 接收一个泛型参数 T，返回 T 
function identity<T>(arg: T): T {
    return arg;
}

// 这样调用时就可以做到，传入任意 T 类型，返回 T 类型。
let output1 = identity<string>("myString");  // 类型为 string
let output2 = identity<number>(100);         // 类型为 number
```

## 认识泛型

TS 中的泛型提供了一种方式来创建可重用的组件，同时保持了类型的完整性。它们允许我们定义函数、类或接口时延迟指定一个或多个类型，直到实际使用时才确定具体的类型。

**01 泛型函数**

定义一个泛型函数时，我们需要在函数名后面添加 `<T>` (或其它字母)，其中 `T` 是一个类型变量，代表用户将提供的类型

在下面代码中 `identity` 函数能够接受任何类型的参数，并返回相同的类型。在使用时，可以明确指定 `T` 的具体类型，如 `string` 和 `number`

```typescript
function identity<T>(arg: T): T {
  return arg;
}

let output1 = identity<string>("Hello");
let output2 = identity<number>(123);
```

**02 泛型接口**

泛型也可以用在接口上，允许在实现接口时指定具体的类型

在下面的代码中，`GenericIndentityFn` 是一个泛型接口，它定义了一个函数签名，`identity` 函数符合这个签名，并且在使用时指定了 `number` 类型。

```typescript
interface GenericIdentityFn<T> {
    (arg: T): T;
}

function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: GenericIdentityFn<number> = identity;
```

**03 泛型类**

泛型也可以用于类，但不能用于类的静态成员

```typescript
class GenericNumber<T> {
  zeroValue: T;
  add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };

```

**04 泛型约束**

我们可以定义一个接口来描述约束，并使用这个接口和 `extends` 关键字来实现泛型约束。

```typescript
interface Lengthwise {
  length: number;
}

// loggingIdentity 函数的泛型 T 被约束为必须拥有 length 属性
function loggingIdentity<T extends Lengthwise>(arg: T): T {
  console.log(arg.length);  // 现在我们知道它有一个 .length 属性
  return arg;
}
```

**05 多个类型变量**

我们可以定义多个类型变量来创建更复杂的泛型类型

```typescript
function merge<U, V>(obj1: U, obj2: V): U & V {
  return { ...obj1, ...obj2 };
}

let result = merge({ name: 'John' }, { age: 30 });
// result 的类型是 { name: string; age: number; }
```

## 泛型函数

TS 中的泛型函数允许在函数定义时提供一种方法来确保类型的一致性，同时保持函数的通用性和灵活性。

**01 定义泛型函数**

泛型函数使用一种或多种类型变量（例如`T`），这些类型变量作为函数参数类型和返回类型的一部分。

在下面的代码中 `T` 是一个类型变量，它捕获了由调用者提供的类型（例如 `string` `number` 等），然后被用作参数 `arg` 的类型和返回类型。

```typescript
  function identity<T>(arg: T): T{
    return arg
  }
```

**02 调用泛型函数**

调用泛型函数时，可以显式地指定类型参数，也可以让 TS 编译器自动推断类型

```typescript
// 显式指定类型
let outputExplicit = identity<string>("myString"); 

// 类型推断：自动推断出 T 是 string 类型
let outputInferred = identity("myString");         
```

**03 使用泛型约束**

我们可以使用 `extends` 关键字约束泛型。这意味着我们可以声明一个泛型类型必须符合某个接口或具有特定结构。

```typescript

// 定义一个接口 Lengthwise，约束对象结构有一个 length 属性
interface Lengthwise {
  length: number;
}

// 定义函数，设置泛型T，要求T 继承接口。此后我们传入的 T 必然包含了 length
function loggingIdentity<T extends Lengthwise>(arg: T): T {
  // 现在知道它有一个.length属性
  console.log(arg.length);  
  return arg;
}
```

**04 多个类型变量**

泛型函数可以有多个类型变量，这为创建复杂的函数提供了灵活性

```typescript
// 返回值是 T 与 U 的交叉类型
function merge<T, U>(arg1: T, arg2: U): T & U {
  return { ...arg1, ...arg2 };
}
```

**05 默认泛型类型**

我们可以为泛型类型提供默认类型，以便在不显式指定类型时使用

```typescript
// 定义类型 T 时，设置一个默认类型 string 
function createArray<T = string>(length: number, value: T): T[] {
  return new Array(length).fill(value);
}
```

**06 泛型类型参数推断**

在一个情况下，TS 可以依据传递给泛型函数的参数自动推断出类型变量的类型

```typescript
// 将来在我们调用 combine 方法的时候可以不用显式的传入固定的类型
// 只需要传入具体的 arr1 arr2 ，TS 会自动推断出类型变量 T 的类型
function combine<T>(arr1: T[], arr2: T[]):T[]{
  return arr1.concat(arr2)
}
```

**07 泛型函数类型**

我们可以定义泛型函数类型，用于声明函数的类型

```typescript
// 下面的语法就是之前提过的 函数调用签名
// 表示被该接口约束的变量是一个可调用的类型
interface GenericFunction<T>{
  (arg: T): T
}
```

## 泛型接口

泛型接口是一种强大的方式，用于定义具有泛型的接口。泛型接口可以使接口更加灵活，能够用于多种类型，同时保持类型安全。

**01 基本泛型接口**

泛型接口的定义与普通接口相似，但在接口名称后添加 `<T>`（或其它字母），其中 `T` 是类型变量。

```typescript
// 约束接口包含属性 value，使用类型变量 T 来约束 value 的类型
// 约束接口包含 getValue，它是一个函数类型，且返回值是 T 的类型
interface GenericInterface<T> {
  value: T;
  getValue: () => T;
}
```

**02 使用泛型接口**

实现泛型接口 时，需要指定泛型类型，或允许 TS 推断类型

```typescript
// 定义了一个 MyClass 类，它实现了 GenericInterface
// 显式的给泛型变量 T 赋值为 number 类型
// 实现接口中的约束，定义 value 与 getValue
class MyClass implements GenericInterface<number> {
  value = 10;
  getValue() {
      return this.value;
  }
}
let myInstance = new MyClass();
console.log(myInstance.getValue()); // 输出: 10
```

**03 泛型约束**

只要使用了泛型，泛型约束语法就需要考虑，不论是泛型用在哪里。因为我们往往需要限制类型变量的范围

```typescript
interface Lengthwise {
  length: number;
}

interface GenericInterface<T extends Lengthwise> {
  value: T;
  getValue: () => T;
}
```

**04 多类型变量**

泛型接口同样可以包含多个类型变量，使得接口能够处理多个相关联的泛型类型

```typescript
interface Pair<K, V> {
  key: K;
  value: V;
}
```

**05 泛型函数类型**

泛型接口还可以定义泛型函数的类型

```typescript
// 使用接口定义一个函数调用签名，使用类型变量 T
interface GenericFunction<T> {
  (arg: T): T;
}

// 使用接口来约束 myFunction 类型，给泛型变量 T 传入 number
// 此后 arg 就会被约束为 number 类型，返回值也是 number 类型
let myFunction: GenericFunction<number> = function(arg) {
  return arg;
};
```

**06 默认泛型类型**

这个在泛型函数中也见到了，就是要说明泛型本身具备的默认值语法

```typescript
interface GenericInterface<T = string> {
  value: T;
  getValue: () => T;
}
```

## 泛型类

泛型类型是一种使用泛型来创建可重用组件的方式，它们在定义类时允许指定一个或多个类型参数。然后在整个类的范围内使用这些类型。

**01 定义泛型类**

泛型类在类名后添加 `<T>`,其中 `T` 作为类型变量在整个类中使用

```typescript
// 在类名后设置一个类型变量 T ，在类的内部都可以直接使用
class GenericClass<T> {
  // 使用 T 约束类的实例属性类型
  value: T;

  // 使用 T 约束构造函数参数的属性类型
  constructor(value: T) {
    this.value = value;
  }

  // 使用 T 约束实例方法的返回值类型
  getValue(): T {
    return this.value;
  }
}
```

**02 实例化泛型类**

创建泛型类的实例时，可以显式的指定类型，也可以使和 TS 的类型推断

```typescript
let myInstance = new GenericClass<string>("Hello");
console.log(myInstance.getValue()); // 输出: "Hello"
```

**03 泛型约束**

可以对泛型类型使用约束，用来限制可以用作类型参数的类型

```typescript
interface Lengthwise {
  length: number;
}

// 使用 extends 来说明类型变量 T 需要满足接口的约束
// 在类的内部可以任意使用类型变量 T 
class GenericClass<T extends Lengthwise> {
  value: T;

  constructor(value: T) {
    this.value = value;
  }

  getValueLength(): number {
    return this.value.length;
  }
}
```

**04 使用多个泛型类型参数**

泛型类可以定义多个类型参数，提供更大的灵活性和表达能力

```typescript
// 可以在类名的后面跟上一个或者任意多个类型变量
// 然后在类的内部去使用这些类型变量
class KeyValuePair<K, V> {
  key: K;
  value: V;

  constructor(key: K, value: V) {
    this.key = key;
    this.value = value;
  }
}
```

**05 泛型和静态成员**

在泛型类中，静态成员不能使用类的类型参数

```typescript
class GenericClass<T> {
  static staticValue: T; // 错误：静态成员不能引用类类型参数
}
```

**06 默认泛型类型**

泛型类型也可以为类型参数提供默认类型

```typescript
// 默认类型，是泛型本身的语法，无论是用在类、接口、还是函数或者其它地方
class GenericClass<T = string> {
  value: T;

  constructor(value: T) {
      this.value = value;
  }
}
```

## 泛型约束

泛型约束允许我们指定一个泛型类型必须满足的条件。这是通过使用 `extends` 关键字来实现的，它不仅提高了泛型的灵活性，也增强了代码的安全性。

**01 基本泛型约束**

泛型约束通过在类型变量后添加 `extends` 关键字和一个类型来定义。这个类型可以是接口、类或者复合类型。

```typescript
// 定义接口类型
interface Lengthwise {
  length: number;
}

// 在类型变量 T 的后面使用 extends 来约束 T 
// 要求 T 必须继承自 Lengthwise 这个接口
function loggingIdentity<T extends Lengthwise>(arg: T): T {
  console.log(arg.length);  // 现在知道它具有.length属性
  return arg;
}
```

**02 使用接口约束**

我们可以创建一个接口来描述约束，然后使用这个接口来约束泛型类型

```typescript
interface HasId {
    id: number;
}

// 和基本的泛型约束语法一致，类型变量 T 继承自 HasId 这个接口
function findById<T extends HasId>(items: T[], id: number): T | undefined {
  return items.find(item => item.id === id);
}
```

**03 使用类作为约束**

除了接口，我们也可以使用类作为泛型约束，确何泛型类型至少具有该类的结构

```typescript
// 定义一个类来约束类型变量 T
class Animal {
  name: string;
}

// 同样是通过 extends 关键字来实现
// new() => T 是构造签名，表示该类型可以被 new 
// 所以调用 createInstance 时给 T 赋值具体类型必须能 new 还需要满足 Animal 的结构
function createInstance<T extends Animal>(c: new () => T): T {
  return new c();
}
```

**04 多重约束**

使用交叉类型，可以对泛型应用多重约束

```typescript
interface Nameable {
  name: string;
}

interface Ageable {
  age: number;
}

// 此时 泛型变量 T 要满足的就是 Nameable 与 Ageable 的交叉类型
// 既要包含 name 也要包含 age 属性
function greet<T extends Nameable & Ageable>(person: T): void {
  // 走到这里就明确的知道了 person 必然具有 name 与 age 属性
  console.log(`Hello, ${person.name}, you are ${person.age} years old.`);
}
```

**05 在泛型约束中使用类型变量**

在下面的代码中，首先使用了多个类型变量 T K ，同时我们尝试对 K 进行约束。约束的类型 `keyof T`, keyof 是一个关键字可以将泛型变量 T 中的所有 key 都提取出来做为一个 `{}` 。之后我们用它做为一个类型来约束 K。这样的话就实现了 K 都来自于 T 中 key 的约束。主要是接受泛型约束中可以使用类型变量这个语法。

```typescript
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}
```

**06 默认泛型类型和约束**

在定义约束的同时，也可以为泛型变量提供默认类型

```typescript
// 定义了一个泛型变量 T，给它设置了默认类型 string
// 同时设置了泛型变量 K ，限制了它的类型为 number ,同时赋默认值为 1
function createArray<T = string, K extends number = 1>(length: K, value: T): T[] {
    return new Array(length).fill(value);
}
```

**07 约束中的条件类型**

```typescript
// 定义一个类型 ExtractArray，接收一个泛型变量 T 
// 判断 T 是否继承Array，如果是则推断出类型U 返回，否则 返回 T
type ExtractArray<T> = T extends Array<infer U> ? U : T;

function firstOrValue<T>(arr: T): ExtractArray<T> {
    if (Array.isArray(arr)) {
        return arr[0];
    }
    return arr;
}
```

## 多类型变量

泛型可以有多个类型变量，允许我们创建更灵活和强大的泛型结构。这种功能在设计能同时处理多种类型数据结构的函数、类、或者接口时特别有用。

**01 多类型变量泛型语法**

我们可以在泛型中声明多个类型变量，它们通常用逗号分隔

```typescript
function pair<K, V>(key: K, value: V): [K, V] {
  return [key, value];
}
```

**02 多类型变量的约束**

就像单个泛型类型变量一样，多类型变量也可以有约束

```typescript
// 本质上就是多个参数，每个参数都可以有自己的约束规则
// 在A的泛型约束中，也可以使用到B泛型变量
function merge<T extends object, U extends object>(obj1: T, obj2: U): T & U {
  return { ...obj1, ...obj2 };
}
```

**03 函数中的多类型变量**

多类型变量泛型在函数中的使用提供了额外的灵活性

在下面的代码中， `mapObject` 函数接受一个对象和一个转换函数，然后对对象的每个属性应用这个函数，最后返回一个新对象。

```typescript
function mapObject<K extends string | number, V, U>(obj: Record<K, V>, func: (item: V) => U): Record<K, U> {
  // 使用类型将初始化的 {} 断言为我们希望的类型
  // Record 是一个内置的类型工具，可以返回指定类型
  let result: Record<K, U> = {} as Record<K, U>;
  for (const key in obj) {
    result[key] = func(obj[key]);
  }
  return result;
}
```

**04 类中的多类型变量**

```typescript
class DataStore<K, V> {
  private data: Map<K, V> = new Map();

  setItem(key: K, value: V): void {
      this.data.set(key, value);
  }

  getItem(key: K): V | undefined {
      return this.data.get(key);
  }
}
```

**05 接口中的多类型为量**

```typescript
interface Pair<K, V> {
  key: K;
  value: V;
}

let item: Pair<number, string> = { key: 1, value: "value" };
```

## 映射类型

映射类型是一种高级类型，允许我们依据一个已有的类型创建一个新的类型，其中每个属性都被转换成新的形式。这是一种强大的类型转换机制，用于从现有模型生成新的模型。

**01 基本映射类型**

映射类型基于一个旧的类型，按照给定的规则创建一个新类型，它通常使用 `in` 关键字来遍历旧类型中的每个属性，并对其应用一个转换。

```typescript

// 使用 type 类型定义一个类型 Readonly ，它接收一个类型变量 T
// 核心功能就是接收一个 T ，然后返回一个新的类型，所有新类型的 key 都只读
type Readonly<T> = {
  // 使用修饰符来限制所有的属性
  // 使用 in 操作符来遍历 T 的所有属性
  // 从 T 中取出 P 对应的值做为新类型属性的值
  readonly [P in keyof T]: T[P];
};

// 接收一个类型 T ，返回一个新的类型
// 新类型所有的 key 都可选
type Optional<T> = {
  [P in keyof T]?: T[P];
};

interface Person {
  name: string;
  age: number;
}

type ReadonlyPerson = Readonly<Person>;
type OptionalPerson = Optional<Person>;
```

**02 使用预定义的映射类型**

TS 提供了一些内置的映射类型， 如 `Partial<T>` `Readonly<T>` `Record<T>` 等。

```typescript
type PartialPerson = Partial<Person>; // 所有属性变为可选
type ReadonlyPerson = Readonly<Person>; // 所有属性变为只读
```

**03 条件映射类型**

可以结合条件类型来创建更复杂的映射类型，以便依据属性的类型应用不同的转换

在下面的代码中，`NullableProperties` 将一个类型的所有属性转换为原类型或 null

```typescript

interface Person {
  name: string;
  age: number;
}

type NullableProperties<T> = {
  [P in keyof T]: T[P] | null;
};

type NullablePerson = NullableProperties<Person>;
```

**05 映射类型中的属性限制**

我们可以使用类型约束来限制映射类型中的 `in` 关键字遍历的属性

下面的代码中， `StringProperties` 只将 `Person` 类型中的字符串属性保留为字符串类型，其它的类型的属性都变为 `never`类型。
```typescript
type StringProperties<T> = {
  //  T[P] extends string ? string : never 语法是 TS 中的条件类型
  // 如果 T[P] 兼容 string 类型，那么就会返回 string ，否则返回 never
  [P in keyof T]: T[P] extends string ? string : never;
};

type OnlyStringPerson = StringProperties<Person>;
```

**05 映射修饰符**

映射类型还允许我们添加或移除特定的修饰符，如 `readonly` 或 `?`

```typescript
type Mutable<T> = {
  // 表示移除原类型的属性只读修饰符
  -readonly [P in keyof T]: T[P];
};

type NonOptional<T> = {
  // -？ 表示移除原类型的属性可选修饰符
  [P in keyof T]-?: T[P];
};

type MutablePerson = Mutable<ReadonlyPerson>; // 移除只读属性
type NonOptionalPerson = NonOptional<OptionalPerson>; // 移除可选属性
```