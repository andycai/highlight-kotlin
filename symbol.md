# Kotlin 中的符号

## ?

### "?" 修饰在成员变量的类型后面
表示这个变量可以为 null，系统在任何情况下不会报它的空指针异常。  

### "?" 修饰在对象后面
代表该对象如果为 null 则不处理后面逻辑  

如果我们定义的变量是一个可以为空的类型，则要用【String?】

```
//在变量类型后面加上问号，代表该变量是可空变量 
var name: String? = "andy"
name = null // 编译正确
var name: String = "andy"
name = null // 编译错误
val len = name.length // 错误：变量“b”可能为空
val len = name?.length //如果 name非空，就返回 name.length ，否则返回 null，这个表达式的类型是 Int?
```

### let 函数和?. 同时使用来处理可空表达式

```
person?.let{ 
  // 内部的it一定是非空的
}

//如果person为空就不会调用let函数
//如果person不为空才会调用let函数，所以?.和let函数配合使用可以很方便的处理可空表达式

person?.let { 
  println([it.name](http://it.name)) // 输出name
} 
```