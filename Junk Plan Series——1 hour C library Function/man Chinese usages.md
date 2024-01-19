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
