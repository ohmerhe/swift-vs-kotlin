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



