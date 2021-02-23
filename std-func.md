# Kotlin - 标准函数

------

kotlin的标准函数，指的是Standard.kt文件中定义的函数，包括let、also、with、run、apply函数。

- 返回自身
   从 **apply** 和 **also** 中选
   1.作用域中使用this作为参数选择apply
   2.作用域中使用it作为参数选择also
- 不需要返回自身
   从 **run** 和 **let** 中选
   1.作用域中使用this作为参数选择run
   2.作用域中使用it作为参数选择let

**apply**适合对一个对象做附加操作的时候
 **let**适合空判断的时候
 **with**适合对同一个对象进行对此操作的时候

## let函数

let扩展函数的实际上是一个作用域函数，当你需要去定义一个变量在一个特定的作用域范围内，let函数的是一个不错的选择；let函数另一个作用就是可以避免写一些判断null的操作。

**适用场景**
 场景一: 最常用的场景就是使用let函数处理需要针对一个可null的对象统一做判空处理。

```swift
        //没有let函数，需要每次判空，代码不够优雅
         data?.toString()
        data?.toString()
        data?.toString()

data?.let {
            //在函数域中，保证data对象不为，不用多次去判空
            it.toString()
            it.toString()
            it.toString()
        }
```

场景二: 然后就是需要去明确一个变量所处特定的作用域范围内可以使用

```swift
data.let {
            //在函数体内使用it替代该data对象使用
            it.toString()
        }
```

## also函数

also函数使用的一般结构

```css
object.also{

}
```

适用于let函数的任何场景，also函数和let很像，只是唯一的不同点就是let函数最后的返回值是最后一行的返回值而also函数的返回值是返回当前的这个对象。

```bash
        user?.also {
            it.age = 18
            it.name = "小明"
        }.age
```

## with函数

with函数使用的一般结构

```csharp
with(object){
   //todo
 }
```

它是将某对象作为函数的参数，在函数块内可以通过 this 指代该对象。

**适用场景**
 需要设置某个对象多个属性到UI上时，需要不断调用对象，使用with函数能避免对象的重复书写。

```kotlin
        data class User(var name: String, var age: Int)

        with(user){
            Log.i("TAG", "age=$age")
            Log.i("TAG", "name=$name")
        }
```

该age和name变量就是该with参数中的user对象的属性值。

## run函数

run函数使用的一般结构

```css
object.run{
    
}
```

**适用场景**
 适用于let,with函数任何场景。因为run函数是let,with两个函数结合体，准确来说它弥补了let函数在函数体内必须使用it参数替代对象，在run函数中可以像with函数一样可以省略，直接访问实例的公有属性和方法，另一方面它弥补了with函数传入对象判空问题，在run函数中可以像let函数一样做判空处理。

```bash
        user?.run {
            Log.i("TAG", "age=$age")
            Log.i("TAG", "name=$name")
        }
```

## apply函数

apply函数使用的一般结构

```css
object.apply{

}
```

从结构上来看apply函数和run函数很像，唯一不同点就是它们各自返回的值不一样，run函数是以闭包形式返回最后一行代码的值，而apply可以任意调用该对象的任意方法，并返回该对象。

**适用场景**
 整体作用功能和run函数很像，唯一不同点就是它返回的值是对象本身，而run函数是一个闭包形式返回，返回的是最后一行的值。

场景一：apply一般用于一个对象实例初始化的时候，需要对对象中的属性进行赋值。

```kotlin
data class User(var name: String, var age: Int)

ArrayList<User>().apply {
            add(User("小明", 18))
            add(User("小王", 16))

        }.run {
            get(0)
        }       
```

创建User对象并添加入集合，添加完成后取出第一个对象。

场景二：多层级判空问题

```kotlin
    data class Response(var code: Int, var data: Data)
    data class Data(var icon: String, var title: String)
        response?.apply {
            //response不为空时可操作response
        }.data?.apply {
            //data不为空时可操作data
        }.title?.apply {
            //title不为空时可操作title
        }
```

###### let,with,run,apply,also函数总结：

| 函数名 | 函数体内使用的对象       | 返回值       | 适用场景                                                |
| ------ | ------------------------ | ------------ | ------------------------------------------------------- |
| let    | it指代当前对象           | 闭包形式返回 | 适用于处理多处判空的场景                                |
| also   | it指代当前对象           | 返回this     | 适用于let函数的任何场景，一般可用于多个扩展函数链式调用 |
| with   | this指代当前对象或者省略 | 闭包形式返回 | 适用于调用同一个类的多个方法、属性时，省去每次书写类名  |
| run    | this指代当前对象或者省略 | 闭包形式返回 | 适用于let,with函数任何场景                              |
| apply  | this指代当前对象或者省略 | 返回this     | 适用于run函数任何场景，一般可用于多个扩展函数链式调用   |

## takeIf，takeUnless函数

```kotlin
        user.name.takeIf {
            TextUtils.isEmpty(it)
        }.let {
            print(it)
        }

        user.name.takeUnless {
            !TextUtils.isEmpty(it)
        }.let {
            print(it)
        }
```

takeIf的闭包返回一个判断结果，如果为false时takeIf函数返回null；takeUnless与takeIf相反，为true时takeUnless函数返回null。

使用场景：只需要单个if分支语句的时候
 优点：
 可以配合其他作用域函数返回的结果，做出单向判断，保持链式调用
 简化写法，逻辑清晰，减少代码量，代码更优雅



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



