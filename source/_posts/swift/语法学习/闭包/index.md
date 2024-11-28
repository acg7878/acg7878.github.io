---
title: swift语法学习：闭包
date: 2024-11-28 22:30:23
categories: 
    - swift
cover: "https://hexopicture.oss-cn-guangzhou.aliyuncs.com/blog/swift/%E8%AF%AD%E6%B3%95%E5%AD%A6%E4%B9%A0/%E9%97%AD%E5%8C%85/cover.jpeg"
---

## 闭包的基本语法
### 01 基本语法
```swift
{ (参数列表) -> 返回类型 in
    // 闭包体
}
```
说明：

- `{}`：用大括号包裹闭包体。
- `参数列表`：定义闭包可以接受的参数。参数列表可以为空，例如 ()。
- `返回类型`：用 -> 指定。如果没有返回值，可以省略返回类型或写为 Void。
- `in`：关键字，分隔参数声明与闭包主体。
- `闭包体`：闭包的主要代码逻辑部分。

### 02 完整闭包示例
```swift
let sum = { (a: Int, b: Int) -> Int in
    return a + b
}
let result = sum(3, 5)  // result = 8
```

### 03 简化语法规则
(1) 推断参数和返回值
- 如果闭包的参数类型或返回类型可以通过上下文推断，类型声明可以省略：

```swift

let sum: (Int, Int) -> Int = { a, b in
    return a + b
}
let result = sum(4, 6)  // result = 10
```
(2) 单表达式隐式返回
- 对于只有一行逻辑的闭包，可以省略 return 关键字：

```swift
let multiply: (Int, Int) -> Int = { a, b in a * b }
let result = multiply(4, 5)  // result = 20
```

(3) 参数名称的简化（$0, $1, ...）
- 如果闭包体非常简单，可以省略参数名称，直接使用系统提供的简化参数名 $0, $1（按参数顺序排列）：

```swift
let subtract: (Int, Int) -> Int = { $0 - $1 }
let result = subtract(10, 3)  // result = 7
```

(4) 尾随闭包语法
如果闭包是函数的最后一个参数，可以使用尾随闭包的语法，把闭包写在函数括号外：

```swift
func performOperation(operation: (Int, Int) -> Int) {
    let result = operation(10, 5)
    print(result)
}

// 使用尾随闭包
performOperation { $0 + $1 }  // 输出 15
```

## 二、小例子
### 写法 1: 先赋值后推断类型
```swift
let sum = { (a: Int, b: Int) -> Int in
    return a + b
}
```
**解析**：
- `let sum`：定义一个常量 sum。
- `=`：表示将右边的闭包赋值给 sum。
- 闭包：
    - `a`: Int, b: Int：定义闭包的参数及其类型。
    - `-> Int`：定义闭包的返回类型。
    - `return a + b`：闭包体的逻辑，返回两个参数的和。
- 类型推断：
    - Swift 会根据闭包的内容自动推断 `sum` 的类型为 `(Int, Int) -> Int`。
    - 这里 没有显式声明类型，而是完全依赖 Swift 的类型推断。
### 写法 2: 显式声明类型
```swift
let sum: (Int, Int) -> Int = { a, b in
    return a + b
}
```
**解析**：
- `let sum`：定义一个常量 sum。
- `: (Int, Int) -> Int`：显式声明 sum 的类型为一个接受两个 Int 参数并返回一个 Int 的闭包。
- `=`：表示将右边的闭包赋值给 sum。
- 闭包：
    - `a, b`：参数列表，类型已在显式声明中定义，可以省略类型。
    - `return a + b`：闭包体的逻辑，返回两个参数的和。
- 为什么可以省略参数类型？
- 因为 sum 的类型已经显式声明为 `(Int, Int) -> Int`，Swift 知道 a 和 b 必须是 Int 类型，因此闭包中的参数类型可以省略。