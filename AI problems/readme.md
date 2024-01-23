## 问题：如何让a.find0day.cn为一个单独的网站，b.find0day.cn为一个单独的网站，如何在nginx中配置

要在 Nginx 中配置两个单独的网站，分别对应 a.find0day.cn 和 b.find0day.cn，你需要在 Nginx 配置文件中创建两个 server 块。以下是一个示例配置：

```
http {
    # ... 其他配置 ...

    server {
        listen 80;
        server_name a.find0day.cn;

        root /var/www/a.find0day.cn; # 请将此路径更改为 a.find0day.cn 网站的实际路径
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }

    server {
        listen 80;
        server_name b.find0day.cn;

        root /var/www/b.find0day.cn; # 请将此路径更改为 b.find0day.cn 网站的实际路径
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }
}
```



在这个示例中，我们为 a.find0day.cn 和 b.find0day.cn 创建了两个 server 块。每个 server 块都有自己的 root 目录，用于存储网站文件。你需要将这些路径更改为实际的网站文件路径。

保存配置文件后，重新加载 Nginx 以应用更改：

```
sudo nginx -s reload
```



现在，a.find0day.cn 和 b.find0day.cn 应该分别指向它们各自的网站文件夹。

## 如何让腾讯云主机设置允许登录root

| 步骤                     | 方法                                                         |
| :----------------------- | :----------------------------------------------------------- |
| 修改 root 密码           | 执行命令sudo passwd root                                     |
| 输入密码                 | 可以和 ubuntu 密码一致，也可以修改 (密码会让你输入两次)      |
| 修改 ssh 配置            | 执行命令sudo vi /etc/ssh/sshd_config                         |
| 修    改 PermitRootLogin | 进入 ssh 配置界面后找到PermitRootLogin，将它后面改为yes，保存 (按i进入编辑模式，编辑完esc退出，:w保存当前文件，:q退出) |
| 重启 ssh 服务            | 执行命令sudo service ssh restart                             |

## 如何查看ghost进程？

切换为第一个用户，user1是专门为ghost分配的。然后输入ghost ls。

```shell
root@VM-16-12-ubuntu:/home/lighthouse# su user1
user1@VM-16-12-ubuntu:/home/lighthouse$ ghost ls

Love open source? We’re hiring JavaScript Engineers to work on Ghost full-time.
https://careers.ghost.org


+ sudo systemctl is-active ghost_research-find0day-cn
? Sudo Password [hidden]
┌──────────────────────┬───────────────────────────────┬─────────┬──────────────────────┬──────────────────────────────┬──────┬─────────────────┐
│ Name                 │ Location                      │ Version │ Status               │ URL                          │ Port │ Process Manager │
├──────────────────────┼───────────────────────────────┼─────────┼──────────────────────┼──────────────────────────────┼──────┼─────────────────┤
│ research-find0day-cn │ /var/www/research.find0day.cn │ 5.64.0  │ running (production) │ https://research.find0day.cn │ 2368 │ systemd         │
└──────────────────────┴───────────────────────────────┴─────────┴─────────��────────────┴──────────────────────────────┴──────┴─────────────────
```

## 发现网站位置在：/var/www/research.find0day.cn，所以进入该目录，查看网站，并且进行网站增加。

复制这段命令并且执行。

```
root@VM-16-12-ubuntu:/home# cd /var/www/ ; mkdir /var/www/website-0 ; echo "<html><body><h1>Welcome To My Website!</h1></body></html>" >> /var/www/website-0/index.html ; cd /var/www/website-0/
```

出现报错：

![image-20240123150806028](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240123150806028.png)

发现了实际原因是因为website中使用了感叹号，应该使用转移，修改命令如下：

```
root@VM-16-12-ubuntu:/home# cd /var/www/ ; mkdir /var/www/website-0 ; echo "<html><body><h1>Welcome To My Website\!</h1></body></html>" >> /var/www/website-0/index.html ; cd /var/www/website-0/；
```

得到正确的结果：

```shell
root@VM-16-12-ubuntu:/var/www# cd /var/www/ ; mkdir /var/www/website-0 ; echo "<html><body><h1>Welcome To My Website\!</h1></body></html>" >> /var/www/website-0/index.html ; cd /var/www/website-0/
root@VM-16-12-ubuntu:/var/www/website-0# ls
index.html
root@VM-16-12-ubuntu:/var/www/website-0# cat index.html 
<html><body><h1>Welcome To My Website\!</h1></body></html>
root@VM-16-12-ubuntu:/var/www/website-0# 
```

下面开始修改nginx的配置文件：

root@VM-16-12-ubuntu:/var/www# vi /etc/nginx/nginx.conf 

原始配置如下：

```
root@VM-16-12-ubuntu:/var/www# cat /etc/nginx/nginx.conf 
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
        worker_connections 768;
        # multi_accept on;
}

http {

        ##
        # Basic Settings
        ##

        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_timeout 65;
        types_hash_max_size 2048;
        # server_tokens off;

        # server_names_hash_bucket_size 64;
        # server_name_in_redirect off;

        include /etc/nginx/mime.types;
        default_type application/octet-stream;

        ##
        # SSL Settings
        ##

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # Dropping SSLv3, ref: POODLE
        ssl_prefer_server_ciphers on;

        ##
        # Logging Settings
        ##

        access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;

        ##
        # Gzip Settings
        ##

        gzip on;

        # gzip_vary on;
        # gzip_proxied any;
        # gzip_comp_level 6;
        # gzip_buffers 16 8k;
        # gzip_http_version 1.1;
        # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

        ##
        # Virtual Host Configs
        ##

        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;
}


#mail {
#       # See sample authentication script at:
#       # http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
# 
#       # auth_http localhost/auth.php;
#       # pop3_capabilities "TOP" "USER";
#       # imap_capabilities "IMAP4rev1" "UIDPLUS";
# 
#       server {
#               listen     localhost:110;
#               protocol   pop3;
#               proxy      on;
#       }
# 
#       server {
#               listen     localhost:143;
#               protocol   imap;
#               proxy      on;
#       }
#}
root@VM-16-12-ubuntu:/var/www# 
```

修改

```
root@VM-16-12-ubuntu:/var/www# cat /etc/nginx/nginx.conf 
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
        worker_connections 768;
        # multi_accept on;
}

http {

        ##
        # Basic Settings
        ##

        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_timeout 65;
        types_hash_max_size 2048;
        # server_tokens off;

        # server_names_hash_bucket_size 64;
        # server_name_in_redirect off;

        include /etc/nginx/mime.types;
        default_type application/octet-stream;

        ##
        # SSL Settings
        ##

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # Dropping SSLv3, ref: POODLE
        ssl_prefer_server_ciphers on;

        ##
        # Logging Settings
        ##

        access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;

        ##
        # Gzip Settings
        ##

        gzip on;

        # gzip_vary on;
        # gzip_proxied any;
        # gzip_comp_level 6;
        # gzip_buffers 16 8k;
        # gzip_http_version 1.1;
        # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

        ##
        # Virtual Host Configs
        ##
		
        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;
        
        #配置开始
         server {
            listen 80;
            server_name website-0.find0day.cn;

            root /var/www/website-0; # 请将此路径更改为 a.find0day.cn 网站的实际路径
            index index.html;

            location / {
                try_files $uri $uri/ =404;
            }
        }

        server {
            listen 80;
            server_name analysis.find0day.cn;

            root /var/www/analysis.find0day.cn; # 请将此路径更改为 b.find0day.cn 网站的实际路径
            index index.html;

            location / {
                try_files $uri $uri/ =404;
            }
        }
    #配置结束
}


#mail {
#       # See sample authentication script at:
#       # http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
# 
#       # auth_http localhost/auth.php;
#       # pop3_capabilities "TOP" "USER";
#       # imap_capabilities "IMAP4rev1" "UIDPLUS";
# 
#       server {
#               listen     localhost:110;
#               protocol   pop3;
#               proxy      on;
#       }
# 
#       server {
#               listen     localhost:143;
#               protocol   imap;
#               proxy      on;
#       }
#}
root@VM-16-12-ubuntu:/var/www# 
```

## 成功：从include可以看出，应该使用enable.conf下的default文件，进行配置。



## 腾讯云下文件名颜色为黄色代表什么意思

在腾讯云服务器（或其他类似的服务器）中，文件名颜色为黄色通常表示该文件是一个符号链接（也称为软链接）。符号链接是一种特殊类型的文件，它实际上是一个指向另一个文件或目录的引用。

在 Linux 和 Unix 系统中，你可以使用 ls -l 命令查看文件的详细信息，包括文件类型和链接目标。例如：

```
lrwxrwxrwx 1 user user 7 Jan 1 00:00 symlink.txt -> target.txt
```



在这个例子中，symlink.txt 是一个符号链接，它指向名为 target.txt 的文件。注意输出中的 l 字符，表示这是一个符号链接。

在腾讯云服务器中，你可以使用相同的方法查看文件详细信息并了解文件颜色的含义。

![image-20240123201447206](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240123201447206.png)

## 使用echo进行输出，使用命令gl输出当天的commit数量。

![image-20240123202519266](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240123202519266.png)

![image-20240123202632853](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240123202632853.png)