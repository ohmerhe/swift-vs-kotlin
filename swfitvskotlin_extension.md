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
