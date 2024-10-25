---
title: 云服务器基础配置
date: 2024-10-26 01:25:47
categories: 
    - Linux
    - 云服务器
cover: "http://hexopicture.oss-cn-guangzhou.aliyuncs.com/blog/Cloud/%E4%BA%91%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%9F%BA%E7%A1%80%E9%85%8D%E7%BD%AE/cover.png"
---

## 零、前言
白嫖的一个月华为云要到期了，稍微对比了一下转战了京东云，花费48元买了一年，嘿嘿。但是一切都要从头配置了，稍微记录一下叭。
这一个月的云服务器使用体验还是不错的，所以小剁手一下比较果断捏。

## 一、code-server
最有用的一集。整服务器最需要的功能就是实现浏览器敲码，虽然lunarvim也很好用，但是有些场合还是使用一下图形化更方便，两者结合吧。



贴一下安装脚本，从官方项目copy下来的

```shell
curl -fsSL https://code-server.dev/install.sh | sh
```

安装完毕后先运行一次生成配置文件，`code-server`

配置文件的路径为：`~/.config/code-server`，里面有个`config.yaml`

主要是修改密码和（ip：端口）

```yaml
bind-addr: 0.0.0.0:8080 # 改为0.0.0.0,emmm是因为啥？没去搜，然后端口自己选个
auth: password
password: nuoji123 # 密码咯
cert: false
```

### 管理

为了方便，使用systemd管理，写个service

```shell
vim /etc/systemd/system/code-server.service
```

填入：

```sh
[Unit]
Description=code-server
After=network.target

[Service]
Type=simple
User=root
ExecStart=code-server --bind-addr 0.0.0.0:8080
Restart=on-failure

[Install]
WantedBy=multi-user.target

```

最后：

```shell
systemctl start code-server.service # 启动
systemctl start code-server.service # 自启
```

### 换源

没错，我才发现第三方VSCode可以使用微软官方的插件源，我之前还傻乎乎地找插件bin

**配置文件**：

```shell
/usr/lib/code-server/lib/vscode/product.json
```

加入：

```json
  "extensionsGallery": {
    "serviceUrl": "https://marketplace.visualstudio.com/_apis/public/gallery",
    "cacheUrl": "https://vscode.blob.core.windows.net/gallery/index",
    "itemUrl": "https://marketplace.visualstudio.com/items",
    "controlUrl": "",
    "recommendationsUrl": ""
  }
```



