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

要查看当天提交的 commit 数量，可以使用 `git log` 命令结合一些参数和选项来实现。下面是一种可能的方法：

```shell
git log --since=midnight --until=now --format=oneline | wc -l
```

这个命令的含义如下：

- `--since=midnight`：指定起始时间为今天的午夜（即当天的凌晨）。
- `--until=now`：指定结束时间为当前时间。
- `--format=oneline`：以单行的形式输出 commit 信息，每一行包含一个 commit 的哈希值和提交消息。
- `| wc -l`：通过管道将 `git log` 的输出传递给 `wc -l` 命令，用于统计行数（即 commit 数量）。

运行这个命令后，将会输出当天提交的 commit 数量。请注意，这个结果包括了所有分支上的提交。如果你只想查看当前分支上当天的提交数量，可以在运行命令之前切换到相应的分支。

![image-20240123202825940](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240123202825940.png)

![image-20240123202751459](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240123202751459.png)

## 如何为子域名添加证书

![image-20240123204611972](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240123204611972.png)

## 可测试地址：m3u8

持续可用网站：https://hlsjs.video-dev.org/demo/

1. **Tears of Steel m3u8**
   `https://demo.unified-streaming.com/k8s/features/stable/video/tears-of-steel/tears-of-steel.ism/.m3u8`
2. **fMP4 m3u8**
   `https://devstreaming-cdn.apple.com/videos/streaming/examples/img_bipbop_adv_example_fmp4/master.m3u8`
3. **MP4 m3u8**: `https://demo.unified-streaming.com/k8s/features/stable/video/tears-of-steel/tears-of-steel.mp4/.m3u8`
4. **Live Akamai m3u8**
   `https://cph-p2p-msl.akamaized.net/hls/live/2000341/test/master.m3u8`
5. **Live Akamai m3u8**
   `https://moctobpltc-i.akamaihd.net/hls/live/571329/eight/playlist.m3u8`
6. **Dolby VOD m3u8**
   `http://d3rlna7iyyu8wu.cloudfront.net/skip_armstrong/skip_armstrong_stereo_subs.m3u8` – this is served over HTTP, so it might result in media errors. To avoid this, reload your HLS player using HTTP (less secure, but, you can playback this stream!)
7. **Dolby Multichannel m3u8**
   `http://d3rlna7iyyu8wu.cloudfront.net/skip_armstrong/skip_armstrong_multichannel_subs.m3u8` – this is served over HTTP, so it might result in media errors. To avoid this, reload your HLS player using HTTP (less secure, but, you can playback this stream!)
8. **Dolby Multilanguage m3u8**
   `http://d3rlna7iyyu8wu.cloudfront.net/skip_armstrong/skip_armstrong_multi_language_subs.m3u8` – this is served over HTTP, so it might result in media errors. To avoid this, reload your HLS player using HTTP (less secure, but, you can playback this stream!)
9. **Azure HLSv4 m3u8** (copy paste in your browser and it will download the m3u8)
   `http://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest(format=m3u8-aapl)` .. again, served over HTTP.
10. **Azure HLSv4 m3u8** (copy paste in your browser and it will download the m3u8)
    `http://amssamples.streaming.mediaservices.windows.net/69fbaeba-8e92-4740-aedc-ce09ae945073/AzurePromo.ism/manifest(format=m3u8-aapl)` served over HTTP.
11. **Azure 4K HLSv4 m3u8** (copy paste in your browser and it will download the m3u8)
    `http://amssamples.streaming.mediaservices.windows.net/634cd01c-6822-4630-8444-8dd6279f94c6/CaminandesLlamaDrama4K.ism/manifest(format=m3u8-aapl)` served over HTTP.

![image-20240124012306372](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240124012306372.png)

## 后台运行python，并且显示相关pid信息。

要在后台运行 Python 脚本并获取相关的进程 ID (PID)，你可以使用以下命令：

```shell
python your_script.py > output.log &
echo $!
```

在这个命令中，`$!` 是一个特殊的变量，它会返回最近一个在后台运行的进程的 PID。通过在脚本运行后立即使用 `echo $!` 命令，你可以获取 Python 脚本的进程 ID。

执行上述命令后，将会在终端输出 Python 脚本的进程 ID。你可以将其记录下来，以便以后进行操作，如删除该进程。

请注意，`output.log` 是一个示例文件名，用于将脚本的输出重定向到文件中。你可以根据需要自行指定输出文件名。



要终止后台运行的 Python 进程，你可以使用 `kill` 命令发送适当的信号。以下是一种常用的方法：

1. 首先，使用 `ps` 命令结合 `grep` 过滤器，找到正在运行的 Python 进程的 PID。例如，假设你的 Python 进程执行的是 `your_script.py`，可以使用以下命令获取其 PID：

   ```shell
   ps aux | grep your_script.py
   ```

   这将列出匹配进程名为 `your_script.py` 的所有进程，并显示它们的详细信息，包括 PID。

2. 从输出中找到你要终止的 Python 进程的 PID。

3. 使用 `kill` 命令发送适当的信号来终止进程。常用的信号是 SIGTERM（终止信号），它会请求进程正常退出。使用以下命令将信号发送给进程：

   ```shell
   kill <PID>
   ```

   将 `<PID>` 替换为你要终止的 Python 进程的实际 PID。

如果进程没有正常退出，你可以尝试使用更强制的信号，如 SIGKILL。这个信号会立即终止进程，但也可能导致某些资源无法正确释放。使用以下命令发送 SIGKILL 信号：

```shell
kill -9 <PID>
```

同样，将 `<PID>` 替换为要终止的 Python 进程的实际 PID。

请注意，终止进程可能会导致未保存的数据丢失，因此请确保在终止之前保存了必要的数据。

## 报错分析：无法运行

![image-20240124031319534](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240124031319534.png)

![image-20240124031304440](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240124031304440.png)

从报错信息中可以得出以下分析：

1. 报错信息显示导入错误（ImportError），具体错误消息是 Pandas 要求安装 'openpyxl' 的版本应为 '3.0.10' 或更高版本，但当前安装的版本是 '3.0.7'。这意味着当前安装的 'openpyxl' 版本不符合 Pandas 的要求。

2. 报错信息还包含了一个 HTTP 请求信息，请求路径为 /api/export_html，参数包括 filename1 和 sheet1。

根据报错信息可以尝试解决以下问题：

- 检查当前安装的 'openpyxl' 版本，确保其版本号为 '3.0.10' 或更高版本。如果版本不符合要求，可以尝试更新 'openpyxl' 到符合要求的版本。

- 如果使用的是 Python 环境，可以使用以下命令来升级 'openpyxl'：
  ```
  pip install --upgrade openpyxl
  ```

- 如果报错出现在特定的代码文件中，可以尝试在该文件中添加相关的导入语句或更新依赖版本，以确保 'openpyxl' 版本符合要求。

- 如果报错信息中的 HTTP 请求相关，可以检查代码中涉及到该请求的部分，确保传递给请求的参数（如 filename1 和 sheet1）正确且符合预期。

注意：报错信息中的请求路径和参数仅供参考，具体的处理方式可能需要根据实际情况进行调整。

## 为windows创建别名

https://www.cnblogs.com/mq0036/p/16255494.html

## 门户与首页

门户为open.html

首页为index.html

nginx设置首页文件后，使用链接跳转。例如访问：web.find0day.cn

第三个访问analysis.find0day.cn

```
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        root /var/www/html;

        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
                try_files $uri $uri/ =404;
        }

}

server {
       listen 80;
       listen [::]:80;

       server_name website-0.find0day.cn;

       root /var/www/website-0;
       index index.html;

       location / {
               try_files $uri $uri/ =404;
       }
}
server {
       listen 80;
       listen [::]:80;

       server_name analysis.find0day.cn;

       root /var/www/analysis/dist; # 前端文件夹。
       index index.html;

        location /api/ {
                proxy_pass  http://127.0.0.1:8888/api/;
        }
}

server {
       listen 80;
       listen [::]:80;

       server_name web.find0day.cn;

       root /var/www/web;
       index index.html;

       location / {
               try_files $uri $uri/ =404;
       }
}
```

## 另外起一个网站打开，不关闭本页面，html代码

要在当前页面中打开一个新的网站而不关闭本页面，你可以使用`target="_blank"`属性来指定链接的目标为新的浏览器标签或窗口。以下是一个简单的示例，展示如何在新标签中打开一个网站：

```html
<a href="https://www.example.com" target="_blank">打开新网站</a>
```

将上述代码插入到你的HTML文件中的适当位置，将"https://www.example.com"替换为你想要打开的网站的URL。当用户点击该链接时，浏览器将在一个新的标签页中打开指定的网站。