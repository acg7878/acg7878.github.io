---
title: LunarVim配置汇总
date: 2024-09-17 18:43:23
categories: 
    - termux
    - LunarVim
tags:    
    - 配置
cover: "https://hexopicture.oss-cn-guangzhou.aliyuncs.com/blog/LunarVim%E9%85%8D%E7%BD%AE%E6%B1%87%E6%80%BB/LunarVim.png"
---

  

本篇是对Lunar一些插件安装的记录。

## 零、前言

我希望将插件配置文件单独引出，于是进行了一些摸索。

LunarVim（也就是neovim）使用Lua来管理配置，我的需求是使用Lua来调用另外一个Lua。

搜索得知可以使用`require()`函数，但是怎么设置都未能成功，报错为：

```shell
19:01:08 [WARN ] lvim: "Invalid configuration: /data/data/com.termux/files/home/.config/lvim/config.lua:7: module 'my_plugins'
 not found:\n\tno field package.preload['my_plugins']\n\tno file './my_plugins.lua'\n\tno file '/data/data/com.termux/files/us
r/share/luajit-2.1/my_plugins.lua'\n\tno file '/usr/local/share/lua/5.1/my_plugins.lua'\n\tno file '/usr/local/share/lua/5.1/m
y_plugins/init.lua'\n\tno file '/data/data/com.termux/files/usr/share/lua/5.1/my_plugins.lua'\n\tno file '/data/data/com.termu
x/files/usr/share/lua/5.1/my_plugins/init.lua'\n\tno file './my_plugins.so'\n\tno file '/usr/local/lib/lua/5.1/my_plugins.so'\
n\tno file '/data/data/com.termux/files/usr/lib/lua/5.1/my_plugins.so'\n\tno file '/usr/local/lib/lua/5.1/loadall.so'" file="[
C]", line=-1
```

**问题搁置ing**，用了另外一个函数实现

```lua
dofile(vim..fn.sstdpath("config") .. "/my_plugins.lua")
```

成功实现分离。

**2024.10.7补充**：

使用 require 函数加载指定模块的文件时，可能提示找不到对应的文件，原因在于 Lua 有一个默认的查找路径。通过 package.path 和 package.cpath 查找路径。

>package.cpath:代表默认的 so 库的查找路径。
>package.path：代表默认的 lua 文件的查找路径。
>
>如果通过 require 函数加载指定的模块文件时，提示找不到对应的文件，则通过设置 package.path 路径即可

```lua
package.path = package.path..";/home/acg7878/.config/lvim/?.lua"
require("my_plugins")
-- 上述代码表示将 package.path 的路径追加上 「/home/acg7878/.config/lvim/」 路径下的所有 lua 文件
```


另外注：

根据LunarVim文档，安装插件的写法为：

```lua
lvim.plugins = {
    {"xxxx"}
    -- 或者
    {
        "xxxx",
        cmd = "xxx"
    }
}
```



## 一、markdown-preview.nvim

GitHub：[markdown-preview.nvim](https://github.com/iamcco/markdown-preview.nvim)

在`my_plugins.lua`写入：

```lua
-- markdown-preview
lvim.plugins = {
  {
    "iamcco/markdown-preview.nvim",
    cmd = { "MarkdownPreviewToggle", "MarkdownPreview", "MarkdownPreviewStop" },
    ft = { "markdown" },
    build = function() vim.fn["mkdp#util#install"]() end,
  }
}
```

安装后无法使用预览功能，表现为输入`:MarkdownPreview`不打开浏览器进行渲染，无任何反应。前往Github看了下issue，解决方法为在markdown-preview.nvim插件目录下执行`yarn install`安装所需组件。但是我不知道LunarVim组件放哪（），大伙贴出的也都是neovim的，一通寻找，如下：

```bash
~/.local/share/lunarvim/site/pack/lazy/opt/markdown-preview.nvim
```

需要在其`app`文件夹下进行`yarn install`，然后就可以愉快地进行markdown撰写了
