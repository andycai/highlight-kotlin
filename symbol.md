# Kotlin 中的符号

## 一、符号：?

### "?" 修饰在成员变量的类型后面
表示这个变量可以为 null，系统在任何情况下不会报它的空指针异常。  

### "?" 修饰在对象后面
代表该对象如果为 null 则不处理后面逻辑  

如果我们定义的变量是一个可以为空的类型，则要用【String?】

```kotlin
//在变量类型后面加上问号，代表该变量是可空变量 
var name: String? = "andy"
name = null // 编译正确
var name: String = "andy"
name = null // 编译错误
val len = name.length // 错误：变量“b”可能为空
val len = name?.length //如果 name非空，就返回 name.length ，否则返回 null，这个表达式的类型是 Int?
```

### let 函数和?. 同时使用来处理可空表达式

```kotlin
user?.let{ 
  // 内部的it一定是非空的
}

//如果 user 为空就不会调用let函数
//如果 user 不为空才会调用let函数，所以?.和let函数配合使用可以很方便的处理可空表达式

user?.let { 
  println([it.name](http://it.name)) // 输出name
} 
```

### ?.let 和 ?:let 连用

```kotlin
// 输出：not null
var name: String? = "andy"
num?.let {
  println("It's not null")
}?:let {
  println("It's null")
}

// 输出：null
var name: String? = null
num?.let {
  println("not null")
}?:let {
  println("null")
}

```

## 二、符号：!!

### "!!" 放在对象后面
代表该对象如果为 null 则抛出异常

### "!!" 放在方法传递实参后面
也是代表不能为 null，为 null 会抛异常

```kotlin
val len = name!!.length // 如果name不为空，则返回name.length，如果name为空，则抛出异常NullPointerException 
```

使用断!! 可以很方便的在抛出空指针异常的时候定位到异常的变量的位置，但是千万不要连续使用断言!!

```kotlin
student!!.person!!.name // 如果报空指针异常了则无法判断到底是student为空还是person为空，所以不要连续使用断言!! 
```

"!!." 的用法就是相当于 Java 里的 if()else() 判断 null

```kotlin
val nullClass: NullClass ?= null
nullClass!!.nullFun() 
```

转化成 java 代码

```java
NullClass nullClass = null;
if (nullClass != null) {
  nullClass.nullFun();
} else {
  throw new NullPointerException();
}
```

[目录](./README.md)

- [when](./when.md)
- [符号](./symbol.md)
- [object](./object.md)
- [companion](./companion.md)

