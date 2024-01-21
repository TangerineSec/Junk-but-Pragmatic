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
