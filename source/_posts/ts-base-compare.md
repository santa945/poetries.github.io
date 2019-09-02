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

## 八、类型断言

- 有时候你会遇到这样的情况，你会比 `TypeScript` 更了解某个值的详细信息。 通常这会发生在你清楚地知道一个实体具有比它现有类型更确切的类型。
- 通过类型断言这种方式可以告诉编译器，“相信我，我知道自己在干什么”。 类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 `TypeScript` 会假设你，程序员，已经进行了必须的检查。

类型断言有两种形式。 其一是“尖括号”语法：

```js
let someValue: any = 'this is a string'

let strLength: number = (<string>someValue).length
```

> 另一个为 `as` 语法：

```js
let someValue: any = 'this is a string'

let strLength: number = (someValue as string).length
```

> 两种形式是等价的。 至于使用哪个大多数情况下是凭个人喜好；然而，当你在 `TypeScript` 里使用 `JSX` 时，只有` as` 语法断言是被允许


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

## 十四、声明文件与命名空间： declare 和 namespace


```js
declare var // 声明全局变量
declare function // 声明全局方法
declare class // 声明全局类
declare enum // 声明全局枚举类型
declare global // 扩展全局变量
declare module // 扩展模块
```


## 十五、初学者的困惑

### 15.1 如何优雅的声明类型

#### 15.1.1 基础

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

#### 15.1.2 糅合

**独立声明**

> 一个 `ts` 文件只声明一个类型或者接口，文件名为需要暴露的类型名称，方便检索和管理

**就近声明**

> 当一个声明没有被外部引用或者依赖时，可以考虑就近放在使用的地方，典型的场景是 `React` 组件的 `Props` 和 `State` 的类型声明

**按职责分组**

- 在项目中，需要声明类型的可大致分为两类：一类是 `model`，也就是接口请求相关的，包括入参和出参；另一类是 `view`，界面渲染相关的。因此，我在 独立声明 的基础上，可以类型按照`model` 和 `view` 的维度进行分组，相互独立。
- 那么问题来了，如果是独立的类型声明的话，怎么把 model 的数据应用到 `view` 呢？ 可能你需要一个 `adapter` 来做类型的的转换：`DTOTypes` -> `adapter` -> `ViewTypes`, 完成类似于将接口中的字符串映射成枚举类型这之类的转换

**any**

> 当遇到确实解决不了的类型报错的时候，`as any` 能带给你不一样的快感，但是不建议使用啊


### 15.2 如何引用外部库

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

### 15.3 如何组织一个 TS 项目

- TS 项目的目录组织上，跟 JS 项目一样，补充好 types 的声明就可以了
- 需要注意的是，将你希望对外暴露的能力相关的类型声明都暴露出去，不友好的声明会让接入你项目的人非常的痛苦，同时，在 package.json 中需要指定 type 的 path, 比如："types": "dist/types/index.d.ts"
- 另外，务必加上 tslint, 更规范的去用 TS 实现功能，对于入门而言尤为重要

### 15.4 TSX 和 JSX

- 之前我们在用 `JavaScript` 写 `React` 时，对文件的扩展名没有什么特别的要求，`.js` 或者 `.jsx` 都行。
- 但在 `TypeScript` 中，如果你要使用 `JSX` 语法，就不能使用 `.ts`，必须使用 `.tsx`。如果你不知道，或者忘了这么做，那么你会在使用了 `JSX` 代码的地方收到类型报错，但代码本身怎么看都没有问题。这也是刚上手 `TypeScript + React` 时几乎每个人都会遇到的坑。
- 关于这一点，`TypeScript` 只是在官方教程的示例代码中直接用了 `*.tsx`，但并没有明确说明这一问题

### 15.5 变量的 Type 怎么找

- 上手 `TypeScript` 之后很快我们就发现，即便是原生的 `DOM`、或是 `React` 的 `API`，也经常会要我们手动指定类型。但这些结构并不是简单的 `JavaScript `原始类型，在使用 `JavaScript` 编写相关代码时候由于没有这种需要，我们也没关心过这些东西的类型，突然问起来，还真不知道这些类型叫什么名字。
- 不光是这些标准类型，同样的问题在很多第三方的库中也会遇到，比如一些组件库会检查你传入的 `Props`
- 在我看来，这中间其实缺少了一部分的文档，来指导新用户如何找到所需要的类型。既然社区没有提供，那就我来吧。
- 当然，让每个开发者都熟记所有的类型肯定是不现实的，总不能每接触一个新的库，就要去记一堆类型吧。放心，世界还是美好的，这种事情，当然是有方法的。
- 最直白的方法就是去看库的 `Types Definition`，也就是那些 `.*d.ts` 文件。如果你刚好有在用 `VS Code` 的话，有一个非常方便的操作：把鼠标移动到你想知道它类型的代码上（比如某个变量、某个函数调用，或是某个 JSX 标签、某个组件的 props），右键选择「Go to Definition」（或者光标选中后按 F12），就可以跳转到它的类型定义文件了。
- 如果你更习惯使用 VS Code 之外的编辑器，我相信时至今日，它们应该也都早就对 `TypeScript` 提供了支持。具体操作我不太熟悉，你可以自己探索下（我一直用 VS Code，其它的不太熟）
- 一般来说，这个操作可以直接把你带到你想要的地方，但考虑到类型是可以继承的，有时候一次跳转可能不太够，遇到这种情况，那就需要你随机应变一下，沿着继承关系多跳几次，直到找到你想要的内容。
- 对于不熟悉的类型，可以通过这个方法去寻找，慢慢熟悉以后，你会发现，一些常见的类型还是很好找的，稍微联想一下英文的表达方式，配合自动补全的提示，一般都不难找到

### 15.6 常见 Types 之 DOM

- `TypeScript` 自带了一些基本的类型定义，包括 ECMAScript 和 DOM 的类型定义，所有你需要的类型都可以从这里找到。如果你想做一些「纯 TypeScript 开发」的话，有这些就够了
- 比如下面这张截图，就是对 `<div>` 标签的类型定义。我们可以看到，它继承了更加通用的 `HTMLElement` 类型，并且扩展了一个即将被废弃的 `align` 属性，以及两组 `addEventListener` 和 `removeEventListener`，注意这里使用了重载。

![](https://upload-images.jianshu.io/upload_images/1480597-dcfaf93694768f45.png)

> 这里的命名也不是随便起的，都是在 MDN 上可以查到的。还是以 `<div>` 为例，我们已经知道它继承自 `HTMLElement`，其实再往上，`HTMLElement` 继承自 `Element`，`Element` 又继承自 `Node`，顺着这条路，你可以挖掘出所有 `HTML` 标签的类型
   
![](https://upload-images.jianshu.io/upload_images/1480597-6fb995a8d53ec426.png)

> 对于一些 DOM 相关的属性，比如 `onclick`、`onchange` 等，你都可以如法炮制，找到它们的定义。

### 15.7 常见 Types 之 React

- 关于 TypeScript 的问题，有不少其实是在使用第三方库的时候遇到的，React 就是其中比较典型的一个
- 其实方法都一样，只不过相关的类型定义不在 `TypeScript` 中，而是在 `@types/react` 中。
- `React` 的类型定义的名称其实也很直观，比如我们常见的 `React.Component`，在定义 `Class` 组件时，我们需要对 `Props` 和 `State` 预先进行类型定义，为什么呢？答案就在它的类型定义中

![](https://upload-images.jianshu.io/upload_images/1480597-e39fd42e624336d2.png)

- 再比如，当我们在写一些组件时，我们可能会需要向下传递 `this.props.children`，但 `children` 并没有被设为默认值，需要我们自己定义到 `props` 上，那么它的类型应该是什么呢
- 到类型定义中搜一下关键字 `children`，很快我们就找到了下面的定义

![](https://upload-images.jianshu.io/upload_images/1480597-a9af00d02766e131.png)

> 所有 `React` 中 `JSX` 所代表的内容，无论是 `render()` 的返回，还是 `children`，我们都可以定义为一个 `ReactNode`。那这个 `ReactNode` 长什么样呢？我们通过右键继续寻找

![](https://upload-images.jianshu.io/upload_images/1480597-1e3123a0c8b5e815.png)

> 看到这里，我们不光找到了我们想要的类型，还顺带明白了为什么 `render()` 可以返回 `boolean`、`null`、`undefined` 表示不渲染任何内容。
那么事件呢？当我们给组件定义事件处理函数的时候，也经常会被要求指定类型。还是老办法，找不到咱就搜，比如 `onClick` 不清楚，那我们就以它为关键字去搜

![](https://upload-images.jianshu.io/upload_images/1480597-792d8e15f6158762.png)

> 据此我们找到一个叫 `MouseEventHandler` 的定义，这名字，够直白吧。好了，我们找到想要的了。不过既然来了，不如继续看一下，看看还能发现什么。我们右键 `MouseEventHandler` 急需往下看：

![](https://upload-images.jianshu.io/upload_images/1480597-ac3c0a386998058a.png)

> 看到了吗，所有的事件处理函数都有对应的定义，每个都需要一个泛型参数，传递了事件的类型，名称也挺直白的

![](https://upload-images.jianshu.io/upload_images/1480597-726ccc12d8012f4f.png)

> 事件的类型也被我们挖出来了，以后如果需要单独定义一个事件相关的类型，就可以直接用了。以此类推，不管是什么东西的类型，都可以去它们对应的 `@types/xxx `里，按关键字搜

### 15.8 多重 extends

- 我们知道 `Interface` 是可以多继承的，`extends` 后面可以跟多个其它 `Interface`，我们不能保证被继承的多个 `Interface` 一定没有重复的属性，那么当属性重复，但类型定义不同时，最终的结果会怎么样呢？
- 在 `TypeScript` 中，`Interface` 会按照从右往左的顺序去合并多个被继承的 `Interface`，也就是说，同名属性，左边的会覆盖右边的

```js
interface A {
  value?: string
}
interface B {
  value: string
}
interface C {
  value: number
}
interface D extends A, B {}// value?: string
interface E extends B, C {}// value: string
```

### 15.9 obj[prop] 无法访问怎么办

- 有时候我们会定义一些集合型的数据，例如对象、枚举等，但在调用的时候，我们未必会直接通过 `obj.prop` 的形式去调用，可能会是以 `obj[prop]` 这种动态索引的形式去访问，但通过动态索引的方式就无法确定最终访问的元素是否存在，因此在 `TypeScript` 中，默认是不允许这种操作的
- 但这又是个非常合理，而且非常常见的场景，怎么办呢？`TypeScript` 允许为类型添加索引，以实现这一点。

```js
interface Foo {
  x: string,
  y: number
  [index: string]: string | number
}
```

- 这个方法虽然有效，但每次都要手动为类型加索引，重复多了也挺心累的。包括在一些「配置对象」中，我们甚至无法确定有哪些类型，有没有一种更加通用、更加一劳永逸的方法。
- 其实在 `TypeScript `的官方文档中就有提到这个方案，官方管它叫 `OptionBag`，大概就是指 `config`、o`ption` 等用于提供配置信息的这么一类参数。我不是很确定这到底是个常规的英文单词，还是 `TypeScript` 中特定的术语（个人感觉是前者），反正就这么个意思吧。
简单说来，我们可以定义下面这样一个类型：

```js
interface OptionBag {
  [index: string]: any
}
```

- 这是一个非常通用的结构，以字符串为键，值可以是任何类型，并且支持索引 —— 这不就是 `Object` 么。
- 之后所有需要动态索引的结构，或是作为配置对象的结构，都可以直接指定为，或是继承 `OptionBag`。这个方案以牺牲一定的类型检查为代价，换取了操作上的便利。
- 理论上讲，`OptionBag` 可以适用于所有类似对象这样的结构，但不建议各位真就这么做。这个方案只能是用在一些对类型要求不那么严格，或是无法预知类型的场景中，能够确定的类型还是尽可能地写一下，否则就失去了使用 `TypeScript` 意义了


## 十六、其他技巧

**1. 安全导航操作符 ( ?. )和非空断言操作符（!.）**

- **安全导航操作符 ( ?. ) 和空属性路径**： 

> 为了解决导航时变量值为null时，页面运行时出错的问题

- **非空断言操作符**

> 能确定变量值一定不为空时使用。与安全导航操作符不同的是，非空断言操作符不会防止出现 `null` 或 `undefined`

```
let s = e!.name; // 断言e是非空并访问name属性
```

**2. 关键字is的作用，判断一个变量属于某个接口|类型**

> typescript中有一个特殊的关键字，可以用来判断一个变量属于某个接口|类

例如，此时有一个接口A

```js
interface IAProps {
  name: string
  js: any
}
```

现在需要判断一个变量是否为该类型

定义规则：

```js
// 属于接口A
let isAProps = (props: any): props is IAProps => typeof (props as IAProps)['js'] !== 'undefined'
```


> [原文地址](https://github.com/poetries/poetries.github.io/edit/dev/source/_posts/ts-base-compare.md)

