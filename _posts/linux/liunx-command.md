---
layout: post
title:  Linux 一些常用命令
category: Liunx
tags: Liunx
keywords: Liunx
---

## Linux一些常用命令      
- [mkdir](#mkdir)     创建文件夹
- [touch](#touch)     创建文件
- [vi](#vi)        创建OR编辑文件
- [rm](#rm)        删除文件夹或文件

#### 1. <a name="mkdir">mkdir</a>  
+ 语法  
   mkdir (选项)(参数)

+ 选项  
    -Z：设置安全上下文，当使用SELinux时有效；  
    -m<目标属性>或--mode<目标属性>建立目录的同时设置目录的权限；  
    -p或--parents 若所要建立目录的上层目录目前尚未建立，则会一并建立上层目录；  
    --version 显示版本信息。
 
 + 参数  
    目录：指定要创建的目录列表，多个目录之间用空格隔开。  
 
 + 实例  
    在目录/user下创建test文件夹  
    mkdir test

#### 2. <a name="touch">touch</a>  
+ 语法  
   touch (选项)(参数)

+ 选项  
    -a：或--time=atime或--time=access或--time=use 只更改存取时间；  
    -c：或--no-create 不建立任何文件；  
    -d：<时间日期> 使用指定的日期时间，而非现在的时间；  
    -f：此参数将忽略不予处理，仅负责解决BSD版本touch指令的兼容性问题；  
    -m：或--time=mtime或--time=modify 只更该变动时间； 
    -r：<参考文件或目录> 把指定文件或目录的日期时间，统统设成和参考文件或目录的日期时间相同；   
    -t：<日期时间> 使用指定的日期时间，而非现在的时间；  
    --help：在线帮助；   
    --version：显示版本信息。  

 
 + 参数  
    文件：指定要设置时间属性的文件列表。  
 
 + 实例  
    在目录/user下创建test文件  
    touch test.txt

#### 3. <a name="vi">vi</a>  
+ 语法  
   vi (选项)(参数)

+ 选项  
    +<行号>：从指定行号的行开始先是文本内容；  
    -b：以二进制模式打开文件，用于编辑二进制文件和可执行文件；  
    -c<指令>：在完成对第一个文件编辑任务后，执行给出的指令；  
    -d：以diff模式打开文件，当多个文件编辑时，显示文件差异部分；  
    -l：使用lisp模式，打开“lisp”和“showmatch”；  
    -m：取消写文件功能，重设“write”选项；  
    -M：关闭修改功能；  
    -n：不实用缓存功能；  
    -o<文件数目>：指定同时打开指定数目的文件；  
    -R：以只读方式打开文件；  
    -s：安静模式，不现实指令的任何错误信息。  


 
 + 参数  
    文件列表：指定要编辑的文件列表。多个文件之间使用空格分隔开。
 
 + 实例  
    在目录/user下创建test文件  
    touch test.txt


#### 4. <a name="rm">rm</a>