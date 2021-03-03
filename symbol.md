# Kotlin - 符号

---

## 符号：?

### "?"  修饰在成员变量的类型后面
表示这个变量可以为 null，系统在任何情况下不会报它的空指针异常。  

### "?"  修饰在对象后面
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

### let  函数和 ?.  同时使用来处理可空表达式

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

### ?.let  和  ?:let  连用

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

## 符号：!!

### "!!"  放在对象后面
代表该对象如果为 null 则抛出异常

### "!!"  放在方法传递实参后面
也是代表不能为 null，为 null 会抛异常

```kotlin
val len = name!!.length // 如果name不为空，则返回name.length，如果name为空，则抛出异常NullPointerException 
```

使用断 !! 可以很方便的在抛出空指针异常的时候定位到异常的变量的位置，但是千万不要连续使用断言!!

```kotlin
student!!.person!!.name // 如果报空指针异常了则无法判断到底是student为空还是person为空，所以不要连续使用断言!! 
```

"!!."  的用法就是相当于 Java 里的 if()else() 判断 null

```kotlin
val nullClass: NullClass ?= null
nullClass!!.nullFun() 
```

转化成 java 代码

```kotlin
NullClass nullClass = null;
if (nullClass != null) {
  nullClass.nullFun();
} else {
  throw new NullPointerException();
}
```

## 符号：?: Elvis 操作符

foo ?: bar 表达式，意思为:  当对象 foo 值为 null 时，那么它就会返回后面的对象 bar，否则返回 foo。

```kotlin
val len = name?.length ?: -1//当name不为空时，返回name.length，当name为空时，返回-1
val roomList: ArrayList<Room>? = null
val mySize= roomList?.size ?: 0   //mySize 的值就为0，因为 roomList?.size 为空，
val roomList: ArrayList<FPXBean>? = null
if (roomList?.size ?: 0 > 0) {    // 这一行添加了?:
  Log.d("TAG", "-->> 列表数不是0")
} 
```

## 符号：::

双冒号操作符 表示把一个方法当做一个参数，传递到另一个方法中进行使用，通俗的来讲就是引用一个方法

### 反射

利用 ::，甚至可以享受到优于 Java 的反射特性。

```kotlin
inline fun <reified T> T.foo3(string: String) {
    Log.e(T::class.simpleName, string)
}
```

这里暂时不需要理会 inline 和 reified，我们看到，直接使用泛型 T:: 即可反射获得其内部属性，如 class，constructor 等，这在 Java 中是不可能做到的。

```kotlin
//得到类的Class对象
startActivity(Intent(this@KotlinActivity, MainActivity::class.java))
```

### :: 方法名

双冒号跟方法名作用就是将方法作为参数传入。

```kotlin
class ColonMagic {
 
    /**
     * 不需要参数
     */
    private fun sayNoWords() {
        println(" no  msg   ")
    }
 
    /**
     * 一个参数
     * message：String类型
     */
    private fun say(message: String) {
        println(message)
    }
 
    /**
     * 两个参数
     * msg: String类型
     * dosay: (String) -> Unit 一个参数为String不需要返回值的方法体
     */
    private fun peopleDo(msg: String, doSay: (String) -> Unit) {
        //doSay(msg)调用的就是传入的say方法,即say(msg),只不过参数名是doSay本质是say方法
        //此处打印出来 I say !  日志
        doSay(msg)
    }
 
    fun people() {
        //调用peopleDo方法，并传入参数  “I say !” 和 say方法体
        //此处 ::say 是将say方法作为参数传入peopleDo方法
        //此处只是将say作为参数，而不会调用say方法，也就不会println(message)，不会输出日志
        peopleDo("I say !", ::say)
 
        //此处报错，因为需要的是(String) -> Unit； 一个String参数，无返回值的方法体
        //但是sayNoWords方法是没有参数的，所以会报错
        peopleDo("I say !", ::sayNoWords)
    }
 
}
 
fun main(msg: Array<String>) {
    ColonMagic().people();
}
```

## 符号：->

单从形态上看，是一种流向和对应的关系。即前面的语句执行之后，将执行流转到指向的语句，并且是对应的。

## 符号：$

```kotlin
val i = 10
println("i = $i") // prints "i = 10"

val s = "abc"
println("$s.length is ${s.length}") // prints "abc.length is 3"

// 如上面的代码中，要把" i "连接到字符串中，模板表达式以美元符号($)开头，由一个简单名称组成： $i
```

## 符号：""" 多行输入

```kotlin
//三引号的形式用来输入多行文本
val str = """ 
    one
    two
        """
//等价于          
val str = "one\ntwo"
```

三引号之间输入的内容将被原样保留，之中的单号和双引号不转义，其中的不可见字符比如/n和/t都会被保留。

## 符号：_

在Kotlin中，可以使用一个下划线字符（_）作为lambda或表达式函数的参数的名称，或作为解构条目的名称。

1. 作为lambda函数的参数名称

```kotlin
fun main(args: Array<String>) {
    val aa = mapOf(1 to "a",2 to "B")
    aa.forEach { key, value -> println("value:$value") 
}
```

在上述示例中，只是用到了value值，key并没有用到。这样，我们就想不在声明key，那么就需要使用下划线字符（_）作为key替代，即：

```kotlin
fun main(args: Array<String>) {
    val aa = mapOf(1 to "a",2 to "B")
    aa.forEach { _, value -> println("value:$value") 
}
```

2. 作为解构声明的参数

解构声明就是将一个对象解构 (destructure) 为多个变量，也就是意味着一个解构声明会一次性创建多个变量。尽管这样很方便，但是，如果用不到的变量必然也必须得声明，从而造成了变量的冗余。

```kotlin
data class Person(var age: Int, var name: String)
//Peron声明了 age，name两个变量。解构时如果只需要age这一个变量时
val person = Preson(18, "eryang")
val (age, _) = person
```

3. 数字字面值中的下划线

Kotlin 的数字面值可以使用下划线来分隔数值进行分组：

```kotlin
val oneMillion = 1_000_000
val hexBytes = 0xFF_EC_DE_5E
val bytes = 0b11010010_01101001_10010100_10010010

fun main(args: Array<String>) {
    // Log: 1000000
    println(oneMillion)
    // Log: ffecde5e
    println(hexBytes.toString(16))
    // Log: 11010010011010011001010010010010
    println(bytes.toString(2))
}
```

## 符号：@

1. 限定this的类型

```kotlin
class User {
    inner class State{
        fun getUser(): User{
            //返回User
            return this@User
        }
        fun getState(): State{
            //返回State
            return this@State
        }
    }
}
```

2. 作为标签

跳出双层for

```kotlin
loop@ for (itemA in arraysA) {
     var i : Int = 0
      for (itemB in arraysB) {
         i++
         if (itemB > 2) {
             break@loop
         }
         println("itemB:$itemB")
     }
}
```

命名函数自动定义标签

```kotlin
fun fun_run(){
    run {
        println("lambda")
    }
    var i: Int = run {
        return@run 1
    }
    println("$i")
    //匿名函数可以通过自定义标签进行跳转和返回
    i = run (outer@{
        return@outer 2
    })
    println(i)
}
```

从 forEach 函数跳出

```kotlin
fun forEach_label(ints: List<Int>)
{
    var i =2
    ints.forEach {
        //forEach中无法使用continue和break;
//        if (it == 0) continue //编译错误
//        if (it == 2) /*break //编译错误 */
        print(it)
    }
     run outer@{
         ints.forEach {
             if (it == 0) return@forEach //相当于在forEach函数中continue,实际上是从匿名函数返回
             if (it == 2) return@outer //相当于在forEach函数中使用break,实际上是跳转到outer之外
         }
     }

    if (i == 3)
    {
        //每个函数的名字代表一个函数地址，所以函数自动成为标签
        return@forEach_label //等同于return
    }
}
```

## 符号：as

### "不安全的" 类型转换操作符

如果类型转换不成功，类型转换操作符通常会抛出一个异常。因此，我们称之为 不安全的(unsafe)。在 Kotlin 中，不安全的类型转换使用中缀操作符 as

```kotlin
val y = null
val x: String = y as String
// 输出
```

注意 null 不能被转换为 String，因为这个类型不是 可为 null 的 (nullable)，也就是说，如果 y 为 null，上例中的代码将抛出一个异常。为了实现与 Java 相同的类型转换，我们需要在类型转换操作符的右侧使用可为 null 的类型，比如：

```kotlin
val y = null
val x: String? = y as String?
println("x = $x")   // x = null
```

上述代码，表示允许 String 可空，这样当 y = null 时，不会抛异常；但是，当类型转换失败时，还是会崩溃，如下：

```kotlin
val y = 66
val x: String? = y as String?
```

### "安全的" (nullable) 类型转换操作(as?)

为了避免抛出异常，你可以使用 安全的 类型转换操作符 as?，当类型转换失败时，它会返回 null，但不会抛出异常崩溃：

```dart
val y = 66
val x: String? = y as? String
println("x = $x")   // x = null

val y = null
val x: String? = y as? String
println("x = $x")   // x = null
```

尝试把值转换成给定的类型，如果类型不合适就返回 null

## 符号：..

Kotlin 中有区间的概念，区间表达式由具有操作符形式 .. 的 rangeTo 函数辅以 in 和 !in 形成。 区间是为任何可比较类型定义的，但对于整型原生类型，它有一个优化的实现。以下是使用区间的一些示例：

```kotlin
if (i in 1..10) { // 等同于 1 <= i && i <= 10 (1 <= i <= 10)
    println(i)
}
//使用until函数,创建一个不包括其结束元素的区间
for (i in 1 until 10) {   // i in [1, 10) 排除了 10
     println(i)
}

for (i in 1..4) print(i) // 输出“1234”
for (i in 4..1) print(i) // 什么都不输出
```

如果你想倒序迭代数字呢？也很简单。你可以使用标准库中定义的 downTo() 函数

```kotlin
for (i in 4 downTo 1) print(i) // 输出“4321”
```

step()函数，可以指定任意步长

```kotlin
for (i in 1..4 step 2) print(i) // 输出“13”
for (i in 4 downTo 1 step 2) print(i) // 输出“42”
```

## 符号：{}

这里指的是lambda表达式的符号。

```kotlin
// 一个参数
var callback: ((str: String) -> Unit)? = null
callback = { println(it)}
// 判断并使用
callback?.invoke("hello")

//两个参数
var callback2: ((name: String, age: Int) -> Unit)? = null
callback2 = { hello: String, world: Int -> println("$hello's age is $world") }
callback2?.invoke("Tom", 22)

var callback3 :((num1:Int, num2: Int)->String)? = null
//类型可以推断
callback3 = { num1, num2 ->
    var res:Int = num1 + num2
    res.toString()
}

println(callback3?.invoke(1, 2))
```

kotlin 中{}里面整个是 lambda 的一个表达式，而 java8 中 {} 部分只是lambda表达式的body部分。



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

