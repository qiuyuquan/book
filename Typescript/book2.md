[测试地址](https://www.typescriptlang.org/)

## ts 基础类型
```
<!-- boolean类型 -->
let isBoolean: boolean = false;

<!-- Number类型 -->
let count: number = 0

<!-- String类型 -->
let name: string = 'Gworld'

<!-- Array类型 -->
let list: number[] = [1, 2, 3]

<!-- Enum类型 -->
enum Categroy {
  green,
  red,
  white
}
let color: Categroy = Categroy.green // color = 0

<!-- Tuple元祖类型 -->
let tupleType = [string, number]
let arr: tupleType = ['小明', 18]

<!-- Void类型，表示没有任何类型，函数没有返回值的类型就是void -->
function setAge(age: number): void {
  console.log(age)
}

<!-- Null和Undefined类型 -->
let u: undefined = undefined
let n: null = null
```

## 类型断言
类型断言的意思是开发者明确知道这个是什么类型，比如
```
function test(list) {
  if ((list as Array).length) {
    console.log(list.length)
  } else {
    console.log(list)
  }
}

let d = [1, 2, 3]
test(d)
test('gworld')
```

类型断言分为两种形式
1. 尖括号形式
```
let value: any = [1, 2, 3]
let valueLength: number = <string>value.length
```

2. as 关键字
```
let value: any = 'gworld'
let valueLength: number = (value as string).length
```

## 联合类型和类型别名
**联合类型**
取值可以为多种类型中的一种，有点类似||
```
let type: 'string' | number = 123
```

**类型别名**
类型的另一个外号，用type定义
```
<!-- 不是用类型别名 -->
let greet = (message: string | number[]) => {}

<!-- 类型别名 -->
let Message = string | number[]
let greet = message: Message => {}
```

类型别名与interfance有什么区别

## 交叉类型
同时满足多个类型，多种类型叠加成为一种类型，要包含所需所有的特性。
```
interface IInfo {
  id: string
  age: number
}

interface IWorker {
  companyId: string
}

type IPerson = IInfo & IWorker

const person: Iprops = {
  id: '123',
  age: 18,
  companyId: '456'
}
```

## 函数
```
function person(age: number, name: string): string {
  return name
}

const person = (age: number, name: string, sex?: string): string => name
```

**函数重载**
根据参数的类型执行不同的函数
```
<!-- 声明 -->
function add(arg1: string, arg2: string): string
function add(arg1: number, arg2: number): string

<!-- 实现 -->
function add(arg1: number|string, arg2: number|string) {}
```
什么情况下使用，比如限制参数A为number时，不需要传递参数B，参数A为string，需要传递参数B

## 数组
```
const arr: number[] = [1, 2, 3]

const arr: (number | string)[] = [1, '2', 3]

const arr: {name: string, age: number}[] = [{name: 'gwrold', age: 18}]

<!-- 元祖 -->
const arr: [string, string, number] = ['name', 'age', 18]
```

## 接口
```
interface Person {
  readonly id: number
  name: string
  age: number
  sex?: string
  [propName: sting]: any
}

let ject: Person = {
  id: 123,
  name: 'jack',
  age: 18,
  sex: '女',
  title: 'test'
}
```

## 泛型
类型不明确，但是要求其中一部分或者全部类型是一致的情况

```
function test<T>(arg: T): T {
  return arg
}

interface DataProps<T> {
  code: number,
  message: string,
  data: T
}

interface InfoProps {
  name: string
  age: number
}

const data1: DataIProps<InfoProps> = {
  code: 0,
  message: '请求成功',
  data: {
    name: 'gworld',
    age: 100
  }
}

const data2: DataIProps<null> = {
  code: 0,
  message: '请求成功',
  data: null
}
```

## 泛型工具函数
内置的一些常用工具函数

**typeof**
typescript中可以用typeof获取一个变量声明或对象的类型
```
interface Person {
  name: string
  age: number
}

const sem: Person = {name: 'gworld', age: 18}
type sem = typeof sem // -> Person

function getArray(x: number): Array<number> {
  return [x]
}

type Func = typeof getArray(1) // -> (x: number) => number[]
```

**keyof**
用来获取一个对象中的所有key值
```
interface Person {
  name: string
  age: number
}

type p1 = keyof Person // "name" | "age"
type p2 = keyof Person[] // "length" | "toString" | "pop" | "concat" | "join"
```

**in**
in用来遍历枚举类型
```
type keys = "a" | "b" | "c"

type obj = {
  [p in keys]: number
} // -> {a: any, b: any, c: any}
```

**extends**
有时候我们定义的泛型不想过于灵活，或者想继承某些类时，通过extends关键字添加泛型约束
```
<!-- 未加泛型约束时，可任意传值 -->
function getPerson<T>(arg: T): T {
  return arg
}
getPerson(1)
getPerson('name')

interface Person {
  name: string;
}
function getPerson:<T extends Person>(arg:T): T {
  console.log(arg.name)
  return arg
}

<!-- 加了泛型约束后，就不能随意传入值，必须包含必须的属性 -->
getPerson({name: 'gwrold', age: 18})
```

**Partial**
将某个类型的属性全部变为可选项？
```
<!-- 实现 -->
type Partial<T> = {
  [P in keyof T]?: T[p]
}

interface Person {
  name: string
  age: number
}

function addPerson(id: string, person: Partial<Person>) {
  reurn {id, ...person}
}

addPerson('123', {age: 18})

Partial<Person> = {
  name?: string | undefined
  age?: number | undefined
}
```

**Record**
将某个类型的值全部变为指定的值
```
<!-- 实现 -->
type Record<k extends keyof any, T> = {
  [P in K]: T
}

type person = Record<'name' | 'age', string>
==
type person = {
  name: string
  age: string
}
```

还有一些高级方法Required、Readonly、ReturnType等

## 参考文章
[typescript 入门教程](https://ts.xcatliu.com/advanced/class.html)
[typescript 高级技巧](https://shanyue.tech/post/ts-tips.html#_01-keyof)
[1.2W字 | 了不起的 TypeScript 入门教程](https://juejin.im/post/6844904182843965453#heading-56)
[TypeScript语法总结](https://juejin.im/post/6861525441786675208#heading-38)


