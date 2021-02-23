# Kotlin - 其他特性

------

## 集合操作符

Kotlin中可以通过集合操作符直接对集合进行操作，从而得到想要的结果。

> map:对集合中的数据做改变，可以改变数据的类型。
>  filter:得到所有符合Lambda闭包中操作的数据。
>  find:得到符合Lambda闭包中操作的第一个数据。
>  findLast:得到符合Lambda闭包中操作的最后一个数据。
>  reduce:含有两个参数(集合中相邻的两个参数，用于遍历整个集合)，对这两个参数进行操作，返回一个新参数，要求类型与集合中的参数类型相同。

## 协变与逆变

我们约定，在一个泛型类或者泛型接口的方法中，它的参数列表是接收数据的地方，就称为in位置，他的返回值是输出数据的地方，就称为out位置。

```kotlin
fun <T>test(param: T):T {
    return param
}
```

如上test方法中，传入泛型T的位置为in位置，返回类型T的位置为out位置。

泛型的协变：假如定义了一个MyClass<T>的泛型类，其中A是B的子类型，同时MyClass<A>又是MyClass<B>的子类型，我们就可以称MyClass在T这个泛型是协变的。如果泛型都是只读的（泛型加上out关键字），就能实现MyClass<A>是MyClass<B>的子类型
 泛型的逆变：假如定义了一个MyClass<T>的泛型类，其中A是B的子类型，同时MyClass<B>又是MyClass<A>的子类型，我们就可以称MyClass在T这个泛型是逆变的。如果泛型都是只写的（泛型加上in关键字），就能实现MyClass<B>是MyClass<A>的子类型

## 高阶函数

如果一个函数接收另一个函数作为参数，或者返回类型是一个函数，那么这个函数我们就称之为高阶函数。
 简单用例：

```kotlin
class Gaojie {
    fun main() {
        calculate(10, 5, ::add)
    }
}

/**
 * 加法
 */
fun add(a: Int, b: Int): Int {
    return a+b
}

/**
 * 减法
 */
fun minus(a: Int, b: Int): Int {
    return a-b
}

/**
 * 运算
 *
 * operate:(Int, Int)->Int 表示定义的一个函数，将其作为calculate参数的形参，实际传的时候是传入add或minus函数
 */
fun calculate(a:Int, b:Int, operate:(Int, Int)->Int) {
    var result = operate(a,b)
    print("操作结果为 $result")
}
```

###### 使用高阶函数(将函数类型作为返回值)

```kotlin
//此方法的作用是如果type等于1，那么就从Num类中取出num进行乘法操作，如果不等于一，就进行相加操作
    fun getNum(type: Int): (Num) -> Int {
        if (type == 1) {
          //这是一种写法
            return { entity ->
                entity.num * entity.num
            }
        } else {
         //这是另一种简写，可以用it代替entity参数
            return {
                it.num + it.num
            }
        }
    }
```

首先定义了一个高阶函数，他传入三个参数，两个Int型的值和一个函数类型的值，在方法内部调用“函数类型的值”，因为它本身是函数，所以可以直接调用，并且将前两个Int型作为形参传了进去。接下来我们定义了两个函数add和minus，这两个函数实现他们本身的逻辑，最后在main函数里面调用了此高阶函数，其中::add和::minus是固定写法，表示函数的引用。



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

