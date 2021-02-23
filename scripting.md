# Kotlin - scripting

---

## Kotlin 使用命令行执行kts脚本

Kotlin 也可以作为一个脚本语言使用，文件后缀名为 .kts 。

例如我们创建一个名为 list_folders.kts，代码如下：

```kotlin
import java.io.File

val folders = File(args[0]).listFiles { file -> file.isDirectory() }
folders?.forEach { folder -> println(folder) }
```

执行时通过 -script 选项设置相应的脚本文件。

```ruby
$ kotlinc -script list_folders.kts <path_to_folder>
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

