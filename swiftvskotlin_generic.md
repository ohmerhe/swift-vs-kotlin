# Swift vs. Kotlin 漫谈系列之类与继承

《Swift vs. Kotlin 漫谈》系列经过一段时间已经有一些积累了，如果想要查看之前的漫谈系列，可以点击上面的 KotlinThree 关注我们的公众号，目录有之前的所有文章。

# 技术漫谈

**Swift:**

Kotlin 君，好久都没有 PK 了。

**Kotlin:**

😀，是有一段时间了，那么来吧，我们对比一下泛型的用法。

**Swift:**

Bingo，就等你这句话了。我先来，泛型是 Swift 最强大的特性之一，许多 Swift 标准库是通过泛型代码构建的。例如，Swift 的 Array 和 Dictionary 都是泛型集合。我们可以定义「泛型类型」、「泛型函数」，还可以扩展一个泛型类型。

**Kotlin:**

🤔，看来你们泛型也很强大，不要急，我们先从泛型类型说起，Kotlin 定义一个泛型类型和 Java 基本一致。

```
class Box<T>(t: T) {
    var value = t
}
```

**Swift:**

这个我们也差不多。

```
struct Stack<Element> {
    var items = [Element]()
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
}
```

**Kotlin:**

在 Kotlin 中我们用 `:` 代替extends对泛型的的类型上限进行约束。

```
class SwipeRefreshableView<T : View>{}
```

而且我们还可以用 where 进行多个类型同时约束😎。

```
fun <T> cloneWhenGreater(list: List<T>, threshold: T): List<T>
    where T : Comparable,
          T : Cloneable {
  return list.filter { it > threshold }.map { it.clone() }
}
```

**Swift:**

我们也是用 `:` 进行类型约束：

```
func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
    // function body goes here
}
```

我们也可以用 where 为泛型添加多个类型约束。

```
func allItemsMatch<C1: Container, C2: Container>
    (_ someContainer: C1, _ anotherContainer: C2) -> Bool
    where C1.Item == C2.Item, C1.Item: Equatable {
        
        // Check that both containers contain the same number of items.
        if someContainer.count != anotherContainer.count {
            return false
        }
        
        // Check each pair of items to see if they are equivalent.
        for i in 0..<someContainer.count {
            if someContainer[i] != anotherContainer[i] {
                return false
            }
        }
        
        // All items match, so return true.
        return true
}
```

我们可以扩展一个泛型类型，并用 where 为他添加约束：

```
extension Stack where Element: Equatable {
    func isTop(_ item: Element) -> Bool {
        guard let topItem = items.last else {
            return false
        }
        return topItem == item
    }
}
```

**Kotlin:**

你们的扩展的确强大，不像 Kotlin 的扩展，虽然我们已经觉得很好了，跟你们一比还太弱。

**Swift:**

我们为协议定义泛型的时候，需要用到 associatedtype 关键字，来定义一个关键类型。

```
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

**Kotlin:**

这个有什么特别吗？我们的接口和类定义泛型的时候没有差别，都是用的尖括号。

**Swift:**

在 Swift 中，为协议定义只能用 associatedtype 这种写法。

**Kotlin:**

这个有点想不明白🤔，感觉有点多此一举。

Kotlin 为泛型提供了型变注解 `out` 和 `in`，可以让泛型变得协变和逆变。

```
abstract class Source<out T> {
    abstract fun nextT(): T
}

fun demo(strs: Source<String>) {
    val objects: Source<Any> = strs // 这个没问题，因为 T 是一个 out-参数
    // ……
}
```

这里 Source<String> 是 Source<out T> 类型的子类，`out` 让泛型类型变得协变。

```
abstract class Comparable<in T> {
    abstract fun compareTo(other: T): Int
}

fun demo(x: Comparable<Number>) {
    x.compareTo(1.0) // 1.0 拥有类型 Double，它是 Number 的子类型
    // 因此，我们可以将 x 赋给类型为 Comparable <Double> 的变量
    val y: Comparable<Double> = x // OK！
}
```

这里通过 `in` 让泛型类型变得逆变。

**Swift:**

赞，Swift 中的泛型是不变的（Invariance），不支持逆变与协变，例如：

```swift
struct Thing<T> {
    var thing: T
}
var view: Thing<UIView> = Thing(thing: UIView())
var button: Thing<UIButton> = Thing(thing: UIButton())
view = button 
// 报错 error：Cannot assign value of type 'Thing<UIButton>' to type 'Thing<UIView>'
```

但是 Swift 标准库中的符合 `Collection` 协议的类型通常情况下是协变的（Convariance），例如：

```swift
var views: Array<UIView> = [UIView()]
var buttons: Array<UIButton> = [UIButton()]
views = buttons
```
这样的协变特性，目前只有系统库能享有，我们自己定义的泛型是无法做到的。

**Kotlin:**

而且不仅在定义一个泛型类型的时候我们可以用型变注解，在使用的时候也可以。

Kotlin 的 Array 类既不是协变也不是逆变的，所以下面的代码不能编译通过：

```
fun copy(from: Array<Any>, to: Array<Any>) {
    assert(from.size == to.size)
    for (i in from.indices)
        to[i] = from[i]
}
val ints: Array<Int> = arrayOf(1, 2, 3)
val any = Array<Any>(3) { "" } 
copy(ints, any) // 错误：期望 (Array<Any>, Array<Any>)
```

我们为 from 参数加上型变注解 `out`，这时 from 不仅仅是一个数组，而是一个受限制的数组：我们只可以调用返回类型为类型参数 T 的方法，如上，这意味着我们只能调用 get()。

```
fun copy(from: Array<out Any>, to: Array<Any>) {
 // ……
}
```

这个时候上面的代码就可以执行了，当然逆变也是可以的。

**Swift:**

👍，还有泛型函数的用法，我们在函数名后面添加泛型类型。

```
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

**Kotlin:**

恩，泛型函数的定义我们和 Java 一样需要把泛型类型写在方法名前面：

```
fun <T> singletonList(item: T): List<T> {
    // ……
}

fun <T> T.basicToString() : String {  // 扩展函数
    // ……
}
```

**Swift:**

有一点点差别，整体来说两边的泛型还是蛮像的，不像之前 OC 连泛型都没有。

**Kotlin:**

😄，是不是又站在鄙视链的顶端了。

**Swift:**

😂

## Kotlin

### 泛型定义

简单声明一个泛型，和Java没有什么大的区别

```
class Box<T>(t: T) {
    var value = t
}
```

### 泛型约束

和类的继承一样，Kotlin中使用:代替extends对泛型的的类型上限进行约束。

```
class SwipeRefreshableView<T : View>{}
```

不过 Kotlin 允许你可以进行多个类型的上限约束:

```
class SwipeRefreshableView<T>
    where T : View,
          T : Refreshable {
}
// 或者
fun <T> cloneWhenGreater(list: List<T>, threshold: T): List<T>
    where T : Comparable,
          T : Cloneable {
  return list.filter { it > threshold }.map { it.clone() }
}
```

### 泛型函数

不仅类可以有类型参数。函数也可以有。类型参数要放在函数名称之前：

```
fun <T> singletonList(item: T): List<T> {
    // ……
}

fun <T> T.basicToString() : String {  // 扩展函数
    // ……
}
```

调用泛型函数，在调用处函数名之后指定类型参数即可：

```
val l = singletonList<Int>(1)
```

如果 Kotlin 能够通过类型推断知道泛型的类型时，上面泛型的类型可以省略。

```
val l = singletonList(1)
```

### 型变

在 Java 中的泛型是不型变的，这意味着 List<String> 并不是 List<Object> 的子类型，如下代码会通过编译然后导致运行时异常：

```java
List<String> strs = new ArrayList<String>();
List<Object> objs = strs; // ！！！即将来临的问题的原因就在这里。Java 禁止这样！
objs.add(1); // 这里我们把一个整数放入一个字符串列表
String s = strs.get(0); // ！！！ ClassCastException：无法将整数转换为字符串
```

不过我们可以用带 extends 限定（上界）的通配符类型使得类型是协变，用带 super 限定（下界）的通配符类型使得类型是逆变。

通配符类型参数 ? extends E 表示此方法接受 E 或者 E 的 一些子类型对象的集合，而不只是 E 自身。 这意味着我们可以安全地从其中（该集合中的元素是 E 的子类的实例）读取 E，但不能写入， 因为我们不知道什么对象符合那个未知的 E 的子类型。

Joshua Bloch 称那些你只能从中读取的对象为生产者，并称那些你只能写入的对象为消费者。他建议：“为了灵活性最大化，在表示生产者或消费者的输入参数上使用通配符类型”，并提出了以下助记符：

> PECS 代表生产者-Extens，消费者-Super（Producer-Extends, Consumer-Super）。

#### 声明型变

在 Kotlin 中，在声明一个泛型类型的时候，可以用 `out` 让类型变得型变，用 `in` 让类型变的逆变。其中 `out` 和 `in` 修饰符被称为 **型变注解**。

```
abstract class Source<out T> {
    abstract fun nextT(): T
}

fun demo(strs: Source<String>) {
    val objects: Source<Any> = strs // 这个没问题，因为 T 是一个 out-参数
    // ……
}
```

```
abstract class Comparable<in T> {
    abstract fun compareTo(other: T): Int
}

fun demo(x: Comparable<Number>) {
    x.compareTo(1.0) // 1.0 拥有类型 Double，它是 Number 的子类型
    // 因此，我们可以将 x 赋给类型为 Comparable <Double> 的变量
    val y: Comparable<Double> = x // OK！
}
```

#### 使用型变（类型映射）

将类型参数 T 声明为 out 非常方便，并且能避免使用处子类型化的麻烦，但是有些类实际上不能限制为只返回 T！ 一个很好的例子是 Array：

```
class Array<T>(val size: Int) {
    fun get(index: Int): T { ///* …… */ }
    fun set(index: Int, value: T) { ///* …… */ }
}
```

该类在 T 上既不能是协变的也不能是逆变的。这造成了一些不灵活性。考虑下述函数：

```
fun copy(from: Array<Any>, to: Array<Any>) {
    assert(from.size == to.size)
    for (i in from.indices)
        to[i] = from[i]
}
```

这个函数应该将项目从一个数组复制到另一个数组。让我们尝试在实践中应用它：

```
val ints: Array<Int> = arrayOf(1, 2, 3)
val any = Array<Any>(3) { "" } 
copy(ints, any) // 错误：期望 (Array<Any>, Array<Any>)
```

于是，我们可以：

```
fun copy(from: Array<out Any>, to: Array<Any>) {
 // ……
}
```

这里发生的事情称为类型投影：我们说from不仅仅是一个数组，而是一个受限制的（投影的）数组：我们只可以调用返回类型为类型参数 T 的方法，如上，这意味着我们只能调用 get()。这就是我们的使用处型变的用法，并且是对应于 Java 的 Array<? extends Object>、 但使用更简单些的方式。

我们也可以使用 in 投影一个类型：

```
fun fill(dest: Array<in String>, value: String) {
    // ……
}
```


## Swift

泛型是 Swift 最强大的特性之一，许多 Swift 标准库是通过泛型代码构建的。例如，Swift 的 Array 和 Dictionary 都是泛型集合。

### 泛型函数

下面是一个泛型函数 `swapTwoInts(_:_:)`，用来交换两个任意相同类型的值：

```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

在这个泛型函数中，并没有指明参数的实际类型，而是使用了占位类型名 `T` 来代替实际类型名，只有函数在调用时，才能根据所传入的实际类型决定 `T` 所代表的类型.

```swift
var someInt = 3
var anotherInt = 107
swapTwoValues(&someInt, &anotherInt)
// someInt is now 107, and anotherInt is now 3
 
var someString = "hello"
var anotherString = "world"
swapTwoValues(&someString, &anotherString)
// someString is now "world", and anotherString is now "hello"
```

### 泛型类型

除了泛型函数，Swift 还允许你定义泛型类型.

现自定义一个泛型集合类————栈（Stack）：

```swift
struct Stack<Element> {
    var items = [Element]()
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
}
```
如此实现的栈，最大优势在于能够匹配任何类型，就像 `Array` 和 `Dictionary` 那样。
可以通过在尖括号中写出栈中需要存储的数据类型来创建并初始化一个 `Stack` 实例。例如，要创建一个 `String` 类型的栈，可以写成 `Stack<String>()`：

```swift
var stackOfStrings = Stack<String>()
stackOfStrings.push("uno")
stackOfStrings.push("cuatro")
// the stack now contains 2 strings
let fromTheTop = stackOfStrings.pop()
// fromTheTop is equal to "cuatro", and the stack now contains 1 strings
```

### 扩展一个泛型类型

泛型也可以像普通类型一样进行扩展，扩展泛型并不需要再定义类型参数列表，而是使用泛型已有的类型参数名称。

例如，扩展上面定义的 `Stack`，为其添加了一个名为 `topItem` 的只读计算型属性：

```swift
extension Stack {
    var topItem: Element? {
        return items.isEmpty ? nil : items[items.count - 1]
    }
}
```
### 类型约束

Swift 中可以为泛型添加类型约束，如下：

```swift
func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
    // function body goes here
}
```
该函数有两个类型参数。第一个类型参数 `T`，要求 `T` 必须是 `SomeClass` 的子类；第二个类型参数 `U`，要求 `U` 必须符合 `SomeProtocol`。

比如为上面定义的 `Stack` 添加一个方法，来判断一个元素是否在栈顶：

```swift
extension Stack {
    func isTop(_ item: Element) -> Bool {
        guard let topItem = items.last else {
            return false
        }
        return topItem == item // 编译错误！！！
    }
}
```
这里会得到了一个编译错误，因为两个参数没有实现 `Equtable` 协议是不能进行比较的。

实际上可以在定义 `Stack`时，为泛型 `Element` 增加约束条件来解决这个问题:

```swift
struct Stack<Element: Equatable> { }
```

除此之外，也可以使用带有泛型 `where` 语句的扩展来添加约束条件：

```swift
extension Stack where Element: Equatable {
    func isTop(_ item: Element) -> Bool {
        guard let topItem = items.last else {
            return false
        }
        return topItem == item
    }
}
```

### 关联类型

与 `class`、`struct` 以及 `enum` 不同，Swift 中 `protocol` 不支持泛型类型参数。`protocol` 如要实现泛型类似的目的，则需要使用关联类型。与泛型不同，关联类型不是在实例化一个类或者结构体时指明具体类型，而且在服从该协议时，指明其具体类型。

下面例子定义了一个 `Container` 协议，该协议定义了一个关联类型 `Item`:

```swift
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```
这个协议没有指定容器中元素该如何存储，以及元素必须是何种类型，只指定义了任何遵从 `Container` 协议的类型必须提供的三个功能。 协议声明了一个关联类型 `Item`，这个信息将留给遵从该协议的类型来提供。

就是这样！关联类型只是协议中表示泛型的一种语法。

### 使用类型注释约束关联类型

可以给协议中的关联类型添加一个类型注释来约束关联类型，如下：

```swift
protocol Container {
    associatedtype Item: Equatable
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```
这里通过类型注释的方式约束了 `Container` 协议中的关联类型 `Item` 要符合 `Equatable` 协议。

### 使用泛型 Where 语句约束关联类型

除了采用类型注释的方式来给关联类型添加约束外，也可以使用泛型 `where` 语句来限制关联类型。

如下定义一个泛型函数来检查两个容器对象中是否每个元素都完全匹配：

```swift
func allItemsMatch<C1: Container, C2: Container>
    (_ someContainer: C1, _ anotherContainer: C2) -> Bool
    where C1.Item == C2.Item, C1.Item: Equatable {
        
        // Check that both containers contain the same number of items.
        if someContainer.count != anotherContainer.count {
            return false
        }
        
        // Check each pair of items to see if they are equivalent.
        for i in 0..<someContainer.count {
            if someContainer[i] != anotherContainer[i] {
                return false
            }
        }
        
        // All items match, so return true.
        return true
}
```

这个函数在类型参数列表中定义了对两个类型参数的要求：`C1` 和 `C2` 必须符合 `Container` 协议； 通过泛型 `where` 语句对其关联类型进行了约束：
`C1` 的 `Item` 必须和 `C2` 的 `Item` 类型相同，且 `C1` 的 `Item` 必须符合 `Equatable` 协议。

### 具有泛型 Where 语句的扩展

也可以使用泛型 `where` 语句去扩展一个协议，来给协议中的关联类型添加新的约束：

```swift
extension Container where Item: Equatable {
    func startsWith(_ item: Item) -> Bool {
        return count >= 1 && self[0] == item
    }
}
if [42, 9, 9].startsWith(42) {
    print("Starts with 42.")
}
// Prints "Starts with 42"
```

上述示例中的泛型 `where` 语句约束协议中的关联类型 `Item` 要符合 `Equatable` 协议，当然也可以使用泛型 `where` 语句限制 `Item` 为特定类型，例如：

```swift
extension Container where Item == Double {
    func average() -> Double {
        var sum = 0.0
        for index in 0..<count {
            sum += self[index]
        }
        return sum / Double(count)
    }
}
print([1260.0, 1200.0, 98.6, 37.0].average())
// Prints "648.9"
```

### 具有泛型 Where 语句的关联类型

关联类型可以包含泛型 Where 语句，例如：

```swift
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
    
    associatedtype Iterator: IteratorProtocol where Iterator.Element == Item
    func makeIterator() -> Iterator
}
```
这里在关联类型 `Iterator` 上添加了一个泛型 `where` 语句，来约束迭代器遍历的元素类型与 `Container` 协议中声明的 `Item` 类型相同。

### 泛型下标

swift 中可以定义泛型下标，泛型下标也可以通过泛型 `where` 语句来添加约束：

```swift
extension Container {
    subscript<Indices: Sequence>(indices: Indices) -> [Item]
        where Indices.Iterator.Element == Int {
            var result = [Item]()
            for index in indices {
                result.append(self[index])
            }
            return result
    }
}
```
这里给 `Container` 协议添加了一个下标方法，该下标接受一个 `Indices` 类型参数，该泛型要求要符合标准库中的 `Sequence` 协议，并通过泛型 `where` 语句约束这个序列的迭代器只能遍历 `Int` 类型元素。

假设我们上面定义的 `Stack` 符合了 `Container` 协议，则可以通过下标方式来访问栈中的元素：

```swift
var stackOfStrings = Stack<String>()
stackOfStrings.push("uno")
stackOfStrings.push("dos")
stackOfStrings.push("tres")

print(stackOfStrings[1...2])
// Prints ["dos", "tres"]
```

### 泛型的逆变与协变

Swift 中的泛型是不变的（Invariance），不支持逆变与协变，例如：

```swift
struct Thing<T> {
    var thing: T
}
var view: Thing<UIView> = Thing(thing: UIView())
var button: Thing<UIButton> = Thing(thing: UIButton())
view = button 
// 报错 error：Cannot assign value of type 'Thing<UIButton>' to type 'Thing<UIView>'
```

但是 Swift 标准库中的符合 `Collection` 协议的类型通常情况下是协变的（Convariance），例如：

```swift
var views: Array<UIView> = [UIView()]
var buttons: Array<UIButton> = [UIButton()]
views = buttons
```
这样的协变特性，目前只有系统库能享有，我们自己定义的泛型是无法做到的。