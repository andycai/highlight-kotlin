# Kotlin - extension function

------

扩展函数可以为任何类添加上一个函数，从而代替工具类
扩展函数和成员函数相同时，成员函数优先被调用
拓展韩束是静态解析的，在编译时就确定了调用函数（没有多态）

扩展函数表示即使在不修改某个类的源码的情况下，我们仍然可以对某个类添加方法，进行扩展。例如我们对User类增加登录的功能，实现在类外面添加方法。

```kotlin
fun User.login() {
  Log.i("TAG","去登录")
}

data class User(var name: String, var age: Int)
```

扩展了login方法，就可以使用user.login()。可以看到，扩展函数的主要写法就是在定义方法名的时候，通过Class.直接声明是在哪个类中。

## [目录](./README.md)

- [when](./when.md)
- [符号](./symbol.md)
- [object](./object.md)
- [companion](./companion.md)
- [标准函数](./std-func.md)
- [类型判断](./type-check.md)
- [lambda](./lambdas.md)
- [委托](./delegates.md)
- [扩展函数](./extension.md)
- [脚本](./scripting.md)
- [协程](./coroutines.md)
- [泛型](./generics.md)
- [其他特性](./other.md)

