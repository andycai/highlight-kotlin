# Kotlin - lambda

------

lambda 本质上是可以传递给函数的一小段代码，Kotlin 与 Java 中的 Lambda 有一定的区别，除了对 lambda 的全面支持外，还有内联函数等简洁高效的特性。下面我们来仔细看一下。

## 高阶函数

我们知道，lambda 的应用场景就是高阶函数，我们可以把一个 lambda 当做参数传递到高阶函数中，获取返回一个 lambda。
准确的来说，高阶函数就是以另一个函数作为参数或者返回值的函数。

## 函数类型

声明函数类型，需要将函数参数类型放在括号中，紧接着是一个箭头和函数的返回类型：

```kotlin
(Int, Int) -> Unit
```

一般情况下，函数的返回值如果是 Unit 是可以省略的，但是声明函数类型时必须显示的声明返回类型，不可省略。
返回值可标记为空类型：

```kotlin
(Int, Int) -> Unit?
```

由于 Kotlin 的类型推导，可以省略函数类型的声明，直接写出函数体：

```kotlin
val sum = { x: Int, y: Int -> x + y }
val action = { println(42) }
```

## 调用作为参数的函数

```kotlin
fun twoAndThree(operation: (Int, Int) -> Int){
    //调用函数类型的参数
    val result = operation(2, 3)
    println("The result is $result")
}

fun main(arg: Array<String>) {
    twoAndThree{ a, b -> a + b}
    twoAndThree{ a, b -> a * b}
}
```

## 在 Java 中使用函数类

使用 Java 调用 Kotlin 函数类型时，函数类型会被声明为普通的接口：一个函数类型的变量时 FunctionN 接口的一个实现。Kotlin 定义了一系列接口，这些接口对应不同参数数量的函数：Function0<R>(没有参数的函数)、Function1<P1,R>(一个参数的函数)等等。每个接口定义了一个 invoke 方法。
Java 8 中的 lambda 会被自动转成函数类型的值。
Kotlin 中 Unit 类型是有一个值的，所以需要显示的返回它，一个返回 void 的 lambda 不能作为返回 Unit 的函数类型的实参。

## 函数类型的参数默认值和 null 值

声明函数类型的参数的时候可以指定默认值。

```kotlin
fun <T> Collection<T>.joinToString(
        separator: String = ",",
        prefix: String = "",
        postfix: String = "",
        transform: (T) -> String = { it.toString() }//指定参数默认值
): String {
    val result = StringBuilder(prefix)
    for ((index, element) in this.withIndex()) {
        if (index > 0) result.append(separator)
        result.append(transform(element))
    }
    result.append(postfix)
    return result.toString()
}
```

或者可空函数类型：

```kotlin
fun <T> Collection<T>.joinToString(
        separator: String = ",",
        prefix: String = "",
        postfix: String = "",
        transform: ((T) -> String)?//可空函数类型
): String {
    val result = StringBuilder(prefix)
    for ((index, element) in this.withIndex()) {
        if (index > 0) result.append(separator)
        //调用 invoke 方法执行函数
        result.append(transform?.invoke(element) ?: element.toString())
    }
    result.append(postfix)
    return result.toString()
}
```

## 返回函数的函数

```kotlin
enum class Delivery { STANDARD, EXPEDITED }

class Order(val itemCount: Int)

fun getShippingCostCalculator(
        delivery: Delivery): (Order) -> Double {
    if (delivery == Delivery.EXPEDITED) {
        return { order -> 6 + 2.1 * order.itemCount }
    }
    return { order -> 1.2 * order.itemCount }
}
```

## Lambda 表达式的语法

```kotlin
{x: Int, y: Int -> x + y}
```

可以把 lambda 表达式存储在一个变量中，把这个变量当做普通函数对待：

```kotlin
val sum = {x: Int, y: Int -> x + y}
println(sum(1, 2))
```

也可以直接调用 lambda 表达式：

```kotlin
{ println(42) }()
```

但这样语法毫无可读性，也没意义，如果确定需要把一小段代码封闭在一个代码块中，可以使用库函数 run 来执行传给它的 lambda：

```kotlin
run{ println(42) }
```

因此，上述的 maxBy 函数在不用简明语法的情况下可这么写：

```kotlin
people.maxBy({ p: Person -> p.age} )
```

Kotlin 有这样的一个约定：如果 lambda 表达式是函数调用的最后一个实参，它可以放到括号的外边。

```kotlin
people.maxBy(){ p: Person -> p.age}
```

当 lambda 是函数唯一实参时，还可以去掉代码中的空括号对：

```kotlin
people.maxBy{ p: Person -> p.age}
```

如果 lambda 参数的类型可以被推导出来，你就不需要显示地指定它：

```kotlin
people.maxBy{ p -> p.age}
```

可使用默认参数名称 it 命名参数。如果当前上下文期望的是只有一个参数的 lambda 且这个参数的类型可以推导出来，就会生成这个名称：

```kotlin
people.maxBy{ it.age}
```

如果使用变量存储 lambda，那么久没有可以推断出参数类型的上下文，所以你必须显示地指定参数类型：

```kotlin
val getAge = {p: Person -> p.age}
people.maxBy(getAge)
```

此外，如果 lambda 表达式由多个语句构成，那么最后一个表达式就是 lambda 的结果：

```kotlin
val sum = { x: Int, y: Int ->
    println("Computing the sum of $x and $y...")
    x + y
}
println(sum(1, 2))
```

## 成员引用

如果要当做参数传递的代码已经被定义成了函数，可以传递一个调用这个函数的 lambda。
但这样有点多余，如果把函数转换成一个值，就可以直接传递它，使用 :: 运算符来转换。

```kotlin
val getAge = Person::age
```

这种表达式称为成员引用，它提供了一个简明语法，来创建一个调用单个方法或者访问单个属性的函数值。双冒号把类名称与你要引用的成员（一个方法或者一个属性）名称隔开。

## 使用 Java 函数式接口

Java 中只有一个抽象方法的接口被称为函数式接口，或者 SAM 接口，SAM 接口代表抽象方法。
Kotlin 允许你在调用接受函数式接口作为参数的方法时使用 lambda。
与 Java 不同，Kotlin 具有完全的函数类型。因此，需要接受 lambda 作为参数的 Kotlin 函数应该使用函数类型而不是函数式接口类型作为这些参数的类型。

## 把 lambda 当做参数传递给 Java 方法

可以把 lambda 传给任何期望函数式接口的方法。

```kotlin
/* Java 方法*/
void postponeComputation(int delay, Runnable computation);

/* 使用 Kotlin 调用 */
postponeComputation(1000){ println(42) }
```

当我们用 Kotlin 调用上面的 Java 方法时，编译期会自动把 lambda 表达式转换成一个 Runnable 的实例。
同样我们也可以显示的创建一个匿名内部类来实现同样的效果：

```kotlin
postponeComputation(1000， object : Runnable{
    override fun run(){
        println(42)
    }
})
```

但这样有一点不同的是，显示的声明对象时，每次调用都会创建一个新的实例。使用 lambda 的情况不同：如果 lambda 没有访问任何来自定义它的函数的变量，相应的匿名类实例可以在多次调用之间重用。
但如果 lambda 从包围他的作用域中捕捉了变量，每次调用就不再是重用同一个实例了。

## SAM 构造方法：显示地把 lambda 转换成函数式接口

SAM 构造方法是编译期自动生成的函数，让你执行从 lambda 到函数接口实例的显示转换，可以在编译期不会自动应用转换的上下文中使用它。例如，如果有一个方法返回的是一个函数式接口的实例，不能直接返回一个 lambda，要用 SAM 构造方法包装起来。

```kotlin
fun createAllDoneRunnable(): Runnable {
    return Runnable { println("All done!") }
}
>>>createAllDoneRunnable().run()
```

SAM 构造方法的名称和底层函数接口的名称一样。SAM 构造方法只接收一个参数——一个被用作函数式接口单独抽象方法体的 lambda——并返回实现了这个接口的类的一个实例。
除了返回值外，SAM 构造方法还可以用在需要把从 lambda 生成的函数式接口实例存储在一个变量中的情况。

```kotlin
val listener = View.OnClickListener{view ->
    val text = when(view.id){
        R.id.btn1 -> "First button"
        R.id.btn2 -> "Second button"
        else -> "Unknown button"
    }
    Toast.makeText(this, text, Toast.LENGTH_SHORT).show()
}
btn1.setOnClickListener(listener)
btn2.setOnClickListener(listener)
```

## 内联函数：消除 lambda 带来的运行时开销

一般而言，lambda 表达式会被正常的编译成匿名类，这表示每调用一次 lambda 表达式，一个额外的类就会被创建。并且，如果 lambda 捕捉了某个变量，那么每次调用的时候都会创建一个新的对象。这导致效率较低。
但使用 inline 修饰符标记一个函数，可避免这种开销。

## 内联函数如何运作

当一个函数被声明为 inline 时，它的函数体是内联的——换句话说，函数体会被直接替换导函数被调用的地方，而不是被正常调用。

```kotlin
//使用 inline 修饰带有函数类型参数的函数
inline fun <T> synchronized(lock: Lock, action: () -> T): T {
    lock.lock()
    try {
        return action()
    } finally {
        lock.unlock()
    }
}

fun foo(l: Lock){
    println("Before sync")
    //调用 synchronized 函数
    synchronized(l){
        println("Action")
    }
    println("After sync")
}
```

使用 inline 修饰后，foo 函数会被编译成如下函数：

```kotlin
fun foo(l: Lock) {
    println("Before sync")
    //inline 修饰的函数被内联到此处
    l.lock()
    try {
        println("Action")
    } finally {
        l.unlock()
    }
    println("After sync")
}
```

## 内联函数的限制

并不是所有的函数都会被内联，如果 lambda 参数直接被调用，这样很容易内联。
反之如果 lambda 参数在某个地方被保存起来，这种情况下 lambda 表达式的代码将不能被内联。
一般来说，lambda 参数如果被直接调用，或者作为参数传给另一个 inline 函数，它是可以被内联的。否则编译期会提示错误信息。
即，如下代码不会被内联：

```kotlin
inline fun <T> synchronized(lock: Lock, action: () -> T): T {
    //此处代码将不会被内联，因为 doSynchronized 并没有声明为内联
    return doSynchronized(lock, action)
}

fun <T> doSynchronized(lock: Lock, action: () -> T): T {
    lock.lock()
    try {
        return action()
    } finally {
        lock.unlock()
    }
}
```

上述代码甚至无法通过编译，会直接报错：Illegal usage of inline-parameter...
另外，如果一个函数有两个或更多 lambda 参数，可以选择内联其中一些参数，非内联参数可使用 noinline 修饰符标记：

```kotlin
inline fun foo(inlined: () ->Unit, noinline notInlined: () ->Unit){
    //..
}
```

## 内联集合操作

Kotlin 中的集合定义了很多高阶函数，例如 filter、map 等：

```kotlin
data class Person(val name: String, val age: Int)

fun main(arg: Array<String>) {
    val person = listOf(Person("Alice", 29), Person("Bob", 31))
    println(person.filter { it.age > 30 })
}
```

上面代码调用了集合的 filter 函数，因此会被内联，不必担心性能问题。
如果我们连续调用 filter 和 map 函数：

```kotlin
fun main(arg: Array<String>) {
    val person = listOf(Person("Alice", 29), Person("Bob", 31))
    println(person.filter { it.age > 30 }.map { it.name })
}
```

filter 和 map 函数都会被内联，因此会创建中间集合保存过滤的结果，从而导致性能的降低。
这种情况下可以调用 asSequence 将集合转为序列，序列中定义的函数并没有声明为 inline。
所以在使用时应该酌情选择。

## 决定何时将函数声明称内联

如果内联函数很大，将它的字节码拷贝到每一个调用点将会极大的增加字节码的长度。此时应该将那些与 lambda 参数无关的代码抽取到一个独立的非内联函数中。

## 使用内联 lambda 管理资源

Kotlin 提供了一个叫 withLock 的函数，将锁作为一个参数，使用前获取锁，使用完释放。
它的定义如下：

```kotlin
public inline fun <T> Lock.withLock(action: () -> T): T {
    lock()
    try {
        return action()
    } finally {
        unlock()
    }
}
```

我们可以这么使用它：

```kotlin
val l: Lock = ...
l.withLock { 
    //do something
}
```

此外，Java7 中引入了 try-with-resource 语句，用来简化资源的使用及释放：

```kotlin
static String readFirstLineFromFile(String path) throws IOException{
    //try-with-resource 语句
    try(BufferedReader br = new BufferedReader(new FileReader(path))){
        return br.readLine();
    }
}
```

Kotlin 中没有提供等价的语法，但标准库中提供了 use 函数可以实现相同的功能：

```kotlin
fun readFirstLineFromFile(path: String): String{
    BufferedReader(FileReader(path)).use { br ->
        return br.readLine()
    }
}
```

use 是一个扩展函数，被用来操作可关闭资源，接受一个 lambda 作为参数。use 方法同样为内联函数。

## 高阶函数中的控制流

## lambda 中的返回语句：从一个封闭函数返回

如果在 lambda 中使用 return 关键字，它会从调用 lambda 的函数中返回，并不只是从 lambda 中返回。这样的 return 语句叫作非局部返回，因为它从一个比包含 return 的代码块更大的代码块中返回了。

```kotlin
fun lookForAlice(people: List<Person>){
    people.forEach{
        if(it.name == "Alice"){
            println("Found!")
            //此处 return 将会从整个函数中返回
            return
        }
    }
    println("Alice is not Found!")
}
```

需要注意的是，只有在以 lambda 作为参数的函数是内联函数的时候才能从更外层的函数返回。

## 从 lambda 返回：使用标签返回

也可以在 lambda 中使用局部返回。lambda 中的局部返回会终止 lambda 执行，并接着从调用 lambda 的代码处执行，要区分局部返回和非局部返回，要使用标签。

```kotlin
fun lookForAlice(people: List<Person>){
    //给 lambda 表达式加上标签
    people.forEach label@{
        if(it.name == "Alice") return@label//引用了这个标签
    }
    println("Alice might be somewhere!")//这一行总是会被调用
}
```

要标记一个 lambda 表达式，在 lambda 的花括号之前放一个标签名（可以是任何标识符），接着放一个 @ 符号。要从一个 lambda 返回，在 return 关键字后面放一个 @ 符号，接着放标签名。
另一种选择是，使用 lambda 作为参数的函数的函数名可以作为标签。如果显示的制定了 lambda 表达式的标签，再使用函数名作为标签没有任何效果，一个 lambda 表达式的标签数量不能多于一个。

同样的规则也适用于 this 标签：

```kotlin
println(StringBuilder().apply sb@{
    listOf("1", "2", "3").apply {
        this@sb.append(this.toString())
    }
})
```

局部返回的语句相当冗长，如果一个 lambda 包含多个返回语句会变得更加笨重。
解决方案是，可以使用另一种可选的语法来传递代码块：匿名函数。

## 匿名函数：默认使用局部返回

匿名函数看起来跟普通函数很相似，除了它的名字和参数类型被省略了外。

```kotlin
fun lookForAlice(people: List<Person>){
    people.forEach(fun (person){//使用匿名函数取代 lambda 表达式
        if(person.name == "Alice") return//return 指向一个最近函数：匿名函数
        println("${person.name} is not Alice")
    })
}
```

在匿名函数中，不带标签的 return 表达式会从匿名函数返回，而不是从包含匿名函数的函数返回。
规则很简单：return 从最近使用 fun 关键字声明的函数返回。
lambda 没有使用 fun 关键字，所以 lambda 中的 return 从最外层的函数返回。匿名函数使用了 fun，因此从匿名函数内返回。



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

