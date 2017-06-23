# Swift vs. Kotlin 漫谈系列之接口

Kotlin 君和 Swift 君在一个团队一起开发已经很久了，由于平台的差异性，他们经常会进行一些技术上的交流（PK），「Kotlin vs. Swift」课程就是他们在互相切磋是的语录。内容会由简及深，慢慢深入。

# 技术漫谈

**Kotlin:**
**Swift:**
**Kotlin:**
**Swift:**
**Kotlin:**
**Swift:**
**Kotlin:**
**Swift:**
**Kotlin:**
**Swift:**
**Kotlin:**
**Swift:**
**Kotlin:**
**Swift:**
**Kotlin:**
**Swift:**
**Kotlin:**
**Swift:**
**Kotlin:**
**Swift:**
**Kotlin:**
**Swift:**
**Kotlin:**
**Swift:**

# 技术知识

## Kotlin

Kotlin 在类里面用 val 定义只读属性，用 var 定义可变属性。类的对象可以用过名称访问类的属性。

Kotlin 定义变量的完整语法

```
var <propertyName>[: <PropertyType>] [= <property_initializer>]
    [<getter>]
    [<setter>]
```

其中 initializer、getter 和 setter 是并不是必须的。在属性类型可以通过 initializer 推断的情况下，属性类型也可以不声明。

```
val a: Int = 1  // 直接声明
val b = 2   // `Int` 类型是推断出来的
val c: Int  // 在没有初始化赋值的化，需要定义类型
```

### Getters and Setters

我们可以自定义属性的访问器，定义在属性声明内部。

```kotlin
val isEmpty: Boolean
    get() = this.size == 0
```

```kotlin
var stringRepresentation: String
    get() = this.toString()
    set(value) {
        setDataFromString(value) // 解析字符串并赋值给其他属性
    }
```

从 1.1 起，Kotlin 还可以根据 Getter 方法推断出属性的类型。

```kotlin
val isEmpty get() = this.size == 0  // 具有类型 Boolean
```

#### 修改可见性和注解
Kotlin 可以修改 Setter 方法的可见性，但是只能定义比属性本身相等或者更小的可见性，修改可见性的时候可以只定义可见性关键字和 set 关键字。

``` Kotlin 
var setterVisibility: String = "abc"
    private set // 此 setter 是私有的并且有默认实现

var setterWithAnnotation: Any? = null
    @Inject set // 用 Inject 注解此 setter
```

### 支持域

Kotlin 类里面并没有域，但是为了自定义访问器 Kotlin 提供了「支持域(Backing Fields)」来达到目的，我们用 `field` 关键字表示。

```kotlin
var counter = 0 // the initializer value is written directly to the backing field
    set(value) {
        if (value >= 0) field = value
    }
```

`field` 关键字只能用在属性的访问器中。

Kotlin 会为属性的默认访问器生成支持域，如果属性的访问器全部被自定义，且在自定义的访问器中没有用到支持域，那么就不会有支持域了。

### 支持属性

支持属性其实就是将属性通过自定义的访问器完全委托给另外一个相同类型的私有属性，我们就将这个私有属性我们就可以称为支持属性。

```kotlin
private var _table: Map<String, Int>? = null
public val table: Map<String, Int>
    get() {
        if (_table == null) {
            _table = HashMap() // Type parameters are inferred
        }
        return _table ?: throw AssertionError("Set to null by another thread")
    }
```

### 编译期常量

已知值的属性可以使用 const 修饰符标记为 编译期常量。 这些属性需要满足以下要求：

- 位于顶层或者是 object 的一个成员
- 用 String 或原生类型 值初始化
- 没有自定义 getter

这些属性可以用在注解中：

```
const val SUBSYSTEM_DEPRECATED: String = "This subsystem is deprecated"

@Deprecated(SUBSYSTEM_DEPRECATED) fun foo() { ... }
```

### 延迟加载

通常，声明为非空类型的属性必须在构造函数中初始化。但是，很多时候这并不方便。 例如，可以通过依赖注入或单元测试的设置方法初始化属性。在这种情况下，您不能在构造函数中提供非空的初始值，但是在引用类的体内属性时，仍然希望避免空检查。

Kotlin 提供了 `lateinit` 关键字来修饰一个属性，可以让属性支持延迟加载。

```kotlin
public class MyTest {
    lateinit var subject: TestSubject

    @SetUp fun setup() {
        subject = TestSubject()
    }

    @Test fun test() {
        subject.method()  // dereference directly
    }
}
```

使用 `lateinit` 关键字有一些限制，

- 只能用于在一个类的主体内声明的var属性（不在主构造函数中）
- 该属性没有自定义的getter或setter
- 属性的类型必须为非空，并且不能为原始类型

当访问一个还没有被初始化的延迟加载属性，会抛出相应的异常，告知该属性还未被初始化。

### 覆盖属性

属性覆盖和函数覆盖类似，需要使用 `open` 和 `override` 关键字，覆盖属性的类型必须兼容。每个声明的属性可以被具有初始化器的属性或具有getter方法的属性覆盖。

```
open class Foo {
    open val x: Int get { ... }
}

class Bar1 : Foo() {
    override val x: Int = ...
}
```

我们可以用 `var` 属性覆盖 `val` 属性，反过来却不行。这是因为 `var` 本质上是多声明了一个 `setter` 方法。

此外，我们还可以在类的主构造函数的参数也可以同样实现属性覆盖。

```kotlin
interface Foo {
    val count: Int
}

class Bar1(override val count: Int) : Foo

class Bar2 : Foo {
    override var count: Int = 0
}
```

### 属性委托

定义一个委托属性的语法是 `val/var <property name>: <Type> by <expression>`，其中 `by` 后面的就是属性的委托。属性委托不用继承什么特别的接口，只要拥有用 `operator` 修饰的 `getValue()` 和 `setValue()` (适用 var)的函数就可以了。

```kotlin
class Delegate {
    operator fun getValue(thisRef: Any?, property: KProperty<*>): String {
        return "$thisRef, thank you for delegating '${property.name}' to me!"
    }
 
    operator fun setValue(thisRef: Any?, property: KProperty<*>, value: String) {
        println("$value has been assigned to '${property.name} in $thisRef.'")
    }
}
```

对于参数的意义：

- `thisRef`，属性的拥有者；
- `property`，对属性的描述，是 KProperty<*> 类型或是它的父类；
- `value`，属性的值。

#### lazy

通过 `lazy` 我们可以定义一个懒加载的属性，该属性的初始化不会再类创建的时候发生，而是在第一次用到它的时候赋值。

```kotlin
val propLazy: Int by lazy{1}
```

#### Observable

官方推荐另外一个委托属性的应用就是 `Observable`，让属性在发生变动的时候可以被关注的地方观察到。

```kotlin
class User {
    var name: String by Delegates.observable("<no name>") {
        prop, old, new ->
        println("$old -> $new")
    }
}

fun main(args: Array<String>) {
    val user = User()
    user.name = "first"
    user.name = "second"
}
```

#### Storing

`Storing` 的使用场景是被模型的属性全部委托到 `Map` 的结构去真实的存储数据，用于解析 `Json` 或者做一些动态的事情。

```kotlin
class User(val map: Map<String, Any?>) {
    val name: String by map
    val age: Int     by map
}
``` 




## Swift