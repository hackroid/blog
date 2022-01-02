---
title: 饥荒带mod独服搭建
date: 2022-01-02 17:22:47
tags: [game, dst]
---

> 和女票在圣诞节的时候，趁 steam 促销，一起买了饥荒来玩。但是呢，由于网络连接并不顺畅，pc 开 p2p 服 host 不卡，但是 clients 网卡加电脑也卡，遂去阿里云整了个 2G 轻量级，一年也就几十块。
>
> 开始上网搜罗饥荒服搭建指南，惊讶的是无论中文英文指南，都讲的一塌糊涂，一个讲的全的都没有，也完全没有后续更新和 troubleshooting，东拼西凑了几个教程可算把大概的思路整理好了。
>
> 本文面向有一定 Linux 基础的玩家，所以会省略一些基操。

### 0. 准备工作

云服务器搭建过程略，别忘了把 `TCP:8768/8769,10889,11000/11001,27018/27019` 端口开了

### 1. 基础搭建

> 这部分主要参见 [Dedicated Server Quick Setup Guide - Linux](https://forums.kleientertainment.com/forums/topic/64441-dedicated-server-quick-setup-guide-linux)

#### 1.1 安装依赖

For a 64-bit machine:

`sudo apt-get install libstdc++6:i386 libgcc1:i386 libcurl4-gnutls-dev:i386`

For a 32-bit machine:

`sudo apt-get install libstdc++6 libgcc1 libcurl4-gnutls-dev lib32gcc1`

#### 1.2 安装 steamcmd

Download and install steamcmd by following the instructions here: https://developer.valvesoftware.com/wiki/SteamCMD#Linux . This guide will assume that you have installed steamcmd to `~/steamcmd/` . You can skip the part about creating a new user if you wish.

A shortened version of the necessary commands:

```
mkdir -p ~/steamcmd/
cd ~/steamcmd/
wget "https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz"
tar -xvzf steamcmd_linux.tar.gz
```

#### 1.3 从 klei 下载并配置服务器设置

太长，参见原 [po](https://forums.kleientertainment.com/forums/topic/64441-dedicated-server-quick-setup-guide-linux) 的 3. Configure and download the server settings 部分。

配置服务器名称人数密码等在这里。

#### 1.4 服务器启动脚本

下载

Download this [shell script](https://accounts.klei.com/assets/gamesetup/linux/run_dedicated_servers.sh) and move it to `~/run_dedicated_servers.sh`

给予运行权限

run: `chmod u+x ~/run_dedicated_servers.sh`

启动

run: `~/run_dedicated_servers.sh`

#### 1.5 搭建完成

以上，一个最基本的服务器就搭建完成了。用 screen 把进程挂在后台即可。

### 2. mod 设置

mod 分本地 mod 和服务器 mod，都可以在 steam 的 workshop 中 subscribe，知乎上可找到各种 mod 推荐，随便给个[链接](《饥荒》新手怎么入门？ - 不和憨憨争短长的回答 - 知乎 https://www.zhihu.com/question/53324225/answer/1274525414)。

全部 subscribe 后需要重启游戏以生效。

本地 mod 生效后在游戏设置中即可启用，下面主要讲服务器 mod 的繁琐设定。

**以下内容均为 macOS 系统上的操作。**

#### 2.1 本地准备工作

mod 的配置极为繁琐，包括 mod setup 与每个 mod 中的详细参数设定，如果直接去上手搞 mod 配置的 rawcode，有可能会不知道什么地方是干什么的，哪些代码能否删改，配置成怎样会产生怎样的效果。**你都不知道。**所以推荐本地先搭服务器，以测试服务器 mod 的效果，获取完整 mod 设置，以便上云，本方法主要参考 [link](https://zhuanlan.zhihu.com/p/111932228)。

##### 2.1.1 本地开服

开启游戏=>创建游戏=>创建新世界。

游戏风格：和你预计搭建的云服务器一致。

森林：默认，或者你懂的你就改，和你预计搭建的云服务器一致。

洞穴：默认，或者你懂的你就改，和你预计搭建的云服务器一致。

模组：因为你已经 subscribe 了，所以只要核对一下 mod 是不是全的，然后本地 mod 和服务器 mod 全部勾选启用。每个 mod 界面右下角的螺丝钉可以配置 mod 的详细参数，先默认，或者你懂的你就改。

进入你创建的世界，根据实际效果和个人喜好，返回修改 mod 参数。

**⚠️注意：本教程没有涉及如何配置世界参数，本人是新手，为了游戏体验完全默认。所以请自行了解如何配置世界参数。**

##### 2.1.2 本地 mod 设置获取

macOS 系统中饥荒本地数据存档的存放点是 `~/Documents/Klei/DoNotStarveTogether` ，下面给出文件树，用 `...` 省略了非重点文件和目录。

```
DoNotStarveTogether
├── 317961492
│   ├── Cluster_2
│   │   ├── Caves
│   │   │   ├── backup
│   │   │   │   └── ...
│   │   │   ├── leveldataoverride.lua
│   │   │   ├── modoverrides.lua
│   │   │   ├── save
│   │   │   │   └── ...
│   │   │   ├── server.ini
│   │   │   ├── server_chat_log.txt
│   │   │   └── server_log.txt
│   │   ├── Master
│   │   │   ├── backup
│   │   │   │   └── ...
│   │   │   ├── leveldataoverride.lua
│   │   │   ├── modoverrides.lua
│   │   │   ├── save
│   │   │   │   └── ...
│   │   │   ├── server.ini
│   │   │   ├── server_chat_log.txt
│   │   │   └── server_log.txt
│   │   └── cluster.ini
│   ├── client.ini
│   └── client_save
│       └── ...
├── backup
│   └── ...
├── client_chat_log.txt
└── client_log.txt

注1: 317961492 这串数字可能每人会不一样
注2: Cluster_2 这里的数字2可能会不一样，你需要找到你先前新建世界的 Cluster_X
```

我们只需要 `Master` 目录下的 `modoverrides.lua` ，**后面会用**。下面给出基础配置结构，用 `...` 省略了大部分 mod 详细参数。

```lua
return {
  ["workshop-2032577827"]={
    configuration_options={
      ...
    },
    enabled=true 
  },
  ["workshop-375850593"]={
    configuration_options={
      ...
    },
    enabled=true
  },
  ["workshop-378160973"]={
    configuration_options={
      ...
    },
    enabled=true 
  },
  ["workshop-661253977"]={
    configuration_options={
      ...
    },
    enabled=true 
  } 
}
```

我这里 subscribe 并启用了四个服务器 mod ，核对 `workshop-xxxxxxxxxx` 数量是四个，并且记下这四串数字，**后面要用**。

#### 2.2 mod 设置上云

> 本小节内容主要参考 steam 社区的一个 [guide](https://steamcommunity.com/sharedfiles/filedetails/?id=1970898848)。

配置云服务器上的 mod 重要文件有两个：`dedicated_server_mods_setup.lua` 和 `modoverrides.lua` ，前者是从 steam workshop 下载并配置这个服务器都拥有哪些 mod，后者是配置在一个世界中启用哪些 mod 及其详细参数。

只要你之前都依照本教程配置服务器，这两个文件的所在目录应为：

`/root/dontstarvetogether_dedicated_server/mods/dedicated_server_mods_setup.lua`

`/root/.klei/DoNotStarveTogether/MyDediServer/Master/modoverrides.lua`

`/root/.klei/DoNotStarveTogether/MyDediServer/Caves/modoverrides.lua`

但由于之前 `run_dedicated_servers.sh` 脚本的某些原因，这两个文件都有可能会在服务启动后被覆盖掉，从而导致服务器 mod 配置失败，下面给出解决办法。

##### 2.2.1 mods_setup 编辑配置

`dedicated_server_mods_setup.lua` 的内容应为如下：

```lua
--There are two functions that will install mods, ServerModSetup and ServerModCollectionSetup. Put the calls to the functions in this file and they will be executed on boot.

--ServerModSetup takes a string of a specific mod's Workshop id. It will download and install the mod to your mod directory on boot.
	--The Workshop id can be found at the end of the url to the mod's Workshop page.
	--Example: http://steamcommunity.com/sharedfiles/filedetails/?id=350811795
	--ServerModSetup("350811795")
ServerModSetup("2032577827")
ServerModSetup("375850593")
ServerModSetup("378160973")
ServerModSetup("661253977")

--ServerModCollectionSetup takes a string of a specific mod's Workshop id. It will download all the mods in the collection and install them to the mod directory on boot.
	--The Workshop id can be found at the end of the url to the collection's Workshop page.
	--Example: http://steamcommunity.com/sharedfiles/filedetails/?id=379114180
	--ServerModCollectionSetup("379114180")
```

其中 `ServerModSetup` 四次调用的参数，就是之前提到的四个服务器 mod 的数字串。

##### 2.2.2 新建配置存储目录并写入配置

```shell
mkdir /root/dontstarvetogether_dedicated_server/mod_config
vim /root/dontstarvetogether_dedicated_server/mod_config/dedicated_server_mods_setup.lua
# 写入上一小节 2.2.1 中 dedicated_server_mods_setup.lua 的内容
vim /root/dontstarvetogether_dedicated_server/mod_config/modoverrides.lua
# 完整复制之前已经得到的 modoverrides.lua 的内容进来
```

##### 2.2.3 配置 mod 设置迁移脚本

```shell
vim /root/dontstarvetogether_dedicated_server/bin/copy_mod_settings.sh
# 内容如下（只是三行 cp 命令，并非六行，因为目录太长）

cp /root/dontstarvetogether_dedicated_server/mod_config/dedicated_server_mods_setup.lua /root/dontstarvetogether_dedicated_server/mods/dedicated_server_mods_setup.lua
cp /root/dontstarvetogether_dedicated_server/mod_config/modoverrides.lua /root/.klei/DoNotStarveTogether/MyDediServer/Master/modoverrides.lua
cp /root/dontstarvetogether_dedicated_server/mod_config/modoverrides.lua /root/.klei/DoNotStarveTogether/MyDediServer/Caves/modoverrides.lua
```

##### 2.2.4 更改启动服务脚本

更改先前写好的 `run_dedicated_servers.sh` ，前后部分过长用 `...` 省略。

原本的：

```shell
...
check_for_file "$dontstarve_dir/$cluster_name/Master/server.ini"
check_for_file "$dontstarve_dir/$cluster_name/Caves/server.ini"

./steamcmd.sh +force_install_dir "$install_dir" +login anonymous +app_update 343050 validate +quit

check_for_file "$install_dir/bin64"

cd "$install_dir/bin64" || fail
...
```

修改后：

```shell
...
check_for_file "$dontstarve_dir/$cluster_name/Master/server.ini"
check_for_file "$dontstarve_dir/$cluster_name/Caves/server.ini"

./steamcmd.sh +force_install_dir "$install_dir" +login anonymous +app_update 343050 validate +quit
sleep 1
sh /root/dontstarvetogether_dedicated_server/bin/copy_mod_settings.sh

check_for_file "$install_dir/bin64"

cd "$install_dir/bin64" || fail
...
```

#### 2.3 配置完成

至此 mod 部分就配置完成了，启动服务以测试效果。

### 3. 一键脚本

pending



