---
title: Qt学习(2)：VSC创建和编译Qt工程
date: 2024-07-04 01:25:26
categories: 
    - Qt
tags:
    - Qt
    - vsc
cover: "https://hexopicture.oss-cn-guangzhou.aliyuncs.com/blog/Qt%E5%AD%A6%E4%B9%A0(2)%EF%BC%9AVSC%E5%88%9B%E5%BB%BA%E5%92%8C%E7%BC%96%E8%AF%91Qt%E5%B7%A5%E7%A8%8B%2F2024-07-04-01-28-19.jpg"
---
## 一、环境变量配置

需要配置4处环境变量，可以参照一下路径自行添加

```txt
D:\Qt\6.7.2\mingw_64
D:\Qt\6.7.2\mingw_64\bin
D:\Qt\Tools\CMake_64\bin
D:\Qt\Tools\mingw1120_64\bin
```

## 二、VSC插件

需要安装几个插件：

1. `Cmake`和`Cmake Tools`
2. `Qt tools`：拥有启动Qt Creator等功能

3. `Qt Configure`：创建Qt工程

### 插件配置

#### Qt Configure

**Qt Configure: Mingw Path**

> MinGW 安装路径，需要到 bin 之前的那个目录

```txt
D:\Qt\Tools\mingw1120_64 //示例
```

**Qt Configure: Qt Dir**

>Qt 安装路径

```txt
D:\Qt
```

**Qt Configure: Qt Kit Dir**

> Qt 套件路径

```txt
D:\Qt\6.7.2\mingw_64
```

#### Cmake Tools

**Cmake: Cmake Path**

填入Qt的cmake执行文件的路径

```txt
D:\Qt\Tools\CMake_64\bin\cmake.exe
```

## 三、工程创建和编译

### 工程创建

vscode使用`shift+ctrl+p`调出主命令框，输入`QtConfigure:New Project`。按提示创建即可

注意生成的`CMakeLists.txt`中使用的是Qt5，使用Qt6的小伙伴需要修改成Qt6

```cmake
find_package(Qt6 COMPONENTS Widgets REQUIRED) #具体是这两条
target_link_libraries(${PROJECT_NAME} PRIVATE Qt6::Widgets) #具体是这两条
```

### 编译器路径设置

vscode使用`shift+ctrl+p`调出主命令框，输入`C/C++: 编辑配置(UI)`

首先修改一下两项

![C++](https://hexopicture.oss-cn-guangzhou.aliyuncs.com/blog/Qt%E5%AD%A6%E4%B9%A0(2)%EF%BC%9AVSC%E5%88%9B%E5%BB%BA%E5%92%8C%E7%BC%96%E8%AF%91Qt%E5%B7%A5%E7%A8%8B%2F2024-07-04-01-12-27.png)

![C++2](https://hexopicture.oss-cn-guangzhou.aliyuncs.com/blog/Qt%E5%AD%A6%E4%B9%A0(2)%EF%BC%9AVSC%E5%88%9B%E5%BB%BA%E5%92%8C%E7%BC%96%E8%AF%91Qt%E5%B7%A5%E7%A8%8B%2F2024-07-04-01-12-34.png)

同时可以添加头文件路径以消除vscode头文件冒红问题

![include](https://hexopicture.oss-cn-guangzhou.aliyuncs.com/blog/Qt%E5%AD%A6%E4%B9%A0(2)%EF%BC%9AVSC%E5%88%9B%E5%BB%BA%E5%92%8C%E7%BC%96%E8%AF%91Qt%E5%B7%A5%E7%A8%8B%2F2024-07-04-10-41-27.png)

如果无法在GUI模式下填入路径，可以通过json进行修改（ **C/C++: 编辑配置(JSON)** ）

```json
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "D:\\Qt\\6.7.2\\mingw_64\\include\\**",
                "${workspaceFolder}\\**"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "windowsSdkVersion": "10.0.22000.0",
            "compilerPath": "D:/Qt/Tools/mingw1120_64/bin/g++.exe",
            "cStandard": "c17",
            "cppStandard": "c++17",
            "intelliSenseMode": "windows-gcc-x64"
        }
    ],
    "version": 4
}
```



此外，还需要设置一下工具包。在vscode状态栏的cmake tools处选择Qt的工具包

![Cmake Tools](https://hexopicture.oss-cn-guangzhou.aliyuncs.com/blog/Qt%E5%AD%A6%E4%B9%A0(2)%EF%BC%9AVSC%E5%88%9B%E5%BB%BA%E5%92%8C%E7%BC%96%E8%AF%91Qt%E5%B7%A5%E7%A8%8B/2024-07-04-01-15-43.png)

### 工程编译运行

至此，即可在状态栏点击构建与运行了，抑或通过快捷键`F5`进行编译运行
