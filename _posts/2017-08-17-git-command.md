---
layout: post
title: git简略使用教程
categories: 工具
description: git简略使用教程
keywords: GIT
---

到官网下载git并进行安装[git下载](https://git-scm.com/downloads)  

### 设置用户的姓名和邮件，使用以下命令 
	git config --global user.name "name"  
	git config --global user.email "email"  
	
   这里的user.name是说明历史上的一个修改时谁提交的，第二个邮件就是便于联系修改者。可以使用"git config --global --list"，看之前输入的姓名邮件是否都在。  
git的全局变量有130多个，只有这两个是必须设置的。最后可以使用git config --global color.ui "auto"命令来设置是否使用不同的颜色显示不同类型的内容，这个依据个人喜好。

### 使用Git图形界面  
点击Git GUI就可以使用图形界面了，命令行的命令都可以通过Git GUI来完成。

### 一些常用命令

	git clone git地址 本地地址   
	从远程版本库中拷贝到本地来  

	git log : 查看日志

	git gc :压缩版本库，优化历史记录

	git archive:导出版本库

	git init:初始化版本库

	git add:增加一个或者多个文件到版本库暂存区中

	git commit:提交更新版本库

	git branch:列出本地分支

	git diff :比较差异

	git fetch:从远程版本库中取得修改变化，但不合并到本地版本库

	git pull:从远程版本库中取得修改变化，合并到本地版本库

	git push;将变化推入远程版本库中

	git remote rm：删除远程版本库的内容
