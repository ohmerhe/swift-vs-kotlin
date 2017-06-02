# 类的定义

Swift 中用 `class` 关键字来定义类：

```swift
class SomeClass {

}
```

然后可以创建这个类的实例：

```
let instance = SomeClass()
```

# 构造函数和析构函数

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

# 指定初始化器和便捷初始化器
 
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

# 类方法和实例方法

用 `class` 或 `static` 关键字修饰的方法就是类方法，这两个关键字的区别是 `class` 修饰的类方法可以被子类复写，`static` 修饰的类方法不行。

既没用 `class` 修饰，也没用 `static` 修饰的就是实例方法。

```
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

# 类的继承

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



