# Swift vs. Kotlin 漫谈系列之接口

Kotlin 君和 Swift 君在一个团队一起开发已经很久了，由于平台的差异性，他们经常会进行一些技术上的交流（PK），「Kotlin vs. Swift」课程就是他们在互相切磋是的语录。内容会由简及深，慢慢深入。

# 技术漫谈

**Kotlin:**

- static 和 class 有什么分别
- required 是腰子子类必须复写方法？
- 扩展与实现协议的差别
- 协议合成，你们有typealias吗
- 有哪些限制，只能被类实现的协议？

**Swift:**

## Kotlin


## Swift

### Protocol

`Swift` 是一门支持面向协议编程的语言，在 `Swift` 语言中，协议被赋予了更多的功能和更广阔的使用空间。恰逢苹果发布了 `swift 4`，以下都是基于最新的 `swift 4` 语法进行讲述。

#### 协议语法

协议声明：

```
protocol SomeProtocol {
    // protocol definition goes here
}
```

要让自定义类型符合某个协议，需要在类型名称后加上协议名称，中间以冒号（`:`）分隔。符合多个协议时，各协议之间用逗号（`,`）分隔. `swift` 中，符合协议的类型可以是 `class`、`struct` 或 `enum`。


```
struct SomeStructure: FirstProtocol, AnotherProtocol {
    // structure definition goes here
}
```
需要注意的是，如果某个类在符合某个协议的同时又继承自某个父类，应将其父类名放在其符合的协议名之前。

#### 协议属性声明

协议中可以声明符合此协议的类型必须实现的属性：

```
protocol SomeProtocol {
    var mustBeSettable: Int { get set }
    var doesNotNeedToBeSettable: Int { get }
}
```
协议不指定属性是存储型属性还是计算型属性，它只指定属性的名称和类型，以及属性是可读的还是可读可写的。

协议总是用 `var` 关键字来声明变量属性，在类型声明后加上 `{ set get }` 来表示属性是可读可写的，用 `{ get }` 来表示属性是只读的。

协议中可以声明类型属性，一般使用 `static` 关键字定义类型属性，如果是类类型符合该协议，除了 `static`，还可以使用 `class` 关键字来声明类型属性。

```
protocol AnotherProtocol {
    static var someTypeProperty: Int { get set }
}
```

#### 协议方法声明

协议可以要求遵循协议的类型实现某些指定的实例方法或类方法。需要注意的是，不支持为协议中的方法的参数提供默认值。

```
protocol RandomNumberGenerator {
    func random() -> Double
}
```
与属性类似，在协议中也可以使用 `static` 定义类方法，当类类型遵循协议时，还可以使用 `class` 关键字来修饰.

如果协议中定义的实例方法会改变遵循该协议的类型的实例，那么需要在该方法前加 `mutating` 关键字, 表示可以在该方法中修改它所属的实例以及实例的任意属性的值, 例如：

```
protocol Togglable {
    mutating func toggle()
}

enum OnOffSwitch: Togglable {
    case off, on
    mutating func toggle() {
        switch self {
        case .off:
            self = .on
        case .on:
            self = .off
        }
    }
}
var lightSwitch = OnOffSwitch.off
lightSwitch.toggle()
// lightSwitch 现在的值为 .On
```

### 协议构造器声明

协议可以要求遵循协议的类型实现指定的构造器：

```
protocol SomeProtocol {
    init(someParameter: Int)
}

class SomeClass: SomeProtocol {
    required init(someParameter: Int) {
        // initializer implementation goes here
    }
}
```
在符合协议的类中实现构造器，必须在构造器实现前加上 `required` 修饰符。使用 `required` 修饰符可以确保所有子类也必须提供此构造器实现，从而也能符合协议. 如果类已经被标记为 `final`，那么不需要在协议构造器的实现中使用 `required` 修饰符，因为 `final` 类不能有子类.

协议还可以为遵循协议的类型定义可失败构造器。

#### 委托（代理）模式

`Swift` 可以使用 `Protocol` 来实现委托（代理）模式，委托（代理）模式允许类或结构体将一些需要它们负责的功能委托给其他类型的实例，如下：

```
protocol RentHouserDelegate{
    func rent(_ name:String);
}

class Tenant {
    var name = "lucy"
    var delegate: RentHouserDelegate?
    func rentHouse(){
        delegate?.rent(name)
    }
}

class Intermediary: RentHouserDelegate {
    var name="lily"
    func rent(_ name:String) {
        print("\(name) 请 \(self.name) 帮她租一套房子");
    }
}

var person = Tenant();
person.delegate = Intermediary()
person.rentHouse()
// lucy 请 lily 帮她租一套房子
```
#### 通过扩展遵循协议

可以通过扩展令已有类型遵循并符合协议：

```
protocol TextRepresentable {
    var textualDescription: String { get }
}

struct Circular {
    var radius: Int
}

extension Circular: TextRepresentable {
    var textualDescription: String {
        return "The circular's radius is \(radius)"
    }
}

let circular = Circular(radius: 2)
print(circular.textualDescription)
// The circular's radius is 2
```
当一个类型已经符合了某个协议中的所有要求，却还没有声明遵循该协议时，可以通过空扩展来使该类型遵循该协议:

```
struct Square {
    var width: Int
    var textualDescription: String {
        return "The square's width is \(width)"
    }
}
extension Square: TextRepresentable {}

let square = Square(width: 3)
let squareTextRepresentable: TextRepresentable = square
print(squareTextRepresentable.textualDescription)
// The square's width is 3
```

#### 协议类型

尽管协议本身并未实现任何功能，但是协议可以被当做一个成熟的类型来使用。
协议类型也可以在数组或者字典这样的集合中使用：

```
let things: [TextRepresentable] = [circular, square]
for thing in things {
    print(thing.textualDescription)
}
```
#### 协议的继承

协议能够继承一个或多个其他协议，可以在继承的协议的基础上增加新的要求：

```
protocol PrettyTextRepresentable: TextRepresentable {
    var prettyTextualDescription: String { get }
}

extension Square: PrettyTextRepresentable {
    var prettyTextualDescription: String {
        var output = textualDescription + ": "
        output += "the area is \(width*width)"
        return output
    }
}

print(square.prettyTextualDescription)
// The square's width is 3: the area is 9
```

### Class 类型专属协议

通过让协议继承 `AnyObject` 协议来限制协议只能被 `Class` 类型遵循，而结构体或枚举不能遵循该协议

```
protocol SomeClassOnlyProtocol: AnyObject, SomeInheritedProtocol {
    // class-only protocol definition goes here
}
```

#### 协议合成

可以采用 `&` 符号将多个协议进行组合：

```
protocol Named {
    var name: String { get }
}
protocol Aged {
    var age: Int { get }
}
struct Person: Named, Aged {
    var name: String
    var age: Int
}
func wishHappyBirthday(to celebrator: Named & Aged) {
    print("Happy birthday, \(celebrator.name), you're \(celebrator.age)!")
}
let birthdayPerson = Person(name: "Malcolm", age: 21)
wishHappyBirthday(to: birthdayPerson)
// Prints "Happy birthday, Malcolm, you're 21!"
```
这里 `wishHappyBirthday(to:)` 函数的参数类型为 `Named & Aged`, 这意味着它不关心参数的具体类型，只要参数符合这两个协议即可。

除了协议与协议组合外，协议还可以与 `class` 进行组合：

```
class Location {
    var latitude: Double
    var longitude: Double
    init(latitude: Double, longitude: Double) {
        self.latitude = latitude
        self.longitude = longitude
    }
}
class City: Location, Named {
    var name: String
    init(name: String, latitude: Double, longitude: Double) {
        self.name = name
        super.init(latitude: latitude, longitude: longitude)
    }
}
func beginConcert(in location: Location & Named) {
    print("Hello, \(location.name)!")
}
 
let seattle = City(name: "Seattle", latitude: 47.6, longitude: -122.3)
beginConcert(in: seattle)
```
这里的 `beginConcert(in:)` 函数的参数要求是 `Location` 的子类，且必须符合 `Named` 协议.

#### 检查协议一致性

可以通过 `is` `as?` `as` 来检查某个实例是否符合某个协议：

```
let things: [Any] = [circular, square, "abc"]
for thing in things {
    if let object = thing as? TextRepresentable {
        print(object.textualDescription)
    } else {
        print("It does not conform to TextRepresentable")
    }
}
```

#### 可选协议

原生的 `Swift protocol` 里没有可选项，所有定义的方法都是必须实现的。如果想要像 `Objective-C` 里那样定义可选的接口方法，就需要在 `protocol` 定义之前加上 `@objc`，将 `protocol` 变为 `Objective-C` 的。然后使用 `optional` 关键字来声明某些方法或属性在符合该协议的类中可以不实现，如下：

```
@objc protocol CounterDataSource {
    @objc optional func incrementForCount(count: Int) -> Int
    @objc optional var fixedIncrement: Int { get }
}
```
需要注意的是，标记为 `@objc` 的 `protocol` 只能被 `class` 实现，不能被 `struct` 和 `enum` 类型实现，而且实现它的 `class` 中的方法还必须也被标注为 `@objc`，或者整个类就是继承自 `NSObject`。这还是很蛋疼的。

#### 协议扩展

协议可以通过扩展来为遵循协议的类型提供属性和方法的实现，即使协议中没有声明。这样就无需在每个遵循协议的类型中都重复同样的实现，也无需使用全局函数。

```
protocol TextRepresentable {
    var textualDescription: String { get }
}

extension TextRepresentable {
    func hasDescription() -> Bool {
        return !textualDescription.isEmpty
    }
}
```
还可以通过协议扩展来为协议要求的属性、方法提供默认的实现。这样在遵循这个协议的类型中，可以不用实现这个属性或方法，调用的时候默认调 `extension` 中的实现。这也相当于变相将 `protocol` 中的属性或方法设定为了 ``optional`. 

```
extension TextRepresentable {
    var textualDescription: String {
        return "This is a shape"
    }
}
```
在扩展协议的时候，也可以指定一些限制条件，只有遵循协议的类型满足这些限制条件时，才能获得协议扩展提供的默认实现。

```
extension Collection where Iterator.Element: TextRepresentable {
    var textualDescription: String {
        let itemsAsText = self.map { $0.textualDescription }
        return "[" + itemsAsText.joined(separator: ", ") + "]"
    }
}

let circulars = [Circular(radius: 1), Circular(radius: 2)]
print(circulars.textualDescription)
// [The circular's radius is 1, The circular's radius is 2]
```

这里扩展了 `Collection` 协议，但是限制只适用于集合中的元素遵循了 `TextRepresentable` 协议的情况。 因为 `Swift` 中 `Array` 符合 `Collection` 协议，而 `Circular` 类型又符合 `TextRepresentable` 协议，所以 `circulars` 可以使用 `textualDescription` 属性得到数组内容的文本表示

## 关于《Swift vs. Kotlin 漫谈》系列

- 《Swift vs. Kotlin 漫谈》系列之变量定义
- 《Swift vs. Kotlin 漫谈》系列之函数定义
- 《Swift vs. Kotlin 漫谈》系列之控制流
- 《Swift vs. Kotlin 漫谈》系列之基本类型

《Swift vs. Kotlin 漫谈》系列是由 KotlinThree 发起的，旨在把 Swift 和 Kotlin 进行一个全面的对比，帮助 iOS 和 Android 开发对彼此的语言之间有一个更加深入的认识。如果你有兴趣可以关注我们公众号，也可以通过「原文链接」查看详情。

---
![](http://7xpox6.com1.z0.glb.clouddn.com/kotlin-three-wechat.jpg)