---
title: Qt学习(3)：信号与槽函数
date: 2024-07-07 16:36:26
categories: 
    - Qt
tags:
    - Qt
    - 
cover: "https://hexopicture.oss-cn-guangzhou.aliyuncs.com/blog/Qt%E5%AD%A6%E4%B9%A0(3)%EF%BC%9A%E4%BF%A1%E5%8F%B7%E4%B8%8E%E6%A7%BD%E5%87%BD%E6%95%B0/cover.png"
---

## 一、MOC

### 概念

moc 全称是 Meta-Object Compiler，也就是 **元对象编译器** 。

Qt 程序在交由标准编译器编译之前，先要使用 moc 分析 C++ 源文件。如果它发现在一个 **头文件** 中包含了宏 Q_OBJECT，则会生成另外一个 C++ 源文件。

注意，由于 moc 只处理头文件中的标记了Q_OBJECT的类声明，不会处理 cpp 文件中的类似声明。因此，如果我们的类位于 main.cpp 中，是无法得到 moc 的处理的。解决方法是，我们手动调用 moc 工具处理 main.cpp，并且将 main.cpp 中的#include “xxx.h”改为#include “moc_xxx.h”就可以了。不过，这是相当繁琐的步骤，为了避免这样修改，我们还是将其放在头文件中。

这个源文件中包含了 Q_OBJECT 宏的实现代码。这个新的文件名字将会是原文件名前面加上`moc_` 构成。这个新的文件同样将进入编译系统，最终被链接到二进制代码中去。因此我们可以知道，这个新的文件不是“替换”掉旧的文件，而是与原文件一起参与编译。

另外，我们还可以看出一点， **moc 的执行是在预处理器之前** 。因为预处理器执行之后，Q_OBJECT 宏就不存在了。

### 使用

在创建Qt工程时，Qt Creator会自动在CMakeLists.txt中添加`set(CMAKE_AUTOMOC ON) `以开启MOC，否则无法编译成功

```cmake
set(CMAKE_AUTOMOC ON)
```

可以使用moc命令生成标准C++文件

```shell
moc yourfilename.h -o moc_youfilename.cpp
```

## 二、信号与槽

### 信号

> 信号是类的一部分，当特定事件发生时自动发出。你可以把它想象成一个对象说：“某事发生了！”信号不需要指定接收者，也不关心谁会接收到这个信号，它只是简单地宣布一个事件的发生。

信号是 **public** 类型的，可以从任何地方发射。但是推荐在定义信号的类内部发射

### 槽函数

> 槽是普通的C++成员函数，可以被任何类定义。槽是用来响应信号的，即当一个信号被发出时，连接到该信号的槽函数会被自动调用。槽可以有返回值，也可以没有，它可以执行任何操作，包括调用其他函数、更新用户界面、处理数据等。

槽函数就是普通的C++函数。唯一特别的地方是：可以连接到信号。

槽函数可以通过 信号-槽 的连接而被任何组件调用，这与槽的访问权限无关。这意味着 **private** 的槽也可以被信号调用。

### 使用

#### 过程
1.  **声明信号和槽** ：在类的定义中使用宏`signals:`声明信号，使用普通成员函数的方式声明槽函数。信号不需要实现，而槽函数则需要。

2.  **连接信号和槽** ：使用`QObject::connect()`函数将信号和槽连接起来。此函数需要四个参数：信号发出者、信号、槽函数所属对象以及槽函数。连接成功后，当信号被发出时，相应的槽函数会被调用。
3.  **连接类型** ：`connect()`函数还允许指定连接类型，如直接连接（DirectConnection）、队列连接（QueuedConnection）等，这影响了信号和槽函数调用的时机和线程安全。

```C++
connect(sender, &SenderClass::signal, receiver, &ReceiverClass::slot);

QMetaObject::Connection QObject::connect(const QObject *sender, const char *signal, const QObject *receiver, const char *method, Qt::ConnectionType type = Qt::AutoConnection);
//第一个参数发送者，第二个信号，第三个接收者，第四个接收者的响应函数。
//第五个参数，只是一般使用默认值，在满足某些特殊需求的时候可能需要手动设置。能够设置信号与槽的连接方式
```

#### 例子

`counter.h`

```cpp
#ifndef COUNTER_H
#define COUNTER_H

#include <QObject>

class Counter : public QObject {
    Q_OBJECT

  public:
    Counter() : m_value(0) {}

    int value() const { return m_value; }

  public slots:
    void setValue(int value);

  signals:
    void valueChanged(int newValue);

  private:
    int m_value;
};

#endif // COUNTER_H

```

`counter.cpp`

```cpp
#include "counter.h"

void Counter::setValue(int value)
{
    if (value != m_value) {
        m_value = value;
        emit valueChanged(value);
    }
}
```

`main.cpp`

```cpp
#include "counter.h"
#include <QCoreApplication>
#include <QDebug>

int main(int argc, char *argv[]) {
    QCoreApplication app(argc, argv);

    Counter a, b;
	 
    // 连接 a 的 valueChanged 信号到 b 的 setValue 槽
    QObject::connect(&a, &Counter::valueChanged, &b, &Counter::setValue);
	// 连接 b 的 valueChanged 信号到一个 lambda 表达式，用于打印新的值
    QObject::connect(&b, &Counter::valueChanged, [](int newValue) {
        qDebug() << "b's value changed to" << newValue;
    });

    a.setValue(12); // 设置 a 的值为 12，这将触发 a 的 valueChanged 信号并调用 b 的 setValue 槽
    b.setValue(48); // 设置 b 的值为 48，这将触发 b 的 valueChanged 信号并调用连接的 lambda 表达式

    return app.exec();
}

```

`CMakeLists.txt`

```cmake
cmake_minimum_required(VERSION 3.5)

project(CounterExample)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_AUTOMOC ON)

find_package(Qt6Core REQUIRED)

add_executable(CounterExample main.cpp counter.cpp)

target_link_libraries(CounterExample Qt6::Core)
```



