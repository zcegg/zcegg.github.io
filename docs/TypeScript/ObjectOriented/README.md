## TS中的类

类（class）是一个非常重要的概念，它为 JavaScript 提供了基于类的面向对象编程的功能。 TS 中的类不仅包括了 ES6 的类语法，还添加了类型注解和一些其它特性。

**01 定义类**

定义一个类的基本结构包括类名、构造函数和成员函数（方法）

```typescript
class MyClass {
  constructor() {
    // 构造函数代码
  }

  myMethod() {
    // 方法代码
  }
}
```

**02 属性和方法**

类可以包含属性（成员变量）和方法（成员函数）

```typescript
class MyClass {
  myProperty: string; // 属性

  constructor(value: string) {
      this.myProperty = value;
  }

  myMethod(): void {
      console.log(this.myProperty);
  }
}
```

**03 访问修饰符**

TS 支持三种访问修饰符：`public(默认)`、`private` 和 `protected`

- `public`：成员可以在任何地方被访问（任意）
- `private`：成员只能被类本身访问(类的内部)
- `protected`：成员可以被类本身及其子类访问（类内和子类）

```typescript
class MyClass {
  public publicProperty: string;
  private privateProperty: string;
  protected protectedProperty: string;
}
```

**04 静态属性和方法**

静态属性和方法属于类本身，而不是类的实例

```typescript
class MyClass {
  // 通过 static 关键字定义静态属性
  static staticProperty: string = "static";

  // 静态方法同样不属于 this
  static staticMethod(): void {
    // 访问静态属性时通过 类名，而非 this
    console.log(MyClass.staticProperty);
  }
}
```

**05 继承**

```typescript
class BaseClass {
  baseMethod(): void {}
}

// 通过 extends 关键字即可使用继承
class DerivedClass extends BaseClass {
  derivedMethod(): void {}
}
```

**06 抽象类和方法**

抽象类是供其它类继承的基类，不能被实例化。抽象方法是必须在派生类中实现的方法

```typescript

// 通过 abstract 关键字来定义一个抽象，不能被实现，只能被继承
abstract class AbstractClass {
  // 抽象类中的抽象方法，将来必须在派生类中进行实现。
  abstract abstractMethod(): void;
}

class ConcreteClass extends AbstractClass {
  abstractMethod(): void {
    // 实现代码
  }
}
```

**07 接口实现**

类可以实现一个或多个接口，表示必须实现接口中定义的所有方法

```typescript
// 使用 interface 定义接口
interface MyInterface {
  myMethod(): void;
}

// 通过 implements 来实现某个类接口
// 此时就要按接口的规则来进行实现
class MyClass implements MyInterface {
  myMethod(): void {
    // 实现代码
  }
}
```

**08 构造器参数属性**

我们可以在构造函数中直接使用访问修饰符，这将自动创建并初始化相应的成员变量

```typescript
  class MyClass{
    // 手动指定成员属性
    public name:string = ''

    // 构造函数中简写
    // 就会自动的创建一个 age 成员属性
    constructor(public age:number){}
  }
```

**09 Getter 和 Setter**

TS 支持 getter 和 setter 方法，用于控制对类的成员的访问和赋值

```typescript
class MyClass {
  // 定义一个私有属性，只希望在类的内部进行访问
  // 但的确又存在外部对它赋值的情况
  private _property: string;

  // 定义一个getter，对外暴露一个接口，可以拿到它的值
  get property(): string {
    return this._property;
  }

  // 定义一个 setter，接收一个参数，可以修改 property 属性
  set property(value: string) {
    this._property = value;
  }
}
```

## 类的修饰符

类的访问修饰符用于控制类属性和方法的可访问性，这有助于封装类的内部状态和行为。TS 提供了三种主要的访问修饰符：`public`、`private`、`projected`。

**01 语法细节**

1. `public(公开)`
   1. public 修饰符是默认的访问级别，公开成员可以在任何地方被访问
   2. 开发时，我们通常不需要显式的声明成员为 public
2. `private(私有)`
   1. private 修饰符限制成员只能在类本身内部被访问
   2. 私有成员不能在类的实例或子类中访问
3. `protected(受保护)`
   1. projected 修饰符允许成员在类及其子类中被访问
   2. 与 private 类似，但是它提供了派生类的访问

**02 语法示例**

```typescript
class Person {
  public name: string;        // 公开属性，可以在任何地方访问
  private age: number;        // 私有属性，只能在 Person 类内部访问
  protected email: string;    // 受保护属性，可以在 Person 类及其子类中访问

  // 上面显式的定义了几个成员属性，同时设置了访问修饰符和类型
  // 在构造函数中对上述的三个属性进行初始化
  constructor(name: string, age: number, email: string) {
    this.name = name;
    this.age = age;
    this.email = email;
  }

  // name 是一个 public 访问权限，在任意地方随时访问
  public greet(): void {
    console.log(`Hello, my name is ${this.name}`);
  }

  // age 是一个 private 访问权限，只能在类的内部进行访问
  private getAge(): void {
    console.log(`I am ${this.age} years old`);
  }

  // email 是一个 protected 属性，与 private 类似，可以在类的内部，同时也能在派生类中访问
  protected getEmail(): void {
    console.log(`My email is ${this.email}`);
  }
}

// Employee 类继承了 Person ，所以是它的子类或派生类
class Employee extends Person {
  constructor(name: string, age: number, email: string, public department: string) {
    // 调用父类的构造方法
    super(name, age, email);
  }

  // email 在父类中定义，是 projected
  // 所以在子类中是可以访问的
  public showEmail(): void {
    console.log(`My work email is ${this.email}`); // 可以访问受保护的成员
  }
}

const bob = new Person("Bob", 30, "bob@example.com");

// name 一直都是 public 所以任意地方随时访问
console.log(bob.name); // 正确

// age 是私有的，不能直接通过实例访问，可以设置 getter
// console.log(bob.age); // 错误：'age' 是私有的

// 受保护的可以在子类中访问，但是不能通过实例访问
// console.log(bob.email); // 错误：'email' 受保护，只能在类 'Person' 及其子类中访问

const alice = new Employee("Alice", 28, "alice@work.com", "Engineering");

// showEmail 方法是一个 public ，可以通过实例来访问。
alice.showEmail(); // 正确
```

## 类的继承

类继承是面向对象编程的一个核心概念。它允许我们创建一个类（子类）来继承另一个类（父类、基类）的成员（属性和方法）

**01 基本语法**

```typescript
class BaseClass {
    baseMethod() {
        console.log("Method in BaseClass");
    }
}

class DerivedClass extends BaseClass {
    derivedMethod() {
        console.log("Method in DerivedClass");
    }
}
```

**02 构造函数和super关键字**

在 TS 中，如果在派生类中定义了构造函数，必须先调用 `super()`，也就是基类的构造函数

```typescript
class BaseClass {
    constructor(public name: string) {}
}

class DerivedClass extends BaseClass {
  // 派生类中存在自己的构造方法，则一定要使用 super 调用父类
    constructor(name: string, public description: string) {
        super(name);
    }
}
```

**03 重写方法**

派生类可以重写基类中的方法

```typescript
// 定我基类 BaseClass，拥有成员方法 greet
class BaseClass {
    greet() {
        console.log("Hello from BaseClass");
    }
}

class DerivedClass extends BaseClass {
    // 派生类继承了 BaseClass，同时可以重写父类同名方法
    greet() {
        console.log("Hello from DerivedClass");
    }
}
```

**04 访问基类的方法**

使用 `super` 关键字可以在派生类中访问基类的方法

```typescript
class BaseClass {
  greet() {
    console.log("Hello from BaseClass");
  }
}

class DerivedClass extends BaseClass {
  greet() {
    // 在派生类的方法中如果想要调用父类的方法可以通过 super 关键字来实现。这样既可以执行子类自己的逻辑，也可以执行父类的逻辑
    super.greet(); // 调用基类的方法
    console.log("Additional greeting from DerivedClass");
  }
}
```

**05 属性继承**

派生类继承基类的所有公共和受保护成员，但不继承私有成员

```typescript
class BaseClass {
    public publicProperty: string;
    protected protectedProperty: string;
    private privateProperty: string;
}

class DerivedClass extends BaseClass {
    useProperties() {
      // 原型链，先找自身的，如果没有则向上查找
      console.log(this.publicProperty);     // OK
      console.log(this.protectedProperty); // OK
      // console.log(this.privateProperty); // 错误: 'privateProperty' 是私有的
    }
}
```

**06 抽象类和继承**

抽象类不能被实例化，通常作为其它类的基类

```typescript
// 抽象类可以拥有抽象方法，不能有实现
abstract class AbstractClass {
  abstract abstractMethod(): void;
}

class ConcreteClass extends AbstractClass {
    abstractMethod() {
        console.log("Implemented in ConcreteClass");
    }
}
```

**07 多态**

继承支持多态，派生类的实例可以赋值给基类类型的变量

```typescript
class BaseClass {}
class DerivedClass extends BaseClass {}

let instance: BaseClass = new DerivedClass();
```

## 抽象类语法

抽匀类（Abstract Class）是一种特殊的类，它不能被直接实例化。抽象类主要用于定义子类应遵循的模板和实现一些共能功能。而半一些具体的实现留给子类来完成。

**01 定义抽象类**

使用 `abstract` 关键字定义一个抽象类

```typescript

// Animal 是一个抽象类，它定义了一个抽象方法 `makeSound` 和一个普通方法 `move`
abstract class Animal {
  abstract makeSound(): void;
  move(): void {
      console.log('roaming the earth...');
  }
}
```

**02 抽象方法**

抽象类中的抽象方法也使用 `abstract` 关键字标记，并且只能在抽象类的内部定义，抽象方法只能声明方法签名。不能包含具体的实现

```typescript
abstract class Animal {
  abstract makeSound(): void;
}
```

**03 实现抽象类**

抽象类的子类必须实现抽象类中「所有的抽象方法」，否则它们也必须标记为抽象类

```typescript
class Dog extends Animal {
  makeSound(): void {
    console.log('Woof! Woof!');
  }
}
```

**04 抽象类的构造函数**

虽然抽象类不能被实例化，它们可以包含构造函数，这个构造函数在派生类中被调用

```typescript
abstract class Animal {
  constructor(public name: string) {}
}

class Dog extends Animal {
  makeSound(): void {
    console.log(`Woof! My name is ${this.name}`);
  }
}
```

**05 抽象属性**

抽象类可以包含抽象属性，这些属性必须在派生类中被实现

```typescript
abstract class Animal {
  abstract readonly numberOfLegs: number;
}

class Dog extends Animal {
  readonly numberOfLegs = 4;
}
```

**06 注意事项**

1. 抽象类主要用于定义接口和部分实现，而具体的实现通常留给子类
2. 抽象类可以包含非抽象的方法和属性
3. 抽象类和方法提供了一种强制要求子类遵守某些规则的方式

## 类的多态

多态是面向对象编程的一个核心概念，它允许我们使用父类的类型来引用一个子类的对象。多态的使用可以让我们的代码在处理多种不同的类时更加灵活和可护展。

**01 基类和派生类**

多态基于继承，我们需要有一个基类（或父类），以及一个或多个继承自该基类的派生类（或子类）

```typescript
// 定义一基类
class Animal {
    makeSound(): void {
        console.log('Some sound');
    }
}

// Dog 类继承自 Animal
class Dog extends Animal {
    makeSound(): void {
        console.log('Woof! Woof!');
    }
}

// Cat 类也继承自 Animal
class Cat extends Animal {
    makeSound(): void {
        console.log('Meow');
    }
}
```

**02 使用基类引用子类对象**

多态允许我们使用基类类型的引用或指针来调用派生自该基类的任何子类的方法

```typescript
// 设置一个变量标记为 基类 的类型
let myAnimal: Animal;

// myAnimal 可以是任意符合 Animal 类型的实例对象

// Dog 的实例赋值给 Animal 类型的变量
myAnimal = new Dog();
myAnimal.makeSound();  // 输出: Woof! Woof!

// Cat 的实例赋值给 Animal 类型的变量
myAnimal = new Cat();
myAnimal.makeSound();  // 输出: Meow

```

**03 方法覆盖写**

多态通常与方法覆写（也称为重写）结合使用。在子类中，我们可以重写继承自父类的方法。提供特定于子类的实现

Animal 的 makeSound 方法可以用不同的实现，可以是 Dog 的 ，可以是 Bird 的。可以是 Cat 的。

```typescript
class Bird extends Animal {
  // 重写了基类中的方法，然后符合 Animal ，那么父类引用就可以指向一个子类的实例，这样 myAnimal 就是一个 Bird 的实例，也可以调用 makeSound 方法。
  makeSound(): void {
    console.log('Tweet');
  }
}
```

**04 调用子类特有的方法**

如果需要调用子类中特有的方法，我们需要先对基类引用进行类型断言，从而指明具体的子类类型

```typescript
if (myAnimal instanceof Dog) {
  // 现在 TypeScript 知道 myAnimal 是 Dog 类型
  (myAnimal as Dog).someDogSpecificMethod();
}
```

**05 抽象类和多类**

抽象类经常用于创建多态行为，我们可以定义一个抽象基类，其中包含抽象方法。然后在各个派生类中实现实些方法

```typescript
abstract class Shape {
  abstract draw(): void;
}

class Circle extends Shape {
  draw(): void {
    console.log('Drawing a circle');
  }
}

class Rectangle extends Shape {
  draw(): void {
    console.log('Drawing a rectangle');
  }
}
```

## 索引签名细节

索引签名用于描述那些可以通过动态名称（如字符串或数字）访问的类型。它们用于定义对象或类的实例，属性名是不确定的，但属性值的类型是已知的。

**01 基本语法**

索引签名可以用字符串或数字类型的键

```typescript
// index 是一个仅为了定义签名而使用的变量名，我们可以自由命名
// valueType 是属性值的类型
interface StringIndex {
  [index: string]: ValueType;
}

interface NumberIndex {
  [index: number]: ValueType;
}
```

**02 字符串索引签名**

```typescript
interface StringDictionary {
  // 约束接口的属性名都是字符串类型，对应的值都是 string 类型
  [key: string]: string;
}

let myDict: StringDictionary = {
  "prop1": "value1",
  "prop2": "value2"
};
```

**03 数字索引签名**

数字索引用于那些通过数字索引的方式访问的类型，类似于数组，但索引签名定义了值的类型

```typescript
interface NumberDictionary {
  [index: number]: string;
}

let myArray: NumberDictionary = {
  0: "value1",
  1: "value2"
};
```

**04 混合索引签名**

我们可以在同一个接口中同时使用字符串和数字索引，但需要注意，数字索引的返回值必须是字符串索引返回值类型的子类型，因为 JS 在运行时会将数字索引转为字符串。

```typescript
interface MixedDictionary {
  // index:number 的值类型，必须是 string 类型的子类型
  [index: number]: string;
  // 因为 JS 在运行时会将 Number 的 key 转为 string
  [key: string]: string | number;
}

let myMixedDict: MixedDictionary = {
  0: "value1",
  "prop2": 1234
};
```


