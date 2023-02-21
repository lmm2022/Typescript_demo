一、基础篇
1. 类型基础
● 强类型语言：不能改变变量的数据类型，除非进行强制类型转换（Java：Boolean=>int 不行；字符型=>int ASCII码强制类型转换）
● 弱类型语言：变量可以被赋予不同的数据类型（JavaScript）
● 静态类型语言：在编译阶段确定所有变量的类型
● 动态类型语言：在执行阶段确定所有变量的类型
弱类型和强类型的区分是：变量类型是否可以修改
动态类型和静态类型的区分是：在上面阶段确定所有变量的类型。
● 第一个demo：
依赖：
npm i typescript -g
npm i webpack webpack-cli webpack-dev-server -D  webpack构建环境和配置以及生产环境
npm i ts-loader typescript -D  
npm i html-webpack-plugin -D  快速生成网站首页的插件
npm i clean-webpack-plugin -D  在每次构建后清空build目录
npm i webpack-merge -D  把两个配置文件合并
2. 基本类型、
类型注解：（变量/函数）：类型
● number： 
let data: number = 123
● string： 
let data: string = "123"
● boolean：
let data: boolean = true
● array： 
let data: number[] = [1,2,3]	
let data: Array<number | string> = [1,2,3,"4"]
● tuple(元组)： 
let data: [number,string] = [1,"2"] （限定数据类型和个数，可以push添加元素但不支持访问该指定范围外）
● function： 
let add = (x: number, y: number): number => x + y  (第三个num可忽略)
let compute: (x: number, y: number) => number
compute = (a, b) => a + b
● object： 
错误：let obj: object = { x: 1, y: 2 }
正确：let obj: { x: number, y: number } = { x: 1, y: 2 }
● symbol(具有为唯一的值)：
let s1: symbol = Symbol()
let s2 = Symbol()
console.log(s1 === s2); // false
● underfined(未定义) null(没有值)：
let un: undefined = undefined
let nu: null = null
（Typescript的tsconfig中需调整strictNullChecks为false）
num = undefined
num = null
（underfined和null是所有类型的子类型）
严格类型：
let num: number | underfined | null = 123
● void(操作符，返回underfined)：
let noReturn = () => {} 没有任何返回值的类型为void
任何类型的子类型
● any(不指定为默认)：
任何类型的子类型
● never(不会有返回值或者死循环函数)：
let error = () => {
    throw new Error("error")
}
let endless = () => {
    while(true){}
}
任何类型的子类型
3. 枚举类型（应用于硬编码和一些常量）
● 数字枚举：
原理：反向映射
enum Role { data1 = 1, data2, data3 }
console.log(Role.data1, Role.data2, Role.data3); 1,2,3
(既可以用下标索引，也可以用值索引)
● 字符串枚举
● 异构枚举
enum Answer { N, Y = "Yes" }
● 枚举成员
值为只读，定义后不可修改
enum Char {
    // 常量枚举 编译阶段 编译后被移除（应用：不需要对象但需要值）
    a,
    b = Char.a,
    c = 1 + 3,
    // 变量枚举 执行阶段
    d = Math.random(),
    e = '123'.length
}
● 枚举类型
enum E { a, b }
enum F { a = 0, b }
enum E { a = "apple", b = 'banana' }
无初始值，赋值枚举，字符串枚举
4. 接口
● 对象类型接口：
interface List {
    readonly id: number, //只读属性
    name: string,
    /* [x: string]: any 第三种方法：字符串索引*/
    age?: number // 可选属性
}
interface Result { data: List[] }
function render(result: Result) {
    result.data.forEach((value) => {
        console.log(value.id, value.name);
        if (value.age) {
            console.log(value.age);
        }
        // value.id++ 只读属性不可写
    })
}
let result = {
    data: [
        { id: 1, name: 'a', sex: 'male' }, // 传入对象满足接口的必要条件
        { id: 2, name: 'b', age: 10 }
    ]
}
render(result)

// render( /*<Result> 该用法与类型断言同*/{
//     data: [
//         { id: 1, name: 'a', sex: 'male' }, // 直接传入自变量多余字段报错
//         { id: 2, name: 'b' }
//     ]
// } as Result) // 类型断言：表明知晓对象的类型为Result

interface StringArray { // 任意数字索引该接口都会得到string类型
    [index: number]: string
}
let chars: StringArray = ['a', 'b']

interface Name {
    [x: string]: string //any
    // y: number //不可取
    [z: number]: string //number //保持类型兼容性
}
● 函数类型接口：
// let add: (x: number, y: number) => number


// interface Add {
//     (x: number, y: number): number
// }


type Add = (x: number, y: number) => number


let add: Add = (a, b) => a + b


//混合类型接口
interface Lib {
    (): void
    versoin: string
    doSomeThing(): void
}


//防止暴露lib变量
function getLib() {
    let lib: Lib = (() => { }) as Lib //类型断言
    lib.versoin = '1'
    lib.doSomeThing = () => { }
    return lib
}


let lib1 = getLib() //创建实例
lib1()
lib1.doSomeThing()
let lib2 = getLib()
5. 函数
// 四种定义函数
function add1(x: number, y: number) { //在ts中实参形参必须一一对应 add(1,2)
    return x + y
}
let add2: (x: number, y: number) => number //变量定义函数类型
type add3 = (x: number, y: number) => number //类型别名定义函数类型
interface add4 { //接口定义函数类型
    (x: number, y: number): number
}
function add5(x: number, y?: number) {// 可选参数必须在必选参数后面
    return y ? x + y : x
}
function add6(x: number, y = 0, z: number, q = 1) { //必选参数前的默认值必须传underfiner
    return x + y + z + q
}
console.log(add6(1, undefined, 3));
function add7(x: number, ...rest: number[]) {
    return x + rest.reduce((val1, val2) => val1 + val2)
}
console.log(add7(1, 2, 3, 4, 5));
//函数重载 名称相同，类型或个数不同
function add8(...rest: number[]): number
function add8(...rest: string[]): string
function add8(...rest: any[]): any {
    let first = rest[0]
    if (typeof first === 'string') {
        return rest.join('')
    }
    if (typeof first === 'number') {
        return rest.reduce((val1, val2) => val1 + val2)
    }
}
console.log(add8(1, 2, 3));
console.log(add8('a', 'b', 'c'));
6. 类
● 继承和成员修饰符：
class Dog {
    // protected constructor(name: string) { 不能被实例化只能继承
    constructor(name: string) {
        this.name = name
    }
    name: string
    run() { }
    private pri() { }
    protected pro() { } //只允许在类或者子类中访问
    readonly legs: number = 4 //只读属性必须声明且初始化
    static food : string = 'bones'// 类的静态成员只能通过类名来调用，且可继承
}
// 类成员的属性都是实例属性，类成员的方法都是实例方法
console.log(Dog.prototype);
let dog = new Dog('ww')
console.log(dog);
// dog.pri() 不被允许
// dog.pro() 不被允许
console.log(Dog.food);
// console.log(dog.food); 不被允许



class Husky extends Dog {
    constructor(name: string, public color: string) {// 构造函数中加public变为实例属性 更简洁
        super(name)
        this.color = color
        // this.pri() 不被允许
        // this.pro() 可以
    }
    // color: string
}
console.log(Husky.food);
● 抽象类与多态：
abstract class Animal {
    eat() {
        console.log('eat');
    }
    abstract sleep(): void
}
// let animal = new Animal() //无法创建抽象类的实例
class Dog extends Animal {
    constructor(name: string) {
        super()
        this.name = name
    }
    name: string
    run() { }
    sleep() {
        console.log("dog sleep");
    }
}
let dog = new Dog('ww')
dog.eat()
class Cat extends Animal {
    sleep() {
        console.log('Cat sleep');
    }
}
let cat = new Cat()
let animal: Animal[] = [dog, cat]
animal.forEach(i=>{
    i.sleep()
})
class WorkFlow{
    step1(){
        return this
    }
    step2(){
        return this
    }
}
new WorkFlow().step1().step2()
class Myflow extends WorkFlow{
    next(){
        return this
    }
}
new Myflow().next().step1().next().step2()
7. 泛型
● 泛型函数与泛型接口：
● 泛型类与约束：
8. 类型检查机制
● 类型推断：
● 类型兼容性：
● 类型保护：
9. 高级类型
● 交叉类型与联合类型：
● 索引类型：
● 映射类型：
● 条件类型：
二、工程篇
三、实战篇




