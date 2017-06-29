Swift -vs- Kotlin: 扩展篇 👊
==========================
`脱单秘诀`

***Swift:***

Kotlin 君，我看你最近加班多，都快没有时间谈恋爱了？你和你的女神近况怎么样？

***Kotlin***

Swift君，别闹。我们只是朋友关系；

***Swift***

傻子都看得出来你对你的女神有意思，看你单身20余载，恋爱经验匮乏，要不要我传授你撩妹秘诀？

***Kotlin***

懂我者Swift君也，快说说看；

***Swift***

以我们Swift为例，我们先定义一个人类，然后你就是`kotliner`和你女神`angel`

```swift
class Human {
    var name: String
    var age: Int
    var lover: Human?
    init(_ name:String, age: Int) {
        self.name = name
        self.age = age
    }
}

var kotliner = Human.init("Kotliner", age: 26)
var angel = Human.init("Angel", age: 27)
```
***Kotlin***

先定义一个`Human`是吧，好，我也完成了。

```kotlin
class Human(name:String){
    var lover: Human?
    constructor(name:String ,age: Int) : this(name){

    }
}

val kotliner = Human("Kotliner", age: 26)
val angel = Human("Angel", age: 27)
```
***Swift***

然后：第一步，我教你先识别你的女神是不是单身, extension 就可以给 Human 添加一个是否单身的属性（计算性属性）

```swift
//note: Swift扩展只能是计算型属性，也必须使用var修饰，且不能赋值操作
extension Human {
    var isSingle: Bool {
        return lover == nil
    }
}
let angelIsSingle = angel.isSingle // true
```
你看, 恭喜👏恭喜🎉，女神还单身，你有机会是吧！

***Kotlin***

这TMD也可以啊，看我也现学现卖一下。是否单身的属性我们也可以加，就是写法不太一样；

```kotlin
fun Human.isSingle Boolean {
    return this.lover == null
}

val angelIsSingle = angel.isSingle // true
```

***Swift***

哎呦不错哦，看样子Kotlin君天资聪慧，不久就可以脱单了；
再来：我看你女神比较高冷，试试使用撩妹秘籍，通过Extension，让Human学会一个陷入爱河的方法（func）
如果是你来撩的话

```swift
extension Human {
    func fallInLoveBy撩X秘籍(_ lover: Human) {
        self.lover = lover;
    }
}

angel.fallInLoveBy撩X秘籍(kotliner)
let isSingle = angel.isSingle // false
let angelsLover = angel.lover?.name // Kotliner
```
看，你的女神不是单身了，而且爱上你了；

当然，只要你博爱，在Swift里甚至可以让全世界男女都爱上你 🙃

```swift
//note: 类方法扩展，未实例对象
extension Human {
    class func perfectLoversName() -> String {
        return "Kotliner"
    }
}
var name = Human.perfectLoversName() // Kotliner
```


***Kotlin***

这招狠啊，我也试试看看，有没有觉得我们的写法要更简洁点：

```kotlin
fun Human.fallInLoveBy撩X秘籍(lover: Human){
    this.lover = lover
}

angel.fallInLoveBy撩X秘籍(kotliner)

val isSingle = angel.isSingle // false
val angelsLover = angel.lover?.name // Kotliner
```

***Swift***

是啊，厉害厉害。

***Kotlin***

我还悟出了其他的撩妹技巧，你也看看你以前用过没有；

```kotlin
fun Human.壁咚(lover: Human){
    this.lover = null
    lover = null
}

kotliner.壁咚(angel)  //卒......
```

***Swift***

哈哈哈哈哈，强行壁咚的下场太可怕了，我们也一样只是换成`nil`,但这个扩展是同源的，和前面一样的。
我还有一招献上

***Kotlin***

霸王硬上弓？😆

***Swift***

那不是你卒，是你两全卒。🤣
回正题，你知道以前我们父母他们那一辈有媒妁之约，男孩女孩一出生就婚定一生是吧！前面一大堆的扩展学习太麻烦都可以省去，我直接在你女神出生那一刻就给你定娃娃亲，这事就简单了;（构造器）

`当你的女神还是一个baby的时候，订个娃娃亲就好简单了`

```swift
// note: 构造器扩展只能用便利构造器 详见 《类与继承》 漫谈篇
extension Human {
    // 刚刚出生 age=0
    convenience init(birthWithName name: String, lover: Human?) {
        self.init(name, age: 0)
        self.lover = lover
    }
}
// Angelababy（感情你女神是杨颖啊）
var angelBaby = Human.init(birthWithName: "Angel", lover: kotliner)
```
哈哈哈，一下子就成了。

***Kotlin***

这个好，你们可以事后诸葛，但我们貌似是没法做构造器扩展的,倒是可以在创建类时添加一个的构造器:

```kotlin
class Human(name:String){
    ...
    constructor(name:String ,lover: Human,age: Int = 0) : this(name){

    }
}
var angelBaby = Human(name = "Angel", lover = kotliner)
```

试了这么多方法有好有坏，都这么快就学会了，女神我算是已经追求到手了，我打算明天求婚。

***Swift***

等等等一下。。好不容易追到你女神，你要爱惜她 尊重她 安慰她 保护着她，俩人同心建立起美满的家庭。。。
你愿意这样做吗？

***Kotlin***

Yes, I do

***Swift***

好在你是程序员，物质基础比较牢靠，结婚后需要有一份稳定的工作做保证；

***Kotlin***

是是是，我这边公司加班太多了，以后都没法陪老婆孩子，想换一家工作时间分配比较合理一点的公司，你给我推荐一下吧；

***Swift***

好，这份我们公司的程序员工作合同，签了它就可以过来上班了

```swift
protocol DeveloperJobProtocol {
    var job:String { get }
    var salary: Int { get }
    var level: Int { get }
    var childSex: Sex { get }
}
```

签合同很简单，看我给你演示一下

```swift

enum Sex {
    case male, female
}

extension Human: DeveloperJobProtocol {
    var job:String {
        return "Andriod"
    }
    var salary: Int {
        return level*2000 + 6000
    }
    var level: Int {
        return 6
    }
    var childSex: Sex {
        return .female
    }
}

kotliner.salary // 18k
```

这是我们公司根据你的情况给你做的合同

***Kotlin***

18k，很好。知足了

```kotlin
enum class Sex {
    male,female
}

val Human.job: String
get():String {
     return "Andriod"
    }
var Human.salary: Int
get():Int {
     return level*2000 + 6000
    }
var Human.level: Int
get():Int {
     return 6
    }
var Human.childSex: Sex
get():Int {
     return Sex.female
    }

kotliner.salary // 18k
```

合同我签了，但 childSex 是什么鬼？

***Swift***

你做了程序员，那你不就注定生女儿了嘛。我都想象到未来你们生女儿一起生活的画面了！（扩展嵌套类型）

```swift
extension Human {
    class Child: Human {
        var sex:Sex = kotliner.childSex
        weak var father: Human? = kotliner
        weak var mother: Human? = kotliner.lover
    }
    var daughter: Child {
        return Child.init("littleAngel", age: 0)
    }
}

let yourChild = kotliner.daughter
let childFatherName = yourChild.father?.name // Kotliner
```

***Kotlin***

哇！！！当程序员居然有这个福利。我喜欢生女儿，生女儿好；可是为什么我女儿的父亲母亲要用`weak`

***Swift***

你傻啊，你女儿终有一天会嫁出去的，不用`weak`的话就和你循环引用释放不了，你还想留你女儿一辈子？但是Human的lover我没有加weak，就是故意让你们相互引用，相守一身不离不弃；

***Kotlin***

我。。。（感动ing）

***Swift***

好了好了，你先关注当下
婚礼的事情只欠东风，你这还没有结婚就有孩子了，明儿求婚还不是百分百搞定；

***Kotlin***

Swift君，多谢兄弟帮忙；婚礼的事情也希望请你多多指教，我没有经验；

***Swift***

其实都差不多了，就差一本结婚证，你们能够遵守婚姻协议后这事就成了

```swift
// 婚姻协议
protocol MarriageProtocol {
    var promise:String { get }
    var approveByGovernment: Bool {get}
}

// 这协议的内容我已经限制人类身上了，动物结婚不需要政府允许的
extension MarriageProtocol where Self: Human {
    // 誓言承诺
    var promise: String {
        return "我愿意无论是顺境或逆境，富裕或贫穷，健康或疾病，快乐或忧愁，都将毫无保留地爱ta，对ta忠诚直到永远。无论是健康或疾病。贫穷或富有，无论是年轻漂亮还是容颜老去，我都始终愿意与ta，相亲相爱，相依相伴，相濡以沫，一生一世，不离不弃"
    }
    // 政府允许
    var approveByGovernment: Bool {
        return true
    }
}
```

剩下的你们遵守婚姻协议就好了

```swift
// 人类遵守了婚姻协议：1.履行承诺；2.政府认可
extension Human: MarriageProtocol {}

let kotlinerPromise = kotliner.promise
let angelPromise = angel.promise
let angelAuthentic = kotliner.approveByGovernment // true
let kotlinerAuthentic = angel.approveByGovernment // true
```

好了，你们的婚姻都有政府见证，都有对方的誓言。恭喜你kotlin君🎉，你脱单了

***Kotlin***

Swift君 😭，我说不出话了。好兄弟无言表达我的谢意；

***Swift***

这没什么，我只是分享指导了我的撩妹技巧而已；

**`内心独白：`**

TMD,为啥我Swift君指导别人都能成功，自己单身狗当着都是偷偷摸摸的。不做ios了，我要转Android开发；脱单的可能不是考秘籍，而是靠Google爸爸啊 😩











拓展 Extensions 知识点 🎓( Swift 篇 )
==========================
扩展 就是为一个已有的类、结构体、枚举类型或者协议类型添加新功能。这包括在没有权限获取原始源代码的情况下扩展类型的能力。扩展和 Objective-C 中的分类类似。

- 拓展语法
- 计算属性
- 构造器
- 方法
- 下标
- 嵌套类型
- 协议

拓展语法
--------
使用关键字 `extension` 来声明扩展：

```swift
extension SomeType {
    // 为 SomeType 添加的新功能
}
```

可以通过扩展来扩展一个已有类型，使其采纳一个或多个协议。在这种情况下，无论是类还是结构体，协议名字的书写方式完全一样：

```swift
extension SomeType: SomeProtocol, AnotherProctocol {
    // 协议实现写到这里
}
```

计算型属性
--------
扩展可以为已有类型添加计算型实例属性和计算型类型属性


```swift

// 基础类型计算型属性扩展
// 为Swift Double 类型添加了五个计算型实例属性
extension Double {
    var km: Double { return self / 1000.0 }
    var m : Double { return self }
    var cm: Double { return self * 100.0 }
    var mm: Double { return self * 1000.0 }
}
let distance = Double(7353)
distance.km

// 结构体计算型属性扩展：与类（引用类型）类似
struct iOSer {
    var name: String
}
extension iOSer {
    var isValid: Bool {
        return !name.isEmpty
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
扩展可以为已有类型添加新的构造器。
这可以让你扩展其它类型，将你自己的定制类型作为其构造器参数，或者提供该类型的原始实现中未提供的额外初始化选项。

> Note: 扩展能为类添加新的便利构造器，但是它们不能为类添加新的指定构造器或析构器。指定构造器和析构器必须总是由原始的类实现来提供。

```swift
// 参考已有的如上代码：iOSer、Human

extension iOSer {
    // 结构体
    init(firstName: String, secondName: String) {
        self.name = firstName + secondName
    }
}

extension Human {
    // 类
    convenience init(firstName: String, secondName: String) {
        self.init(name: firstName + secondName)
    }
}
```

方法
-----
扩展可以为已有类型添加新的实例方法和类型方法、静态方法

> Note: 结构体的静态方法相当于引用类型的类型方法，类的静态方法和类方法相似，但是static修饰类时是不会被override（重写的），但class可以

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
扩展可以为已有的类、结构体和枚举添加新的嵌套类型：

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
扩展可以为已有类型添加新下标。
常见数组，字典等等

这个例子为 Swift 内建类型 Int 添加了一个整型下标。该下标 [n] 返回十进制数字从右向左数的第 n 个数字：

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
746381295[0] // 5
746381295[5] // 3
```
如果该 Int 值没有足够的位数，即下标越界，那么上述下标实现会返回 0，犹如在数字左边自动补 0：

```swift
746381295[9]
// 返回 0，即等同于：
0746381295[9]
```

扩展 && 协议
----------
详细可以参考漫谈的协议部分，其中包含面向协议编程

通过扩展遵循协议
-------------

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
----------------------------

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



扩展 Extensions ( Kotlin 篇 )
==========================
在Kotlin中，允许对类进行扩展，不需要继承或使用 Decorator 模式，通过一种特殊形式的声明，来实现某一具体功能。扩展函数是静态解析的，并未对原类增添函数或者属性，也就是说对其本身没有丝毫影响。

## 扩展函数：

在kotlin里，通过扩展函数，我们可以很方便的为一个类添加一个方法。
```kotlin
fun String.lastChar(){
    this.get(this.length - 1)
}

fun String.lastChar() = get(this.length - 1)
```
上面两种写法其实是一样的。其中，this关键字指代接收者对象(receiver object)(也就是调用扩展函数时, 在点号之前指定的对象实例)，有时可以省略。

### 扩展函数的调用

以上面的函数为例
```kotlin
 "Android".lastChar()
```

### 扩展函数的声明格式：

```kotlin
fun receiverType.functionName(params){
    body
}
```
其中
 - receiverType：表示函数的接收者，也就是函数扩展的对象
 - functionName：扩展函数的名称
 - params：扩展函数的参数，可以为NULL
 - body 函数体

### 扩展函数是静态解析的
扩展方法是静态解析的，而并不是真正给类添加了这个方法。
调用的扩展函数是由函数调用所在的表达式的类型来决定的，而不是由表达式运行时求值结果决定的。
```kotlin
open class Animal{

}
class Cat : Animal()

object Main {
    fun Animal.food() = "food"

    fun Cat.food() = "fish"

    fun Animal.printFood(anim: Animal){
        println(anim.food())
    }

    @JvmStatic fun main(args: Array<String>) {
        Animal().printFood(Cat())
    }
}
```
最终的输出是 food ，而不是 fish 。
因为扩展方法是静态解析的，在添加扩展方法的时候类型为Animal，那么即便运行时传入了子类对象，也依旧会执行参数中声明时类型的方法。

### 成员函数和扩展函数
如果一个类定义有一个成员函数和一个扩展函数，而这两个函数又有相同的接收者类型、相同的名字并且都适用给定的参数，这种情况总是优先调用成员函数。
```kotlin
class Dog : Animal(){
    fun printFood(){
          println("meat")
    }
}

object Main {
    fun Dog.printFood(){
        println("bone")
    }

    @JvmStatic fun main(args: Array<String>) {
        val dog: Dog = Dog()
        dog.printFood()
    }
}
```
这里输出 meat。

## 扩展属性

```kotlin
val TextView.leftMargin:Int
get():Int {
     return (layoutParams as ViewGroup.MarginLayoutParams).leftMargin
    }
set(value) {
        (layoutParams as ViewGroup.MarginLayoutParams).leftMargin=value
    }
```
由于扩展没有实际的将成员插入类中，因此对扩展属性来说幕后字段是无效的。所以,对于扩展属性不允许存在初始化器。 扩展属性的行为只能由显式提供的 getters/setters 定义。也就意味着扩展属性只能被声明为val而不能被声明为var.如果强制声明为var，即使进行了初始化，在运行也会报异常错误，提示该属性没有幕后字段。

## 伴生对象的扩展
如果一个类定义有一个伴生对象 ，你也可以为伴生对象定义扩展函数和属性：
```kotlin
fun Animal.Companion.eat() {
    println("eat")
}

val Animal.Companion.food: String
get() = "food"

@JvmStatic fun main(args: Array<String>) {

    println("food:${Animal.food}")
    Animal.eat()
}
```
对于伴生对象的扩展属性和方法，只需用类名作为限定符去调用他们就可以了。

## 扩展的作用域

我们在a包中定义的扩展方法
```kotlin
package a

fun Cat.food() { …… }
```
要在a包之外使用这个扩展，我们需要在调用方导入它：
```kotlin
package b
import a.food // 导入所有名为“food”的扩展
                   // 或者
import a.*   // 从“a包”导入一切

fun usage(cat: Cat) {
    cat.food()
```
