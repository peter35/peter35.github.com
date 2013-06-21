---
layout: post
title: 关于内存泄漏
description: Leaks
category: blog
---

# [{{ page.title }}][1]
2013-06-17 By {{ site.author_info }}

Leaks: 释放没有释放干净，如在退出某界面时该释放的没有释放掉，所以一般是退出某界面时才会看到是否有泄漏。
查看该工程是否使用arc: Build Settings中找到Objective-C Automatic Reference Counting  ，NO是没开启ARC的工程
在非ARC工程使用ARC文件，Compile Sources 中给该文件Compiler Flags标记为 -fobjc-arc，相对的是 -fno-objec-arc。

