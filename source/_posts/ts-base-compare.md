---
title: Typescript基础写法对比[转]
date: 2019-08-17 11:55:24
tags: 
   - JavaScript
   - Typescript
categories: Front-End
---

> TS基础篇，思维导图 https://www.jianshu.com/p/ae20eac598e5

## 一、原始数据类型

- JavaScript 的类型分为两种：原始数据类型和对象类型。
- 原始数据类型包括：布尔值、数值、字符串、null、undefined 以及 ES6 中的新类型 Symbol
- 本节主要介绍前五种原始数据类型在 TypeScript 中的应用。
- 布尔值是最基础的数据类型，在 TypeScript 中，使用 boolean 定义布尔值类型

**正确的写法**

```js
➖➖➖➖➖➖➖➖➖布尔➖➖➖➖➖➖➖➖➖
// 布尔值
let isDone: boolean = false;  

// 事实上 `new Boolean()` 返回的是一个 `Boolean` 对象
let createdByNewBoolean: Boolean = new Boolean(1);

//(直接调用 `Boolean` 也可以返回一个 `boolean` 类型) 
let createdByBoolean: boolean = Boolean(1); 

➖➖➖➖➖➖➖➖➖数值➖➖➖➖➖➖➖➖➖
// 数值
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;

// ES6 中的二进制表示法
let binaryLiteral: number = 0b1010;

// ES6 中的八进制表示法
let octalLiteral: number = 0o744;
let notANumber: number = NaN;
let infinityNumber: number = Infinity;
➖➖➖➖➖➖➖➖➖字符串➖➖➖➖➖➖➖➖➖
let myName: string = 'Tom';
➖➖➖➖➖➖➖➖➖空值➖➖➖➖➖➖➖➖➖
// 没有返回值的函数为void
function alertName(): void {
    alert('My name is Tom');
}

//声明一个 void 类型的只能将它赋值为 undefined 和 null
let unusable: void = undefined;
➖➖➖➖➖➖➖➖➖Null 和 Undefined➖➖➖➖➖➖➖➖➖
// undefined 类型的变量只能被赋值为 undefined，null 类型的变量只能被赋值为 null
let u: undefined = undefined;
let n: null = null;
```

**错误的写法**

> 注意:正确的很好记,大多数人都会写正确的,关键是要记住这些错误的!!!

```js
➖➖➖➖➖➖➖➖➖布尔➖➖➖➖➖➖➖➖➖
// 注意，使用构造函数 `Boolean` 创造的对象不是布尔值
let createdByNewBoolean: boolean = new Boolean(1);❌

➖➖➖➖➖➖➖➖➖数值➖➖➖➖➖➖➖➖➖
let decLiteral: number = "6";❌

➖➖➖➖➖➖➖➖➖字符串➖➖➖➖➖➖➖➖➖
let myName: string = 999;❌

➖➖➖➖➖➖➖➖➖空值➖➖➖➖➖➖➖➖➖
// 没有返回值的函数为void
function alertName(): void {❌
   return 666;
}
//声明一个 void 类型的只能将它赋值为 undefined 和 null
let unusable: void = 'I love you';❌

➖➖➖➖➖➖➖➖➖Null 和 Undefined➖➖➖➖➖➖➖➖➖
// undefined 类型的变量只能被赋值为 undefined，null 类型的变量只能被赋值为 null
let u: undefined = 888;❌
let n: null = 999;❌
```

## 二、任意值

**正确的写法**

```js
// 顾名思义,可以被任何值赋值
let anyThing: any = 'hello';
let anyThing: any = 888;
let anyThing: any = true;
let anyThing: any = null;
let anyThing: any = undefined;

// 变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型：
let any;
any =true;
```

## 三、类型推论

**正确的写法**

```js
// 如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型。
let myFavoriteNumber = 'seven';  
//等价于
let myFavoriteNumber :string= 'seven';
```

**错误的写法**

```js
// 第一句已经被推论为String类型了
let myFavoriteNumber = 'seven';
myFavoriteNumber = 7;❌
```

## 四、联合类型

**正确的写法**

```js
// 联合类型（Union Types）表示取值可以为多种类型中的一种。
// 当你允许某个变量被赋值多种类型的时候,使用联合类型,管道符进行连接
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;

// 也可用于方法的参数定义, 都有toString方法,访问 string 和 number 的共有属性是没问题的
function getString(something: string | number): string {
    return something.toString();
}
```

**错误的写法**

```js
// number类型没有length属性.所以编译错误,因为我们只能访问此联合类型的所有类型里共有的属性或方法：
function getLength(something: string | number): number {❌
    return something.length;
}
```

## 五、对象的类型——接口

**正确的写法**

```js
// 赋值的时候，变量的形状必须和接口的形状保持一致(不能多也不能少,类型还必须一致)
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};


IUserInfo{
  age : any;//定义一个任何变量的 age.
  userName :string;//定义一个 username.
}
function getUserInfo(user : IUserInfo):string{
    return user.age+"======"+user.userName; 	
}
  ➖➖➖➖➖➖➖➖➖可选属性➖➖➖➖➖➖➖➖➖

interface Person {
    name: string;
    age?: number; // 表示这个属性可有可无
}

let tom: Person = {
    name: 'Tom'
};
  ➖➖➖➖➖➖➖➖➖任意属性➖➖➖➖➖➖➖➖➖

//希望一个接口允许有任意的属性，可以使用如下方式：旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集
interface Person {
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    name: 'Tom',
    gender: 'male' // 可以加其他的属性
};

➖➖➖➖➖➖➖➖➖只读属性➖➖➖➖➖➖➖➖➖
interface Person {
    readonly id: number; // 
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    id: 89757, // 只读
    name: 'Tom',
    gender: 'male'
};
```

**错误的写法**

```js
// 一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集
interface Person {
    name: string;
    age?: number;
    [propName: string]: string;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'❌
};
上例中，任意属性的值允许是 string，但是可选属性 age 的值却是 number，number 不是 string 的子属性，所以报错了。

➖➖➖➖➖➖➖➖➖只读属性➖➖➖➖➖➖➖➖➖
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    name: 'Tom',
    gender: 'male'
};

tom.id = 89757; // 不能被二次赋值❌
```

## 六、数组的类型

**正确的做法**

```js
let fibonacci: number[] = [1, 1, 2, 3, 5];
let fibonacci: Array<number> = [1, 1, 2, 3, 5];

➖➖➖➖➖➖➖➖➖用接口表示数组➖➖➖➖➖➖➖➖➖
interface NumberArray {
    [index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];

➖➖➖➖➖➖➖➖➖any 在数组中的应用➖➖➖➖➖➖➖➖➖
let list: any[] = ['Xcat Liu', 25, { website: 'http://xcatliu.com' }];

➖➖➖➖➖➖➖➖➖类数组➖➖➖➖➖➖➖➖➖
function sum() {
    let args: IArguments = arguments;
}
```

**错误的做法**

```js
// 数组的项中不允许出现其他的类型：
let fibonacci: number[] = [1, '1', 2, 3, 5];❌

// push 方法只允许传入 number 类型的参数，但是却传了一个 string 类型的参数，所以报错了。
let fibonacci: number[] = [1, 1, 2, 3, 5];
fibonacci.push('8');❌


// 类数组（Array-like Object）不是数组类型，比如 arguments
function sum() {❌
    let args: number[] = arguments;
}
```

## 七、函数的类型

**正确的做法**

```js
// 需要把输入和输出都考虑到
function sum(x: number, y: number): number {
    return x + y;
}

➖➖➖➖➖➖➖➖➖函数表达式➖➖➖➖➖➖➖➖➖
let mySum = function (x: number, y: number): number {
    return x + y;
};
// 不要混淆了 TypeScript 中的 => 和 ES6 中的 =>
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
➖➖➖➖➖➖➖➖➖接口定义函数的形状➖➖➖➖➖➖➖➖➖
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source, subString) {
    return source.search(subString) !== -1;
}

➖➖➖➖➖➖➖➖➖可选参数➖➖➖➖➖➖➖➖➖
function buildName(firstName: string, lastName?: string) {
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');


➖➖➖➖➖➖➖➖➖参数默认值➖➖➖➖➖➖➖➖➖
function buildName(firstName: string, lastName: string = 'Cat') {
    return firstName + ' ' + lastName;
}

➖➖➖➖➖➖➖➖➖剩余参数➖➖➖➖➖➖➖➖➖
// rest 参数只能是最后一个参数，关于 rest 参数,是一个数组
function push(array: any[], ...items: any[]) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a = [];
push(a, 1, 2, 3);
```

**错误的做法**

```js
// 输入多余的（或者少于要求的）参数，是不被允许的：
function sum(x: number, y: number): number {
    return x + y;
}
sum(1, 2, 3); ❌
sum(1);❌

// 输入多余的（或者少于要求的）参数，是不被允许的：
function sum(x: number, y: number): number {
    return x + y;
}
sum(1, 2, 3);

// 可选参数后面不允许再出现必须参数了：
function buildName(firstName?: string, lastName: string) {❌
    if (firstName) {
        return firstName + ' ' + lastName;
    } else {
        return lastName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName(undefined, 'Tom');
```

## 八、断言

**正确的做法**

```js
// 可以使用类型断言，将 something 断言成 string
function getLength(something: string | number): number {
    if ((<string>something).length) {
        return (<string>something).length;
    } else {
        return something.toString().length;
    }
}
```

**错误的做法**

```js
// 只能访问此联合类型的所有类型里共有的属性或方法
function getLength(something: string | number): number { ❌
    return something.length;
}
```

## 九、类型别名

**正确的做法**

```js
// 使用 type 创建类型别名,类型别名常用于联合类型
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

## 十、枚举

**正确的做法**

```js
// 枚举（Enum）类型用于取值被限定在一定范围内的场景，比如一周只能有七天	
// 枚举就是枚举值到枚举名进行反向映射

enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
console.log(Days["Sun"]); // 0
console.log(Days[0]); // 'Sun'

enum Days {Sun = 7, Mon = 1, Tue, Wed, Thu, Fri, Sat};
console.log(Days["Sun"]); // 7
```

## 十一、类

**正确的做法**

```js
➖➖➖➖➖➖➖➖➖类➖➖➖➖➖➖➖➖➖
class Animal {
    constructor(name) {
        this.name = name;
    }
    sayHi() {
        return `My name is ${this.name}`;
    }
}

let a = new Animal('Jack');
console.log(a.sayHi()); // My name is Jack
➖➖➖➖➖➖➖➖➖继承➖➖➖➖➖➖➖➖➖
class Cat extends Animal {
    constructor(name) {
        super(name); // 调用父类的 constructor(name)
        console.log(this.name);
    }
    sayHi() {
        return 'Meow, ' + super.sayHi(); // 调用父类的 sayHi()
    }
}

let c = new Cat('Tom'); // Tom
console.log(c.sayHi()); // Meow, My name is Tom
➖➖➖➖➖➖➖➖➖存储器➖➖➖➖➖➖➖➖➖
class Animal {
    constructor(name) {
        this.name = name;
    }
    get name() {
        return 'Jack';
    }
    set name(value) {
        console.log('setter: ' + value);
        this.name = value;
    }
}

let a = new Animal('Kitty'); // setter: Kitty
a.name = 'Tom'; // setter: Tom
console.log(a.name); // Jack
➖➖➖➖➖➖➖➖➖静态方法➖➖➖➖➖➖➖➖➖
class Animal {
    static isAnimal(a) {
        return a instanceof Animal;
    }
}

let a = new Animal('Jack');
Animal.isAnimal(a); // true
// 只能通过类名调用
a.isAnimal(a); // TypeError: a.isAnimal is not a function ❌
➖➖➖➖➖➖➖➖➖抽象类➖➖➖➖➖➖➖➖➖
abstract class Animal {
  abstract makeSound():void
  move():void {
    console.log('roaming the earch...')
  }
}
// 子类必须实现抽象类的抽象方法
```

## 十二、public private 和 protected

- `public` 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 `public` 的
- `private` 修饰的属性或方法是私有的，不能在声明它的类的外部访问
- `protected` 修饰的属性或方法是受保护的，它和 `private` 类似，区别是它在子类中也是允许被访问的

## 十三、泛型

> 泛型就是解决 类 接口 方法的复用性、以及对不特定数据类型的支持

**正确的做法**

```js
//只能返回string类型的数据
function getData(value:string):string{
  return value;
}

//同时返回 string类型 和number类型  （代码冗余）
function getData1(value:string):string{
  return value;
}
function getData2(value:number):number{
  return value;
}

>>>>>>>>>>使用泛型后就可以解决这个问题
// T表示泛型，具体什么类型是调用这个方法的时候决定的
// 表示参数是什么类型就返回什么类型~~~
function getData<T>(value:T):T{
  return value;
}
getData<number>(123);
getData<string>('1214231');

// 定义接口
interface ConfigFn{
    <T>(value:T):T;
}
var getData:ConfigFn=function<T>(value:T):T{
  return value;
}
getData<string>('张三');
getData<string>(1243);  //错误
```

## 十四、初学者的困惑

### 如何优雅的声明类型

#### 1.1 基础

```js
interface Basic {
  num: number;
  str: string | null;
  bol?: boolean;
}
```

> 五种 JS 值类型就声明好了。那数组、函数呢？

```js
interface Func {
  func(str: string): void;
}

interface Arr {
  str: string[];
  mixed: Array<string | number>;
  fixedStructure: [string, number];
  basics: Basic[];
}
```

> 枚举类型也是很常用的，比如声明一个状态机的各个状态

```js
enum Status {
  Draft,
  Published
}

// 也可指定值
enum Status {
  Draft = 'Draft',
  Published = 'Published'
}
```

#### 1.2 糅合

**独立声明**

> 一个 `ts` 文件只声明一个类型或者接口，文件名为需要暴露的类型名称，方便检索和管理

**就近声明**

> 当一个声明没有被外部引用或者依赖时，可以考虑就近放在使用的地方，典型的场景是 `React` 组件的 `Props` 和 `State` 的类型声明

**按职责分组**

- 在项目中，需要声明类型的可大致分为两类：一类是 `model`，也就是接口请求相关的，包括入参和出参；另一类是 `view`，界面渲染相关的。因此，我在 独立声明 的基础上，可以类型按照`model` 和 `view` 的维度进行分组，相互独立。
- 那么问题来了，如果是独立的类型声明的话，怎么把 model 的数据应用到 `view` 呢？ 可能你需要一个 `adapter` 来做类型的的转换：`DTOTypes` -> `adapter` -> `ViewTypes`, 完成类似于将接口中的字符串映射成枚举类型这之类的转换

**any**

> 当遇到确实解决不了的类型报错的时候，`as any` 能带给你不一样的快感，但是不建议使用啊


### 如何引用外部库

> 在 `JS` 中，`npm` 上有丰富的海量的库帮我们完成日常的编码，可能并不是所有的库都能完全被应用到 `TS` 中，因为有些缺少类型声明

比如，在 `TS` 中使用 `react `, 你会得到这样的一个类型检查错误：

![](https://pic2.zhimg.com/80/v2-fdfb8e5f2be67d8c978e216254b80a9d_hd.jpg)

- 因为 react 的库中并没有类型声明
- 现在比较通用的做法是，实现和类型实现独立成两个库，也就是你需要再安装类型声明的库: `@types/react`
- 当遇到上述问题的时候，尝试安装一下 `@types/[package]`
- 然而，并不是所有的库都有类型声明的实现，也会有很多不支持 TS 的存在，然而又必须得使用这个库的时候该怎么办？

**自己写声明**

> 以 `progressbar.js`为例，基本使用方法

```js
import * as ProgressBar from 'progressbar.js';

new ProgressBar.Circle(this.$progress, {
  strokeWidth: 8,
  trailColor: '#e5e4e5',
  trailWidth: 8,
  easing: 'easeInOut'
});
```

我们需要对库中暴露出的 api 去做声明，对上述例子做个分解：暴露了 Circle 类，Circle 构造函数包含两个参数，一个 HTMLElement，一个 options. OK

```js
// 首先声明一下模块：
declare module 'progressbar.js' {
  // 模块中暴露了 Circle 类
  export class Circle {
    constructor(container: HTMLElement, options: Options);
  }

  // 构造函数的 Options 需要单独声明 
  interface Options {
    easing?: string;
    strokeWidth?: number;
    trailColor?: string;
    trailWidth?: number;
  }
}
```

> 如此我们便完成了一个简单的声明，当然实际使用中的 API 肯定比上述情况复杂，根据使用情况，用了哪些 API 或者参数，就补充那些的声明即可

### 如何组织一个 TS 项目

- TS 项目的目录组织上，跟 JS 项目一样，补充好 types 的声明就可以了
- 需要注意的是，将你希望对外暴露的能力相关的类型声明都暴露出去，不友好的声明会让接入你项目的人非常的痛苦，同时，在 package.json 中需要指定 type 的 path, 比如："types": "dist/types/index.d.ts"
- 另外，务必加上 tslint, 更规范的去用 TS 实现功能，对于入门而言尤为重要


