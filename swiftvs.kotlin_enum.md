


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
 - 方法1

```swift
switch sister {
case .father(let age):
    age
case .sister(let age):
    age
default:()
}

//: - 方法2 类似隐式解析
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
