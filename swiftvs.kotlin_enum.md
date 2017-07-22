


枚举 🇨🇳 😎
==========
> 枚举的强大犹如Stuct一般,这里做简单介绍（包括协议、拓展等和struct相同的知识点以后一起串讲）

演示
---
这是一个动画的枚举示例 -> 手机投屏演示 (gif待添加)

```swift
enum AnimationPeriod: Int {
    // 动画执行的五个阶段
    case start = 0, first, second, third, end

    func description() -> String {
        switch self {
        case .start, .first:    return "正在提取学校最新\n录取条件"
        case .second:           return "正在与学校进行匹配"
        case .third, .end:      return "正在根据匹配结果\n生成选校方案"
        }
    }
    // 枚举等不可添加存储类型的变量
    var duration: TimeInterval {
        switch self {
        case .start:    return 0.8
        case .first:    return 1
        case .second:   return 2
        case .third:    return 0.5
        case .end:      return 0.25
        }
    }
}

extension AnimationPeriod {
    // mutating 关键字修饰：自身被修改
    mutating func next() {
        if rawValue < AnimationPeriod.end.rawValue {
            self = AnimationPeriod(rawValue: rawValue+1)!
        }
    }
}

var animate = AnimationPeriod.third
if animate == .third {
    print("动画在第三阶段")
}
animate.rawValue
animate.next()
animate.rawValue
animate.next()
animate.rawValue

```

枚举：简单枚举
------------

```swift
enum LOL {
    case Marksman, Mage, Assassin, Tank, Fighter, Support
}

let myHero: LOL = .Marksman
```


枚举：嵌套枚举 👍
------------

```swift
enum LOL_II {
    enum Tank:String {
        case arthur = "亚瑟"
        case lvbu = "吕布"
    }
    enum Mage:String {
        case agnel = "安其拉"
        case fire = "火舞"
    }
    enum Assassin:String {
        case Athena = "雅典娜"
        case Luna = "露娜"
    }
}

let myHeroII = LOL_II.Assassin.Luna
myHeroII.rawValue
```


枚举：携带参数 👍🏻👍🏻
------------
> 携带参数，参数可以是元组，闭包，一样也可以是枚举

```swift
enum HeroType {
    case tank(difficulty: Float)
    case mage(difficulty: Float)
    case assassin(difficulty: Float)
}

enum Hero {
    case Athena(type: HeroType)
    case Luna(type: HeroType)
}

let luna = Hero.Luna(type: .assassin(difficulty: 0.9))
if case Hero.Luna(type: .assassin(let difficulty)) = luna {
    // 难度系数
    difficulty
}

enum FamilyType {
    case father(age: Int)
    case mother(age: Int)
    case sister(age: Int)

    // 非存储变量
    var gift: String {
        switch(self) {
        case .sister(let age):
            if age > 15 {
                return "iphone"
            } else {
                return "toy"
            }
        case .mother(let age) where age < 40:
            return "cloth"
        default:
            return "book"
        }
    }
}
var sister = FamilyType.sister(age: 15)
let mother = FamilyType.mother(age: 50)

let sisterGift = sister.gift
let motherGift = mother.gift
```


枚举取值的方法
------------


```swift
switch sister {
case .father(let age):
    age
case .sister(let age):
    age
default:()
}

//: 类似隐式解析
if case .sister(let age) = sister {
    age
}

//: 枚举的初始化方法
enum AppleDevice {
    case iMac(price:Int)
    case iPod(price:Int)
    case iPhone(price:Int)
    init (costMoney: Int) {
        if costMoney > 10000 {
            self = .iMac(price: costMoney)
        } else if costMoney > 2500 {
            self = .iPhone(price: costMoney)
        } else {
            self = .iPod(price: costMoney)
        }
    }
}
let myDevice = AppleDevice(costMoney: 6000)
```

元祖结合
-------
多元方式组合枚举

```swift
enum HumanHabit {
    case Reading
    case PlayGame
    case Traveling
}

typealias HumanInfo = (age: Int, name: String, habit: HumanHabit)

enum Desktop {
    case Cube(HumanInfo)
    case Tower(HumanInfo)
    case Rack(HumanInfo)
}
let aTower = Desktop.Tower((20, "XcodeYang", .Traveling) as HumanInfo)
let Cube = Desktop.Cube((21, "XcodeYang", .PlayGame))


//: switch对象：元祖
var myIPhone6s = AppleDevice.iPhone(price: 6088)
var myIPhone4s = AppleDevice.iPhone(price: 2000)

func sameDevice(_ firstDevice: AppleDevice, secondDevice: AppleDevice) -> Bool {
    switch (firstDevice, secondDevice) {
    case (.iPhone(let a), .iPhone(let b)) where a == b:
        return true
    case (.iPod(let a), .iPod(let b)) where a == b:
        return true
    case (.iMac, .iMac):
        return true
    default:
        return false
    }
}

print(sameDevice(myIPhone6s, secondDevice: myIPhone4s))

```

枚举 enum
```kotlin
enum class Color(val r: Int, val g: Int, val b: Int) {
RED(255, 0, 0), ORANGE(255, 165, 0),
YELLOW(255, 255, 0), GREEN(0, 255, 0), BLUE(0, 0, 255),
INDIGO(75, 0, 130), VIOLET(238, 130, 238);

fun getWarmth(color: Color) = when(color)
{ Color.RED, Color.ORANGE, Color.YELLOW ->"warm" 
  Color.GREEN -> "neutral"
  Color.BLUE, Color.INDIGO, Color.VIOLET -> "cold"
}
```

###枚举
枚举类的最基本的用法是实现类型安全的枚举，即 Week.Monday的形式（每一个枚举常量都是这个枚举类的实例而且不提供公开的构造方法）。
```kotlin
enum class Lang {
    PHP, Java, OC, Python, Ruby, Go, Swift, Kotlin
}
```
枚举类的主构造函数默认是私有的。如果对每个枚举常数设置属性值，需在主构造函数里进行声明，并且在枚举常量处初始化
```kotlin
enum class Week(var i:int) {
    Monday(1), Tuesday(2), Wednesday(3), Thursday(4), Friday(5), Saturday(6),Sunday(0)
}
```
就像在 Java 中一样，Kotlin 中的枚举类也有合成方法允许列出所有定义的枚举常量以及通过名称获取枚举常量
```kotlin
valueof(value:String)  //转换指定name为枚举值，若未匹配成功，会抛出IllegalArgumentException
values() //以数组的形式，返回枚举值

val week = Week.valueof("Monday")
val weeks: Array<Week> = Week.values()
```

自 Kotlin 1.1 起，可以使用 enumValues<T>() 和 enumValueOf<T>() 函数以泛型的方式访问枚举类中的常量 ：
```kotlin
enum class RGB { RED, GREEN, BLUE }

inline fun <reified T : Enum<T>> printAllValues() {
    print(enumValues<T>().joinToString { it.name })
}

printAllValues<RGB>() // 输出 RED, GREEN, BLUE
```

```kotlin
name():String //获取枚举名称
ordinal():Int //获取枚举值在所有枚举数组中定义的顺序

Week.Monday.name //Monday
Week.Monday.ordinal //1
```
###匿名类与抽象方法
在枚举类中声明了抽象方法，所有的枚举常量都应声明其匿名类，并在匿名类中实现枚举类中声明的抽象方法。
```kotlin
enum class Person(var type: Int) {
    STUDENT(1) {
        override fun getPerson(ordinal: Int): Person {
            return Person.values()[ordinal]
        }

        fun doClick() {
            println("student do click")
        }
    },
    TEACHER(2) {
        override fun getPerson(ordinal: Int): Person {
            return Person.values()[ordinal]
        }
    },
    DEAN(3) {
        override fun getPerson(ordinal: Int): Person {
            return Person.values()[ordinal]
        }
    },
    PRINCIPAL(4) {
        override fun getPerson(ordinal: Int): Person {
            return Person.values()[ordinal]
        }
    };

    abstract fun getPerson(ordinal: Int): Person
}

// Test
val  per = Person.STUDENT
println("getPerson:${per.getPerson(1)}") // 打印：getPerson:TEACHER
```
如果枚举类中定义了任何成员, 你需要用分号将枚举常数的定义与枚举类的成员定义分隔开.
匿名内部类中声明的方法，并不能在外部使用，即使是其枚举类型的实例，也不可调用。但重写的枚举类中声明的方法，可以被其实例调用。
###枚举类与接口
枚举类实现接口的情况与抽象方法类似，所有的枚举常量都应在其匿名类中实现接口的方法
```kotlin
enum class Person(var type: Int): onClickListener{
    STUDENT(1) {
        override fun onClick() {
            println("on click")
        }
    },
    TEACHER(2) {
        override fun onClick() {
            println("on click")
        }
    },
    DEAN(3) {
        override fun onClick() {
            println("on click")
        }
    },
    PRINCIPAL(4) {
        override fun onClick() {
            println("on click")
        }
    };

}
```

###密封类