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
- Spring叶子，Soring报错，测未授权，输入druid后缀。还是没看到实际有上海的。`python Spring-Scan.py -u "127.0.0.1:8080"`
- js.map泄露，接口未授权。
- 找到学号，我也能找。
- 在找回密码的地方输入学号：13402232，下一步需要输入手机号。输入自己的手机号，13596213721，校验了手机号。肯定有手机号验证，不然搞笑呢。
- 验证码爆破插件：captcha，500次1块钱。
- 并发超过10次才收到，fofa搜索“千校携手”很拉。
- 太累了，麻烦，还找不到什么东西。

>  针对小程序，挖edu，每天都有洞，简称有手就行。

![image-20240119201028665](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240119201028665.png)

EDU未授权和逻辑漏洞：

![image-20240119203800443](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240119203800443.png)

- 总结：搞半天没有什么真实出洞的东西。7点30到21点，果然开始就是个错误，我就是没耐心，屁股都做不舒服了。
- 总结2：浪费时间，浪费硬盘空间。