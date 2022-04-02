# 初识 TypeScript



## Contents

[类型](#类型)	[数组](#数组)	[元组](#元组)	[枚举](#枚举)	[函数](#函数)

[类](#类)	[接口](#接口)	[类与接口](#类与接口)	[泛型](#泛型)

[声明合并](#声明合并)	[声明文件](#声明文件)	[内置对象](#内置对象)



## Fulltexts

### 类型

#### 原始数据类型 Primitive Data Type

- 布尔值 Boolean

    - 注意：使用构造函数 `Boolean` 创造的对象**不是**布尔值
    - 事实上 `new Boolean()` 返回的是一个 `Boolean` 对象
    - 直接调用 `Boolean` 也可以返回一个 `boolean` 类型

- 数值 

    - 比如 `0b1010` 和 `0o744` 是 [ES6 中的二进制和八进制表示法](http://es6.ruanyifeng.com/#docs/number#二进制和八进制表示法)，会被编译为十进制数字 10 和 484

- 字符串

    - `、` 用来定义 [ES6 中的模板字符串](http://es6.ruanyifeng.com/#docs/string#模板字符串)，`${expr}` 用来在模板字符串中嵌入表达式

- void

    - JavaScript 没有空值（Void）的概念，在 TypeScript 中，可以用 `void` 表示没有任何返回值的函数
    - 声明一个 `void` 类型的变量没有意义，因为只能将它赋值为 `undefined` 和 `null`

    - 与 `void` 的区别是，`undefined` 和 `null` 是所有类型的**子类型**
    - 而 `void` 类型的变量不能赋值给 `number` 类型的变量

- null

- undefined

- Symbol

#### 任意值类型 Any Type

- 普通类型，在赋值过程中改变类型是不被允许的
- 如果是 `any` 类型，则允许被赋值为任意类型
- Any 上访问任何属性都是允许的，也允许调用任何方法，不会报错
- 声明一个变量为任意值之后，对它的**任何操作**，返回的内容的类型都是**任意值**
- 变量如果在声明的时候，**未指定其类型**，那么它会被识别为任意值

#### 联合类型 Union Type

- 联合类型使用 `|` 分隔每个类型

- 联合类型只能访问共有属性

    ```js
    function getString(something: string | number): string {
        return something.toString();
    }
    ```

- 与单类型的不同，在定义后的赋值会做类型推论，而不是识别为 Any 类型

#### 对象数据类型 Object Type

- 由 type interfaces 等初始化的对象数据

#### 类型推论

- 在没有明确的指定类型的时候推测出一个类型，这就是**类型推论**
- 定义变量而不进行赋值，则不会推测类型，全部识别为 Any 类型

#### 类型断言

Type Assertion 用来手动指定一个值的类型

- 用法： 值 as 类型（React tsx写法）或者 \<类型\>值

    ```tsx
    function getLength(something: string | number): number {
        if ((<string>something).length) {
            return (<string>something).length;
        } else {
            return something.toString().length;
        }
    }
    ```

- 注意：断言成一个**联合类型**中不存在的类型是不允许的

#### 类型别名

类型别名用来给一个类型起个新名字 NameOrResolver 

- ```tsx
    type Name = string;
    type NameResolver = () => string;
    type NameOrResolver = Name | NameResolver;
    function getName(n: NameOrResolver): Name {
        if (typeof n === 'string') {
            return n;
        } else {
            return n();
        }
    }
    ```

#### 字符串字面量类型

字符串字面量类型用来约束取值只能是某几个字符串中的一个

- ```tsx
    type EventNames = 'click' | 'scroll' | 'mousemove';
    function handleEvent(ele: Element, event: EventNames) {
        // do something
    }
    
    handleEvent(document.getElementById('hello'), 'scroll');  // 没问题
    handleEvent(document.getElementById('world'), 'dbclick'); // 报错，event 不能为 'dbclick'
    
    // index.ts(7,47): error TS2345: Argument of type '"dbclick"' is not assignable to parameter of type 'EventNames'.
    ```



---

### 数组

#### 数组定义

`let fibonacci: number[] = [1, 1, 2, 3, 5];`

#### 数组泛型（Array Generic）

`let fibonacci: Array<number> = [1, 1, 2, 3, 5];`

#### 类数组

- 类数组除了限制 索引类型外还进一步限制了元素属性 callee，length

```js
    function sum() {
        let args: {
            [index: number]: number;
            length: number;
            callee: Function;
        } = arguments;
    }
```

- 数组可以转化为类数组，类数组不可转化为数组
- 类数组不像原生数组有自带 function（查看 \__proto__）

#### Any 类型元素

- `let list: any[] = ['baidu', 25, { website: 'https://www.baidu.com' }];`



---

### 元组

数组合并了相同类型的对象，而元组（Tuple）合并了不同类型的对象？

#### 定义

```tsx
let tom: [string, number];
tom = ['Tom', 25];
```

#### 赋值 & 访问

```tsx
let tom: [string, number];
tom[0] = 'Tom';
tom[1] = 25;

tom[0].slice(1);  // "om"
tom[1].toFixed(2); // "25.00"
```

#### 越界

```tsx
let tom: [string, number];
tom = ['Tom', 25];
tom.push('male');
tom.push(true);

// Argument of type 'true' is not assignable to parameter of type 'string | number'.
```



---

### 枚举

枚举（Enum）类型用于取值被限定在一定范围内的场景，比如一周只能有七天，三原色限定为红黄蓝等。

####定义

```tsx
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 0); // true
console.log(Days["Mon"] === 1); // true
console.log(Days["Tue"] === 2); // true
console.log(Days["Sat"] === 6); // true

console.log(Days[0] === "Sun"); // true
console.log(Days[1] === "Mon"); // true
console.log(Days[2] === "Tue"); // true
console.log(Days[6] === "Sat"); // true
```

**枚举的元素会从 0 开始被赋值，同时也会对枚举值到枚举名进行反向映射。**

#### 手动赋值

```tsx
enum Days {Sun = 7, Mon = 1.5, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 7); // true
console.log(Days["Mon"] === 1.5); // true
console.log(Days["Tue"] === 2.5); // true
console.log(Days["Sat"] === 6.5); // true
```

未被手动赋值的枚举元素会按已赋值的数字加 1 递增

#### 常数项和计算所得项

枚举项有两种类型：常数项（constant member）和计算所得项（computed member）。

**如果紧接在计算所得项后面的是未手动赋值的项，那么它就会因为无法获得初始值而报错**：

```tsx
enum Color {Red = "red".length, Green, Blue};

// index.ts(1,33): error TS1061: Enum member must have initializer.
// index.ts(1,40): error TS1061: Enum member must have initializer.
```

当满足以下条件时，枚举成员被当作是常数：

- 不具有初始化函数并且之前的枚举成员是常数。在这种情况下，当前枚举成员的值为上一个枚举成员的值加 `1`。但第一个枚举元素是个例外。如果它没有初始化方法，那么它的初始值为 `0`。
- 枚举成员使用常数枚举表达式初始化。常数枚举表达式是 TypeScript 表达式的子集，它可以在编译阶段求值。当一个表达式满足下面条件之一时，它就是一个常数枚举表达式：
    - 数字字面量
    - 引用之前定义的常数枚举成员（可以是在不同的枚举类型中定义的）如果这个成员是在同一个枚举类型中定义的，可以使用非限定名来引用
    - 带括号的常数枚举表达式
    - `+`, `-`, `~` 一元运算符应用于常数枚举表达式
    - `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `>>>`, `&`, `|`, `^` 二元运算符，常数枚举表达式做为其一个操作对象。若常数枚举表达式求值后为 NaN 或 Infinity，则会在编译阶段报错

所有其它情况的枚举成员被当作是需要计算得出的值。

#### 常数枚举

```tsx
const enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];

// 编译结果
var directions = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];

// 若包含 计算所得项 则会报错
const enum Color {Red, Green, Blue="blue".length};

// index.ts(1,38): error TS2474: In 'const' enum declarations member initializer must be constant expression.
```

#### 外部枚举

外部枚举（Ambient Enums）是使用 `declare enum` 定义的枚举类型：

```tsx
declare enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];

// 编译结果
var directions = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
```

外部枚举与声明语句一样，主要出现在声明文件中。



---

### 函数

#### 定义方式

函数的定义方式有两种：函数声明 和 函数表达式

- ```js
    // 函数声明（Function Declaration）
    function sum(x, y) {
        return x + y;
    }
    
    // 函数表达式（Function Expression）
    let mySum = function (x, y) {
        return x + y;
    };
    ```

#### 参数

- 可选参数

    ```ts
    function buildName(firstName: string, lastName?: string) {
        if (lastName) {
            return firstName + ' ' + lastName;
        } else {
            return firstName;
        }
    }
    let tomcat = buildName('Tom', 'Cat');
    let tom = buildName('Tom');
    ```

- 参数默认值

    ```js
    function buildName(firstName: string = 'Tom', lastName: string) {
        return firstName + ' ' + lastName;
    }
    let tomcat = buildName('Tom', 'Cat');
    let cat = buildName(undefined, 'Cat');
    ```

- 剩余参数

    ```ts
    function push(array: any[], ...items: any[]) {
        items.forEach(function(item) {
            array.push(item);
        });
    }
    
    let a = [];
    push(a, 1, 2, 3);
    ```

    rest 参数只能是最后一个参数

#### 重载

- ```ts
    function reverse(x: number): number;
    function reverse(x: string): string;
    function reverse(x: number | string): number | string {
        if (typeof x === 'number') {
            return Number(x.toString().split('').reverse().join(''));
        } else if (typeof x === 'string') {
            return x.split('').reverse().join('');
        }
    }
    ```

    TypeScript 会优先从最前面的函数定义开始匹配，所以多个函数定义如果有包含关系，需要优先把**精确**的定义写在前面



---

### 类

JavaScript 通过构造函数实现类的概念，通过原型链实现继承。

在 ES6 中，正式加入 `class`。

TypeScript 除了实现了所有 ES6 中的类的功能以外，还添加了一些新的用法。

#### 类的概念

类（Class）：定义了一件事物的抽象特点，包含它的属性和方法

对象（Object）：类的实例，通过 `new` 生成

面向对象（OOP）的三大特性：封装、继承、多态

封装（Encapsulation）：将对数据的操作细节隐藏起来，只暴露对外的接口。外界调用端不需要（也不可能）知道细节，就能通过对外提供的接口来访问该对象，同时也保证了外界无法任意更改对象内部的数据

继承（Inheritance）：子类继承父类，子类除了拥有父类的所有特性外，还有一些更具体的特性

多态（Polymorphism）：由继承而产生了相关的不同的类，对同一个方法可以有不同的响应。比如 `Cat` 和 `Dog` 都继承自 `Animal`，但是分别实现了自己的 `eat` 方法。此时针对某一个实例，我们无需了解它是 `Cat` 还是 `Dog`，就可以直接调用 `eat` 方法，程序会自动判断出来应该如何执行 `eat`

存取器（getter & setter）：用以改变属性的读取和赋值行为

修饰符（Modifiers）：修饰符是一些关键字，用于限定成员或类型的性质。比如 `public` 表示公有属性或方法

抽象类（Abstract Class）：抽象类是供其他类继承的基类，抽象类不允许被实例化。抽象类中的抽象方法必须在子类中被实现

接口（Interfaces）：不同类之间公有的属性或方法，可以抽象成一个接口。接口可以被类实现（implements）。一个类只能继承自另一个类，但是可以实现多个接口

#### 属性和方法

使用 `class` 定义类，使用 `constructor` 定义构造函数。

通过 `new` 生成新实例的时候，会自动调用构造函数。

```tsx
class Animal {
    constructor(name) {
        this.name = name;
    }
    sayHi() {
        return `My name is ${this.name}`;
    }
}

let a = new Animal('thebs');
console.log(a.sayHi()); // My name is thebs
```

#### 类的继承

使用 `extends` 关键字实现继承，子类中使用 `super` 关键字来调用父类的构造函数和方法。

```tsx
class Cat extends Animal {
    constructor(name) {
        super(name); // 调用父类的 constructor(name)
        console.log(this.name);
    }
    sayHi() {
        return 'Meow, ' + super.sayHi(); // 调用父类的 sayHi()
    }
}

let c = new Cat('thebs'); // thebs
console.log(c.sayHi()); // Meow, My name is thebs
```

#### 存取器

使用 getter 和 setter 可以改变属性的赋值和读取行为

```tsx
class Animal {
    constructor(name) {
        this.name = name;
    }
    get name() {
        return 'thebs';
    }
    set name(value) {
        console.log('setter: ' + value);
    }
}

let a = new Animal('lulu'); // setter: lulu
a.name = 'pixar'; // setter: pixar
console.log(a.name); // pixar
```

#### 静态方法

使用 `static` 修饰符修饰的方法称为静态方法，它们不需要实例化，而是直接通过类来调用：

```tsx
class Animal {
    static isAnimal(a) {
        return a instanceof Animal;
    }
}

let a = new Animal('thebs');
Animal.isAnimal(a); // true
a.isAnimal(a); // TypeError: a.isAnimal is not a function
```

#### TS 中类的用法

TypeScript 可以使用三种访问修饰符（Access Modifiers），分别是 `public`、`private` 和 `protected`。

- `public` 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 `public` 的
- `private` 修饰的属性或方法是私有的，不能在声明它的类的外部访问
- `protected` 修饰的属性或方法是受保护的，它和 `private` 类似，区别是它在子类中也是允许被访问的

```tsx
class Animal {
    private name;
    public constructor(name) {
        this.name = name;
    }
}

let a = new Animal('thebs');
console.log(a.name); // thebs
a.name = 'pixar';

// index.ts(9,13): error TS2341: Property 'name' is private and only accessible within class 'Animal'.
// index.ts(10,1): error TS2341: Property 'name' is private and only accessible within class 'Animal'.

class Animal {
    protected name;
    public constructor(name) {
        this.name = name;
    }
}

// 如果是用 protected 修饰，则允许在子类中访问
class Cat extends Animal {
    constructor(name) {
        super(name);
        console.log(this.name);
    }
}
```

需要注意的是，TypeScript 编译之后的代码中，并没有限制 `private` 属性在外部的可访问性。

#### readonly

只读属性关键字，只允许出现在属性声明或索引签名或构造函数中

```tsx
class Animal {
    readonly name;
    public constructor(name) {
        this.name = name;
    }
}

let a = new Animal('thebs');
console.log(a.name); // thebs
a.name = 'pixar';

// index.ts(10,3): TS2540: Cannot assign to 'name' because it is a read-only property.

```

注意如果 `readonly` 和其他访问修饰符同时存在的话，需要写在其后面。

```tsx
class Animal {
    // public readonly name;
    public constructor(public readonly name) {
        // this.name = name;
    }
}
```

#### 抽象类

`abstract` 用于定义抽象类和其中的抽象方法

- 抽象类是不允许被实例化的
- 抽象类中的抽象方法必须被子类实现

```tsx
abstract class Animal {
    public name;
    public constructor(name) {
        this.name = name;
    }
    public abstract sayHi();
}

class Cat extends Animal {
    public sayHi() {
        console.log(`Meow, My name is ${this.name}`);
    }
}

let cat = new Cat('thebs');
```



---

### 接口

TypeScript 使用接口（Interfaces）来定义对象的类型

#### 基本属性

- 具体如何行动需要由类（classes）去实现（implement）
- 除了可用于对类的一部分行为进行抽象以外，也常用于对 对象的形状（Shape）进行描述
- 定义的属性数量和类型必须保持一致，属性先后定义顺序不做限制
- 接口一般首字母大写。有些编程语言会建议接口的名称加上 `I` 前缀

#### 可选属性

- 在定义的属性名称后加上`?` 定义为可选属性，对象可不带该属性

#### 任意属性

- 使用 `[propName: string]` 定义了任意属性取 `string` 类型的值
- 初始化的对象可以添加任意属于该定义子集的属性

#### 只读属性

- 使用 readonly 来定义只读属性`readonly id: number;`
- 初始化定义对象只读属性将是不可改变的



---

### 类与接口

#### 类实现接口

实现（implements）是面向对象中的一个重要概念。一般来讲，一个类只能继承自另一个类，有时候不同类之间可以有一些共有的特性，这时候就可以把特性提取成接口（interfaces），用 `implements` 关键字来实现。这个特性大大提高了面向对象的灵活性。

```tsx
interface Alarm {
    alert(): void;
}

class Door {
}

// 继承自 门 的 防盗门 具有 报警 接口
class SecurityDoor extends Door implements Alarm {
    alert() {
        console.log('SecurityDoor alert');
    }
}

// 汽车 同样可以实现 报警 接口
class Car implements Alarm {
    alert() {
        console.log('Car alert');
    }
}

```

同一个类可以实现多个接口

```tsx
interface Alarm {
    alert(): void;
}

interface Light {
    lightOn(): void;
    lightOff(): void;
}

// 汽车可以实现 报警 和 开关车灯 的功能
class Car implements Alarm, Light {
    alert() {
        console.log('Car alert');
    }
    lightOn() {
        console.log('Car light on');
    }
    lightOff() {
        console.log('Car light off');
    }
}
```

#### 接口继承接口

```tsx
interface Alarm {
    alert(): void;
}

// 灯光报警 继承自 报警 接口，并实现开关功能
interface LightableAlarm extends Alarm {
    lightOn(): void;
    lightOff(): void;
}
```

#### 接口继承类

不同其它面向对象语言接口不可继承类，实际上 TypeScripts 在声明 `class Point` 时，除了会创建一个名为 `Point` 的类之外，同时也创建了一个名为 `Point` 的类型（实例的类型），当声明 `interface Point3d extends Point` 时，`Point3d` 继承的实际上是类 `Point` 的实例的类型。

```tsx
class Point {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
}

interface Point3d extends Point {
    z: number;
}

let point3d: Point3d = {x: 1, y: 2, z: 3};
```

实际接口继承关系等价于：

```tsx
class Point {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
}

interface PointInstanceType {
    x: number;
    y: number;
}

// 等价于 interface Point3d extends PointInstanceType
interface Point3d extends Point {
    z: number;
}

let point3d: Point3d = {x: 1, y: 2, z: 3};
```

**注意**：同样的，在接口继承类的时候，也只会继承它的实例属性和实例方法。



---

### 泛型

泛型（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。

```tsx
function createArray(length: number, value: any): Array<any> {
    let result = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']

// 通过 泛型 来指定返回值的类型与输入值同为 T
function createArray<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray<string>(3, 'x'); // ['x', 'x', 'x']
```

#### 多种类型的参数

定义泛型的时候，可以使用元组一次定义多个类型的参数

```tsx
function swap<T, U>(tuple: [T, U]): [U, T] {
    return [tuple[1], tuple[0]];
}

swap([7, 'seven']); // ['seven', 7]
```

#### 泛型约束

泛型 `T` 不一定包含属性 `length`，在编译的时候会报错。

需要对泛型进行约束，只允许这个函数传入那些包含 `length` 属性的变量。

```tsx
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
}
```

#### 泛型接口

在使用泛型接口的时候，需要定义泛型的类型 T

```tsx
interface CreateArrayFunc<T> {
    (length: number, value: T): Array<T>;
}

let createArray: CreateArrayFunc<any>;
createArray = function<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```

#### 泛型类

```tsx
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };
```



---

### 声明合并

如果定义了两个相同名字的函数、接口或类，那么它们会合并成一个类型

#### 函数的合并

```tsx
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```

#### 类or接口的合并

```tsx
interface Alarm {
    price: number;
}
interface Alarm {
    price: string;  // 类型不一致，会报错
    weight: number;
}

// index.ts(5,3): error TS2403: Subsequent variable declarations must have the same type.  Variable 'price' must be of type 'number', but here has type 'string'.


```



---

### 声明文件

当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。

####ES6 新语法

```
declare var 声明全局变量
declare function 声明全局方法
declare class 声明全局类
declare enum 声明全局枚举类型
declare namespace 声明（含有子属性的）全局对象
interface 和 type 声明全局类型
export 导出变量
export namespace 导出（含有子属性的）对象
export default ES6 默认导出
export = commonjs 导出模块
export as namespace UMD 库声明全局变量
declare global 扩展全局变量
declare module 扩展模块
/// <reference /> 三斜线指令
```

#### 使用场景

全局变量：通过 `` 标签引入第三方库，注入全局变量

npm 包：通过 `import foo from 'foo'` 导入，符合 ES6 模块规范

UMD 库：既可以通过 `` 标签引入，又可以通过 `import` 导入

直接扩展全局变量：通过 `` 标签引入后，改变一个全局变量的结构

npm 包或 UMD 库中扩展全局变量：引用 npm 包或 UMD 库后，改变一个全局变量的结构

模块插件：通过 `` `或 `import` 导入后，改变另一个模块的结构

####jQuery.d.ts

声明文件必需以 `.d.ts` 为后缀。

一般来说，ts 会解析项目中所有的 `*.ts` 文件，当然也包含以 `.d.ts` 结尾的文件。

当我们将 `jQuery.d.ts` 放到项目中时，其他所有 `*.ts` 文件就都可以获得 `jQuery` 的类型定义了。



---

### 内置对象

内置对象是指根据标准在全局作用域（Global）上存在的对象

例如 `Boolean`、`Error`、`Date`、`RegExp` 等。

在 TypeScript 中可将变量定义为这些类型：

```tsx
let b: Boolean = new Boolean(1);
let e: Error = new Error('Error occurred');
let d: Date = new Date();
let r: RegExp = /[a-z]/;
```

#### TypeScripts 核心库的定义文件

https://github.com/Microsoft/TypeScript/tree/master/src/lib

#### 使用 TS 写 Nodejs

Node.js 不是内置对象的一部分，如果想用 TypeScript 写 Node.js，则需要引入第三方声明文件：

```shell
npm install @types/node --save-dev
```


---

### END










