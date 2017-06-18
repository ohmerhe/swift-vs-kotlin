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




## Swift