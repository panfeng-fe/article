# Golang安装

## 1、下载

根据适合自己的版本

https://studygolang.com/dl



# 2、安装

选择安装路径（任何地方）

新建文件Go文件

```
mkdir Go
```

新建子文件夹

```
mkdir Go/GOPATH

mkdir Go/GOROOT
```

**（注：GOPATH为项目开发目录，GOROOT为Go安装目录）**

安装步骤下一步即可，在选择路径是请安装到你创建的文件夹下



## 3、设置环境变量

新增环境变量

```
GOPATH   安装目录\Go\GOPATH

GOROOT   安装目录 \Go\GOROOT
```

在path下新增

安装目录\Go\GOPATH\bin   安装目录\Go\GOROOT\bin



## 4、设置代理

$env:GOPROXY = "https://goproxy.cn"



## 5、验证

go version 查看go版本

go env  查看go的开发环境

