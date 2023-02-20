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
5. 函数
6. 类
7. 泛型
8. 类型检查机制
9. 高级类型
二、工程篇
三、实战篇




