# Swift vs. Kotlin 漫谈系列之类与继承

# 技术漫谈



# 技术知识

## Kotlin

### 类的定义

Kotlin 使用 `class` 关键字声明类。

```kotlin
class A{
}
```

类声明由类名称、类头（指定其类型参数、主 构造函数等）和由大括号包围的类体构成。类头和类体都是可选的； 如果一个类没有类体，可以省略花括号。

```kotlin
class A
```

### 实例化

但我们想要实例化一个类的对象的时候，不需要提供 `new` 关键字。

```kotlin
val invoice = Invoice()

val customer = Customer("Joe Smith")
```

### 构造函数

Kotlin 的构造函数分为 **主构造函数** 和 **次构造函数**，主构造函数只能有一个，次构造函数可以有多个。

#### 主构造函数

主构造函数是类头的一部分：它跟在类名（和可选的类型参数）后，用 `constructor ` 关键字表示。

```kotlin
class Person constructor(firstName: String) {
}
```

当主构造函数没有任何注解或者可见性修饰符，可以省略这个 `constructor ` 关键字。

```kotlin
class Person(firstName: String)
```

主构造函数没有自己的函数块，所以不能写任何代码，Kotlin 提供了以 `init` 为关键字的初始化块用来写初始化代码，以解决主构造函数不能写代码的问题，所以在初始化块中可以随意访问主构造函数的参数。

```kotlin
class Customer(name: String) {
    init {
        logger.info("Customer initialized with value ${name}")
    }
}
```

默认情况下主构造函数的参数只能被 `初始化块` 和 `属性初始化` 访问。

```kotlin
class Customer(name: String) {
    val customerKey = name.toUpperCase()
}
```

不过我们可以通过在主构造函数参数前面添加 `val` 或者 `var` 修饰符，这样朱构造函数的参数就变成了类的属性。

```kotlin
class Person(val firstName: String, val lastName: String, var age: Int) {
    // ……
}
```

**PS:如果构造函数有注解或可见性修饰符，这个 constructor 关键字是必需的，并且 这些修饰符在它前面**

#### 次构造函数

次构造函数也是用 `constructor` 修饰，写在类体里面，可以有多个。

```kotlin
class Person {
    constructor(parent: Person) {
        parent.children.add(this)
    }
}
```

如果类有主构造函数的话，次构造函数必须直接或间接（通过其他次构造函数）委托给主构造函数，委托到同一个类的另一个构造函数用 `this ` 关键字。

```kotlin
class Person(val name: String) {
    constructor(name: String, parent: Person) : this(name) {
        parent.children.add(this)
    }
}
```

如果一个非抽象类没有声明任何（主或次）构造函数，它会有一个生成的 不带参数的主构造函数。构造函数的可见性是 public。如果你不希望你的类 有一个公有构造函数，你需要声明一个带有非默认可见性的空的主构造函数。

```kotlin
class DontCreateMe private constructor () {
}
```

### 类的成员

Kotlin 类可以包含下面这些

- 构造函数和初始化块
- 函数
- 属性
- 嵌套类和内部类
- 对象声明

### 继承

我们用 `:` 声明要继承一个超级类，放在冒号后面。

```kotlin
open class Base(p: Int)

class Derived(p: Int) : Base(p)
```

如果类没有明确声明一个父类的话，默认是继承自 `Any`，`Any` 并不是 java.lang.Object；它除了 equals()、hashCode()和toString()外没有任何成员。 更多细节请查阅[Java互操作性](https://kotlinlang.org/docs/reference/java-interop.html#object-methods)部分。

在 Kotlin 里面，所有的非抽象类默认都是静态的，也就是相当于 Java 中的 final。如果想要让某个类可以被继承，必须要现式的为该类添加 `open` 的关键字，该关键字提供了和 Java 中 final 相反的功能。

如果类没有主构造函数，那么每个次构造函数必须 使用 super 关键字初始化其基类型，或委托给另一个构造函数做到这一点。 注意，在这种情况下，不同的次构造函数可以调用基类型的不同的构造函数：

```kotlin
class MyView : View {
    constructor(ctx: Context) : super(ctx)

    constructor(ctx: Context, attrs: AttributeSet) : super(ctx, attrs)
}
```

#### 抽象类与接口

同 Java 一样，Kotlin 用 `abstract` 声明一个抽象类，用 `interface` 关键字来定义接口，与 Java8 相似，接口中可以有函数的实现。

与 Java 不同，你可以在接口中定义属性。在接口中声明的属性要么是抽象的，要么提供 访问器的实现。在接口中声明的属性不能有幕后字段（backing field），因此接口中声明的访问器 不能引用它们。

```kotlin
interface MyInterface {
    val prop: Int // 抽象的

    val propertyWithImplementation: String
        get() = "foo"

    fun foo() {
        print(prop)
    }
}

class Child : MyInterface {
    override val prop: Int = 29
}
```

#### 覆盖方法

默认类的函数也是静态的，如果一个函数需要被重写，我们必须手动让他变成开放的，即在函数前面添加 `open` 关键字。如果子类想要重写某个方法，必须用 `override` 关键字修饰该方法，否则会报错。

```kotlin
pen class Base {
    open fun v() {}
    fun nv() {}
}
class Derived() : Base() {
    override fun v() {}
}
```

被 `override` 修饰的函数默认也是开放的，如果不想它再被继承，需要 `final` 来修饰该函数。

```kotlin
open class AnotherDerived() : Base() {
    final override fun v() {}
}
```

#### 覆盖属性

属性覆盖和函数覆盖类似，需要使用 `open` 和 `override` 关键字，覆盖属性的类型必须兼容。

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

#### 覆盖规则

在 Kotlin 中，如果一个类从它的直接超类继承相同成员的多个实现（由于接口函数可以有实现），它必须覆盖这个成员并提供其自己的实现。 为了表示采用从哪个超类型继承的实现，我们使用由尖括号中超类型名限定的 super，如 super<Base>。

```kotlin
open class A {
    open fun f() { print("A") }
    fun a() { print("a") }
}

interface B {
    fun f() { print("B") } // interface members are 'open' by default
    fun b() { print("b") }
}

class C() : A(), B {
    // The compiler requires f() to be overridden:
    override fun f() {
        super<A>.f() // call to A.f()
        super<B>.f() // call to B.f()
    }
}
```

同时继承 A 和 B 没问题，并且 a() 和 b() 也没问题因为 C 只继承了每个函数的一个实现。 但是 f() 由 C 继承了两个实现，所以我们必须在 C 中覆盖 f() 并且提供我们自己的实现来消除歧义。

### 伴生对象

Kotlin 中可以用 `object` 关键字直接定义一个对象，在类内部，我们可以用 `companion` 为类声明一个伴生对象。伴生对象的成员可通过只使用类名作为限定符来调用，伴生对象的成员看起来像 Java 的静态成员，在运行时他们仍然是真实对象的实例成员。在 JVM 平台，如果使用 @JvmStatic 注解，你可以将伴生对象的成员生成为真正的 静态方法和字段。更详细信息请参见[Java 互操作](https://kotlinlang.org/docs/reference/java-to-kotlin-interop.html#static-fields)性一节。

```kotlin
class MyClass {
    companion object Factory {
        fun create(): MyClass = MyClass()
    }
}

val instance = MyClass.create()
```

### 内部类和嵌套类

我们可以直接在一个类里面定义另外一个类。当里面的类被 `Inner` 修饰符修饰，则是类的内部类，否则我们只是定义了一个嵌套类。

**嵌套类：**

```kotlin
class Outer {
    private val bar: Int = 1
    class Nested {
        fun foo() = 2
    }
}

val demo = Outer.Nested().foo() // == 2
```

**内部类：**

```kotlin
class Outer {
    private val bar: Int = 1
    inner class Inner {
        fun foo() = bar
    }
}

val demo = Outer().Inner().foo() // == 1
```

在 Kotlin 也有匿名内部类的概念，不过使用 `object` 表达式修饰：

```kotlin
window.addMouseListener(object: MouseAdapter() {
    override fun mouseClicked(e: MouseEvent) {
        // ...
    }
                                                                                                            
    override fun mouseEntered(e: MouseEvent) {
        // ...
    }
})
```

## Swift

### 类的定义

Swift 中用 `class` 关键字来定义类：

```swift
class SomeClass {

}
```

然后可以创建这个类的实例：

```swift
let instance = SomeClass()
```

### 构造函数和析构函数

构造函数也可以叫做初始化器（Initializer）  
用 `init` 关键字来定义类的构造函数

```swift
class SomeClass {
    init(string: String) {
	
    }
}

let instance = SomeClass(string: "KotlinThree")
```

用 `deinit` 关键字来定义析构函数，析构函数在类销毁时调用。

```swift
class SomeClass {
    deinit {
	
    }
}
```

### 指定初始化器和便捷初始化器
 
指定初始化器（Designated Initializer）是类的主要初始化器，每个类都至少需要有一个指定初始化器。

在上面的例子中用 init 定义的就是指定初始化器。

便捷初始化器（Convenience Initializer）需要用 `convenience` 来修饰。便捷初始化器需要去调用指定初始化器来完成初始化。

```swift
class SomeClass {
    private let string: String
    
    init(string: String) {
        self.string = string
    }
    
    convenience init() {
        self.init(string: "KotlinThree")
    }
    
    func printString() {
        print(string)
    }
}

let instance = SomeClass()
instance.printString()
/*
输出结果：
KotlinThree
*/
```

### 类方法和实例方法

用 `class` 或 `static` 关键字修饰的方法就是类方法，这两个关键字的区别是 `class` 修饰的类方法可以被子类复写，`static` 修饰的类方法不行。

既没用 `class` 修饰，也没用 `static` 修饰的就是实例方法。

```swift
class SomeClass {
	class func classMethod() {
	
	}
	
	static func cannotBeOverridedClassMethod() {
	
	}
	
	func instanceMethod() {
	
	}
}

// 调用类方法
SomeClass.classMethod()
// 调用实例方法
let instance = SomeClass()
instance. instanceMethod()
```

### 类的继承

Swift 中用 `:` 来声明类的继承关系

```swift
class BaseClass {
    func baseFunction() {
	
    }
}

class SomeClass: BaseClass {

}
```

子类会获得父类的非 private 的属性和方法

```swift
let instance = SomeClass()
instance.baseFunction()
```

子类如果需要复写父类的方法，需要用 `override` 关键字来修饰

```
class SomeClass: BaseClass {
    override func baseFunction() {
        // ...
    }
}
```



