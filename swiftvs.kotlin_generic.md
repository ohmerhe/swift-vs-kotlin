## Kotlin

### Kotlin 中的集合接口

![kotlin](https://cdn.kymjs.com/kotlin/6-1-1.png)

所有类声明的泛型尖括号里面如果加入了 out 关键字，则说明这个类的对象是只读的，例如他只有：get()、size（）等方法，而没有 set()、remove()等方法。
相反的，如果没有加 out 关键字，或者说如果开头是 MutableXXX 那就是一个不仅可以存储也可以读取的集合类。   
与 out 对应的，还有一个 in。表示泛型参数是只写的，也就是只有set()、remove()等方法。

### 泛型的逆变与协变

in 与 out 除了用在集合类上，用于读写权限限定以外，也可以用于普通的泛型类。   
kotlin 的泛型类是支持逆变与协变的。  
逆变就是你可以让泛型对象返回一个泛型参数父类的类型。反过来，协变就是你可以给泛型对象传入一个泛型参数类型的子类。  

```
open class A
open class B : A()
open class C : B()

val mutableList: MutableList<B> = mutableListOf(B(), B(), C())
val list: List<A> = mutableList;
```

在 Kotlin 中，可以直接把 `List<C>` 的对象赋值给 `List<A>`   
知道了上面这一点，我们接下来看 `in` 的用法。  
泛型声明中，用 `in` 声明泛型参数，表示这个参数只能是入参，不能对外返回这个参数。  

```
open class A
open class B : A()
open class C : B()

class TypeArray<in A> {
	
	//in 修饰了 A，表示 A 是可以作为参数的。
    fun getValue(a: A): Int? {
        return a?.hashCode()
    }
    
    //这段代码是非法的，因为A 不能被返回
    fun getA(a: A): A? {
        return a
    }
}
```


### 集合的初始化

在 Kotlin 中，集合类一般不使用构造方法去初始化，而是使用同一的入口方法，例如初始化一个 `MutableList`，我们使用的是如下代码：

```
 val mutableList = mutableListOf(0, 1, 2, 3) 
```

类似的初始化集合对象的方法还有

```
//创建一个 List<> 对象
var list = listOf(0, 1, 2)

//创建一个 Set<> 对象
val ss = setOf(1, 2, 4)
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








