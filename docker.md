1 、root权限跟新Yum包
yum update

2 、卸载旧版本：（如果安装过旧版本的话）
yum -y remove docker docker-common docker-selinux docker-engine

3 、设置Yum源
yum install -y yum-utils device-mapper-persistent-data lvm2

4 、添加docker的Yum源
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
（此处可能会报错 （原因是国内访问不到docker官方镜像的缘故）
解决办法：
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

5 、查看所有仓库中docker版本，并选择特定版本安装：(此处我们查看社区版 docker-ce)
yum list docker-ce --showduplicates | sort -r

6 、配置阿里镜像源
mkdir -p /etc/docker
tee /etc/docker/daemon.json <<-'EOF'
 > { 
 >    "registry-mirrors": ["https://6l28qear.mirror.aliyuncs.com"]
 > }
 EOF
systemctl daemon-reload    （导入操作）

7 、安装docker
yum install docker-ce-18.03.1.ce  


8.启动并加入开机启动
systemctl start docker       (重启命令  $  systemctl restart docker ) 
systemctl enable docker   开机启动
docker version  查看docker版本号

如果出现：Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
解决办法
systemctl daemon-reload
sudo service docker restart