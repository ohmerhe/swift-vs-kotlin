# Swift vs. Kotlin 漫谈系列之高阶函数

# 技术漫谈

# 技术知识

## Kotlin 

### 高阶函数

高阶函数是将函数用作参数或返回值的函数。如：

```
fun <T> lock(lock: Lock, body: () -> T): T {
    lock.lock()
    try {
        return body()
    }
    finally {
        lock.unlock()
    }
}
```

在上面的 `lock` 函数中第二个参数是一个函数类型的参数，所以该函数是一个高阶函数。

### 函数类型

我们用 `(<params>) -> <Type>` 的方式定义一个函数类型，括号中定义类函数接受的参数类型及数量，`->` 后面的类型定义了该函数的返回类型。如：

```
fun <T> max(collection: Collection<T>, less: (T, T) -> Boolean): T? {
    var max: T? = null
    for (it in collection)
        if (max == null || less(max, it))
            max = it
    return max
}
```

参数 less 的类型是 (T, T) -> Boolean，即一个接受两个类型T的参数并返回一个布尔值的函数： 如果第一个参数小于第二个那么该函数返回 true。

可以通过定义函数类型的方式来定义一个函数：

```
val compare: (x: T, y: T) -> Int = ……
```

这里参数的命名是可选的，如果为了清晰的知道每个参数的意义，建议为每个参数提供一个名字。

当然我们还可以定义一个可空函数：

```
var sum: ((Int, Int) -> Int)? = null
```

### Lambda 表达式和匿名函数

Lambda 表达式和匿名函数是一个“字面函数”，既一个没有被声明的函数，作为表达式直接传递。例如：

```
max(strings, { a, b -> a.length < b.length })
```

#### Lambda 表达式

`{<params> -> expresions}` 的形式表示一个 Lambda 表达式，Lambda 表达式用大括号括着， 参数声明放在括号内，在类型可以被推断的情况下，是可选的，函数体跟在 `->` 符号后面。如果推断出的该 Lambda 的返回类型不是 Unit，那么该 Lambda 主体中的最后一个（或可能是单个）表达式的结果作为返回值。

```
val sum = { x: Int, y: Int -> x + y }
```

或者

```
val sum: (Int, Int) -> Int = { x, y -> x + y }
```

一个 lambda 表达式只有一个参数是很常见的。 如果 Kotlin 可以自己计算出签名，它允许我们不声明唯一的参数，并且将隐含地为我们声明其名称为 it：

```
ints.filter { it > 0 } 
```

除了用最后一个表达式的值作为返回值，我们也可以手动返回：

```
ints.filter {
    val shouldFilter = it > 0 
    shouldFilter
}

ints.filter {
    val shouldFilter = it > 0 
    return@filter shouldFilter
}
```

> 如果一个函数接受另一个函数作为最后一个参数，lambda 表达式参数可以在圆括号参数列表之外传递。 

#### 匿名函数

Lambda 表达式不具备定义返回类型的能力，在大多数情况下，这是不必要的。因为返回类型可以自动推断出来。然而，如果确实需要显式指定，可以使用另一种语法「匿名函数」。

`fun(<parames>: <Type>) = expression/block` 的方式声明，匿名函数看起来非常像一个常规函数声明，除了其名称省略了。

```
fun(x: Int, y: Int) = x + y

fun(x: Int, y: Int): Int {
    return x + y
}
```

Lambda表达式和匿名函数之间的另一个区别是非局部返回的行为。一个不带标签的 return 语句总是在用 fun 关键字声明的函数中返回。这意味着 lambda 表达式中的 return 将从包含它的函数返回，而匿名函数中的 return 将从匿名函数自身返回。

#### 闭包

Lambda 表达式或者匿名函数（以及局部函数和对象表达式） 可以访问其 闭包 ，即在外部作用域中声明的变量。与 Java 不同的是可以修改闭包中捕获的变量：

```
var sum = 0
ints.filter { it > 0 }.forEach {
    sum += it
}
print(sum)
```

### 带接受者的字面函数

Kotlin 提供了使用指定的 接收者对象 调用函数字面值的功能。 在函数字面值的函数体中，可以调用该接收者对象上的方法而无需任何额外的限定符。 这类似于扩展函数，它允许你在函数体内访问接收者对象的成员。

这样的函数字面值的类型是一个带有接收者的函数类型：

```
sum : Int.(other: Int) -> Int
```

该函数字面值可以这样调用，就像它是接收者对象上的一个方法一样：

```
1.sum(2)
```

当接收者类型可以从上下文推断时，lambda 表达式可以用作带接收者的函数字面值。

```
class HTML {
    fun body() { …… }
}

fun html(init: HTML.() -> Unit): HTML {
    val html = HTML()  // 创建接收者对象
    html.init()        // 将该接收者对象传给该 lambda
    return html
}


html {       // 带接收者的 lambda 由此开始
    body()   // 调用该接收者对象的一个方法
}
```

## Swift

### 函数类型

每个函数都有种特定的函数类型，函数的类型由函数的参数类型和返回类型组成。例如：

```
func addTwoInts(_ a: Int, _ b: Int) -> Int {
    return a + b
}
func multiplyTwoInts(_ a: Int, _ b: Int) -> Int {
    return a * b
}
``` 
这两个函数的类型是 (Int, Int) -> Int。

### 使用函数类型

在 Swift 中，使用函数类型就像使用其他类型一样。例如，你可以定义一个类型为函数的常量或变量，并将适当的函数赋值给它：

```
var mathFunction: (Int, Int) -> Int = addTwoInts
```
addTwoInts 和 mathFunction 有同样的类型，所以这个赋值过程在 Swift 类型检查(type-check)中是允许的。
现在，你可以用 mathFunction 来调用被赋值的函数了：

```
mathFunction = multiplyTwoInts
print("Result: \(mathFunction(2, 3))")
// Prints "Result: 6"
```

### 函数类型作为参数类型

你可以用 (Int, Int) -> Int 这样的函数类型作为另一个函数的参数类型。这样你可以将函数的一部分实现留给函数的调用者来提供。

```
func printMathResult(_ mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    print("Result: \(mathFunction(a, b))")
}
printMathResult(addTwoInts, 3, 5)
// 打印 "Result: 8"
```
### 函数类型作为返回类型

你可以用函数类型作为另一个函数的返回类型。你需要做的是在返回箭头（->）后写一个完整的函数类型。

```
func stepForward(_ input: Int) -> Int {
    return input + 1
}
func stepBackward(_ input: Int) -> Int {
    return input - 1
}
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    return backward ? stepBackward : stepForward
}
var currentValue = 3
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
// moveNearerToZero 现在指向 stepBackward() 函数。
```

### 嵌套函数

你也可以把函数定义在别的函数体中，称作 嵌套函数（nested functions）。

你可以用返回嵌套函数的方式重写 chooseStepFunction(backward:) 函数：

```
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1 }
    func stepBackward(input: Int) -> Int { return input - 1 }
    return backward ? stepBackward : stepForward
}
```

### 闭包表达式

闭包是自包含的函数代码块，可以在代码中被传递和使用。Swift 中的闭包与 C 和 Objective-C 中的代码块（blocks）以及其他一些编程语言中的匿名函数比较相似。
闭包可以捕获和存储其所在上下文中任意常量和变量的引用。

闭包表达式的形式如下：

```
{ (parameters) -> return type in
    statements
}
```

下面的闭包表达式示例使用 sorted(by:) 方法对一个 String 类型的数组进行字母逆序排序。

```
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
var reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
    return s1 > s2
})
```

### 根据上下文推断类型

实际上，通过内联闭包表达式构造的闭包作为参数传递给函数或方法时，总是能够推断出闭包的参数和返回值类型。这意味着闭包作为函数或者方法的参数时，你几乎不需要利用完整格式构造内联闭包。

```
var reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )
```

### 单表达式闭包隐式返回

单行表达式闭包可以通过省略 return 关键字来隐式返回单行表达式的结果，如上版本的例子可以改写为：

```
var reversedNames = names.sorted(by: { s1, s2 in s1 > s2 } )
```

### 参数名称缩写

`Swift` 自动为内联闭包提供了参数名称缩写功能，你可以直接通过 `$0`，`$1`，`$2` 来顺序调用闭包的参数，以此类推。

如果你在闭包表达式中使用参数名称缩写，你可以在闭包定义中省略参数列表，并且对应参数名称缩写的类型会通过函数类型进行推断。`in` 关键字也同样可以被省略，因为此时闭包表达式完全由闭包函数体构成：

```
var reversedNames = names.sorted(by: { $0 > $1 } )
```
### 运算符方法

实际上还有一种更简短的方式来编写上面例子中的闭包表达式。Swift 的 String 类型定义了关于大于号（>）的字符串实现，其作为一个函数接受两个 String 类型的参数并返回 Bool 类型的值。而这正好与 sorted(by:) 方法的参数需要的函数类型相符合。因此，你可以简单地传递一个大于号，Swift 可以自动推断出你想使用大于号的字符串函数实现：

```
var reversedNames = names.sorted(by: >)
```

### 尾随闭包

尾随闭包是一个书写在函数括号之后的闭包表达式，函数支持将其作为最后一个参数调用。在使用尾随闭包时，你不用写出它的参数标签：

```
func someFunctionThatTakesAClosure(closure: () -> Void) {
    // 函数体部分
}

// 以下是不使用尾随闭包进行函数调用
someFunctionThatTakesAClosure(closure: {
    // 闭包主体部分
})

// 以下是使用尾随闭包进行函数调用
someFunctionThatTakesAClosure() {
    // 闭包主体部分
}
```

因此上面排序闭包可以采用尾随闭包改写为：

```
var reversedNames = names.sorted() { $0 > $1 }
```
如果闭包表达式是函数或方法的唯一参数，则当你使用尾随闭包时，你甚至可以把 () 省略掉：

```
var reversedNames = names.sorted { $0 > $1 }
```

### 逃逸闭包

当一个闭包作为参数传到一个函数中，但是这个闭包在函数返回之后才被执行，我们称该闭包从函数中逃逸。当你定义接受闭包作为参数的函数时，你可以在参数名之前标注 `@escaping`，用来指明这个闭包是允许“逃逸”出这个函数的。

```
var completionHandlers: [() -> Void] = []
func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
    completionHandlers.append(completionHandler)
}
```
`someFunctionWithEscapingClosure(_:)` 函数接受一个闭包作为参数，该闭包被添加到一个函数外定义的数组中。如果你不将这个参数标记为 `@escaping`，就会得到一个编译错误。
