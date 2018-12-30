---
title: Typescript总结篇（二）
date: 2018-12-30 12:30:14
tags: 
  - Typescript
  - Javascript
categories: Front-End
---


# 一、简介

## 1.1 什么是 TypeScript

> `TypeScript` 是 `JavaScript` 的一个超集，主要提供了类型系统和对 `ES6 `的支持

## 1.2 为什么选择 TypeScript

**TypeScript 增加了代码的可读性和可维护性**

- 类型系统实际上是最好的文档，大部分的函数看看类型的定义就可以知道如何使用了
- 可以在编译阶段就发现大部分错误，这总比在运行时候出错好
- 增强了编辑器和 `IDE` 的功能，包括代码补全、接口提示、跳转到定义、重构等

**TypeScript 非常包容**

- `TypeScript` 是 `JavaScript` 的超集，`.js` 文件可以直接重命名为 `.ts` 即可
- 即使不显式的定义类型，也能够自动做出类型推论
- 可以定义从简单到复杂的几乎一切类型
- 即使 `TypeScript` 编译报错，也可以生成 `JavaScript` 文件
- 兼容第三方库，即使第三方库不是用 `TypeScript` 写的，也可以编写单独的类型文件供 `TypeScript` 读取

**TypeScript 拥有活跃的社区**

- 大部分第三方库都有提供给 `TypeScript` 的类型定义文件
- `Google` 开发的` Angular2` 就是使用 `TypeScript` 编写的
- `TypeScript` 拥抱了 `ES6` 规范，也支持部分 `ESNext` 草案的规范

**TypeScript 的缺点**


- 有一定的学习成本，需要理解接口（`Interfaces`）、泛型（`Generics`）、类（`Classes`）、枚举类型（`Enums`）等前端工程师可能不是很熟悉的概念
- 短期可能会增加一些开发成本，毕竟要多写一些类型的定义，不过对于一个需要长期维护的项目，`TypeScript` 能够减少其维护成本
- 集成到构建流程需要一些工作量
- 可能和一些库结合的不是很完美

## 1.3 安装 TypeScript

**typescript 安装**

```js
npm i typescript -g
```

> 全局安装完成后，我们新建一个`hello.ts`的`ts`文件

```js
// hello.ts内容
let a = "poet"
```

> 接下来我们在命令行输入`tsc hello.ts`来编译这个`ts`文件，然后会在同级目录生成一个编译好了的`hello.js`文件

```js
// hello.js内容
var = "poet"
```

> 那么我们每次都要输`tsc hello.ts`命令来编译，这样很麻烦，能否让它自动编译？答案是可以的，使用`vscode`来开发，需要配置一下`vscode`就可以。

> 首先我们在命令行执行`tsc --init`来生成配置文件，然后我们在目录下看到生成了一个`tsconfig.json`文件

![image.png](https://upload-images.jianshu.io/upload_images/1480597-064a181236aedb92.png)

> 这个`json`文件里有很多选项

- `target`是选择编译到什么语法
- `module`则是模块类型
- `outDir`则是输出目录，可以指定这个参数到指定目录

> 更多细节 https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/tsconfig.json.html

> 接下来我们需要开启监控了，在`vscode`任务栏中

![image.png](https://upload-images.jianshu.io/upload_images/1480597-da2ea080476546f2.png)



## 1.4 Hello TypeScript

> 将以下代码复制到 `hello.ts` 中

```js
function sayHello(person: string) {
    return 'Hello, ' + person;
}

let user = 'poetries';
console.log(sayHello(user));
```

```js
tsc hello.ts
```

```js
//这时候会生成一个编译好的文件 hello.js：

function sayHello(person) {
    return 'Hello, ' + person;
}
var user = 'poetries';
console.log(sayHello(user));
```

> `TypeScript` 中，使用 `:` 指定变量的类型，`:` 的前后有没有空格都可以

- `TypeScript` 只会进行静态检查，如果发现有错误，编译的时候就会报错
- `TypeScript` 编译的时候即使报错了，还是会生成编译结果，我们仍然可以使用这个编译之后的文件

# 二、基础

## 2.1 原始数据类型

> `JavaScript` 的类型分为两种：原始数据类型（`Primitive data types`）和对象类型（`Object types`）。

- 原始数据类型包括：`布尔值`、`数值`、`字符串`、`null`、`undefined` 以及 `ES6 `中的新类型 `Symbol`。

> 本节主要介绍前五种原始数据类型在 `TypeScript` 中的应用

### 2.1.1 布尔值

> 布尔值是最基础的数据类型，在 `TypeScript` 中，使用 `boolean` 定义布尔值类型

```js
let isDone: boolean = false;

// 编译通过
// 后面约定，未强调编译错误的代码片段，默认为编译通过
```

> 注意，使用构造函数 `Boolean` 创造的对象不是布尔值

```js
let createdByNewBoolean: boolean = new Boolean(1);

// index.ts(1,5): error TS2322: Type 'Boolean' is not assignable to type 'boolean'.
// 后面约定，注释中标出了编译报错的代码片段，表示编译未通过
```

- 事实上 `new Boolean()` 返回的是一个 `Boolean` 对象：

```
let createdByNewBoolean: Boolean = new Boolean(1);
```

- 直接调用 `Boolean` 也可以返回一个 `boolean` 类型：

```
let createdByBoolean: boolean = Boolean(1);
```

- 在 `TypeScript` 中，`boolean `是 `JavaScript` 中的基本类型，而 `Boolean` 是 `JavaScript `中的构造函数。其他基本类型（除了 `null` 和 `undefined`）一样

### 2.1.2 数值

> 使用 `number` 定义数值类型

```js
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;

// ES6 中的二进制表示法
let binaryLiteral: number = 0b1010;

// ES6 中的八进制表示法
let octalLiteral: number = 0o744;
let notANumber: number = NaN;
let infinityNumber: number = Infinity;
```

```js
//编译结果：

var decLiteral = 6;
var hexLiteral = 0xf00d;
// ES6 中的二进制表示法
var binaryLiteral = 10;
// ES6 中的八进制表示法
var octalLiteral = 484;
var notANumber = NaN;
var infinityNumber = Infinity;
```

> 其中 `0b101`0 和 `0o744 `是 `ES6` 中的二进制和八进制表示法，它们会被编译为十进制数字

### 2.1.3 字符串

> 使用 `string` 定义字符串类型：

```js
let myName: string = 'Tom';
let myAge: number = 25;

// 模板字符串
let sentence: string = `Hello, my name is ${myName}.
I'll be ${myAge + 1} years old next month.`;
```

### 2.1.4 空值

> `JavaScript` 没有空值（`Void`）的概念，在 `TypeScript` 中，可以用 `void` 表示没有任何返回值的函数

```js
function alertName(): void {
    alert('My name is Tom');
}
```

> 声明一个 `void` 类型的变量没有什么用，因为你只能将它赋值为 `undefined `和 `null`：

```
let unusable: void = undefined;
```

### 2.1.5 Null 和 Undefined

> 在 `TypeScript` 中，可以使用 `null` 和 `undefined `来定义这两个原始数据类型：

```js
let u: undefined = undefined;
let n: null = null;
```

> `undefined` 类型的变量只能被赋值为 `undefined`，`null` 类型的变量只能被赋值为 `null`

- 与 `void` 的区别是，`undefined `和 `null` 是所有类型的子类型。也就是说 `undefined` 类型的变量，可以赋值给 `number` 类型的变量

```js
// 这样不会报错
let num: number = undefined;

// 这样也不会报错
let u: undefined;
let num: number = u;
```

> 而 `void` 类型的变量不能赋值给 `number` 类型的变量：

```js
let u: void;
let num: number = u;

// index.ts(2,5): error TS2322: Type 'void' is not assignable to type 'number'.
```


## 2.2 任意值Any

> 如果是一个普通类型，在赋值过程中改变类型是不被允许的

```js
let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```

> 但如果是 `any` 类型，则允许被赋值为任意类型。

```js
let myFavoriteNumber: any = 'seven';
myFavoriteNumber = 7;
```

**任意值的属性和方法**

在任意值上访问任何属性都是允许的：

```js
let anyThing: any = 'hello';

console.log(anyThing.myName);
console.log(anyThing.myName.firstName);
```

**也允许调用任何方法**：

```js
let anyThing: any = 'Tom';

anyThing.setName('Jerry');
anyThing.setName('Jerry').sayHello();
anyThing.myName.setFirstName('Cat');
```

> 可以认为，声明一个变量为任意值之后，对它的任何操作，返回的内容的类型都是任意值

**未声明类型的变量**

> 变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型：

```js
let something;
something = 'seven';
something = 7;

something.setName('Tom');
```

等价于

```js
let something: any;
something = 'seven';
something = 7;

something.setName('Tom');
```



## 2.3 类型推论

> 如果没有明确的指定类型，那么 `TypeScript` 会依照类型推论（`Type Inference`）的规则推断出一个类型

**什么是类型推论**

以下代码虽然没有指定类型，但是会在编译的时候报错：

```
let myFavoriteNumber = 'seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```

事实上，它等价于：

```
let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```

`TypeScript` 会在没有明确的指定类型的时候推测出一个类型，这就是类型推论

**如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 any 类型而完全不被类型检查**

```js
let myFavoriteNumber;

myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```



## 2.4 联合类型

> 联合类型（`Union Types`）表示取值可以为多种类型中的一种

```js
// 简单例子

let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

```js
let myFavoriteNumber: string | number;
myFavoriteNumber = true;

// index.ts(2,1): error TS2322: Type 'boolean' is not assignable to type 'string | number'.
//   Type 'boolean' is not assignable to type 'number'.
```

- 联合类型使用 `|` 分隔每个类型。
- 这里的 `let myFavoriteNumber: string | number` 的含义是，允许 `myFavoriteNumber` 的类型是 `string` 或者 `number`，但是不能是其他类型

**访问联合类型的属性或方法**

> 当 `TypeScript` 不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里共有的属性或方法

```js
function getLength(something: string | number): number {
    return something.length;
}

// length 不是 string 和 number 的共有属性，所以会报错
// index.ts(2,22): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
```

> 访问 `string` 和 `number` 的共有属性是没问题的

```js
function getString(something: string | number): string {
    return something.toString();
}
```

> 联合类型的变量在被赋值的时候，会根据类型推论的规则推断出一个类型

```js
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';

console.log(myFavoriteNumber.length); // 5

myFavoriteNumber = 7;
console.log(myFavoriteNumber.length); // 编译时报错

// index.ts(5,30): error TS2339: Property 'length' does not exist on type 'number'.
```

- 上例中，第二行的 `myFavoriteNumber` 被推断成了 `string`，访问它的 `length` 属性不会报错。
- 而第四行的 `myFavoriteNumber` 被推断成了 `number`，访问它的 `length` 属性时就报错了



## 2.5 对象的类型——接口

### 2.5.1 简单例子

> 在 `TypeScript` 中，我们使用接口（`Interfaces`）来定义对象的类型

**什么是接口**

- 在面向对象语言中，接口（`Interfaces`）是一个很重要的概念，它是对行为的抽象，而具体如何行动需要由类（`classes`）去实现（`implements`）。
- `TypeScript` 中的接口是一个非常灵活的概念，除了可用于对类的一部分行为进行抽象以外，也常用于对「对象的形状（`Shape`）」进行描述。

接口一般首字母大写

```js
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};
```

> 上面的例子中，我们定义了一个接口 `Person`，接着定义了一个变量 `tom`，它的类型是 `Person`。这样，我们就约束了 `tom` 的形状必须和接口 `Person` 一致


**定义的变量比接口少了一些属性是不允许的**


```js
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom'
};

// index.ts(6,5): error TS2322: Type '{ name: string; }' is not assignable to type 'Person'.
//   Property 'age' is missing in type '{ name: string; }'.
```

**多一些属性也是不允许的**

```js
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};

// index.ts(9,5): error TS2322: Type '{ name: string; age: number; gender: string; }' is not assignable to type 'Person'.
//   Object literal may only specify known properties, and 'gender' does not exist in type 'Person'.
```


> 可见，赋值的时候，变量的形状必须和接口的形状保持一致。


### 2.5.2 可选属性

> 有时我们希望不要完全匹配一个形状，那么可以用可选属性

可选属性的含义是该属性可以不存在

```js
interface Person {
    name: string;
    age?: number;
}

let tom: Person = {
    name: 'Tom'
};
```


```js
interface Person {
    name: string;
    age?: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};
```

### 2.5.3 任意属性

> 有时候我们希望一个接口允许有任意的属性，可以使用如下方式


```js
interface Person {
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    name: 'Tom',
    gender: 'male'
};
```

- 使用 `[propName: string]` 定义了任意属性取 `string` 类型的值
- 需要注意的是，**一旦定义了任意属性，那么确定属性和可选属性都必须是它的子属性**

```js
interface Person {
    name: string;
    age?: number;
    [propName: string]: string;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};

// index.ts(3,5): error TS2411: Property 'age' of type 'number' is not assignable to string index type 'string'.
// index.ts(7,5): error TS2322: Type '{ [x: string]: string | number; name: string; age: number; gender: string; }' is not assignable to type 'Person'.
//   Index signatures are incompatible.
//     Type 'string | number' is not assignable to type 'string'.
//       Type 'number' is not assignable to type 'string'.
```

- 上例中，任意属性的值允许是 `string`，但是可选属性 `age` 的值却是 `number`，`number `不是 `string` 的子属性，所以报错了。
- 另外，在报错信息中可以看出，此时 `{ name: 'Tom', age: 25, gender: 'male' } `的类型被推断成了 `{ [x: string]: string | number; name: string; age: number; gender: string; }`，这是联合类型和接口的结合

### 2.5.4 只读属性

> 有时候我们希望对象中的一些字段只能在创建的时候被赋值，那么可以用 `readonly `定义只读属性

```js
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    id: 89757,
    name: 'Tom',
    gender: 'male'
};

tom.id = 9527;

// index.ts(14,5): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
```

> 上例中，使用 `readonly` 定义的属性 `id` 初始化后，又被赋值了，所以报错了

**注意，只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候**


```js
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

tom.id = 89757;

// index.ts(8,5): error TS2322: Type '{ name: string; gender: string; }' is not assignable to type 'Person'.
//   Property 'id' is missing in type '{ name: string; gender: string; }'.
// index.ts(13,5): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
```

- 上例中，报错信息有两处，第一处是在对 `tom` 进行赋值的时候，没有给 `id` 赋值。
- 第二处是在给 `tom.id` 赋值的时候，由于它是只读属性，所以报错了


## 2.6 数组的类型

> 在 `TypeScript` 中，数组类型有多种定义方式，比较灵活。

### 2.6.1「类型 + 方括号」表示法

> 最简单的方法是使用「类型 + 方括号」来表示数组：

```js
let fibonacci: number[] = [1, 1, 2, 3, 5];
```

> 数组的项中不允许出现其他的类型

```js
let fibonacci: number[] = [1, '1', 2, 3, 5];

// index.ts(1,5): error TS2322: Type '(number | string)[]' is not assignable to type 'number[]'.
//   Type 'number | string' is not assignable to type 'number'.
//     Type 'string' is not assignable to type 'number'.
```

- 上例中，`[1, '1', 2, 3, 5]` 的类型被推断为 `(number | string)[]`，这是联合类型和数组的结合。
- 数组的一些方法的参数也会根据数组在定义时约定的类型进行限制

```js
let fibonacci: number[] = [1, 1, 2, 3, 5];
fibonacci.push('8');

// index.ts(2,16): error TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.
```

> 上例中，`push` 方法只允许传入 `number` 类型的参数，但是却传了一个 `string` 类型的参数，所以报错了

### 2.6.2 数组泛型

> 也可以使用数组泛型（`Array Generic`）` Array<elemType> `来表示数组

```js
let fibonacci: Array<number> = [1, 1, 2, 3, 5];
```

### 2.6.3 用接口表示数组

```js
interface NumberArray {
    [index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];
```

> `NumberArray` 表示：只要 `index` 的类型是 `number`，那么值的类型必须是 `number`

### 2.6.4 any 在数组中的应用

> 一个比较常见的做法是，用 `any` 表示数组中允许出现任意类型：

```js
let list: any[] = ['poetries', 22, { website: 'http://blog.poetries.top' }];
```

### 2.6.5 类数组

> 类数组（`Array-like Object`）不是数组类型，比如 `arguments`

```js
function sum() {
    let args: number[] = arguments;
}

// index.ts(2,7): error TS2322: Type 'IArguments' is not assignable to type 'number[]'.
//   Property 'push' is missing in type 'IArguments'.
```

> 事实上常见的类数组都有自己的接口定义，如 `IArguments`, `NodeList`, `HTMLCollection` 等：

```js
function sum() {
    let args: IArguments = arguments;
}
```



## 2.7 函数的类型

### 2.7.1 函数声明

> 在 `JavaScript` 中，有两种常见的定义函数的方式——函数声明（`Function Declaration`）和函数表达式（`Function Expression`）

```js
// 函数声明（Function Declaration）
function sum(x, y) {
    return x + y;
}

// 函数表达式（Function Expression）
let mySum = function (x, y) {
    return x + y;
};
```

> 一个函数有输入和输出，要在 `TypeScript` 中对其进行约束，需要把输入和输出都考虑到，其中函数声明的类型定义较简单

```js
function sum(x: number, y: number): number {
    return x + y;
}
```

**注意，输入多余的（或者少于要求的）参数，是不被允许的：**

```js
function sum(x: number, y: number): number {
    return x + y;
}
sum(1, 2, 3);

// index.ts(4,1): error TS2346: Supplied parameters do not match any signature of call target.
```

```js
function sum(x: number, y: number): number {
    return x + y;
}
sum(1);

// index.ts(4,1): error TS2346: Supplied parameters do not match any signature of call target.
```

### 2.7.2 函数表达式

> 如果要我们现在写一个对函数表达式（`Function Expression`）的定义，可能会写成这样

```js
let mySum = function (x: number, y: number): number {
    return x + y;
};
```

> 这是可以通过编译的，不过事实上，上面的代码只对等号右侧的匿名函数进行了类型定义，而等号左边的 `mySum`，是通过赋值操作进行类型推论而推断出来的。如果需要我们手动给 `mySum` 添加类型，则应该是这样

```js
// =>左边 (x: number, y: number) 是输入类型 
// =>右边number是输出类型
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
```

**注意不要混淆了 TypeScript 中的 => 和 ES6 中的 =>**

> 在 `TypeScript` 的类型定义中，`=>` 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。



### 2.7.3 用接口定义函数的形状

> 我们也可以使用接口的方式来定义一个函数需要符合的形状

```js
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
```

**需要注意的是，可选参数必须接在必需参数后面。换句话说，可选参数后面不允许再出现必须参数了**

```js
function buildName(firstName?: string, lastName: string) {
    if (firstName) {
        return firstName + ' ' + lastName;
    } else {
        return lastName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName(undefined, 'Tom');

// index.ts(1,40): error TS1016: A required parameter cannot follow an optional parameter.
```


### 2.7.4 参数默认值


> 在 `ES6 `中，我们允许给函数的参数添加默认值，`TypeScript` 会将添加了默认值的参数识别为可选参数

```js
function buildName(firstName: string, lastName: string = 'Cat') {
    return firstName + ' ' + lastName;
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```

**此时就不受「可选参数必须接在必需参数后面」的限制了**

```js
function buildName(firstName: string = 'Tom', lastName: string) {
    return firstName + ' ' + lastName;
}
let tomcat = buildName('Tom', 'Cat');
let cat = buildName(undefined, 'Cat');
```


### 2.7.5 剩余参数

> ES6 中，可以使用 `...rest` 的方式获取函数中的剩余参数（`rest` 参数）

```js
function push(array, ...items) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a = [];
push(a, 1, 2, 3);
```

> 事实上，items 是一个数组。所以我们可以用数组的类型来定义它


```js
function push(array: any[], ...items: any[]) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a = [];
push(a, 1, 2, 3);
```

> 注意，rest 参数只能是最后一个参数

### 2.7.6 函数重载

- 重载允许一个函数接受不同数量或类型的参数时，作出不同的处理。

> 比如，我们需要实现一个函数 `reverse`，输入数字 `123` 的时候，输出反转的数字 `321`，输入字符串 `'hello'` 的时候，输出反转的字符串 `'olleh'`

**利用联合类型，我们可以这么实现**

```js
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```

> 然而这样有一个缺点，就是不能够精确的表达，输入为数字的时候，输出也应该为数字，输入为字符串的时候，输出也应该为字符串

**这时，我们可以使用重载定义多个 reverse 的函数类型**

```js
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

- 上例中，我们重复定义了多次函数 `reverse`，前几次都是函数定义，最后一次是函数实现。在编辑器的代码提示中，可以正确的看到前两个提示。

> **注意**，`TypeScript` 会优先从最前面的函数定义开始匹配，所以多个函数定义如果有包含关系，需要优先把精确的定义写在前面



## 2.8 类型断言

> 类型断言（`Type Assertion`）可以用来手动指定一个值的类型。

**语法**

```
<类型>值

// 或

值 as 类型
```

> 在 `tsx` 语法（`React` 的 `jsx` 语法的 `ts` 版）中必须用后一种

**例子：将一个联合类型的变量指定为一个更加具体的类型**


> 当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里共有的属性或方法


```js
function getLength(something: string | number): number {
    return something.length;
}

// index.ts(2,22): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
```

> 而有时候，我们确实需要在还不确定类型的时候就访问其中一个类型的属性或方法，比如

```js
function getLength(something: string | number): number {
    if (something.length) {
        return something.length;
    } else {
        return something.toString().length;
    }
}

// index.ts(2,19): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
// index.ts(3,26): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
```

> 上例中，获取 `something.length `的时候会报错

**此时可以使用类型断言，将 something 断言成 string**

```js
function getLength(something: string | number): number {
    if ((<string>something).length) {
        return (<string>something).length;
    } else {
        return something.toString().length;
    }
}
```

> 类型断言的用法如上，在需要断言的变量前加上 `<Type>` 即可

**类型断言不是类型转换，断言成一个联合类型中不存在的类型是不允许的**

```js
function toBoolean(something: string | number): boolean {
    return <boolean>something;
}

// index.ts(2,10): error TS2352: Type 'string | number' cannot be converted to type 'boolean'.
//   Type 'number' is not comparable to type 'boolean'.
```



## 2.9 声明文件

> 当使用第三方库时，我们需要引用它的声明文件

### 2.9.1 声明(declare)语句

> 假如我们想使用第三方库，比如 `jQuery`，我们通常这样获取一个 `id` 是 `foo` 的元素

```js
$('#foo');
// or
jQuery('#foo');
```

> 但是在 `TypeScript` 中，我们并不知道 `$` 或 `jQuery `是什么东西

```js
jQuery('#foo');

// index.ts(1,1): error TS2304: Cannot find name 'jQuery'.
```

> 这时，我们需要使用 `declare` 关键字来定义它的类型，帮助` TypeScript` 判断我们传入的参数类型对不对

```js
declare var jQuery: (selector: string) => any;

jQuery('#foo');
```

> `declare` 定义的类型只会用于编译时的检查，编译结果中会被删除

```js
//上例的编译结果是：

jQuery('#foo');
```

### 2.9.2 声明文件(约定.d.ts后缀)

> 通常我们会把类型声明放到一个单独的文件中，这就是声明文件


```js
// jQuery.d.ts

declare var jQuery: (string) => any;
```

- 我们约定声明文件以 `.d.ts` 为后缀。
- 然后在使用到的文件的开头，用`「三斜线指令」///`表示引用了声明文件

```js
/// <reference path="./jQuery.d.ts" />

jQuery('#foo');
```

### 2.9.3 第三方声明文件

> 当然，`jQuery` 的声明文件不需要我们定义了，已经有人帮我们定义好了：[jQuery in DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/jquery/index.d.ts)

```js
// https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/jquery/index.d.ts

// Type definitions for jquery 3.3
// Project: https://jquery.com
// Definitions by: Leonard Thieu <https://github.com/leonard-thieu>
//                 Boris Yankov <https://github.com/borisyankov>
//                 Christian Hoffmeister <https://github.com/choffmeister>
//                 Steve Fenton <https://github.com/Steve-Fenton>
//                 Diullei Gomes <https://github.com/Diullei>
//                 Tass Iliopoulos <https://github.com/tasoili>
//                 Jason Swearingen <https://github.com/jasons-novaleaf>
//                 Sean Hill <https://github.com/seanski>
//                 Guus Goossens <https://github.com/Guuz>
//                 Kelly Summerlin <https://github.com/ksummerlin>
//                 Basarat Ali Syed <https://github.com/basarat>
//                 Nicholas Wolverson <https://github.com/nwolverson>
//                 Derek Cicerone <https://github.com/derekcicerone>
//                 Andrew Gaspar <https://github.com/AndrewGaspar>
//                 Seikichi Kondo <https://github.com/seikichi>
//                 Benjamin Jackman <https://github.com/benjaminjackman>
//                 Poul Sorensen <https://github.com/s093294>
//                 Josh Strobl <https://github.com/JoshStrobl>
//                 John Reilly <https://github.com/johnnyreilly>
//                 Dick van den Brink <https://github.com/DickvdBrink>
//                 Thomas Schulz <https://github.com/King2500>
//                 Terry Mun <https://github.com/terrymun>
// Definitions: https://github.com/DefinitelyTyped/DefinitelyTyped
// TypeScript Version: 2.3

// 引入声明文件
/// <reference types="sizzle" />
/// <reference path="JQueryStatic.d.ts" />
/// <reference path="JQuery.d.ts" />
/// <reference path="misc.d.ts" />
/// <reference path="legacy.d.ts" />

export = jQuery;
```

- 我们可以直接下载下来使用，但是更推荐的是使用工具统一管理第三方库的声明文件- 社区已经有多种方式引入声明文件，不过 `TypeScript 2.0 `推荐使用 `@types` 来管理。
- `@types` 的使用方式很简单，直接用 `npm` 安装对应的声明模块即可，以 `jQuery` 举例


```bash
npm install @types/jquery --save-dev
```

**可以在这个页面搜索你需要的声明文件**

> http://microsoft.github.io/TypeSearch/


## 2.10 内置对象


> `JavaScript` 中有很多内置对象，它们可以直接在 `TypeScript` 中当做定义好了的类型

> 内置对象是指根据标准在全局作用域（`Global`）上存在的对象。这里的标准是指 `ECMAScript` 和其他环境（比如 `DOM`）的标准

### 2.10.1 ECMAScript 的内置对象

**ECMAScript 标准提供的内置对象有**

> `Boolean`、`Error`、`Date`、`RegExp` 等

我们可以在 `TypeScript` 中将变量定义为这些类型：

```js
let b: Boolean = new Boolean(1);

let e: Error = new Error('Error occurred');

let d: Date = new Date();

let r: RegExp = /[a-z]/;
```

> 更多的内置对象，可以查看 [MDN 的文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)

> 而他们的定义文件，则在 [TypeScript 核心库的定义文件中](https://github.com/Microsoft/TypeScript/tree/master/src/lib)

### 2.10.2 DOM 和 BOM 的内置对象

**DOM 和 BOM 提供的内置对象有**

> `Document`、`HTMLElement`、`Event`、`NodeList` 等。

> TypeScript 中会经常用到这些类型

```js
let body: HTMLElement = document.body;
let allDiv: NodeList = document.querySelectorAll('div');

document.addEventListener('click', function(e: MouseEvent) {
  // Do something
});
```

> 它们的定义文件同样在 [TypeScript 核心库的定义文件中](https://github.com/Microsoft/TypeScript/tree/master/src/lib)

### 2.10.3 TypeScript 核心库的定义文件

> [TypeScript 核心库](https://github.com/Microsoft/TypeScript/tree/master/src/lib)的定义文件中定义了所有浏览器环境需要用到的类型，并且是预置在 TypeScript 中的

> 当你在使用一些常用的方法的时候，`TypeScript` 实际上已经帮你做了很多类型判断的工作了，比如

```js
Math.pow(10, '2');

// index.ts(1,14): error TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.
```

> 上面的例子中，`Math.pow` 必须接受两个 `number` 类型的参数。事实上 `Math.pow `的类型定义如下

```js
interface Math {
    /**
     * Returns the value of a base expression taken to a specified power.
     * @param x The base value of the expression.
     * @param y The exponent value of the expression.
     */
    pow(x: number, y: number): number;
}
```

> 再举一个 `DOM` 中的例子

```js
document.addEventListener('click', function(e) {
    console.log(e.targetCurrent);
});

// index.ts(2,17): error TS2339: Property 'targetCurrent' does not exist on type 'MouseEvent'.

```

> 上面的例子中，`addEventListener` 方法是在 `TypeScript` 核心库中定义的

```js
interface Document extends Node, GlobalEventHandlers, NodeSelector, DocumentEvent {
    addEventListener(type: string, listener: (ev: MouseEvent) => any, useCapture?: boolean): void;
}
```

> 所以 `e` 被推断成了 `MouseEvent`，而 `MouseEvent` 是没有 `targetCurrent` 属性的，所以报错了

**注意，TypeScript 核心库的定义中不包含 Node.js 部分**

### 2.10.4 用 TypeScript 写 Node.js

> `Node.js` 不是内置对象的一部分，如果想用 `TypeScript` 写 `Node.js`，则需要引入第三方声明文件

```bash
npm install @types/node --save-dev
```



# 三、进阶

## 3.1 进阶

## 3.2 类型别名

## 3.3 字符串字面量类型

## 3.4 元组

## 3.5 枚举

## 3.6 类

## 3.7 类与接口

## 3.8 泛型

## 3.9 声明合并

## 3.10 扩展阅读

# 四、工程

## 4.1 代码检查

# 五、参考

- [Typescript中文网](https://www.tslang.cn/docs/handbook/typescript-in-5-minutes.html)



