# 主要使用man命令查看函数原型及解释。

开始时间：16：25

预计结束时间：17：25

问题：

- man命令如何使用不影响之前的内容查看？
- 如何在windows的git bash上安装man命令？
- man命令如何移动？
- man命令对于每一个函数包括什么内容？
- 如何快速查看主要的信息？

问题解决方案：

1. 首先在git bash命令行输入man --help，显示命令为找到，然后输入where man，也显示命令未找到。

   ```
   普通的橘子@▒▒ͨ▒▒▒▒▒▒ MINGW64 /usr/share
   $ man --help
   bash: man: command not found
   
   普通的橘子@▒▒ͨ▒▒▒▒▒▒ MINGW64 /usr/share
   $ where man
   信息: 用提供的模式无法找到文件。
   ```

​	![image-20240119165120348](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240119165120348.png)

​	实际上，man手册是不可用的，建议可以使用本地虚拟机或者Linux服务器来快速使用。



2. 快捷键：Shortcut Keys，快捷键的目的是使得查看的速度变得更快，使用的所有快捷如下：

| 快捷键 | 功能                                                         |
| ------ | ------------------------------------------------------------ |
| j      | 按下j可以前进，如果在j键前面添加一个数字，则可以向前移动那么多行，如6j，代表视角向下移动六行。 |
| k      | 按下k可以向上移动一行，类似地，如果在k前面添加一个数字，如1k，代表视角向上移动六行。 |
| g      | 如果想要滚动到顶部，直接按下g快捷键。                        |
| G      | 按下大写的G键，如使用快捷键Shift+g，则可以滚动到底部。       |
| f      | 按下f快捷键，可以向下滚动一个屏幕的距离，也可以使用空格键space代替使用。 |
| b      | 按下b快捷键，可以返回上一步的操作，                          |
| d      | 按下d快捷键，向下滑动半个窗口，这个快捷键比较常用、有效。    |
| u      | 按下u快捷键，类似地，向上滑动半个窗口，与d快捷键匹配使用。   |
| q      | 按下q快捷键，可以立即退出手册。                              |

manual手册使用的键位图，快捷键图：

![image-20240119172556676](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240119172556676.png)

> 键盘图片来源链接：https://r-charts.com/miscellaneous/ggkeyboard/

有趣的是，一个小时非常有价值，总的来说，仅仅让我解决一个问题。

https://www.baeldung.com/linux/man-pages

## 问题：man命令对于每一个函数包括什么内容？

- man命令如何使用不影响之前的内容查看？
- 如何在windows的git bash上安装man命令？
- man命令如何移动？
- man命令对于每一个函数包括什么内容？
- 如何快速查看主要的信息？

对于此问题，首先输入`man strcat`查看如何使用命令查看strcat函数的原型。

![image-20240121085317002](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240121085317002.png)

​	出现的第一页是前三部分，然后分别是函数名的来源，函数原型摘要以及函数功能描述三部分。其中第一部分帮助理解函数取名的来源，第二部分用于查看函数的原型，第三部分指出了函数的漏洞来源，暗示strcat函数连接两个字符串时，可能因为dest空间不够大，所以导致缓冲区溢出漏洞和程序行为不可测。

`	strcat`函数用于将源字符串（src）连接到目标字符串（dest）的末尾。与`strncat`不同，`strcat`函数会将整个源字符串连接到目标字符串的末尾，直到遇到源字符串的空字符（'\0'）。需要确保目标字符串有足够的空间来容纳源字符串的字符。

![image-20240121090752420](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240121090752420.png)

根据快捷键d，按下表示下滑半个窗口。所以。

原来不只是难的容易睡觉，简单的也会导致睡觉->开窗关空调，开热气->不冷，但是开始空气清新。

![image-20240121095507548](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240121095507548.png)

实现过程及代码解释如下：

1. 使用`strlen`函数计算目标字符串`dest`的长度，并将结果存储在`dest_len`变量中。
2. 声明一个变量`i`，用作循环计数器。
3. `for`循环从`i = 0`开始迭代，直到`i`达到`n-1`或者`src`字符串的末尾（`src[i] != '\0'`）。
4. 在循环内部，将`src`字符串的每个字符复制到`dest`字符串的位置`dest[dest_len + i]`。这样就将`src`的字符追加到了`dest`的末尾。
5. 循环结束后，将空字符（`'\0'`）添加到连接后的字符串的末尾，即`dest[dest_len + i]`。这样确保连接后的字符串被正确终止。
6. 最后，函数返回指向连接后的字符串的指针，即`dest`。

```c
char* strncat(char *dest, const char *src, size_t n)
{
    size_t dest_len = strlen(dest);  // 计算目标字符串dest的长度
    size_t i;

    for (i = 0; i < n && src[i] != '\0'; i++)
    {
        dest[dest_len + i] = src[i];  // 将src字符串的字符复制到dest字符串的末尾
    }

    dest[dest_len + i] = '\0';  // 在连接后的字符串末尾添加空字符

    return dest;  // 返回连接后的字符串的指针
}
```

如何让这段程序运行起来？

要让这个程序运行起来，你需要在程序中调用该函数并提供合适的参数。以下是一个示例程序，展示如何调用 `strncat` 函数并输出结果：

```c
#include <stdio.h>

// 函数声明
char* strncat(char *dest, const char *src, size_t n);

int main(void) {
    char dest[20] = "Hello";
    const char *src = " World";
    size_t n = 6;

    // 调用 strncat 函数
    strncat(dest, src, n);

    // 输出连接后的字符串
    printf("Concatenated string: %s\n", dest);

    return 0;
}
```

在上面的示例中，在 `main` 函数中声明了一个目标字符串 `dest`，一个源字符串 `src`，以及一个表示要连接的字符数的变量 `n`。然后调用 `strncat` 函数将 `src` 字符串的前 `n` 个字符连接到 `dest` 字符串的末尾。最后，使用 `printf` 函数输出连接后的字符串。

你可以将上面的示例代码保存为一个 C 语言源文件（例如 `program.c`），然后使用 C 编译器编译并运行它。运行程序后，你将在输出中看到连接后的字符串。请确保在编译和运行程序之前，你已经正确配置了 C 编译环境。

![image-20240121101512897](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240121101512897.png)

下面man信息分别分别表示：

- "attributes"：属性。在编程中，属性是用于描述对象或实体的特征、状态或行为的值或元数据。属性可以用于提供额外的信息或约束，并在程序中进行处理或访问
- "conforming to"：符合于，遵循。在软件开发领域，常用于描述某个实现、规范或标准是否符合特定要求或规则。
- "note"：注释、注意事项。在文档或代码中，用于提供额外的解释、说明或警告。
- "example"：示例、例子。用于展示某个概念、方法或代码的具体应用和用法。

![image-20240121171126770](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240121171126770.png)

下面是关于属性的解释，并列举了一些接口及其对应的属性和值。

- "Interface"（接口）列出了两个函数：`strcat()` 和 `strncat()`。
- "Attribute"（属性）列出了这些接口的特性。在这里，我们看到的属性是 "Thread safety"（线程安全性）。
- "Value"（值）给出了相应属性的取值。在这个例子中，属性 "Thread safety" 的取值是 "MT-Safe"（多线程安全）。

下面是conforming和notes的解释，如下：

- "CONFORMING TO"（符合于）：这些函数符合的标准和规范列表。在这里，列出了一系列标准和规范，包括 POSIX.1-2001、POSIX.1-2008、C89、C99、SVr4 和 4.3BSD。这意味着这些函数符合这些标准和规范的要求，并且在这些标准和规范所定义的环境中可以使用。

- "NOTES"（注意事项）：这段文字提供了一些相关说明。在这里，提到了一种名为 `strlcat()` 的函数。在某些系统（如 BSD、Solaris 等）中提供了这个函数。`strlcat()` 函数用于将以空字符结尾的源字符串 `src` 追加到目标字符串 `dest` 的末尾。它最多从 `src` 中复制 `size-strlen(dest)-1` 个字符到 `dest` 中，并在结果末尾添加一个空字符，除非 `size` 小于 `dest` 的长度。这个函数修复了 `strcat()` 的缓冲区溢出问题，但是调用者仍然需要处理 `size` 太小导致的数据损失可能性。函数返回 `strlcat()` 尝试创建的字符串的长度；如果返回值大于等于 `size`，则表示发生了数据损失。如果数据损失很重要，调用者必须在调用函数之前检查参数，或者测试函数的返回值。`strlcat()` 在 glibc 中不可用，并且在 POSIX 中没有进行标准化，但在 Linux 上可以通过 libbsd 库使用。

这段文字提供了一些关于 `strlcat()` 函数的额外信息。它是一种可用于某些系统上的字符串连接函数，解决了 `strcat()` 的缓冲区溢出问题。尽管它没有在标准库中定义，但在某些系统上可以使用，并且在 Linux 上可以通过 libbsd 库使用。

执行提供的Program source：

![image-20240121171657434](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240121171657434.png)

![image-20240121171740584](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240121171740584.png)

程序的执行流程如下：

1. 引入所需的头文件：`string.h`、`time.h` 和 `stdio.h`。
2. 定义一个常量 `LIM`，表示要生成的字符串的最大长度。
3. 声明变量 `j`，用于循环计数。
4. 声明一个字符数组 `p`，大小为 `LIM + 1`，用于存储生成的字符串，额外的 1 字节用于存储字符串的终止空字符。
5. 声明一个 `time_t` 类型的变量 `base`，用于记录起始时间。
6. 使用 `time(NULL)` 函数获取当前时间，并将其赋值给 `base`。
7. 将字符数组 `p` 的第一个元素设为终止空字符，即初始化为空字符串。
8. 进入一个循环，循环条件是 `j < LIM`，即循环执行 LIM 次。
9. 在循环中，如果 `j` 的值是 10000 的倍数，则使用 `printf` 函数输出当前循环迭代次数 `j` 和从起始时间到当前时间的差值（单位为秒）。
10. 使用 `strcat` 函数将字符串 "a" 追加到字符数组 `p` 的末尾。
11. 循环结束后，程序执行完毕。

这个程序的目的是生成一个包含大量字符的字符串，并在每次追加一个字符后输出当前的迭代次数和经过的时间。通过这种方式，可以观察字符串生成的速度。

执行结果如下：

![image-20240121173414952](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240121173414952.png)

改进输出后结果如下：

![image-20240121174538656](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240121174538656.png)

进一步改进输出：

![image-20240121174733795](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240121174733795.png)
