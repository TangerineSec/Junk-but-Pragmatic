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

## 禁止ping和除了cf以外的其他流量。

https://limbopro.com/archives/1204.html

```
#!/bin/bash
# Name  : Anti IP Leakage
# Author: Zhys
# Date  : 2019

# 禁止来自IPv4的所有HTTP/S访问请求
iptables -I INPUT -p tcp --dport 80 -j DROP
iptables -I INPUT -p tcp --dport 443 -j DROP

# 对Cloudflare CDN IPv4地址开放HTTP/S入站访问
for i in `curl https://www.cloudflare.com/ips-v4`; do iptables -I INPUT -s $i -p tcp --dport 80 -j ACCEPT; done
for i in `curl https://www.cloudflare.com/ips-v4`; do iptables -I INPUT -s $i -p tcp --dport 443 -j ACCEPT; done

# 禁止来自IPv6的所有HTTP/S访问请求
ip6tables -I INPUT -p tcp --dport 80 -j DROP
ip6tables -I INPUT -p tcp --dport 443 -j DROP

# 对Cloudflare CDN IPv6地址开放HTTP/S入站访问
for i in `curl https://www.cloudflare.com/ips-v6`; do ip6tables -I INPUT -s $i -p tcp --dport 80 -j ACCEPT; done
for i in `curl https://www.cloudflare.com/ips-v6`; do ip6tables -I INPUT -s $i -p tcp --dport 443 -j ACCEPT; done

# 保存iptables配置
iptables-save
ip6tables-save

# 注意：80/443为默认HTTP/S协议通讯使用端口，若实际应用使用非80/443端口进行，请依葫芦画瓢自行修改脚本
# Ubuntu系统可以使用UFW则类似：for i in `curl https://www.cloudflare.com/ips-v4`; do ufw allow proto tcp from $i to any port 80; done
# 基于Linux系统兼容性考虑脚本使用iptables配置系统防火墙，请自行根据各自系统、防火墙不同做相应配置调整实施
```



# 通过 iptables 禁止 ping

https://blog.csdn.net/m0_47696151/article/details/119913934

sudo iptables -A INPUT -p icmp --icmp-type echo-request -j DROP

![image-20240124060057246](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240124060057246.png)

![image-20240124060155452](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240124060155452.png)