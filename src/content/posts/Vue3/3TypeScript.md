---
title: TypeScript
published: 2023-04-21
tags: [TS, Vue3]
category: Vue3
---
# TypeScript🏫

TypeScript 简称 TS ，既是一门新语言，也是 JS 的一个超集，它是在 JavaScript 的基础上增加了一套类型系统，它支持所有的 JS 语句，为工程化开发而生，最终在编译的时候去掉类型和特有的语法，生成 JS 代码。

## 检查类型

TypeScript在编译时可以执行检查来避免类型变化导致的BUG

## Hello TypeScript

### 1.安装TS开发两个主要依赖包

- [typescript](https://www.npmjs.com/package/typescript) 这个包是用 TypeScript 编程的语言依赖包
- [ts-node](https://www.npmjs.com/package/ts-node) 是让 Node 可以运行 TypeScript 的执行环境

```
npm install -D typescript ts-node
```

这次添加了一个 `-D` 参数，因为 TypeScript 和 TS-Node 是开发过程中使用的依赖，所以将其添加到 package.json 的 `devDependencies` 字段里。

### 2.然后修改 scripts 字段

增加一个 `dev:ts` 的 script

```json
"dev:ts": "ts-node src/ts/index.ts",
```

> 请注意， `dev:ts` 这个 script 是用了 `ts-node` 来代替原来在用的 `node` ，因为使用 `node` 无法识别 TypeScript 语言。

### 3.在ts文件中编写

在 `src/ts/index.ts` 里

```tsx
// src/ts/index.ts
function getFirstWord(msg) {
  console.log(msg.split(' ')[0])
}

getFirstWord('Hello World')

getFirstWord(123)
```

### 4.运行

命令行运行 `npm run dev:ts`

```bash
TSError: ⨯ Unable to compile TypeScript:
src/ts/index.ts:1:23 - error TS7006: Parameter 'msg' implicitly has an 'any' type.

1 function getFirstWord(msg) {
                        ~~~
```

这是告知 `getFirstWord` 的入参 `msg` 带有隐式 any 类型

### 5.修正函数参数

```js
function getFirstWord(msg: string)
```

函数的入参 `msg` 已经变成了 `msg: string` ，为TypeScript 指定参数为字符串类型的一个写法。

再运行 `npm run dev:ts`，报错变成

```bash
TSError: ⨯ Unable to compile TypeScript:
src/ts/index.ts:7:14 - error TS2345:
Argument of type 'number' is not assignable to parameter of type 'string'.

7 getFirstWord(123)
               ~~~
```

这次的报错代码是在 `getFirstWord(123)` 这里，告诉 `number` 类型的数据不能分配给 `string` 类型的参数，也就是故意传入一个会报错的数值进去，被 TypeScript 检查出来了！

**没有报错的 `getFirstWord('Hello World')` 也没有打印出结果，这是因为 TypeScript 需要先被编译成 JavaScript ，然后再执行**

这个机制让有问题的代码能够被及早发现，一旦代码出现问题，编译阶段就会失败。

### 6.移除报错代码

运行 `npm run dev:ts`

```bash
npm run dev:ts

> demo@1.0.0 dev:ts
> ts-node src/ts/index.ts

Hello
```

## 常用TS类型定义

### 原始数据类型

| 原始数据类型 | JavaScript | TypeScript |
| :----------: | :--------: | :--------: |
|    字符串    |   String   |   string   |
|     数值     |   Number   |   number   |
|    布尔值    |  Boolean  |  boolean  |
|    大整数    |   BigInt   |   bigint   |
|     符号     |   Symbol   |   symbol   |
|    不存在    |    Null    |    null    |
|    未定义    | Undefined | undefined |

转为全小写即可

**实际的编程过程中，原始数据类型的类型定义是可以省略的，因为 TypeScript 会根据声明变量时赋值的类型，自动推导变量类型**

### 数组

| 数组里的数据 | 类型写法 1 |      类型写法 2      |
| :----------: | :---------: | :-------------------: |
|    字符串    |  string[]  |  Array `<string>`  |
|     数值     |  number[]  |  Array `<number>`  |
|    布尔值    |  boolean[]  |  Array `<boolean>`  |
|    大整数    |  bigint[]  |  Array `<bigint>`  |
|     符号     |  symbol[]  |  Array `<symbol>`  |
|    不存在    |   null[]   |   Array `<null>`   |
|    未定义    | undefined[] | Array `<undefined>` |

eg:

```tsx
// 字符串数组
const strs: string[] = ['Hello World', 'Hi World']

// 数值数组
const nums: number[] = [1, 2, 3]

// 布尔值数组
const bools: boolean[] = [true, true, false]
```

实际的编程过程中，如果的数组一开始就有初始数据（数组长度不为 0 ），那么 TypeScript 也会根据数组里面的项目类型，正确自动帮推导这个数组的类型

```tsx
// 这种有初始项目的数组， TS 也会推导它们的类型
const strs = ['Hello World', 'Hi World']
```

### 对象(接口)

#### 定义对象的类型

对象的类型定义有两个语法支持： `type` 和 `interface` 。

`type`

```tsx
type UserItem = {
  // ...
}
```

`interface`

```tsx
interface UserItem {
  // ...
}
```

#### 了解接口的使用

对象的类型 `interface` 也叫做接口，用来描述对象的结构。

> 对象的类型定义通常采用 Upper Camel Case 大驼峰命名法，也就是每个单词的首字母大写，例如 `UserItem` 、 `GameDetail` ，这是为了跟普通变量进行区分（变量通常使用 Lower Camel Case 小驼峰写法，也就是第一个单词的首字母小写，其他首字母大写，例如 `userItem` ）。

eg:

```tsx
// 定义用户对象的类型
interface UserItem {
  name: string
  age: number
}

// 在声明变量的时候将其关联到类型上
const petter: UserItem = {
  name: 'Petter',
  age: 20,
}
```

#### 可选的接口属性

```tsx
interface UserItem {
  name: string
  // 这个属性变成了可选
  age?: number
}

const petter: UserItem = {
  name: 'Petter',
}
```

#### 调用自身接口的属性

下面例子里的 `friendList` 属性，用户的好友列表，它就可以继续使用 `UserItem` 这个接口作为数组的类型：

```tsx
interface UserItem {
  name: string
  age: number
  enjoyFoods: string[]
  // 这个属性引用了本身的类型
  friendList: UserItem[]
}

const petter: UserItem = {
  name: 'Petter',
  age: 18,
  enjoyFoods: ['rice', 'noodle', 'pizza'],
  friendList: [
    {
      name: 'Marry',
      age: 16,
      enjoyFoods: ['pizza', 'ice cream'],
      friendList: [],
    },
    {
      name: 'Tom',
      age: 20,
      enjoyFoods: ['chicken', 'cake'],
      friendList: [],
    }
  ],
}
```

#### 接口的继承

要对用户设置管理员，管理员信息也是一个对象，但要比普通用户多一个权限级别的属性，那么就可以使用继承，它通过 `extends` 来实现：

```tsx
// 这里继承了 UserItem 的所有属性类型，并追加了一个权限等级属性
interface Admin extends UserItem {
  permissionLevel: number
}
```

可以在继承的过程中舍弃某些属性，通过 `Omit` 帮助类型来实现，`Omit` 的类型如下：

```tsx
type Omit<T, K extends string | number | symbol>
```

 `T` 代表已有的一个对象类型， `K` 代表要删除的属性名，如果只有一个属性就直接是一个字符串，如果有多个属性，用 `|` 来分隔开

```tsx
interface UserItem {
  name: string
  age: number
  enjoyFoods: string[]
  friendList?: UserItem[]
}

// 这里在继承 UserItem 类型的时候，删除了两个多余的属性
interface Admin extends Omit<UserItem, 'enjoyFoods' | 'friendList'> {
  permissionLevel: number
}

// 现在的 admin 就非常精简了
const admin: Admin = {
  name: 'Petter',
  age: 18,
  permissionLevel: 1,
}
```

### 类

```tsx
// 定义一个类
class User {
  // constructor 上的数据需要先这样定好类型
  name: string

  // 入参也要定义类型
  constructor(userName: string) {
    this.name = userName
  }

  getName() {
    console.log(this.name)
  }
}

// 通过 new 这个类得到的变量，它的类型就是这个类
const petter: User = new User('Petter')
petter.getName() // Petter
```

- 类与类之间可以继承

```tsx
// 这是一个基础类
class UserBase {
  name: string
  constructor(userName: string) {
    this.name = userName
  }
}

// 这是另外一个类，继承自基础类
class User extends UserBase {
  getName() {
    console.log(this.name)
  }
}

// 这个变量拥有上面两个类的所有属性和方法
const petter: User = new User('Petter')
petter.getName()
```

- 类可以提供给接口继承

```tsx
// 这是一个类
class UserBase {
  name: string
  constructor(userName: string) {
    this.name = userName
  }
}

// 这是一个接口，可以继承自类
interface User extends UserBase {
  age: number
}

// 这样这个变量就必须同时存在两个属性
const petter: User = {
  name: 'Petter',
  age: 18,
}
```

如果类上面本身有方法存在，接口在继承的时候也要相应的实现，当然也可以借助在 [对象（接口）](https://vue3.chengpeiquan.com/typescript.html#对象-接口) 提到的 `Omit` 帮助类型来去掉这些方法。

```tsx
// 接口继承类的时候也可以去掉类上面的方法
interface User extends Omit<UserBase, 'getName'> {
  age: number
}
```

### 联合类型

当一个变量可能出现多种类型的值的时候，可以使用联合类型来定义它，类型之间用 `|` 符号分隔。

```tsx
// 可以在 demo 里运行这段代码
function counter(count: number | string) {
  console.log(`The current count is: ${count}.`)
}

// 不论传数值还是字符串，都可以达到的目的
counter(1)  // The current count is: 1.
counter('2')  // The current count is: 2.
```

> 注意在上面 `counter` 函数的 `console.log` 语句里，使用了一个 `<code>````</code>`符号来定义字符串，这是 ES6 语法里的 [模板字符串](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Template_literals) ，它和传统的单引号 / 双引号相比更为灵活，特别是遇到字符串需要配合多变量拼接和换行的情况。

### 函数

#### 函数的基本的写法

JS中

```js
// 注意：这是 JavaScript 代码

// 写法一：函数声明
function sum1(x, y) {
  return x + y
}

// 写法二：函数表达式
const sum2 = function (x, y) {
  return x + y
}

// 写法三：箭头函数
const sum3 = (x, y) => x + y

// 写法四：对象上的方法
const obj = {
  sum4(x, y) {
    return x + y
  },
}

// 还有很多……
```

TS中

```tsx
// 注意：这是 TypeScript 代码

// 写法一：函数声明
function sum1(x: number, y: number): number {
  return x + y
}

// 写法二：函数表达式
const sum2 = function(x: number, y: number): number {
  return x + y
}

// 写法三：箭头函数
const sum3 = (x: number, y: number): number => x + y

// 写法四：对象上的方法
const obj = {
  sum4(x: number, y: number): number {
    return x + y
  }
}

// 还有很多……
```

#### 函数的可选参数

有一些函数入参是可选，可以用和 [对象（接口）](https://vue3.chengpeiquan.com/typescript.html#对象-接口) 一样，用 `?` 来定义：

```tsx
// 注意 isDouble 这个入参后面有个 ? 号，表示可选
function sum(x: number, y: number, isDouble?: boolean): number {
  return isDouble ? (x + y) * 2 : x + y
}

// 这样传参都不会报错，因为第三个参数是可选的
sum(1, 2) // 3
sum(1, 2, true) // 6
```

> **需要注意的是，可选参数必须排在必传参数的后面。**

#### 无返回值的函数

用 `void` 来定义它的返回，也就是空

```tsx
// 注意这里的返回值类型
function sayHi(name: string): void {
  console.log(`Hi, ${name}!`)
}

sayHi('Petter') // Hi, Petter!
```

> `void` 和 `null` 、 `undefined` 不可以混用，如果的函数返回值类型是 `null` ，那么是真的需要 `return` 一个 `null` 值：

有时候要判断参数是否合法，不符合要求时需要提前终止执行（比如在做一些表单校验的时候），这种情况下也可以用 `void` ：

```tsx
function sayHi(name: string): void {
  // 这里判断参数不符合要求则提前终止运行，但它没有返回值
  if (!name) return

  // 否则正常运行
  console.log(`Hi, ${name}!`)
}
```

#### 异步函数的返回值

对于异步函数，需要用 `Promise<T>` 类型来定义它的返回值，这里的 `T` 是泛型，取决于的函数最终返回一个什么样的值（ `async / await` 也适用这个类型）。

这是一个异步函数，会 `resolve` 一个字符串，所以它的返回类型是 `Promise<string>` （假如没有 `resolve` 数据，那么就是 `Promise<void>` ）。

```tsx
// 注意这里的返回值类型
function queryData(): Promise<string> {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('Hello World')
    }, 3000)
  })
}

queryData().then((data) => console.log(data))
```

#### 函数本身的类型

如果确实有必要，可以这样来定义等号左边的类型

```ts
const sum: (x: number, y: number) => number = (x: number, y: number): number =>
  x + y
```

这里出现了 2 个箭头 `=>` ，注意第一个箭头是 TypeScript 的，第二个箭头是 JavaScript ES6 的。

实际上上面这句代码是分成了三部分：

1. `const sum: (x: number, y: number) => number` 是这个函数的名称和类型
2. `= (x: number, y: number)` 这里是指明了函数的入参和类型
3. `: number => x + y` 这里是函数的返回值和类型

第 2 和 3 点相信从上面的例子已经能够理解了，所以注意力放在第一点：

TypeScript 的函数类型是以 `() => void` 这样的形式来写的：左侧圆括号是函数的入参类型，如果没有参数，就只有一个圆括号，如果有参数，就按照参数的类型写进去；右侧则是函数的返回值。

由于 TypeScript 会推导函数类型，所以很少会显式的去写出来，除非在给对象定义方法：

```tsx
// 对象的接口
interface Obj {
  // 上面的方法就需要显式的定义出来
  sum: (x: number, y: number) => number
}

// 声明一个对象
const obj: Obj = {
  sum(x: number, y: number): number {
    return x + y
  }
}
```

#### 函数的重载

在调用函数时，可准确获得类型

```tsx
// 这一次用了函数重载
function greet(name: string): string  // TS 类型
function greet(name: string[]): string[]  // TS 类型
function greet(name: string | string[]) {
  if (Array.isArray(name)) {
    return name.map((n) => `Welcome, ${n}!`)
  }
  return `Welcome, ${name}!`
}

// 单个问候语，此时只有一个类型 string
const greeting = greet('Petter')
console.log(greeting) // Welcome, Petter!

// 多个问候语，此时只有一个类型 string[]
const greetings = greet(['Petter', 'Tom', 'Jimmy'])
console.log(greetings)
// [ 'Welcome, Petter!', 'Welcome, Tom!', 'Welcome, Jimmy!' ]
```

第 1 行是函数的 TS 类型，告知 TypeScript ，当入参为 `string` 类型时，返回值也是 `string` ;

第 2 行也是函数的 TS 类型，告知 TypeScript ，当入参为 `string[]` 类型时，返回值也是 `string[]` ;

第 3 行开始才是真正的函数体，这里的函数入参需要把可能涉及到的类型都写出来，用以匹配前两行的类型，并且这种情况下，函数的返回值类型可以省略，因为在第 1 、 2 行里已经定义过返回类型了。

### 任意值

不知道应该如何定义一个变量的类型， TypeScript 也允许使用任意值

`function getFirstWord(msg: any)`

> 使用 any 的目的是让在开发的过程中，可以不必在无法确认类型的地方消耗太多时间，不代表不需要注意代码的健壮性。
>
> 一旦使用了 any ，代码里的逻辑请务必考虑多种情况进行判断或者处理兼容。

### npm 包

npm 安装的包都基本自带 TS 类型了，不过也存在一些包没有默认支持 TypeScript ，比如前面提到的 [md5](https://www.npmjs.com/package/md5)

因此开源社区推出了一套 @types 类型包，专门处理这样的情况。

@types 类型包的命名格式为 `@types/<package-name>` ，也就是在原有的包名前面拼接 `@types` ，日常开发要用到的知名 npm 包都会有相应的类型包，只需要将其安装到 package.json 的 `devDependencies` 里即可解决该问题。

```bash
npm install -D @types/md5
```

再次运行就不会报错了

### 类型断言

当一个变量应用了 [联合类型](https://vue3.chengpeiquan.com/typescript.html#联合类型) 时，在某些时候如果不显式的指明其中的一种类型，可能会导致后续的代码运行报错。

这个时候就可以通过类型断言强制指定其中一种类型，以便程序顺利运行下去。

- `值 as 类型`

```tsx
const greeting = greet('Petter') as string
```

- `<类型>值`

```tsx
const greeting = <string>grett('Peter')
```

> 使用类型断言可以让 TypeScript 不检查的代码，它会认为是对的。
>
> 所以，请务必保证自己的代码真的是对的！

### 类型推论

类型推论的前提是变量在声明时有明确的值，如果一开始没有赋值，那么会被默认为 `any` 类型。

```tsx
// 此时相当于 foo: any
let foo

// 所以可以任意改变类型
foo = 1 // 1
foo = true // true
```

## 如何编译为 JavaScript 代码

### 编译单个文件

package.json 里增加一个 build script

```json
 "build": "tsc src/ts/index.ts --outDir dist",
```

命令行运行 `npm run build` 的时候，就会把 `src/ts/index.ts` 这个 TS 文件编译，并输出到项目下与 src 文件夹同级的 dist 目录下。

其中 `tsc` 是 TypeScript 用来编译文件的命令， `--outDir` 是它的一个选项，用来指定输出目录，如果不指定，则默认生成到源文件所在的目录下面

命令行运行 `npm run build`

把 `src/ts/index.ts` 这个 TS 文件编译，并输出到项目下与 src 文件夹同级的 dist 目录下。

在命令行执行 `node dist/index.js` ，像之前测试 JS 文件一样使用 `node` 命令，运行 dist 目录下的 `index.js` 文件，它可以正确运行：

### 编译多个模块

如果 `index.ts` 里引入了其他模块，此时 `index.ts` 是作为入口文件，入口文件 `import` 进来使用的模块也会被 TypeScript 一并编译

拆分一下模块，把 `greet` 函数单独抽离成一个模块文件 `src/ts/greet.ts` ：

```tsx
// src/ts/greet.ts
function greet(name: string): string
function greet(name: string[]): string[]
function greet(name: string | string[]) {
  if (Array.isArray(name)) {
    return name.map((n) => `Welcome, ${n}!`)
  }
  return `Welcome, ${name}!`
}

export default greet
```

在 `src/ts/index.ts` 这边，把这个模块导进来：

```tsx
// src/ts/index.ts
import greet from './greet'

// 单个问候语
const greeting = greet('Petter')
console.log(greeting)

// 多个问候语
const greetings = greet(['Petter', 'Tom', 'Jimmy'])
console.log(greetings)
```

依然只编译 `index.ts` ，但因为导入了 `greet.ts` ，所以 TypeScript 也会一并编译，来试一下运行 `npm run build` ， 现在 dist 目录下有两个文件

命令行执行 `node dist/index.js` ,一样可以得到正确的结果

### 修改编译后的 JS 版本

修改 package.json 里的 build script ，在原有的命令后面增加一个 `--target` 选项：

```json
 "scripts": {
    // ...
    "build": "tsc src/ts/index.ts --outDir dist --target es6"
  }
```

`--target` 选项的作用是控制编译后的 JavaScript 版本，可选的值目前有： `es3` ， `es5` ， `es6` ， `es2015` ， `es2016` ， `es2017` ， `es2018` ， `es2019` ， `es2020` ， `es2021` ， `es2022` ， `esnext` ，分别对应不同的 JS 规范

之前编译出来的 JavaScript 是 [CommonJS 规范](https://vue3.chengpeiquan.com/typescript.html#用-commonjs-设计模块) ，本次配置的是 `es6` ，这是支持 [ES Module 规范](https://vue3.chengpeiquan.com/typescript.html#用-es-module-设计模块) 的版本。

> 通常还需要配置一个 `--module` 选项，用于决定编译后是 CJS 规范还是 ESM 规范，但如果缺省，会根据 `--target` 来决定。

## 了解 tsconfig.json

全局安装TypeScript

```bash
npm install -g typescript
```

项目目录下命令行输入 `tsc --init`，会在根目录生成一个默认的 tsconfig.json 文件。

文件内容改为

```json
{
  "compilerOptions": {
    "target": "es6",
    "module": "es6",
    "outDir": "./dist"
  }
}
```

命令行执行 `tsc`，效果相同
