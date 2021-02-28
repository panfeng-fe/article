# screen初识

# 1、应用场景

在linux命令行窗口下，启动持续监听程序，如web框架的监听端口服务，就会导致命令行使用权得不到释放，用户没有办法继续后面的操作，所以screen的诞生解决了这个问题。



# 2、screen简介与简单命令

screen是一个可以在多个进程之间多路复用一个物理终端的窗口管理器，用户可以在一个screen会话中创建多个screen窗口，在每一个screen窗口中就像操作一个真实的telnet/SSH连接窗口那样。当断开或关闭终端时，只要不杀死screen的进程，待重新连接后任能继续断开前的操作。

## 2.1、创建新的screen会话

```
screen  -S  [screen name]
```



## 2.2、Detach 会话

```
screen –d [screen name]
```



## 2.3、Reattach 会话

```
screen –r [screen-name]
```



## 2.4、查看所有的screen会话

```
screen –ls
```



# 3、screen快捷键

除了一个screen可以包含多个window，也可以在创建多个screen。创建多个screen也很简单，退出screen（Detached退出模式），然后再执行screen命令，就创建了2个screen。用`screen -ls`可以查看有多少个screen被创建。
执行`screen -ls`后，每个列出的screen有个`pid`编号，当我们要恢复某个screen的窗口时，只需输入`screen -r pid` 就可以恢复到指定screen了！

进入screen会话后，可在会话中创建多个窗口（window），并对窗口进行管理，管理命令以`ctrl + a`开头。

```
ctrl + a + c：创建新窗口（create）

ctrl + a + n：切换至下一个窗口（next）

ctrl + a + p：切换至上一个窗口（previous）

ctrl + a + w: 列出所有窗口

ctrl + a + A: 窗口重命名

ctrl + a + d：detach当前会话

ctrl + a + [1-9]: 切换到指定窗口（1-9为窗口号）

ctrl + d：退出（关闭）当前窗口
```

进入screen后，当按下tab键时，会闪屏，可通过ctrl + a & ctrl + g来停止当前screen的闪屏，如果要对所有的screen生效，在~/.screenrc中加入vbell off。

改变screen配置：

```
caption always "%{= kw}%-w%{= BW}%n %t%{-}%+w %-= @%H - %Y-%m-%d %c"

vim /tmp/screenrc

screen -c /tmp/screnrc -S zl
```