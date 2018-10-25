---
title: HackintoRouters-Setup
date: 2018-08-19 15:40:31
tags: [openwrt, 路由器, 科学上网]
---
![lede](https://i.loli.net/2018/08/19/5b78fe9823b94.png)

### HackintoRouters - Setup

捣鼓路由器也有两年了，从上大学开始，买了个 Google Home 后，就对路由器逐渐产生各种需求。从一开始的树莓派到现在的 Linksys WRT1200ACv2 ，学习了很多。今天开的这个 HackintoRouters 系列~~又开新坑作大死咕咕咕~~，正逢 Openwrt 发布 18.06 稳定版，就是想趁热把整个折腾的过程捋一遍，以后还可以加各种各样的 addons ，就酱。

今天这篇要说的是初篇，基本的基本，最低需求，弄完的机子才刚刚能用。本篇采用大量菊苣的教程~~有干嘛不用~~，也是顺带把他们整合在一起了。先说下准备工作：

* 一个路由器
* external storage (flash / HD)
* 网线
* 小飞机

#### 玩具的选择

去买啊，淘宝咸鱼，一般都选 NETGEAR / Linksys / Asus 等 Highly-Customizable 大厂的，我对其他牌子没有什么信心，不要问我小米华为tplink，他们做的是路由器不是玩具，但鉴于本人智商较低只会玩 1200AC 这种玩具 。

#### 刷机

[Openwrt](https://openwrt.org/downloads)，请。刷完用网线打通 ssh ，取消密码登陆，取消 root 密码登陆，添加你的密钥，开启热点，完毕。

#### 扩容

一般个人路由器不会涉及到安装各类软件，存储肯定是不够用的，请参见[这篇文章](http://simplism.life/2014/10/13/install-openwrt-on-703n-and-extend-space-with-usb-flash-drive/)完成配置，拿个U盘扩一两个G大概就可以了。**此步骤可跳过，但不要拖到后面去做，/tmp会被挤爆**。

#### sftp

包名 openssh-sftp-server 。

#### zsh

真好看，舒服，参见这个[教程](https://gist.github.com/fire1ce/65d3e370120750a5deb283abe1d74491)，第一步顺便要把 ca-bundle 安装上后面才不会报错。

#### UI 风格

Material theme 和 中文语言包都可以在 luci-i18n 里找到。

#### OpenWrt-dist

参照[教程](http://openwrt-dist.sourceforge.net/)里的步骤做，ss 的包被墙了，下载到电脑，sftp 传过去 ipk 手动安装。

#### 蕃蔷

先来 `opkg install ip ip-full iptables-mod-tproxy`

这个[教程](https://joybean.org/2018/02/14/configure-router-for-transparent-proxy-by-using-openwrt-and-shadowsocks/)，做完后把 dnsmasq | dns-forwarder | chinadns | shadowsocks **依次**重启一遍，如果要做 [crontab](https://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/crontab.html) 任务，也按这个顺序写 list 。

#### package auto update

> 正式版固件推荐，snapshot 版本不建议使用，第三方定制 rom 禁用！

这个[教程](https://joybean.org/2018/02/14/configure-router-for-transparent-proxy-by-using-openwrt-and-shadowsocks/)。

#### END

好的到这里这篇 setup 就水完了，非常水有木有~~有！~~ ， 嘛也算是做个合集，不用到处翻找资料了。

一定要**顺序操作**。

BTW 喜迎 LEDE 合并。

Hackroid敬上~
