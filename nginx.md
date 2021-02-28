1 、安装编译工具
yum -y install gcc gcc-c++ autoconf pcre-devel make automake
yum -y install wget httpd-tools vim

2 、查看yum源里的nginx版本
yum list | grep nginx

3 、如果版本过低
vim /etc/yum.repos.d/nginx.repo

[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/7/$basearch/
gpgcheck=0
enabled=1
跟新官方yum源

然后安装
yum install nginx
查看版本
nginx -v

4 、查看nginx安装位置
（rpm 是linux的rpm包管理工具，-q 代表询问模式，-l 代表返回列表，这样我们就可以找到nginx的所有安装位置了）
rpm -ql nginx

5 、配置解读
5.1 、nginx.conf文件解读

#运行用户，默认即是nginx，可以不进行设置
user  nginx;

#Nginx进程，一般设置为和CPU核数一样
worker_processes  1; 

#错误日志存放目录
error_log  /var/log/nginx/error.log warn;

#进程pid存放位置
pid        /var/run/nginx.pid;


events {
    # 单个后台进程的最大并发数
    worker_connections  1024; 
}

#文件扩展名与类型映射表
include       /etc/nginx/mime.types;  

#默认文件类型 
default_type  application/octet-stream; 

#设置日志模式
log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';

#nginx访问日志存放位置
access_log  /var/log/nginx/access.log  main;   

#开启高效传输模式
sendfile        on;   

#减少网络报文段的数量
#tcp_nopush     on;    

#保持连接的时间，也叫超时时间
keepalive_timeout  65;  

#开启gzip压缩
#gzip  on;  

#包含的子配置项位置和文件
include /etc/nginx/conf.d/*.conf; 


5.2 、default.conf 配置项讲解
#配置监听端口
listen       80;   
server_name  localhost;  //配置域名

#charset koi8-r;     
#access_log  /var/log/nginx/host.access.log  main;

location / {
    #服务默认启动目录
    root   /usr/share/nginx/html;  
    #默认访问文件   
    index  index.html index.htm;    
}

#配置404页面
#error_page  404              /404.html;   

error_page   500 502 503 504  /50x.html;   #错误状态码的显示页面，配置后需要重启

location = /50x.html {
    root   /usr/share/nginx/html;
}

location ~ \.php$ {
    proxy_pass   http://127.0.0.1;
}

location ~ \.php$ {
    root           html;
    fastcgi_pass   127.0.0.1:9000;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    include        fastcgi_params;
}

location ~ /\.ht {
    deny  all;
}


6 、nginx启动停止等相关命令
启动（centos7.4） 
1 、nginx
2 、systemctl start nginx.service
停止nginx
1 、nginx  -s stop  （立即停止服务）
2 、nginx -s quit   （进程完成当前工作后再停止）
3 、killall nginx   （直接杀死进程）
4 、systemctl stop nginx.service
重启nginx
1 、systemctl restart nginx.service
重新载入nginx配置文件
1 、nginx -s reload

7 、nginx默认占用80端口
查看端口占用
netstat -tlnp

8 、定义错误页和访问设置
在nginx/conf.d/default.conf下
error_page   500 502 503 504  /50x.html;
也可单独配置
error_page 404  /404_error.html;
也可引用外部资源
error_page  404 http://pfzzz.work;

9 、访问控制
在nginx/conf.d/default.conf下

简单实现访问控制
location / {
    deny   123.9.51.42;
    allow  45.76.202.231;
}
配置后重启即可
systemctl restart nginx.service

复杂访问控制权限匹配
location =/img{
    allow all;
}
location =/admin{
    deny all;
}
拒绝访问以php结尾的文件
location ~\.php$ {
    deny all;
}

10 、nginx设置虚拟主机
基于端口号来配置虚拟主机，算是Nginx中最简单的一种方式了。原理就是Nginx监听多个端口，根据不同的端口号，来区分不同的网站。
外部访问域名
xxx.xxxx.xxx不加端口默认访问80端口
新加的配置访问xxx.xxxx.xxx:8081即可访问到新配置的虚拟主机
在nginx/nginx.conf也可在nginx/conf.d/default.conf下新建一个conf
vim 8081.conf
server{
    listen 8001;
    server_name localhost;
    root /usr/share/nginx/html/html8001;
    index index.html;
}

11 、nginx反向代理
在nginx/con.d/8001.conf下设置
server{
        listen 80;
        server_name nginx2.jspang.com;
        location / {
               proxy_pass http://jspang.com;
        }
}
其它指令
proxy_set_header :在将客户端请求发送给后端服务器之前，更改来自客户端的请求头信息。
proxy_connect_timeout:配置Nginx与后端代理服务器尝试建立连接的超时时间。
proxy_read_timeout : 配置Nginx向后端服务器组发出read请求后，等待相应的超时时间。
proxy_send_timeout：配置Nginx向后端服务器组发出write请求后，等待相应的超时时间。
proxy_redirect :用于修改后端服务器返回的响应头中的Location和Refresh。

12 、nginx适配pc端与移动端
Nginx通过内置变量$http_user_agent，可以获取到请求客户端的userAgent，就可以用户目前处于移动端还是PC端，进而展示不同的页面给用户。
server{
     listen 80;
     server_name nginx2.jspang.com;
     location / {
      root /usr/share/nginx/pc;
      if ($http_user_agent ~* '(Android|webOS|iPhone|iPod|BlackBerry)') {
         root /usr/share/nginx/mobile;
      }
      index index.html;
     }
}

13 、gzip压缩
gzip : 该指令用于开启或 关闭gzip模块。
gzip_buffers : 设置系统获取几个单位的缓存用于存储gzip的压缩结果数据流。
gzip_comp_level : gzip压缩比，压缩级别是1-9，1的压缩级别最低，9的压缩级别最高。压缩级别越高压缩率越大，压缩时间越长。
gzip_disable : 可以通过该指令对一些特定的User-Agent不使用压缩功能。
gzip_min_length:设置允许压缩的页面最小字节数，页面字节数从相应消息头的Content-length中进行获取。
gzip_http_version：识别HTTP协议版本，其值可以是1.1.或1.0.
gzip_proxied : 用于设置启用或禁用从代理服务器上收到相应内容gzip压缩。
gzip_vary : 用于在响应消息头中添加Vary：Accept-Encoding,使代理服务器根据请求头中的Accept-Encoding识别是否启用gzip压缩。
http {
    gzip on;
    gzip_types text/plain application/javascript text/css;
}
