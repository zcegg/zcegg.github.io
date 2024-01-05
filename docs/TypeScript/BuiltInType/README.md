## `Partial<T>`

将类型 `T` 的所有属性变为可选，这在某些场景下还是很有用的

```typescript
  interface Person{
    name: string
    age: number
  }

  // 原本接口 Person 约束 name 与 age 都是必填属性
  // 现在可以不用传入 age，说明 Parial 生效了
  const updatePerson: Parial<Person> = {name: 'Alice'}
```

## `Readonly<T>`

将类型 `T` 的所有属性设置为只读，这适用于我们不想更改对象属性的情况

```typescript
const person: Readonly<Person> = { name: "Bob", age: 30 };
// person.name = "Alice"; // 错误：不能修改只读属性
```

## `Record<K, T>`

创建一个类型，其属性键为 `K`, 属性值类型为 `T`。可以创建一个对象，其键是特定类型且所有值都是相同类型。

```typescript
interface Person{
  name: string
  age: number
}

const people: Record<string, Person> = {
  alice: { name: "Alice", age: 25 },
  bob: { name: "Bob", age: 30 }
};
```

## `Pick<T, K>`

从类型 `T` 中挑选一些属性来构造新的类型

```typescript

interface Person{
  name: string
  age: number
}

// 此时的 PersonName 就只包含 name 的约束了
type PersonName = Pick<Person, "name">;
const personName: PersonName = { name: "Charlie" };
```

## `Exclude<T, U>`

从类型 `T` 中排除那些可以赋值给类型 U 的类型。或者说从 T 中将 U 类型拿出。

```typescript
type NonString = Exclude<any, string>;
const nonString: NonString = 123; // OK
// const nonStringError: NonString = "Hello"; // 错误：不能将类型“string”分配给类型“Exclude<any, string>”
```

## `ReturnType<T>`

获取函数类型T 的返回值类型

```typescript
function getUser() {
  return { name: "Daisy", age: 28 };
}

// 使用 typeof 获取 getUser 的函数类型
// 再次函数类型传入 ReturnType 来创建新的类型
type User = ReturnType<typeof getUser>;
```

## `Oimit<T, K>`

从类型 `T` 中去除 `K` 指定的属性，与 `Pick`功能相反

```typescript

interface Person{
  name: string
  age: number
}

type PersonWithoutAge = Omit<Person, "age">;
const personWithoutAge: PersonWithoutAge = { name: "Eve" };
```

## `NonNullable<T>`

从类型 `T` 中排除 `null` 和 `undefined` 

```typescript
// 新的类型就只有 string 了
type NonNullableString = NonNullable<string | null | undefined>;
const nonNullableString: NonNullableString = "Text"; // OK
// const nullString: NonNullableString = null; // 错误：不能将类型“null”分配给类型“NonNullableString”
```

## `Parameters<T>`

获取函数类型 `T` 的参数类型构成的元组类型

```typescript
function greet(name: string, age: number): string {
  return `Hello, I'm ${name} and I'm ${age} years old.`;
}

// 将函数类型的参数类型传入该工具方法
// 可以得到该函数对应的参数的元组类型
type GreetParameters = Parameters<typeof greet>; // [string, number]
```

## `ConstructorParameters<T>`

获取构造函数类型 `T` 的参数类型构成的元组类型，和 Parameters 类似，但是针对于构造函数

```typescript
class Person {
  constructor(public name: string, public age: number) {}
}

 // [string, number]
type PersonConstructorParameters = 
ConstructorParameters<typeof Person>;
```

## `InstanceType<T>`

获取构造函数类型 `T` 的实例类型 

```typescript
type PersonInstance = InstanceType<typeof Person>; // Person
```

## `Required<T>`

将类型 `T` 的所有属性设置为必须的，这与 `Partial` 功能相反。

```typescript
interface PartialPerson {
  name?: string;
  age?: number;
}

const requiredPerson: Required<PartialPerson> = { name: "Alice", age: 30 };
```

## `ThisParameterType<T>`

提取函数类型 `T` 的 `this` 参数的类型，如果没有 `this` 参数，则为 `unknow`

```typescript
// 在 TS 中可以在第一个参数位置上 使用 this 来确定当前函数 this 的类型
// 该值不会做为实参传给函数调用处
function test(this: string, num: number) {
  return this + num;
}

// 首先需要拿到函数类型 typeof test 
// 然后将该类型传给类型工具 ThisParameterType
// 将函数类型的 this 做为一个类型返回，此例中返回 string
type TestThisParameter = ThisParameterType<typeof test>; 
```

## `OmitThisParameter<T>`

和上面的功能相反，该工具可以实现从函数类型 `T` 中移除 `this` 参数

```typescript
// 定义 test 时看似定义了 this 与 num
function test(this: string, num: number) {
  return this + num;
}

// 等价于 (num: number) => string
type TestWithoutThis = OmitThisParameter<typeof test>;

```