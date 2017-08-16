# 枚举

## Swift

### 枚举的定义

枚举为一组相关的值定义了一个共同的类型，使你可以在你的代码中以类型安全的方式来使用这些值。。

通过关键字 enum 来定义枚举，语法如下：

```
enum SomeEnumeration {
    // enumeration definition goes here
}
```

枚举中定义的值（如 north，south，east和west）是这个枚举的成员值（或成员）。你可以使用case关键字来定义一个新的枚举成员值。

例如可以定义指南针的四个点：

```
enum CompassPoint {
    case north
    case south
    case east
    case west
}
```

多个成员值可以出现在同一行上，用逗号隔开：

```
enum CompassPoint {
    case north, south, east, west
}
```

### 枚举和 Switch 语句

可以用 Switch 语句来匹配枚举的值。

```
directionToHead = .south
switch directionToHead {
case .north:
    print("Lots of planets have a north")
case .south:
    print("Watch out for penguins")
case .east:
    print("Where the sun rises")
case .west:
    print("Where the skies are blue")
}
```

### 关联值

枚举的每个成员值可以关联一个或多个关联值，定义方式如下：

```
enum Barcode {
	case upc(Int, Int, Int, Int)
	case qrCode(String)
}
```

定义一个名为Barcode的枚举类型，它的一个成员值是具有(Int，Int，Int，Int)类型关联值的upc，另一个成员值是具有String类型关联值的qrCode。

然后可以使用任意一种条形码类型创建新的条形码，例如：

```
var productBarcode = Barcode.upc(8, 85909, 51226, 3)
```

上面的例子创建了一个名为productBarcode的变量，并将Barcode.upc赋值给它，关联的元组值为(8, 85909, 51226, 3)。

productBarcode 也可以被赋值为：

```
productBarcode = .qrCode("ABCDEFGHIJKLMNOP")
```

可以用 Switch 语句模式匹配来提取关联值：

```
switch productBarcode {
case .upc(let numberSystem, let manufacturer, let product, let check):
    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
case .qrCode(let productCode):
    print("QR code: \(productCode).")
}
// 打印 "QR code: ABCDEFGHIJKLMNOP."
```

如果一个枚举成员的所有关联值都被提取为常量，或者都被提取为变量，为了简洁，你可以只在成员名称前标注一个let或者var：

```
switch productBarcode {
case let .upc(numberSystem, manufacturer, product, check):
    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
case let .qrCode(productCode):
    print("QR code: \(productCode).")
}
// 输出 "QR code: ABCDEFGHIJKLMNOP."
```

### 原始值

枚举成员可以被默认值（称为原始值）预填充，这些原始值的类型必须相同。

这是一个使用 ASCII 码作为原始值的枚举：

```
enum ASCIIControlCharacter: Character {
    case tab = "\t"
    case lineFeed = "\n"
    case carriageReturn = "\r"
}
```

原始值可以是字符串，字符，或者任意整型值或浮点型值。

#### 原始值的隐式赋值

在使用原始值为整数或者字符串类型的枚举时，不需要显式地为每一个枚举成员设置原始值，Swift 将会自动为你赋值。

例如：

```
enum Planet: Int {
    case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
}
```

上面代码中 Planet 被声明为 Int 型，这时 mercury 的原始值会被赋值为 0，venus 被赋值为 1，依次递增。

在看字符串类型的枚举的例子：

```
enum CompassPoint: String {
    case north, south, east, west
}
```

当使用字符串作为枚举类型的原始值时，每个枚举成员的隐式原始值为该枚举成员的名称。
上面的枚举中，north 会被隐式赋值为 "north", south 被隐式赋值为 "south"。

#### 原始值的获取

使用枚举成员的 rawValue 属性可以获取到原始值，例如：

```
let earthsOrder = Planet.earth.rawValue
// earthsOrder 值为 3

let sunsetDirection = CompassPoint.west.rawValue
// sunsetDirection 值为 "west"
```

#### 使用原始值初始化枚举实例

如果在定义枚举类型的时候使用了原始值，那么将会自动获得一个初始化方法，这个方法接收一个叫做rawValue的参数，参数类型即为原始值类型，返回值则是枚举成员或nil。

```
let possiblePlanet = Planet(rawValue: 7)
// possiblePlanet 类型为 Planet? 值为 Planet.uranus
```

### 递归枚举

递归枚举是一种枚举类型，它有一个或多个枚举成员使用该枚举类型的实例作为关联值。使用递归枚举时，编译器会插入一个间接层。你可以在枚举成员前加上indirect来表示该成员可递归。简单的说就是枚举成员的关联值类型就是枚举本身。

下面的例子中，在 addition 和 multiplication 这两个成员前加上 indirect 表示这两个成员是可递归的。

```
enum ArithmeticExpression {
    case number(Int)
    indirect case addition(ArithmeticExpression, ArithmeticExpression)
    indirect case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```

你也可以在枚举类型开头加上indirect关键字来表明它的所有成员都是可递归的：

```
indirect enum ArithmeticExpression {
    case number(Int)
    case addition(ArithmeticExpression, ArithmeticExpression)
    case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```

要操作具有递归性质的数据结构，使用递归函数是一种直截了当的方式。例如，下面是一个对算术表达式求值的函数：

```
func evaluate(_ expression: ArithmeticExpression) -> Int {
    switch expression {
    case let .number(value):
        return value
    case let .addition(left, right):
        return evaluate(left) + evaluate(right)
    case let .multiplication(left, right):
        return evaluate(left) * evaluate(right)
    }
}

print(evaluate(product))
// 打印 "18"
```

### 枚举的属性和方法

枚举跟类和结构体一样，也可以拥有自己的属性和方法。

```
enum ComputerType {
    case desktop
    case laptop
    
    var name: String {
        switch self {
        case .desktop:
            return "台式机"
        case .laptop:
            return "笔记本"
        }
    }
    
    func calculatePrice() -> Double {
        switch self {
        case .desktop:
            return 4000
        case .laptop:
            return 8000
        }
    }
}
```


