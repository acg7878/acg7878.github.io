---
title: termux基础配置篇
date: 2024-09-15 00:22:23
categories: 
    - termux
    - git
tags:    
    - Linux
    - 配置
cover: "https://hexopicture.oss-cn-guangzhou.aliyuncs.com/blog/termux%E5%9F%BA%E7%A1%80%E9%85%8D%E7%BD%AE%E7%AF%87/termux.png"
---
termux出问题重置了，很多配置都忘记了，于是进行一下记录防止之后忘记。

## 一、oh-my-zsh

termux的oh-my-zsh在GitHub有一键配置的脚本，仓库链接为：[Cabbagec/termux-ohmyzsh: Colorize your termux! Oh-my-zsh included! (github.com)](https://github.com/Cabbagec/termux-ohmyzsh)。

通过脚本一键安装即可

```bash
sh -c "$(curl -fsSL https://github.com/Cabbagec/termux-ohmyzsh/raw/master/install.sh)"
```



## 二、git-ssh配置

### 01 生成ssh key

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
# 小ps： -b是指位数的意思，嘿嘿，刚刚gpt问的
```

### 02 将ssh key添加到github

前往`home`目录的`.ssh`文件夹将`id_rsa.pub`里面的内容拷贝到**github > settings > SSH and GPG keys > New SSH key**。

然后测试一下链接

```bash
 ssh -T git@github.com
```

若出现此便为成功
```bash
Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.
```

**注意**：

不要犯傻！！连接仓库时选择了http方式连接，我就说怎么一直询问我账号密码来鉴权，后来才发现应该用ssh的方式建立连接。

你需要这样做

```bash
git remote set-url origin git@github.com:用户名/仓库名.git
```



## 三、autojump安装

这个软件可以快速跳转文件目录，还是很方便的，经常使用。但是安装后需要一些简单的配置，我每次都忘记怎么操作呜呜。

### 01 安装

没什么好说的，直接包管理器就行

```bash
pkg install autojump
```

### 02 配置

需要导入一个shell脚本呢来实现功能。

配置文件在目录：`/data/data/com.termux/files/usr/share/autojump`

你需要将这个目录下的shell脚本导入到`.zshrc`中

即：

```bash
source /data/data/com.termux/files/usr/share/autojump/autojump.zsh
# 这个配置文件有三个，分别是bash、fish、zsh,想必是根据你的shell进行选择
```

然后再`source ~/.zshrc`即可

## 四、XDG
>XDG 是 "X Desktop Group" 的缩写，后来更名为 Freedesktop.org。XDG 主要涉及 Linux 和类 Unix 操作系统的桌面环境标准与规范。它定义了一系列标准，以便不同桌面环境、窗口管理器和应用程序能够在共享资源、配置文件和行为上保持一致。

XDG存在基本目录规范,这是XDG的一个标准，它定义了一些环境变量，用于指定用户配置文件、数据文件、缓存文件等应该存储的位置。

常见的环境变量如下：
- `XDG_CONFIG_HOME`：存储用户的配置信息，默认路径为 **~/.config/**。
- `XDG_DATA_HOME`：存储用户特定的数据文件，默认路径为 **~/.local/share/**。
- `XDG_CACHE_HOME`：存储用户的缓存文件，默认路径为 **~/.cache/**。
- `XDG_RUNTIME_DIR`：存储运行时数据，如进程通信的临时文件，通常为系统提供的一个目录。

为什么在这里提及这个呢，因为termux的路径有点不一样，也许在安装插件啊配置环境啊会报错，也许设置一下这几个环境变量能拯救。
PS：termux有环境变量`PREFIX`，路径为 **/data/data/com.termux/files/usr**

