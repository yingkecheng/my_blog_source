---
title: Keil 5.38 与 Arm Compiler 5 安装
date: 2023-04-08 19:43:24
tags: [软件配置]
---

# Keil 5.38 安装   

最近在给电脑安装Keil MDK的时候，使用原来的 keygen.exe 后，发现编译代码会出现的问题，这是因为 keygen.exe 产生的 license 到 2020 过期，可以在网络上搜索 keygen2032 相关软件，它产生的 license 可以使用至 2032 年。

当我们使用 STM32CubeMX 生成 MDK-ARM 代码后，编译出现 ```*** Target 'firmware' uses ARM-Compiler 'V5.06 update 7 (build 960)' which is not available``` 的错误，原因在于 5.38 版本的 Keil 已经没有默认的 Arm Compiler 5 了，我们需要为 Keil 安装 Arm Compiler 5。

# Arm Compiler 5 安装

[Arm Compiler downloads index](https://developer.arm.com/documentation/ka005198/latest) 可以下载 Arm Compiler 各个版本的编译器，选择 Arm Compiler 5.06 update 7 (build 960)。

![Arm Compiler 5 选择安装版本](https://raw.githubusercontent.com/yingkecheng/my_blog_pic/main/2023.4/Arm%20Compiler%205%20%E9%80%89%E6%8B%A9%E5%AE%89%E8%A3%85%E7%89%88%E6%9C%AC.png)

下载完成后解压，运行 setup.exe ，设置安装路径，我这里设置的路径为 ```D:\Tools\ARM_Compiler_5.06u7\```，不与 Keil 安装在同一路径下。打开 Keil，进入 ```Project > Manage > Project Items```，在 Folders/Extensions 页，Use ARM Compiler 行下添加此前安装的 Arm Compiler 5 路径，如下图。

![设置 Arm Compiler 5 路径](https://raw.githubusercontent.com/yingkecheng/my_blog_pic/main/2023.4/%E8%AE%BE%E7%BD%AE%20Arm%20Compiler%205%20%E8%B7%AF%E5%BE%84.png)

![添加 ArmCC 路径](https://raw.githubusercontent.com/yingkecheng/my_blog_pic/main/2023.4/%E6%B7%BB%E5%8A%A0%20ArmCC%20%E8%B7%AF%E5%BE%84.png)

设置完成后进行编译，还是可以看到原来的错误，原因是独立下载的 Arm Compiler 5 没有进行破解，最简单的方法是重新安装 Arm Compiler 5，将安装路径设置为 MDK-ARM 安装路径下的文件夹 Arm。按照如上操作，再重新编译即可成功。
