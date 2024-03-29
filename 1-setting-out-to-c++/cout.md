# cout

* 从概念上看，输出是一个流，即从程序流出的一系列字符。`cout` 对象表示这种流，其属性是在 `iostream` 文件中定义的。`cout` 的对象属性包括一个插入运算符（`<<`），它可以将其右侧的信息插入到流中。
* 诸如 `endl` 等对于 `cout` 来说有特殊含义的特殊符号被称为控制符（`manipulator`）。
* 显示用引号括起的字符串时，通常使用换行符 `\n`，在其他情况下则使用控制符 `endl`。一个差别是，`endl` 确保程序继续运行前刷新输出（将其立即显示在屏幕上）；而使用 `\n` 不能提供这样的保证，这意味着在有些系统中，有时可能在您输入信息后才会出现提示。
* 在默认情况下，cout以十进制格式显示整数，而不管这些整数在程序中是如何书写的。
* 控制符 `dec`、`hex` 和 `oct`，分别用于指示 `cout` 以十进制、十六进制和八进制格式显示整数。
* `setf()` 这种调用迫使输出使用定点表示法，以便更好地了解精度，它防止程序把较大的值切换为 `E` 表示法，并使程序显示到小数点后6位。参数 `ios_base::fixed` 和 `ios_base::floatfield` 是通过包含 `iostream` 来提供的常量。
* 通常 `cout` 会删除结尾的零，调用 `cout.setf()` 将覆盖这种行为。

