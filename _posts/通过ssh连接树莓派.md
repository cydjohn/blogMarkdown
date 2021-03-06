title: 通过ssh连接树莓派
date: 2015-10-03 14:13:36
tags: 树莓派
---

> 我用的是树莓派B型，Mac OS系统，Raspbian 系统

首先确保树莓派和你的电脑在同一局域网下，并且树莓派已经连上局域网。不知道树莓派如何连接局域网的可以查看[树莓派设置无线上网](http://caoyudong.com/2015/10/03/%E6%A0%91%E8%8E%93%E6%B4%BE%E8%AE%BE%E7%BD%AE%E6%97%A0%E7%BA%BF%E4%B8%8A%E7%BD%91/)

然后，打开终端，通过命令

```sh
ssh pi@你的树莓派的ip地址
```

就可以连上了，Raspbian系统自带了ssh，有些系统没有要先安装一个。

<!--more-->

## 如何获取到树莓派IP地址

正常情况下，我们是不可能知道树莓派的ip地址的。所以只能靠猜，当然也不是漫无目标的猜。

在终端输入`arp －a`可以得到一个列表，表示当前局域网下的用户的IP地址，当然，不包括树莓派的。树莓派的地址应该是最后一个地址 ＋ 1。比如当前最后一个是 192.168.1.106，那么树莓派的就是192.168.1.107

![arp -a](http://7xkfbb.com1.z0.glb.clouddn.com/15-10-3/68809336.jpg)

尝试

```sh
ssh pi@192.168.1.107
```
*也许*就可以连上

如果不行就试试108，109，反正总会连上的……这方法实在土，而且菜，可以尝试更好的方法：

安装一个[nmap](https://nmap.org/)，然后通过命令

```sh
nmap -sP 192.168.1.0/24
```

就可以列出局域网下所有的用户了，因为nmap会吧局域网下所有IP都扫一遍（好像是）。

![nmap](http://7xkfbb.com1.z0.glb.clouddn.com/15-10-3/94847515.jpg)


连接成功后就可以操作树莓派了（默认密码 raspberry）
![](http://7xkfbb.com1.z0.glb.clouddn.com/15-10-3/35973891.jpg)

-----

## 或者

直接

```
ssh pi@raspberrypi.home
```

raspberrypi.home 就是树莓派的地址，今早发现的～