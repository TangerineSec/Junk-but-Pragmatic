德勒的听起来做事情，又有动力又有

Webgoat+docker？

```
set http_server=http://127.0.0.1:38888
set https_server=https://127.0.0.1:38888
```

启动成功后如图所示：

![image-20240123000013431](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240123000013431.png)

运行下面的命令之后，以后就只需要运行一个命令`docker start webgoat`就可以了。

```shell
docker run --name webgoat -it -p 127.0.0.1:8080:8080 -p 127.0.0.1:9090:9090 webgoat/webgoat
```

这样就表示

![image-20240123001731554](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240123001731554.png)