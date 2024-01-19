## 缘起：到底讲了什么？漏洞出现在哪里？

app测试

测试网址：login-m.yinxingyanjing.com

做了什么动作？

- 删除cookie值
- type:'phone'，类型字段修改为admin
- error_code:10002，错误字段修改为10001

edu漏洞测试，并且有人提交：

- 问题：如何挖edu？
- 答案：找特征，然后fofa一次打，迅速提交，必定成功。
- 站点：ywtb.shafc.edu.cn/cas/login
- 若依弱口令：
- 401页面
- oracl诸如语句：and ascii(i),exp(290)，注入不存在
- 万能用户输入用户：admin'--
- F12接口查看，表单数据查看。
- EDU主要是未授权，水平越权，信息泄露
- js插件查看。
- Spring叶子，Soring报错，测未授权，输入druid后缀。

>  针对小程序，挖edu，每天都有洞，简称有手就行。

![image-20240119201028665](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240119201028665.png)