## 压缩命令

合并两个7z文件：

文件合并前：

![image-20240118155118187](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240118155118187.png)

文件合并后：

![image-20240118155335325](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240118155335325.png)

文件使用命令：

copy /b 要合并的所有文件 合并后的文件名

![image-20240118155809835](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240118155809835.png)

## kali系统修改root的密码

修改前：kali系统默认内置root用户和kali用户，前者为超级用户权限，后者为普通用户权限，前者密码为root，后者密码为kali。



修改后：修改超级用户的密码为unix，新密码：unix。修改后可以使用su root切换到root用户，并输入密码unix进入超级用户权限；使用su kali切换到普通用户权限，密码为kali。

修改方法：输入命令`sudo passwd root`，然后输入新密码为unix。

![image-20240118171117962](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240118171117962.png)

同样的，修改普通用户kali的密码为unix，使用命令`sudo passwd kali`，输入密码unix成功修改。

## kali系统的用户命修改

修改前：

![image-20240118172613803](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240118172613803.png)

修改后：

![image-20240118172634015](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240118172634015.png)

修改方法：修改用户名需要编辑三个文件，分别是/etc/passwd /etc/shadow /etc/group，这三个文件负责用户管理，使用`su root`命令首先更改为超级用户，然后可以使用leafpad、vim、nano编辑器去修改三个文件，找到包含kali字样的的文字，全部修改为unix，即可修改为unix用户。

我在倒数第二行找到，下面kali更改为unix的样子。

![image-20240118173041461](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240118173041461.png)
