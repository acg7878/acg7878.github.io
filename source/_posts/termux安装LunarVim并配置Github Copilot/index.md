---
title: termux安装LunarVim并配置Github Copilot
date: 2024-07-10 11:19:23
categories: 
    - LunarVim
tags:    
    - 配置环境
cover: "https://hexopicture.oss-cn-guangzhou.aliyuncs.com/blog/termux%E5%AE%89%E8%A3%85LunarVim/cover.png"
---
## 一、安装篇

简单的安装好neovim先，因为LunarVim是在neovim的基础上改造的。

```bash
pkg install neovim
```

安装LunarVim所需依赖

```bash
pkg install neovim build-essential python vim-python nodejs rust fd ripgrep
```

完成后执行LunarVim安装脚本，可能需要魔法上网

```bash
LV_BRANCH='release-1.4/neovim-0.9' bash <(curl -s https://raw.githubusercontent.com/LunarVim/LunarVim/release-1.4/neovim-0.9/utils/installer/install.sh)
```

```txt
      88\                                                   88\
      88 |                                                  \__|
      88 |88\   88\ 888888$\   888888\   888888\ 88\    88\ 88\ 888888\8888\
      88 |88 |  88 |88  __88\  \____88\ 88  __88\\88\  88  |88 |88  _88  _88\
      88 |88 |  88 |88 |  88 | 888888$ |88 |  \__|\88\88  / 88 |88 / 88 / 88 |
      88 |88 |  88 |88 |  88 |88  __88 |88 |       \88$  /  88 |88 | 88 | 88 |
      88 |\888888  |88 |  88 |\888888$ |88 |        \$  /   88 |88 | 88 | 88 |
      \__| \______/ \__|  \__| \_______|\__|         \_/    \__|\__| \__| \__|
 
--------------------------------------------------------------------------------
Detecting platform for managing any additional neovim dependencies
--------------------------------------------------------------------------------
Would you like to install LunarVim's NodeJS dependencies?
[y]es or [n]o (default: no) : y
Installing node modules with npm..
All NodeJS dependencies are successfully installed
--------------------------------------------------------------------------------
Would you like to install LunarVim's Python dependencies?
[y]es or [n]o (default: no) : y
Verifying that pip is available..
/usr/bin/python3: No module named ensurepip
Installing with pip..
Requirement already satisfied: pynvim in /usr/lib/python3/dist-packages (0.4.2)
All Python dependencies are successfully installed
--------------------------------------------------------------------------------
Would you like to install LunarVim's Rust dependencies?
[y]es or [n]o (default: no) : y
All Rust dependencies are successfully installed
```

执行三次yes。

在termux安装可能会报错，这是因为termux的目录与正常Linux有差异，需要前往修改安装脚本。

```bash
cd ~/.local/share/lunarvim/lvim/utils/installer
nvim install_bin.sh
```

将脚本开头修改为：

```bash
#原来是/usr/bin/env bash

#!/data/data/com.termux/files/usr/bin/env bash
```



**注意**：

在云服务器上配置lunarvim的时候从snap安装的nvim会出问题，从源码编译安装后配置成功了，具体原因未知

## 二、配置篇

修改LunarVim的配置文件，可以通过`lvim`再点击`c`键进行调出

```lua
lvim.plugins = {
	{
		"github/copilot.vim"
	},
}
-- 安装copilot
```

由于copilot接受建议需要按下`tab`，与LunarVim的代码提示冲突，需要进行修改

同样是在刚刚的配置文件，添加上以下代码，可将接受建议改为`Ctrl+E`，也可给成你喜欢的按键

```lua
vim.g.copilot_no_tap_map = true;
vim.api.nvim_set_keymap("i", "<C-E>", 'copilot#Accept("<CR>")', { silent = true, expr = true });
```

还有一点冲突，即是clangd和copilot的缓冲区冲突问题，进入lvim会显示警告：warning: multiple different client offset_encodings detected for buffer, this is not supported yet

可以调整clangd的编码进行修复。

```bash
find ~ -type f -iname "clangd.lua" #寻找clangd的配置文件在哪
lvim /data/data/com.termux/files/home/.local/share/lunarvim/site/pack/lazy/opt/nvim-lspconfig/lua/lspconfig/server_configurations/clangd.lua
```

按`/`进行查找，输入`utf`定位，去掉`utf-8`

```lua
offsetEncoding = { 'utf-16' },
```



