# Kotlin - delegate

------

委托是软件设计的一种模式，当无法或不想访问某个对象或访问某个对象存在困难时，可以交给委托类来处理。

- 类委托：即一个类中定义的方法实际是调用另一个类的对象的方法来实现的。

```kotlin
interface User {
    fun login()
}

class UserImpl(val name: String) : User{
    override fun login() {
        println(name)
    }
}

class VipUser(user: User) : User by user

fun main() {
    VipUser(UserImpl("1号用户")).login()
}
```

可以看到委托类并没有实现User接口，而是通过关键字by,将实现委托给了user。

- 属性委托：指的是一个类的某个属性值不是在类中直接定义，而是将其委托给一个代理类，从而实现对该类属性的统一管理。

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

