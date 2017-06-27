## Kotlin 中的集合接口

![kotlin](https://cdn.kymjs.com/kotlin/6-1-1.png)

所有类声明的泛型尖括号里面如果加入了 out 关键字，则说明这个类的对象是只读的，例如他只有：get()、size（）等方法，而没有 set()、remove()等方法。
相反的，如果没有加 out 关键字，或者说如果开头是 MutableXXX 那就是一个不仅可以存储也可以读取的集合类。   
与 out 对应的，还有一个 in。表示泛型参数是只写的，也就是只有set()、remove()等方法。

## 泛型的逆变与协变

in 与 out 除了用在集合类上，用于读写权限限定以外，也可以用于普通的泛型类。   
kotlin 的泛型类是支持逆变与协变的。  
逆变就是你可以让泛型对象返回一个泛型参数父类的类型。反过来，协变就是你可以给泛型对象传入一个泛型参数类型的子类。  

```
open class A
open class B : A()
open class C : B()

val mutableList: MutableList<B> = mutableListOf(B(), B(), C())
val list: List<A> = mutableList;
```

在 Kotlin 中，可以直接把 `List<C>` 的对象赋值给 `List<A>`   
知道了上面这一点，我们接下来看 `in` 的用法。  
泛型声明中，用 `in` 声明泛型参数，表示这个参数只能是入参，不能对外返回这个参数。  

```
open class A
open class B : A()
open class C : B()

class TypeArray<in A> {
	
	//in 修饰了 A，表示 A 是可以作为参数的。
    fun getValue(a: A): Int? {
        return a?.hashCode()
    }
    
    //这段代码是非法的，因为A 不能被返回
    fun getA(a: A): A? {
        return a
    }
}
```


## 集合的初始化

在 Kotlin 中，集合类一般不使用构造方法去初始化，而是使用同一的入口方法，例如初始化一个 `MutableList`，我们使用的是如下代码：

```
 val mutableList = mutableListOf(0, 1, 2, 3) 
```

类似的初始化集合对象的方法还有

```
//创建一个 List<> 对象
var list = listOf(0, 1, 2)

//创建一个 Set<> 对象
val ss = setOf(1, 2, 4)
```  