---
title: Qt学习(4)：QML与C++的交互
date: 2024-07-10 21:00:14
categories: 
    - Qt
tags:
    - Qt
    - 
cover: "https://hexopicture.oss-cn-guangzhou.aliyuncs.com/blog/Qt%E5%AD%A6%E4%B9%A0(4)%EF%BC%9AQML%E4%B8%8EC%2B%2B%E7%9A%84%E4%BA%A4%E4%BA%92/cover.png"
---

## 一、C++与QML分离的重要性

AI作答：
1. **逻辑分离**：C++通常用于处理后端逻辑、数据处理和计算。将这些功能与QML的前端界面分离，有助于保持代码的清晰性和可维护性。可以在C++中处理复杂的业务逻辑，而在QML中专注于用户界面的设计和交互。
2. **性能优势**：C++是一种高性能的编程语言，特别适合处理计算密集型任务。通过将C++对象暴露给QML，可以利用C++的性能优势，同时仍然能够使用QML的灵活性和可视化能力。
3. **跨平台支持**：Qt框架允许您在不同平台上构建应用程序。通过将C++类暴露给QML，可以实现跨平台的应用程序，而无需为每个平台单独编写界面逻辑。

## 二、暴露C++对象给QML的方法

### qmlRegisterType函数

```c++
int qmlRegisterType(
    const char * uri, // 导出的名称，qml中使用import url来引用
    int versionMajor, // 主版本号
    int versionMinor, // 小版本号
    const char * qmlName // qml中使用此名称作为C++的对象来使用
)
```

`qmlRegisterType`函数用于将C++类注册为QML中的一个普通类型，使其可以在QML中实例化。

注册类型：

```c++
qmlRegisterType<MyClass>("com.example.myapp", 1, 0, "MyClass");
```

QML使用：

```QML
import QtQuick 2.0
import com.example.myapp 1.0

Rectangle {
    width: 200
    height: 200

    MyClass {
        id: myClass
        name: "Hello, World!"
    }

    Text {
        text: myClass.name
        anchors.centerIn: parent
    }
}
```



### qmlRegisterSingletonInstance函数

使用Qt提供的接口`qmlRegisterSingletonInstance` 将实例化好的将C++对象注册为QML中的单例，注意需要在QML引擎加载前注册。

如下是`qmlRegisterSingletonInstance` 接口的说明：

```C++
int qmlRegisterSingletonInstance(
	const char *uri, 		// 导出的名称，qml中使用import url来引用
	int versionMajor, 		// 主版本号
	int versionMinor, 		// 小版本号
	const char *typeName, 	// qml中使用此名称作为C++的对象来使用
	QObject *cppObject		// C++类的实例指针
)

```

### setContextProperty函数

通过qml引擎的属性注册C++类。使用`setContextProperty`函数。会将C++对象添加到QML引擎的全局上下文中。

```c++
engine.rootContext()->setContextProperty("myObject", myObjectInstance);
```

