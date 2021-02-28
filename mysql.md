1 、docker下载
docker pull mysql:5.7

2 、查看镜像
docker images

3 、创建本地文件
创建三个目录  mkdir  -p data logs conf

3 、启动
启动命令：
docker run -p 3306:3306 --name pfmysql -v  /root/mysql/conf:/etc/mysql/conf.d -v /root/mysql/logs:/logs -v /root/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=pfzzzz -d mysql:5.7

参数详解：
-p 3306:3306：将容器的 3306 端口映射到主机的 3306 端口。
--name 容器名字 可以随便自定义哦！
-v  /root/mysql/conf:/etc/mysql/conf.d：将主机当前目录下的 conf/my.cnf 挂载到容器的 /etc/mysql/my.cnf。
-v  /root/mysql/logs:/logs：将主机当前目录下的 logs 目录挂载到容器的 /logs。
-v  /root/mysql/data:/var/lib/mysql ：将主机当前目录下的data目录挂载到容器的 /var/lib/mysql 。
-e MYSQL_ROOT_PASSWORD=123456：初始化 root 用户的密码。
-d 后台启动
mysql：5.6 容器的名称


set password for 'root'@'localhost'=password('pfzzz');