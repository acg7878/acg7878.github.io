---
title: Wayland软件输入法问题汇总
date: 2024-10-6 11:23:47
categories: 
    - ArchLinux
    - Wayland
    - hyprland
cover: "https://hexopicture.oss-cn-guangzhou.aliyuncs.com/blog/ArchLinux/Wayland%E8%BD%AF%E4%BB%B6%E8%BE%93%E5%85%A5%E6%B3%95%E9%97%AE%E9%A2%98%E6%B1%87%E6%80%BB/hyprland.png"
---

##  零、前言

装完软件后，大多是xwayland模式运行下的软件，亦或是electron框架下的软件，无法正常调用fcitx进行中文输入。

搜寻一番后大多是使用万能参数进行启动，那么就需要将启动参数写入desktopentry文件里面了，似乎有些软件又有所不同。

以下单独列出。

**万能参数**：

```shell
--ozone-platform-hint=auto --enable-features=TouchpadOverscrollHistoryNavigation --enable-wayland-ime
```



## 一、edge

将万能参数写入`.desktop`中没有生效，需要单独写配置文件，在`~/.config`下创建`microsoft-edge-stable-flags.conf`，然后在里面填入万能参数即可，edge是从这里读取参数启动的吗？



## 二、VSCode

同为微软的产品，又是electron框架，VSCode也需要在`~/.config`下创建配置文件，命名为：`code-flags.conf`。

只需填入：

```shell
--enable-wayland-ime
```



## 三、QQ

`.desktop` 填入万能参数即可
