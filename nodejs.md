一 、源码安装
1 、下载必要编译软件
yum install gcc gcc-c++


2 、下载源码
wget https://npm.taobao.org/mirrors/node/v12.16.1/node-v12.16.1-darwin-x64.tar.gz

3 、解压源码
tar xzvf node-v* && cd node-v*

4 、编译
./configure
make

5 、安装
make install

6 、查看版本
node -v

二 、使用编译后的版本
1 、 下载
wget https://npm.taobao.org/mirrors/node/v12.16.1/node-v12.16.1-darwin-x64.tar.gz

2 、安装
tar -xzvf node-v* -C /usr/local

三 、yum安装
curl -sL https://rpm.nodesource.com/setup_10.x | bash -

yum install -y nodejs

