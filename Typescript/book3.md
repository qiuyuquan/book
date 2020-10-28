## typescript是什么
typescript是强类型的JavaScript超集，支持es6语法，支持面向对象编程的类、接口、泛型等。typescript不能直接在浏览器上运行，需要编译成javascript运行

## typescript和javascript的区别
**typescript**
支持强类型和静态类型，支持es6，支持模块，泛型。在编译时报错，不能在浏览器上行直接运行。

**javascript**
弱类型语言，不支持es6，不支持模块，泛型。在运行时报错。

强类型: 不容忍隐式类型转换
软类型：可以容忍隐式类型转换
静态类型：编译时知道每个变量的类型，因为类型错误时导致不能运行就是语法错误。
动态类型：编译时不知道每个变量的类型，因为类型错误时导致不能运行就是运行时错误。

## 为什么要用typescript（typescript的优缺点）
**优点**
1. 编译时提供错误检查，在代码运行前会进行错误提示，减少bug产生。
2. 增强代码的可读性和可维护性，typescript是强类型语言，在编写时要求定义规范各种类型，便于维护。
3. 支持es6，提供了es6的优点和更高的生产力
4. 社区活跃，ide良好支持

**缺点**
1. 增加学习成本，需要理解接口，泛型，类等
2. 需要长时间的编译，增加开发成本
3. 使用第三方库时，需要有第三方的定义文件，但并不是所有第三方都提供了定义文件，有些需要自己编写。

## typscript有哪些基础类型
number string boolean Array symbol tuple(元祖) enum(枚举) object never void null undefined any

## typescript面向对象设计的特征
interface 接口
class 类
enum 枚举类型
generics 泛型
modules 模块

## 什么是泛型
1. 泛型是指在定义函数，接口或类的时候，不预先指定具体的类型，使用时在再去指定类型的一种特征
2. 可以把泛型理解为代表类型的参数

## ts的type和interface什么区别
**相同点** 
1. 都可以描述一个对象或者函数
```
interface User {
  name: string
  age: number
}

interface SetUser {
  (name: string, age: number): User
}

type User = {
  name: string
  age: number
}

type SetUser = (name: string, age: number) => void
```

2. 允许扩展
```
<!-- interface extends interface -->
interface Name {
  name: string
}

interface User extends Name {
  age: number
}

--------------------------
<!-- type extends type-->
type Name = {
  name: string
}

type User = Name & {age: number}

------------------------
<!-- interface extends type -->
type Name = {
  name: string
}

interface User extends Name {
  age: number
}

------------------------
<!-- type extends interface -->
interface Name {
  name: string
}
type User = Name & {age: number}
```

**不同点**
1. type可以声明基本类型别名，联合类型，元祖等
```
<!-- 基本类型别名 -->
type Name = string

<!-- 联合类型 -->
interface Dog {
  wong()
}

interface Cat {
  miao()
}

type Pet = Dog & Cat

<!-- 元祖 -->
type PetList = [Dog, Cat]
```

2. interface 可以声明合并
```
interface User {
  name: string
  age: number
}

interface User {
  sex: string
}

User {
  name: string
  age: number
  sex: string
}
```