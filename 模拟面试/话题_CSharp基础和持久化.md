---
tags:
  - 模拟面试
url: https://www.bilibili.com/video/BV1QcSVYWE1M
---

## 你全是个人项目，没有商业项目，不符合我们的要求

解决办法，做一个商业项目!!

表现我掌握 相关技术知识，并且学习能力强


## 项目中打开的UI界面很多，占据大量内存怎么办

**使用平台特定的压缩格式**
安卓端图片压缩算法，
IOS端图片压缩算法，

**即时销毁用户看不见的UI界面**
时间换空间，不是deactive，而是destroy

**TODO 待补充 大坑**

https://blog.csdn.net/zhangjiyuan666/article/details/115526250


---

## C#基础题：普通父类 和 泛型父类 的 静态成员变量

```C#
class Father {
  public static int static_I = 0;
}
class Son1 : Father {}
class Son2 : Father {}
```

```C#
Son1.static_I = 20;
Son2.static_I = 30;
Console.Writeline(Father.static_I);
Console.WriteLine(Son1.static_I);
Console.Writeline(Son2.static_I);
```

输出结果，30 30 30，静态成员是属于类的，继承父类的子类，也共享。


```C#
class Father<T> {
  public static int static_I = 0;
}
class Son1 : Father<Son1> {}
class Son2 : Father<Son2> {}
```

```C#
Son1.static_I = 20;
Son2.static_I = 30;
Console.Writeline(Father<int>.static_I);
Console.Writeline(Father<Son1>.static_I);
Console.WriteLine(Son1.static_I);
Console.Writeline(Son2.static_I);
```

换成泛型类了，会发生什么？

0，20，20，30

泛型是语法糖，本质上是类似模板 创建一些列类，对每个明确的泛型类型，创建一个类。
具体化子类 和 具体化父类 共享静态成员变量。

实际编码时，泛型父类里写 静态成员变量时，rider会给警告的。

**深入回答**
在虚拟机和字节码方面，泛型如何处理泛型类的。

---

## 使用CSharp制作游戏存档功能，有哪些做法？

#### 首先明确是 云端存储 还是 本地存储。

云端存储的思路是，云端数据库后端存储，前端做本地化备份，每次上线时 同步存档数据。

本地存储的思路是，本质就是前端数据库。

#### 本地存储可能的技术路线，存储格式、读写方式

传统关系数据库，sqlite，存储在持久化目录的 db文件；
灵活性一般，非常稳定成熟。

自己管理的基于文件系统的 文档数据库，单机独立游戏大量使用；
非常灵活，可以自由选择 存储协议、加密协议等，用户可以轻松操作存档。

对象数据库，读写方式不是基于关系表的，是基于对象查询的。

介绍自己序列化持久化库 如何实现。

---

## CSharp如何反射获取类内部的私有成员？

#### 反射相关问题

可以获取的，实际开发中 针对unity引擎类的 反射访问，非常重要。
第三方库，对功能扩展，依赖反射，比如程序集扫描 查扩展类型 注册实例。

介绍自己反射库如何实现的的。
#### 反射有性能问题

简化的反射处理方式，最耗费性能的点是 查询类对象的过程，缓存委托 生成委托的方式 处理。

'高端的反射' 实际要做的 代码生成，包括 字节码生成、源代码生成。
对于性能关键的地方，比如 Lua调用、序列化，用代码生成作为解决方案。

代码生成别的用途，Burst，比方说uitk，生成一些样板代码。

---

## 在制作存档功能时，反射能发挥那些作用？

作用太大了。

比方说 unity.Serilization库，通过 unity.Properties库 提供的类反射能力。
创建对象，读写对象字段，获取类元数据；
尤其是获取元数据，用户通过提供类的元数据，指导序列化和反序列化过程。


---