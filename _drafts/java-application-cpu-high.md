---
layout: post
title:  java-服务器应用CPU占用过高问题分析
categories: java
description: 服务器应用CPU占用过高问题分析
keywords: java, tomcat, Linux
---

### 1.使用top查询那个线程占用CPU过高
使用top查询那个线程占用CPU过高，并获取占用CPU过高的进程ID（PID）

![获取占用CPU过高的PID](/images/posts/java/CPU-high/1.png)

### 2.使用htop获取某一进程中的线程ID(TID)
命令：htop 进程ID

![获取占用CPU过高的PID](/images/posts/java/CPU-high/2.png)

### 3.将获取的十进制线程ID转成十六进制





