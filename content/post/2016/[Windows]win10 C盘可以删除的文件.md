---
title: "[Windows]win10 C盘可以删除的文件"
date: "2016-06-08T10:03:40+08:00"
categories:
- Windows
tags:
- Clean
---

Download目录：

    C:\Windows\SoftwareDistribution\Download

C盘中可以删除的VisualStudio相关的文件：

    C:\Users\用户名\AppData\Local\Microsoft\VisualStudio\14.0\Extensions\izpfmoxb.f22\Data\vs14_XXXX

更新VS2015 update2以后，这个目录下的文件有8个多g，删掉后不影响VS运行


这个下面的文件时windows更新文件和windows store的临时文件。官方建议是：可以但是不建议删除，如果c盘空间不足，可以删除。

    C:\Windows\SoftwareDistribution\DeliveryOptimization


===================================

其他方式

管理员权限的CMD中执行命令：

    Dism.exe /online /Cleanup-Image /StartComponentCleanup /ResetBase

具体参考：
https://msdn.microsoft.com/zh-cn/library/windows/hardware/dn898501(v=vs.85).aspx

