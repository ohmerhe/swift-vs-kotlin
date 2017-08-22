# Swift vs. Kotlin 漫谈之枚举


`王者荣耀`


***Swift:***

Kotlin 君，我最近打王者荣耀排位上钻石了；

***Kotlin***

哦是吗！Swift君厉害了，我最高的一次就只是在铂金V

***Swift***

排位越上越难打，最近`王者荣耀`的爆料新闻很多啊，你说同样是Coder，怎么差距就是这么大？

***Kotlin***

那都是名啊，今天我们PK的是枚举技术，要不我们就拿王者荣耀来练练手？

***Swift***

好主意，意淫一下当`王者荣耀`的coder是啥感觉；

```swift
enum KingGloryType {
    case Marksman, Mage, Assassin, Tank, Fighter, Support
}

let myFavoriteHeroType: KingGloryType = .Assassin
```
我的最爱是刺客，嘿嘿 😜

***Kotlin***

呦，差不多啊。区别还是我们一切写对象，但是你们

```kotlin
enum class KingGloryType {
    Marksman, Mage, Assassin, Tank, Fighter, Support
}

val myFavoriteHeroType: KingGloryType = KingGloryType.Fighter
```

***Swift***

确实很像，但若是更根据做细分，Swift枚举可以支持枚举嵌套，也可以设置枚举值（rawValue）

```swift
enum KingGloryType_II {
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

let bestAssassin = KingGloryType_II.Assassin.Luna
bestAssassin.rawValue  // "露娜"
```


***Kotlin***

厉害!这个特性kotlin没有，而且对于我这个菜鸟来说，连那几个英雄都分不清，他们在我手里都是一套打法——无脑往上冲...

```kotlin
//自 Kotlin 1.1 起，可以使用 enumValues () 和 enumValueOf () 函数以泛型的方式访问枚举类中的常量 
enum class Tank {
        "亚瑟","吕布"
    }
enum class Mage {
        "安其拉","火舞"
    }
enum class Assassin {
        "雅典娜","露娜"
    }

inline fun <reified T : Enum<T>> printAllValues() {
    print(enumValues<T>().joinToString { it.name })  //name类似Swift的rawValue

inline fun <reified T : Enum<T>> printValueOf(name: String) {
    print( enumValueOf<T>(name))
}

printAllValues<Tank>() //"亚瑟","吕布"
printAllValues<Mage>() //"安其拉","火舞"
printAllValues<Assassin>() //"雅典娜","露娜"

printValueOf<Tank>("亚瑟") //"亚瑟"
printValueOf<Tank>("火舞") //"火舞"
printValueOf<Tank>("雅典娜") //"雅典娜"

}

```


***Swift***

我介绍几个英雄给你吧，比如灵活强悍的露娜、李白。其中露娜算是最能秀的英雄没有之一，当然难度也比较大。

***Kotlin***

我不行，我初学者能力有限。你推荐一些英雄顺便附带一些难度说明吧

***Swift***

定义英雄类型坦克，法师，刺客；

```swift
enum HeroType {
    case tank(difficulty: Float)
    case mage(difficulty: Float, reason: String)
    case assassin(difficulty: Float, reason: String)
}

enum RecommendHero: str {
    case Athena(type: HeroType)
    case Luna(type: HeroType)
}
```

实现一个推荐英雄及辨识他的方法

```swift
let luna = Hero.Luna(type: .assassin(difficulty: 0.9, reason:"不可断大，月下无限连"))

if case Hero.Luna(type: .assassin(let difficulty, let reason)) = luna {
    // 难度系数
    print("露娜难度系数+\(difficulty),注意事项:\(reason)")
}

```

***Kotlin***

这好强大。if case 取值的方式有点特别啊！！坦克英雄不用解释原因是因为比较简单？

***Swift***

bingo！老话说：**法师靠预判 射手靠走位 打野靠的是意识**，但也不能怎么说，毕竟这是靠团队协作的游戏。

`if case` 取值是其实和guard模式差不多，不然还得一一遍历枚举的个个case？书写效率太低

其实上面的还可以做优化，要是参数再多几个影响书写和阅读，加入元组可以增加可读性；

```swift

typealias DifficultyInfo = (difficulty: Float, reason: String)

enum HeroType {
    case tank(info: DifficultyInfo)
    case mage(info: DifficultyInfo)
    case assassin(info: DifficultyInfo)
}

enum Hero {
    case Athena(type: HeroType)
    case Luna(type: HeroType)
}

let luna = Hero.Luna(type: .assassin(info: (0.9, "不可断大，月下无限连")))

if case Hero.Luna(type: .assassin(let info)) = luna {
    // 难度系数
    print("露娜难度系数+\(info.difficulty),注意事项:\(info.reason)")
}
```


***Kotlin***
```kotlin
fun getWarmth(type: HeroType) = when(type)
{ HeroType.tank ->"..." 
  HeroType.mage -> "...."
  HeroType.assassin -> "....."
}
```
。。。。。

***Swift***

游戏初始就有金币，关于枚举还有很多很多数不完的运用，效果差不多，还不快去先下载王者荣耀试试。

***Kotlin***

哈哈哈，一起开黑啊！





枚举 🇨🇳 😎
==========
> 枚举的强大犹如Stuct一般,这里做简单介绍（包括协议、拓展等和struct相同的知识点以后一起串讲）

### 枚举：简单枚举

基础枚举

```swift
enum LOL {
    case Marksman, Mage, Assassin, Tank, Fighter, Support
}

let myHero: LOL = .Marksman
```

在 C 语言中，枚举会为一组整型值分配相关联的名称。Swift 中的枚举更加灵活，不必给每一个枚举成员提供一个值。如果给枚举成员提供一个值（称为“原始”值），则该值的类型可以是字符串，字符，或是一个整型值或浮点数。

```swift
enum Tank:String {
    case arthur = "亚瑟"
    case lvbu = "吕布"
}

let myHeroTank = Tank.lvbu
myHeroTank.rawValue // "吕布"

switch myHeroTank {
case .arthur:
    print("这个英雄是亚瑟")
case .lvbu:
    print("这个英雄是吕布") // ✅
default:
    print("这个英雄未知")
}

```

### 枚举：嵌套枚举 👍

枚举里嵌套枚举，和类中嵌套类，方法中嵌套方法是一样可行；

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


### 枚举：携带参数 👍🏻👍🏻

> 携带参数，参数可以是元组，闭包，一样也可以是枚举

例子1：王者荣耀英雄

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
```

例子2：家庭成员

```swift
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

### 枚举取值的方法

```swift
//: 常见的
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
```
### 枚举的初始化方法
枚举构造器

```swift
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

### switch对象或元祖
> switch方法可以执行辨别两个对象，或者元组

```swift 
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
```

## 枚举 enum - kotlin

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

### 枚举
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
### 匿名类与抽象方法
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
### 枚举类与接口
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

### 密封类
在使用When表达式的时候，编译器会检查default选项，所以我们常常需要加上else来防止出现其他情况，像这样
```kotlin
interface Expr
class Num(val value: Int) : Expr
class Sum(val left: Expr, val right: Expr) : Expr
fun eval(e: Expr): Int =
when (e) {
is Num -> e.value
is Sum -> eval(e.right) + eval(e.left)
else ->
throw IllegalArgumentException("Unknown expression")
}
```
而且，如果我们添加一个子类，就需要再创建一个分支，如果忘记的话很可能出现问题。

密封类很好的解决了这个问题
虽然密封类也可以有子类，但是所有子类都必须在与密封类自身相同的文件中声明
```kotlin
sealed class Expr {          //要声明一个密封类，需要在类名前面添加 sealed 修饰符。
class Num(val value: Int) : Expr()
class Sum(val left: Expr, val right: Expr) : Expr()
}
fun eval(e: Expr): Int =
when (e) {
is Expr.Num -> e.value
is Expr.Sum -> eval(e.right) + eval(e.left)
// 不再需要 `else` 子句，因为我们已经覆盖了所有的情况
}
```
使用密封类的关键好处在于使用 when 表达式 的时候，如果能够验证语句覆盖了所有情况，就不需要为该语句再添加一个 else 子句了。
请注意，扩展密封类子类的类（间接继承者）可以放在任何位置，而无需在同一个文件中