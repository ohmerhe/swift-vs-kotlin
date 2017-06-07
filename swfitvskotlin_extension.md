
拓展 Extensions 🎓( Swift 篇 )
==========================
Swuft 扩展 就是为一个已有的类、结构体、枚举类型或者协议类型添加新功能。和OC的category类似
- 拓展语法
- 计算属性
- 构造器
- 方法
- 下标
- 嵌套类型
- 协议

拓展语法： 举几个常见的 🌰
---------------


```swift
class Human: NSObject {

    var name = "my_vc"

    init(name: String) {
        self.name
    }
    func work() {
        print("工作中")
    }
}

extension Human {
    func drivingTest() {
        print("通过驾照考试")
    }
}

extension Human: UITableViewDelegate, UITableViewDataSource {
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return 3;
    }
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        return UITableViewCell()
    }
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        print(name)
    }
}
```


计算型属性
--------

```swift
// 基础类型
extension Double {
    var km: Double { return self / 1000.0 }
    var m : Double { return self }
    var cm: Double { return self * 100.0 }
    var mm: Double { return self * 1000.0 }
}
let distance = Double(7353)
distance.km

// 结构体（class 类似）
struct iOSer {
    var name: String
}
extension iOSer {
    var isValid: Bool {
        return !name.isEmpty
    }
    mutating func work() {
        name = name + "_coding_dog"
    }
}

// 枚举拓展
enum AppleDevice {
    case iPhone,iPod,iPad,Mac,iMac
}

extension AppleDevice {
    func whatShouldIBuy(money: Int) -> AppleDevice {
        if money < 10000 {
            return .iPhone
        }
        return .Mac
    }
}
```

支持构造器
---------
值类型可以直接构造，引用类型需要使用便利构造器

```swift
// 参考已有的如上代码：iOSer、Human

extension iOSer {
    init(firstName: String, secondName: String) {
        self.name = firstName + secondName
    }
}

extension Human {
    convenience init(firstName: String, secondName: String) {
        self.init(name: firstName + secondName)
    }
}
```

方法
-----
静态方法，实例方法，类方法

```swift
extension iOSer {
    static func bestPrictise() -> String {
        return "Swift3.1"
    }
    func favoriteLanguage() -> String {
        return "\(name) loved swift"
    }
}
let bp = iOSer.bestPrictise()
let xcodeyang = iOSer(name: "xy")
xcodeyang.favoriteLanguage()


extension Human {
    static func tranlateChinese() -> String {
        return "人类"
    }
    class func tranlateJanpanese() -> String {
        return "ピープル"
    }
    func coding(code:(()->Void)?) {
        code?()
    }
}

let chineseName = Human.tranlateChinese()
let janpaneseName = Human.tranlateJanpanese()
let me = Human(firstName: "yang", secondName: "zhi")
me.coding {
    print("Hello World")
}
```


嵌套类型
------
比如结构体里面的枚举等等

```swift

extension iOSer {

    class Job {
        var experience: Int = 0 {
            didSet{
                salary = experience>3 ? 50:10
            }
        }
        var salary = 0

        init(l: Language) {
            if l != .oc {
                experience = 3
                salary = 30
            }
        }
    }

    enum Language {
        case oc, swift, java, js, c
    }

    var myFavoriteDSL: Language {
        return .js
    }

    var newJob: Job {
        return Job(l: myFavoriteDSL)
    }
}

xcodeyang.newJob.salary

```


下标
----
常见数组，字典等等

```swift

extension Int {
    subscript(digitIndex: Int) -> Int {
        var decimalBase = 1
        for _ in 0..<digitIndex {
            decimalBase *= 10
        }
        return (self / decimalBase) % 10
    }
}
746381295[0] // 7
746381295[5] // 1
```


扩展 && 协议
------
`详细可以参考后面协议的部分`

通过扩展遵循协议
------

```swift

protocol TextRepresentable {
    var textualDescription: String { get }
}

struct Human {
    var name: String
    var textualDescription: String {
        return "这个人叫\(name)"
    }
}

extension Human: TextRepresentable {}

let textProtocal: TextRepresentable = Human(name: "john")
textProtocal.textualDescription
// 或者
let human = Human(name: "john")
human.textualDescription


protocol PrettyTextRepresentable: TextRepresentable {
    var prettyTextualDescription: String { get }
}

extension Human: PrettyTextRepresentable {
    var prettyTextualDescription: String {
        return name.isEmpty ? "❌: 人名无效" : "✅：这个人叫\(name)"
    }
}

let john = Human(name: "john")
let unnamed = Human(name: "")
print(john.prettyTextualDescription)
print(unnamed.prettyTextualDescription)

```





通过扩展遵循协议实现协议的默认实现
------

```swift

protocol PlayGame {
    var gameName: String {get}
    func startGame()
    func endGame()
}

extension PlayGame {
    func startGame() {
        print("\(self.gameName) 游戏开始了")
    }
    func endGame() {
        print(self.gameName + " 结束了")
    }
}

extension Daniel: PlayGame {
    var gameName: String {
        return "王者荣耀"
    }
}

human.gameName
human.startGame()
human.endGame()

```

为协议扩展添加限制条件
-----------------

```swift

// 娱乐项目
protocol Entertainment {
    var name: String {get}
    func haveFun()
}

extension Entertainment where Self: Programmer {
    var name: String {
        return "王者荣耀"
    }
    func haveFun() {
        print("开始玩\(name)啦。。")
    }
}

extension Entertainment where Self: Producter {
    // 拓展里只能是计算属性
    var name: String {
        return "狼人杀"
    }
    func haveFun() {
        print("来一起玩\(name)，怎么样")
    }
}


class Programmer: Entertainment {}

class Producter: Entertainment {}

class Designer: Entertainment {
    func haveFun() {
        print("自己看看\(name)")
    }
    var name: String = "动画片"
}


let prog = Programmer()
prog.haveFun()
let prod = Producter()
prod.haveFun()
let desi = Designer()
desi.haveFun()

```

集合的运用
--------

```swift

extension Collection where Iterator.Element: Entertainment {
    var allNames:String {
        return self.reduce("结果：", {
            $0 + "\n" + $1.name
        })
    }
}

let representableArray = [Designer(),Designer(),Designer()]
// 留下待解决问题
//let representableArray = [Programmer(),Designer()] as [Entertainment]
print(representableArray.allNames)

```
