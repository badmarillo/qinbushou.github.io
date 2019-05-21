---
layout: post
title: "TypeScript学习小结(大雾)"
subtitle: "基本上会JS的学习TS都不会难惹，翻翻文档基本就差不多了，后面就靠多运用熟练了"
author: "marillo"
header-img: "img/about-bg.jpg"
catalog: true
tages:
     - TypeScript

---

### 安装 

TypeScript 的命令行工具安装方法如下：

```
npm install -g typescript
```

### 编译

```
tsc hello.ts
```

我们约定使用 TypeScript 编写的文件以 `.ts` 为后缀，用 TypeScript 编写 React 时，以 `.tsx` 为后缀。

```typescript
// demo.ts
function sayHello(name: string) {
    return 'Hello, ' + name
}

let name = 'yyl'
console.log(sayHello(name))
```

执行`tsc demo.ts`命令后会生成一个编译好的`demo.js`

```javascript
function sayHello(name) {
    return 'Hello, ' + name
}
var name = 'yyl'
console.log(sayHello(name))
```

TypeScript 中，使用 `:` 指定变量的类型，`:` 的前后有没有空格都可以。

TypeScript 只会进行**静态检查**，如果发现有错误，编译的时候就会报错。比如你传的name是一个数字，编辑器就会提示错误，类似`Argument of type 'number' is not assignable to parameter of type 'string'.`，不过好像需要安装以下ts代码检查插件。不过即使报错了，还是会生成编译结果。如果要在报错的时候终止 js 文件的生成，可以在 `tsconfig.json` 中配置 `noEmitOnError` 就OK了。



### 原始数据类型

JavaScript 的类型分为两种：原始数据类型（[Primitive data types](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)）和对象类型（Object types）。

原始数据类型包括：布尔值、数值、字符串、`null`、`undefined` 以及 `Symbol`

---

#### 布尔值

```typescript
let isShow: boolean = false
let isShow: boolean = Boolean(1)
```

**使用构造函数`Boolean`创造的对象不是布尔值**:

```typescript
// index.ts(1,5): error TS2322: Type 'Boolean' is not assignable to type 'boolean'. 编译报错，new Boolean()返回的是一个Boolean对象惹
let booleanObj: boolean = new Boolean(1)
```

#### 数值

```typescript
// 这里列了一些较常见的，ES6中的二进制，八进制等有兴趣的可以自己去看，会自动编译为十进制的数字
let normalNumber: number = 6
let notANumber: number = NaN
let infinityNumber: number = Infinity
```

#### 字符串

```typescript
let name: string = 'yyl'
```

#### 空值

在typescript中，可以用`void`表示没有任何返回值的函数，但是JS里没有这个概念。

声明一个`void`类型的变量没有什么用，因为你只能将它赋值为`undefined`或者`null`

```typescript
function logAny(): void {
    console.log('just a log')
}
```

#### Null 和 Undefined

```
let u: undefined = undefined
let n: null = null
```

`undefined` 类型的变量只能被赋值为 `undefined`，`null` 类型的变量只能被赋值为 `null`。

与 `void` 的区别是，`undefined` 和 `null` 是所有类型的子类型。

```typescript
// 不会报错
let num: number = undefined
```

而 `void` 类型的变量不能赋值给 `number` 类型的变量：

```typescript
let num: number = void

// index.ts(1,5): error TS2322: Type 'void' is not assignable to type 'number'.
```

...

此文终结，我觉得写这个ts的总结有点没必要，再总结下去就是一个《ts文档N号》了，感觉是无用功哦，主要是ts也不是很复杂，学起来翻翻文档就好啦，接下来就靠实际工作中运用了，多做就熟练了。希望接下来的工作里，偶们大前端活动这块能尽快用起来ts嘿嘿嘿，感受一下它滴魔力吧，它的规范、易读、易维护几个特点是很符合前端工程化的，用node写接口的时候有个数据类型检查也更放心哇。不过用ts也有一定的弊端。就是目前前端方向会ts的并不多，要是以后招人招对口的更不好招。但是即使不会，用一天的时间学习一下，接下来在工作中多做多总结也没啥问题。