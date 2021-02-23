# Kotlin - companion object

---

类内部的对象声明可以用 companion 关键字标记，这样它就与外部类关联在一起，我们就可以直接通过外部类访问到对象的内部元素。类似其他面向对象语言的 static 实现的静态属性和方法。

```kotlin
class MyClass {
    companion object Factory {
        fun create(): MyClass = MyClass()
    }
}

val instance = MyClass.create()   // 访问到对象的内部元素
```

我们可以省略掉该对象的对象名，然后使用 Companion 替代需要声明的对象名：

```kotlin
class MyClass {
    companion object {
    }
}

val x = MyClass.Companion
```

**注意：**一个类里面只能声明一个内部关联对象，即关键字 companion 只能使用一次。

请伴生对象的成员看起来像其他语言的静态成员，但在运行时他们仍然是真实对象的实例成员。例如还可以实现接口：

```kotlin
interface Factory<T> {
    fun create(): T
}

class MyClass {
    companion object : Factory<MyClass> {
        override fun create(): MyClass = MyClass()
    }
}
```

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

