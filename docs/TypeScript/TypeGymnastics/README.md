## `Partial<T>`

`Partial` 是一个内置的泛型工具类，它将一个类型的所有属性转换为可选的，`Partial` 类型的实现使用了映射类型和条件类型的特点。

```typescript
type Partial<T> = {
  [P in keyof T]?: T[P];
};
```

**说明**

- `T` 是 `Partial` 泛型接收的类型参数
- `[P in keyof T]` 是映射类型的语法，它会遍历类型 T 的所有属性键（`keyof T`）
- `？` 使每个属性都变成可选的
- `T[P]` 表示属性 `P` 的类型

**示例**

```typescript
// 假设我们有以下接口
interface Person {
  name: string;
  age: number;
}

// 使用 Partial 将 Person 接口的所有属性转为可选的
type PartialPerson = Partial<Person>;

let person: PartialPerson = {
  name: "Alice"
  // 注意：不需要提供 age 属性
};
```

## `Required<T>`

它将一个类型的所有属性从可选变为必需的。 `Required` 类型的实现利用了映射类型，它遍历一个类型的所有属性。并移除它们的可选性

```typescript
type Required<T> = {
  [P in keyof T]-?: T[P];
};
```

**说明**

- `T` 是 `Required` 泛型接收的类型参数
- `[P in keyof T]` 是映射类型的语法，它会遍历 `T` 的所有属性键
- `-?` 用于移除属性的可选性（即移除属性声明中的 `?` 标记）
- `T[P]` 表示属性 `P` 的类型

**示例**

```typescript
// 假设我们有如下接口，其中一些属性本身是可选的
interface Person {
  name: string;
  age?: number;
  address?: string;
}

// 使用 Required 将 Person 接口的所有属性转为必需的
type RequiredPerson = Required<Person>;
let person: RequiredPerson = {
  name: "Alice",
  age: 30,      // 必须提供 age
  address: "123 Main St" // 必须提供 address
};
```

## `Readonly<T>`

Readonly 用于将一个类型的所有属性设置为只读。这意味着一旦一个对象被创建，它的这些属性就不能被修改。 `Readonly` 类型的实现同样通过映射类型来实现

```typescript
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};
```

**说明**

- `T` 是 `Readonly` 泛型接收到的类型参数
- `[P in keyof T]` 是映射类型的语法，它会遍历类型 `T` 的所有属性键
- `readonly` 关键字使每个属性变成只读的
- `T[P]` 表示属性 `P` 的类型

**示例**

```typescript
// 定义一个 Person ，它的属性们本身是可以修改的
interface Person {
  name: string;
  age: number;
}

// 使用 Readonly 将 Person 接口的所有属性设置为只读
type ReadonlyPerson = Readonly<Person>;
let person: ReadonlyPerson = {
  name: "Alice",
  age: 30
};

// 错误：不能分配到 "name" ，因为它是只读属性。
person.name = "Bob"; 
```

## `Record<K, T>`

`Record<K, T>` 用于创建一个对象类型，其属性键为 `K` 类型，属性值为 `T` 类型。`Record` 类型的实现利用了映射类型，它遍历键类型 `K` 的所有可能值，并将它们映射到值类型 `T`

```typescript
type Record<K extends keyof any, T> = {
  [P in K]: T;
};
```

**说明**

- `K extends keyof any` 确保 `K` 可以是任何在型的键，包括 `string` `number` 或 `symbol`
- `[P in K]` 是映射类型语法，它遍历 `K` 类型的所有属性键
- `T` 表示每个属性的值的类型

**示例1**

在下面的代码中，`StringDictionary` 是一个对象类七里，它的属性键为 `string` 类型，每个属性的值也是 `string` 类型。

```typescript
// 创建具有特定值类型的对象
type StringDictionary = Record<string, string>;
const dict: StringDictionary = {
    key1: "value1",
    key2: "value2"
    // key3: 123 // 错误：不能将类型“number”分配给类型“string”
};
```

**示例2**

```typescript
// 映射枚举类型到值类型
enum Color {
  Red,
  Green,
  Blue
}

// {0:string, 1:string, 2:string}
type ColorMap = Record<Color, string>;
const colorNames: ColorMap = {
  [Color.Red]: "red",
  [Color.Green]: "green",
  [Color.Blue]: "blue"
};
```

## `Pick<T, K>`

`Pick<T, K>` 用于从类型 `T` 中挑选一组属性 `K`, 从而构造一个新的类型，这种类型的实现使用了映射类型和键的子集

```typescript

// 限制 K 必需来自于 T
type Pick<T, K extends keyof T> = {
  [P in K]: T[P];
};
```

**说明**

- `T` 是 `Pick` 泛型接收到的类型参数，代表一个对象
- `K extends keyof T` 确保了 `K` 是 `T` 的键的子集
- `[P in K]` 是映射类型的语法，它遍历 `K` 中的每个属性键
- `T[P]` 表示属性 P 的类型

**示例**

```typescript
interface Person {
  name: string;
  age: number;
  address: string;
}

// 使用 Pick 从 Person 接口中挑选一组属性来构造一个新的类型
// {name:string, age: number}
type PersonNameAndAge = Pick<Person, "name" | "age">;
const person: PersonNameAndAge = {
  name: "Alice",
  age: 25
  // address: "123 Main St" // 错误：类型 "{ name: string; age: number; }" 中不存在属性 "address"
};

```

## `Omit<T, K>`

`Omit<T, K>` 功能和 Pick 刚好相反，用于从类型 `T` 中剔除一组属性 `K` ，从而构造一个新的类型。

```typescript
type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>;
```

**说明**

- `T` 是 `Omit` 泛型收到的类型参数，代表一个对象
- `K extends keyof any` 确保了 K 可以是任何类型的键
- `Exclude<keyof T, K>` 从 `T` 的键中排除 `K` ，结果是 `T` 的键中不包含 `K` 的部分
- `Pick<T, ...>` 然后从 `T` 中挑选剩余的键，构造一个新的类型

**示例**

```typescript
interface Person {
  name: string;
  age: number;
  address: string;
}

// 使用 Omit 从 Person 接口中剔除一组属性来构造一个新的类型
type PersonWithoutAddress = Omit<Person, "address">;

const person: PersonWithoutAddress = {
  name: "Alice",
  age: 25
  // address: "123 Main St" // 错误：类型 "{ name: string; age: number; }" 中不存在属性 "address"
};
```

## `Exclude<T, U>`

`Exclude<T, U>` 用于从类型 T 中排除那些可以赋值给类型 U 的类型

```typescript
type Exclude<T, U> = T extends U ? never : T;
```

**说明**

- T 是 Exclude 泛型接收的类型参数，代表一个联合类型
- U 是另一个类型参数，用于指定要从 T 中排除的类型
- T extends U ? never : T 是一个条件类型表达式
  - 如果 T 中的某个类型可以赋值给 U ，结果类型为 never （即排除）
  - 否则，结果类型为 T 中的该类型

**示例**

```typescript
type AllTypes = string | number | boolean;
type StringOrBoolean = string | boolean;

// 使用 Exclude 从 AllTypes 中排除 StringOrBoolean 类型
// 结果类型为 number，因为 string 和 boolean 被排除了
type JustNumber = Exclude<AllTypes, StringOrBoolean>;
```

## `NonNullable<T>`

`NonNullable<T>` 用于从类型 `T` 中排除 `null` 和 `undefined` ，这是一种条件类型的应用，它确保类型 `T` 不包含 `null` 或 `undefined`。

```typescript
type NonNullable<T> = T extends null | undefined ? never : T;
```

**说明**

- `T` 是 `NonNullable` 泛型接收的类型参数
- `T extends null|undedefined ? never : T` 是一个条件类型表达式
  - 如果 `T` 是 `null` 或 `undefined` 结果类型为 `never` （排除这些类型）
  - 否则，结果类型为 `T`

**示例**

```typescript
type MaybeString = string | null | undefined;

// 使用 NonNullable 排除 null 和 undefined
// 结果类型为 string
type StringOnly = NonNullable<MaybeString>;
```

## `InstanceType<>`

`InstanceType<>` 用于获取构造函数类型 T 的实例类型，这是一种高级类型的应用，它使用到了条件类型和类型推断

```typescript
type InstanceType<T extends new (...args: any[]) => any> = T extends new (...args: any[]) => infer R ? R : any;
```

**说明**

- `T extends new (...args:any[]) => any` 确保了 T 是一个构造函数
- `infer R` 是一个类型推断，用于捕获由构造函数类型 T 创建的实例类型
- `T extends new (...args: any[]) => infer R ? R : any` 是一个条件类型
  - 如果 和 `T` 是一个构造函数， `R` 将是它的实例类型
  - 否则，结果为 `any`

**示例**

```typescript
class MyClass {
  constructor(public name: string) {}
}

// 使用 InstanceType 获取 MyClass 的实例类型
// 传入的是 typeof MyClaass
// 结果类型为 MyClass 的实例类型
type MyClassInstance = InstanceType<typeof MyClass>;
```

