---
title: Hello!Socket
date: 2024-10-28 20:08:47
categories: 
    - CPP
cover: "http://hexopicture.oss-cn-guangzhou.aliyuncs.com/blog/CPP/Socket/HelloSocket/Hello_socket.png"
---

##  零、前言
天天摸鱼，终于开始写课程作业了。这周的作业任务是入门Socket编程，老样子我决定使用CPP来写作业。

使用ChatGPT来入门是个不错的方法，我可以让他总结一下我该学习些什么，并问一些常用的基础的函数。


## 一、服务端

**创建 Socket**：使用 `socket()` 函数创建一个 Socket。
**绑定 Socket**：使用 `bind()` 将 Socket 绑定到指定的 IP 和端口。
**监听连接**：使用 `listen()` 准备接受连接。
**接受连接**：使用 `accept()` 接受来自客户端的连接。
**读取数据**：从客户端读取数据。
**发送响应**：向客户端发送响应数据。
**关闭 Socket**：完成通信后关闭 Socket。

### 01 创建Socket

```cpp
 // 创建 Socket
    int server_fd = socket(AF_INET, SOCK_STREAM, 0);
    if (server_fd == -1) {
        std::cerr << "Socket creation failed!" << std::endl;
        return -1; // 返回错误
    }


  // 服务器地址配置
  struct sockaddr_in address;
  address.sin_family = AF_INET;         // IPv4
  address.sin_addr.s_addr = INADDR_ANY; // 本地地址
  address.sin_port = htons(8888);       // 端口号
```

**参数分析**：

1. `AF_INET`：地址簇，表示使用了IPv4，如果使用IPv6则是AF_INET6
2. `SOCK_STREAM`：使用TCP协议
3. `0`：默认协议

### 02 绑定Socket

```cpp
// 绑定 Socket 到地址
  if (bind(server_fd, (struct sockaddr *)&address, sizeof(address)) < 0) {
    std::cerr << "Bind failed! Error: " << strerror(errno) << std::endl;
    close(server_fd);
    return -1;
  }
```

### 03 监听连接

```cpp
// 开始监听
    if (listen(server_fd, 3) < 0) {
        std::cerr << "Listen failed!" << std::endl;
        close(server_fd);
        return -1;
    }
	std::cout << "Server listening on port 8888..." << std::endl;
```

`3` 是“**backlog**”，即**最大等待连接数**，它决定了在接受客户端连接之前，内核为服务器维护的等待连接的队列大小

### 04 接受连接

```cpp
  int addrlen = sizeof(address);
  int client_socket =
      accept(server_fd, (struct sockaddr *)&address, (socklen_t *)&addrlen);
  if (client_socket < 0) {
    std::cerr << "Accept failed! Error: " << strerror(errno) << std::endl;
    close(server_fd);
    return -1;
  }
```

