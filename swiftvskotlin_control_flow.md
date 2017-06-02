# Swift vs. Kotlin 漫谈之控制流

## 技术漫谈

### 控制流

***Kotlin:***

上次交流意犹未尽，今天我们继续煮酒论道吧

***Swift:***

好，那我们今天就切磋一下控制流吧， 你们是怎么做条件判断的？

***Kotlin:***

我们用 `if` 做条件判断，一般传统的形式是这样的：

```
var max: Int
if (a > b) {
    max = a
} else {
    max = b
}
```

***Swift:***

嗯，这与我们一样，我们可以省略 `if` 后面的小括号 `（）`

***Kotlin:***

但是我们 `if` 语句可以用作表达式，可以有值返回，如 `val b = if (a > 0) a else 0`

***Swift:***

这也没啥，与我们的三目运算符如出一辙，`var b = a > 0 ? a : 0`
半斤八两，没啥好炫耀的.

***Kotlin:***

额。。。好吧，那我们 `if` 做表达式的时候后面也可以接代码块，在块代码中随后一行的输出会作为该块代码的返回值，如下：

```
val max = if (a > b) {
    print("Choose a")
    a
} else {
    print("Choose b")
    b
}
```
你行吗？

***Swift:***

这。。。，算你牛×

***Kotlin:***

哈哈，服了吧，需要注意的是，`if` 在用作表达式的时候，后面必须提供 `else` 从句。

***Swift:***

哦，我们的 `if` 语句和大多数语言类似，没有啥花哨的东西，但是我们除了 `if` 做条件判断外，还可以用 `guard` 语句, 形式如下：

```
guard <condition> else {
    <statements>
}

```

***Kotlin:***

这看着和 `if` 语句差不多嘛，有啥区别呢？

***Swift:***

举个栗子你就明白了

```
if 版本：
func greet(person: [String: String]) {
	if let name = person["name"] {
		print("Hello \(name)")
		
		if let location = person["location"] {
			print("weather is nice in \(location).")
		} else {
			print("weather is nice near you.")
		}
	} else {
		print("Hello stranger")
	}
}

guard 版本：
func greet(person: [String: String]) {
    guard let name = person["name"] else {
        print("Hello stranger")
        return
    }
    
    print("Hello \(name)")
    
    guard let location = person["location"] else {
        print("weather is nice near you.")
        return
    }
    
    print("weather is nice in \(location).")
}

```

对比可以看出，`guard` 版本更清晰易读，`if` 版本的一连串的 `if/else` 让人头晕。

***Kotlin:***

赞，说白了，`guard` 就是把不符合条件的处理事件前置，提前退出，避免了代码金字塔的出现，使代码看起来更优雅，更易读。不过跟你说个小秘密，我们自己封装个类似 `guard` 的函数。

***Swift:***

2333，不过我们使用 `guard` 语句的时候也必须要有 `else` 从句

***Kotlin:***

嗯嗯，来说说你们是怎么做条件匹配的

***Swift:***

我们用 `switch` 语句来进行条件匹配，例如：

```
let x = 1

switch x {
case 1:
    print("x == 1")
case 2:
    print("x == 2")
default:
    print("x is neither 1 nor 2")
}

```

***Kotlin:***

差不多，我们是用 `when` 来进行条件匹配，意思是一样的：

```
when (x) {
    1 -> print("x == 1")
    2 -> print("x == 2")
    else -> { // 注意这个块
        print("x is neither 1 nor 2")
    }
}
```

***Swift:***

嗯~ `o(*￣▽￣*)o`，语法不一样而已，看上去你们的更简洁一些嘛。

***Kotlin:***

当有不同的分支需要同样的处理的话，可以用逗号分隔写在一起。

```
when (x) {
    0, 1 -> print("x == 0 or x == 1")
    else -> print("otherwise")
}
```

***Swift:***

我的写法也一样：

```
switch x {
case 0, 1:
    print("x == 0 or x == 1")
default:
    print("otherwise")
}

```

当匹配到的 `case` 中的代码执行完毕后，`switch` 语句会直接退出，而不会继续执行下一个 `case` ，如果确实想执行下一个 `case`，我们也提供了相应的方法。

***Kotlin:***

这也可以？什么方法？

***Swift:***

就是显式的在当前 `case` 中使用 `fallthrough` 语句。例如：

```
let x = 5
var description = "\(x) is"
switch x {
case 2, 3, 5, 7, 11:
    description += " a prime number, and also"
    fallthrough
default:
    description += " an integer."
}
print(description)
// 输出 "5 is a prime number, and also an integer."

```

***Kotlin:***

我们没有这个功能，不过这种贯穿功能似乎也不常用嘛。

***Swift:***

是的，不常用不代表没有用嘛。`switch` 还可以匹配一个值的区间。例如：

```
switch x {
case 1..<10:
    print("single digit")
case 10...99:
    print("double digit")
default:
    print("out of range")
}

```

***Kotlin:***

`when` 也可以：

```
when (x) {
    in 1..10 -> print("x is in the range")
    in validNumbers -> print("x is valid")
    !in 10..20 -> print("x is outside the range")
    else -> print("none of the above")
}

```

***Swift:***

哇~， 你们还可以直接在一个匹配模式之前加 `!` 操作符，好强大，我们不可以。

***Kotlin:***

牛吧，不仅如此，`when` 可以不提供参数，直接顺序匹配满足条件的表达式后，执行其对应后面的表达式。

```
when {
    x.isOdd() -> print("x is odd")
    x.isEven() -> print("x is even")
    else -> print("x is funny")
}

```

***Swift:***

厉害了，我们不行，我们 `switch` 后必须要有参数。

***Kotlin:***

哈哈，服了吧

***Swift:***

不服，看来不给你点厉害的瞧瞧，不知道马王爷有几只眼！

***Kotlin:***

  (⊙ˍ⊙)
  
***Swift:***

我们可以使用元组在同一个 `switch` 语句中测试多个值。元组中的元素可以是值，也可以是区间。另外，可以使用下划线（`_`）来匹配所有可能的值。例如：

```
let somePoint = (1, 1)
switch somePoint {
case (0, 0):
    print("(0, 0) is at the origin")
case (_, 0):
    print("(\(somePoint.0), 0) is on the x-axis")
case (0, _):
    print("(0, \(somePoint.1)) is on the y-axis")
case (-2...2, -2...2):
    print("(\(somePoint.0), \(somePoint.1)) is inside the box")
default:
    print("(\(somePoint.0), \(somePoint.1)) is outside of the box")
}
// 输出 "(1, 1) is inside the box"

```

***Kotlin:***

赞！我们没有元组类型，虽然 Kotlin 也提供了类似的析构功能，但还是没有你们这么灵活，所以也就没有这种模式匹配。

***Swift:***

不仅如此，`case` 分支还允许将匹配的值绑定到一个临时的常量或变量，并且在 `case` 分支体内使用:

```
let anotherPoint = (2, 0)
switch anotherPoint {
case (let x, 0):
    print("on the x-axis with an x value of \(x)")
case (0, let y):
    print("on the y-axis with a y value of \(y)")
case let (x, y):
    print("somewhere else at (\(x), \(y))")
}
// 输出 "on the x-axis with an x value of 2"

```

***Kotlin:***

牛！

***Swift:***

`switch` 分支的模式还可以使用 `where` 语句来判断额外的条件。如下：

```
let yetAnotherPoint = (1, -1)
switch yetAnotherPoint {
case let (x, y) where x == y:
    print("(\(x), \(y)) is on the line x == y")
case let (x, y) where x == -y:
    print("(\(x), \(y)) is on the line x == -y")
case let (x, y):
    print("(\(x), \(y)) is just some arbitrary point")
}
// 输出 "(1, -1) is on the line x == -y"

```
这时候当且仅当控制表达式匹配一个 `case` 的模式且 `where` 子句的表达式为真时，`case` 中的语句才会被执行. 

***Kotlin:***

请收下我的膝盖。。。

***Swift:***

哈哈，接下来我们切磋一下循环语句吧

***Kotlin:***

好，`Kotlin` 的 `for` 循环和 `while` 循环与 `Java` 里面的语法差别不大，不过得益于 `Kotlin` 的析构功能，我们可以在迭代中方便的使用多个参数。

```
for ((index, value) in array.withIndex()) {
    println("the element at $index is $value")
}
```

***Swift:***

我们也有类似的写法。

```
for (index, value) in array.enumerated() {
    print("the element at \(index) is \(value)")
}

```

除了 `for-in`语句外， 我们也有 `while` 和 `repeat-while` 来实现循环遍历。

***Kotlin:***

你的 `repeat-while` 是不是和我们的 `do-while` 一样？

***Swift:***

是的，一模一样。

***Kotlin:***

`Kotlin` 提供了灵活的标签功能，在跳转表达式出现歧义的时候，可以支持以标签的方式制定跳转表达式要跳转的位置，如在嵌套循环中。例如：


```
loop@ for (i in 1..100) {
    for (j in 1..100) {
        if (……) break@loop
    }
}
```

***Swift:***

我们也有类似的标签功能。

```
var sum = 0

rowLoop: for row in 0..<8 {
  columnLoop: for column in 0..<8 {
    if row == column {
      continue rowLoop
    }
    sum += row * column
  }
}
```

***Kotlin:***

嗯嗯，看上去就是标签的定义方式不一样，我们要多写一个 `@`.

不过我们的标签还可以用在函数中，例如：


```
fun foo() {
    ints.forEach lit@ {
        if (it == 0) return@lit
        print(it)
    }
}
```

***Swift:***

我们不能

***Kotlin:***

甚至我们还可以使用隐式的标签。

```
fun foo() {
    ints.forEach {
        if (it == 0) return@forEach
        print(it)
    }
}
```
***Swift:***

嗯，你们的标签功能比我们强大。

***Kotlin:***

好了，今天我们就到此为止吧，和你交流还是收获颇丰的。

***Swift:***

 (＾－＾)V，青山不改，绿水长流，咱们江湖再见。

***Kotlin:***

后会有期。


## 知识点总结

### Kotlin
#### 条件判断

在 Kotlin 中用 `if` 做判断有两种用法，一种是用作传统的条件判断，另一种是做为表达式。

当用作传统的条件判断的时候，跟 Java 没有什么差异。

```
var max: Int
if (a > b) {
    max = a
} else {
    max = b
}
```

当 `if` 用作表达式的时候，可以有值返回，如 `val b = if (a > 0) a else 0`。这样的用法很像 Java 里的三目运算，所以在 Kotlin 里面就没有再单独提供三目运算了。

当 `if` 作为表达式的时候后面也可以接块代码，在块代码中随后一行的输出会作为该块代码的返回值。

```
val max = if (a > b) {
    print("Choose a")
    a
} else {
    print("Choose b")
    b
}
```

需要注意的是，在用作表达式的时候，后面必须提供 `else`。

#### when 表达式

Kotlin 用 when 表达式替代 Java 里面的 switch-case，不过不仅仅是替代，when 表达式提供了更加强大的功能。

##### 普通的条件匹配

```
when (x) {
    1 -> print("x == 1")
    2 -> print("x == 2")
    else -> { // 注意这个块
        print("x is neither 1 nor 2")
    }
}
```

when 会顺序比较所有的条件，匹配到合适的以后机会执行条件后面对应的表达式（类似 if也可以是块），当所有的条件都不满足的时候就会执行 else 后面的表达式。所有的when 表达式中都要提供 else，除非 Kotlin 能推断出不会再有其他条件存在。

当有不同的分支需要同样的处理的话，可以用逗号分隔写在一起。

```
when (x) {
    0, 1 -> print("x == 0 or x == 1")
    else -> print("otherwise")
}
```

##### 匹配任意表达式

when 不仅支持常量的条件匹配，还可以支持任意表达式的匹配，所以你可以这样肆无忌惮的使用它来匹配某个数是否在某某范围。

```
when (x) {
    in 1..10 -> print("x is in the range")
    in validNumbers -> print("x is valid")
    !in 10..20 -> print("x is outside the range")
    else -> print("none of the above")
}
```

或者匹配未知类型的类型。

```
fun hasPrefix(x: Any) = when(x) {
    is String -> x.startsWith("prefix")
    else -> false
}
```

##### 无参数匹配

when 可以不提供参数，直接顺序匹配满足条件的表达式后，执行其对应后面的表达式。

```
when {
    x.isOdd() -> print("x is odd")
    x.isEven() -> print("x is even")
    else -> print("x is funny")
}
```

#### 循环

Kotlin 的 for 循环和 while 循环和 Java 里面的语法差别不大，不过得益于 Kotlin 的析构功能，我们可以在迭代中方便的使用多个参数。

```
for ((index, value) in array.withIndex()) {
    println("the element at $index is $value")
}
```

#### 带标签的返回(return)、中断(break)和继续(continue)

Kotlin 提供了灵活的标签功能，在跳转表达式出现歧义的时候，可以支持以标签的方式制定跳转表达式要跳转的位置，如在嵌套循环中。

```
loop@ for (i in 1..100) {
    for (j in 1..100) {
        if (……) break@loop
    }
}
```

或者在函数中,

```
fun foo() {
    ints.forEach lit@ {
        if (it == 0) return@lit
        print(it)
    }
}
```

甚至我们还可以使用隐式的标签。

```
fun foo() {
    ints.forEach {
        if (it == 0) return@forEach
        print(it)
    }
}
```

### Swift

#### 条件判断

`swift` 使用 `if` 语句或 `guard` 语句来做条件判断。`if` 语句与 `c`语言等其他类型语句没有差别。`guard` 则是把不符合条件的处理事件前置，提前退出，避免了代码金字塔的出现，使代码看起来更优雅，更易读。

例如：

```
func greet(person: [String: String]) {
    guard let name = person["name"] else {
        return
    }
    
    print("Hello \(name)!")
    
    guard let location = person["location"] else {
        print("I hope the weather is nice near you.")
        return
    }
    
    print("I hope the weather is nice in \(location).")
}
 
greet(person: ["name": "John"])
// Prints "Hello John!"
// Prints "I hope the weather is nice near you."
greet(person: ["name": "Jane", "location": "Cupertino"])
// Prints "Hello Jane!"
// Prints "I hope the weather is nice in Cupertino."

```

不同于 `if` 语句，一个 `guard` 语句必须要有一个 `else` 从句与之配对。

#### Switch 语句

`swift` 用 `switch` 语句来进行条件匹配，例如：

```
let x = 1

switch x {
case 1:
    print("x == 1")
case 2:
    print("x == 2")
default:
    print("x is neither 1 nor 2")
}

```
任何类型的值都可以作为控制表达式的值。当有不同的分支需要同样的处理的话，可以将这个几个值组合成一个复合匹配，并且用逗号分开，如下：

```
switch x {
case 0, 1:
    print("x == 0 or x == 1")
default:
    print("otherwise")
}

```

##### Fallthrough 

当匹配到的 `case` 中的代码执行完毕后，`switch` 语句会直接退出，而不会继续执行下一个 `case` ，如果确实想执行下一个 `case`，需要显式的在当前 `case` 中使用 `fallthrough` 语句。例如：

```
let integerToDescribe = 5
var description = "The number \(integerToDescribe) is"
switch integerToDescribe {
case 2, 3, 5, 7, 11, 13, 17, 19:
    description += " a prime number, and also"
    fallthrough
default:
    description += " an integer."
}
print(description)
// 输出 "The number 5 is a prime number, and also an integer."
```

##### 元组

可以使用元组在同一个 `switch` 语句中测试多个值。元组中的元素可以是值，也可以是区间。另外，可以使用下划线（`_`）来匹配所有可能的值。例如：

```
let somePoint = (1, 1)
switch somePoint {
case (0, 0):
    print("(0, 0) is at the origin")
case (_, 0):
    print("(\(somePoint.0), 0) is on the x-axis")
case (0, _):
    print("(0, \(somePoint.1)) is on the y-axis")
case (-2...2, -2...2):
    print("(\(somePoint.0), \(somePoint.1)) is inside the box")
default:
    print("(\(somePoint.0), \(somePoint.1)) is outside of the box")
}
// 输出 "(1, 1) is inside the box"

```

##### 值绑定

不仅如此，`case` 分支还允许将匹配的值绑定到一个临时的常量或变量，并且在 `case` 分支体内使用:

```
let anotherPoint = (2, 0)
switch anotherPoint {
case (let x, 0):
    print("on the x-axis with an x value of \(x)")
case (0, let y):
    print("on the y-axis with a y value of \(y)")
case let (x, y):
    print("somewhere else at (\(x), \(y))")
}
// 输出 "on the x-axis with an x value of 2"

```

##### where 子句

`switch` 分支的模式可以使用 `where` 语句来判断额外的条件， 例如：

```
let yetAnotherPoint = (1, -1)
switch yetAnotherPoint {
case let (x, y) where x == y:
    print("(\(x), \(y)) is on the line x == y")
case let (x, y) where x == -y:
    print("(\(x), \(y)) is on the line x == -y")
case let (x, y):
    print("(\(x), \(y)) is just some arbitrary point")
}
// 输出 "(1, -1) is on the line x == -y"

```
这时候当且仅当控制表达式匹配一个 `case` 的模式且 `where` 子句的表达式为真时，`case` 中的语句才会被执行. 

#### 循环语句

`Swift` 中的循环语句包括：`for-in`, `while` 和 `repeat-while`, `while` 与其他语言的 `while` 无二， `repeat-while` 则与 `do-while` 一致。

`for-in` 语句的形式如下：

```
for <item> in <collection> {
    <statements>
}

```
可以使用 `for-in` 语句来遍历一个序列，例如数组中的元素、数字范围或者字符串中的字符等.

使用 `for-in`语句遍历一个 `Dictionary` 来访问其键值对，如下：

```
let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
for (animalName, legCount) in numberOfLegs {
    print("\(animalName)s have \(legCount) legs")
}

```

`for-in` 还可以用来遍历一个数字范围，例如：

```
for index in 1...10 {
    print("\(index)")
}

```

#### 带标签的语句

在`swift` 中，可以使用标签来标记一个循环语句或 `switch` 语句。对于一个 `switch` 语句，可以使用 `break` 加标签的方式，来结束这个被标记的语句。对于一个循环语句，可以使用 `break` 或 `continue` 加标签，来结束或者继续执行这条被标记的语句。

```
var sum = 0

rowLoop: for row in 0..<8 {
  columnLoop: for column in 0..<8 {
    if row == column {
      continue rowLoop
    }
    sum += row * column
  }
}
```
## 关于《Swift vs. Kotlin 漫谈》系列

《Swift vs. Kotlin 漫谈》系列是由 KotlinThree 发起的，旨在把 Swift 和 Kotlin 进行一个全面的对比，帮助 iOS 和 Android 开发对彼此的语言之间有一个更加深入的认识。如果你有兴趣可以关注我们公众号，也可以通过「原文链接」查看详情。

---
![](http://7xpox6.com1.z0.glb.clouddn.com/kotlin-three-wechat.jpg)