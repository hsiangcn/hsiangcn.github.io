---
layout: wiki
title: Linux/Unix 查看文件内容命令
categories: Liunx
description: Linux/Unix 常用命令 -- 查看文件内容命令
keywords: Liunx
---

## Linux下查看文件内容的命令

* cat     由第一行开始显示内容，并将所有内容输出  
* tac     从最后一行倒序显示内容，并将所有内容输出  
* more    根据窗口大小，一页一页的现实文件内容  
* less    和more类似，但其优点可以往前翻页，而且进行可以搜索字符  
* head    只显示头几行  
* tail    只显示最后几行  
* nl      类似于cat -n，显示时输出行号  
* tailf  类似于tail-f  

### 1.cat 与 tac

cat的功能是将文件从第一行开始连续的将内容输出在屏幕上。但是cat并不常用，原因是当文件大，行数比较多时，屏幕无法全部容下时，只能看到一部分内容。
- cat语法：cat [-n]  文件名 （-n ： 显示时，连行号一起输出）

tac的功能是将文件从最后一行开始倒过来将内容数据输出到屏幕上。我们可以发现，tac实际上是cat反过来写。这个命令也不常用。  
- tac语法：tac 文件名。  


### 2.more和less（常用）

more的功能是将文件从第一行开始，根据输出窗口的大小，适当的输出文件内容。当一页无法全部输出时，可以用“回车键”向下翻行，用“空格键”向下翻页。退出查看页面，请按“q”键。  
另外，more还可以配合管道符“|”（pipe）使用，例如:ls -al | more  

- more的语法：more 文件名   
    Enter 向下n行，需要定义，默认为1行；   
    Ctrl f 向下滚动一屏；   
    空格键 向下滚动一屏；    
    Ctrl b 返回上一屏；    
    = 输出当前行的行号；   
    :f 输出文件名和当前行的行号；   
    v 调用vi编辑器； 
    ! 命令 调用Shell，并执行命令； 
    q 退出more 

less的功能和more相似，但是使用more无法向前翻页，只能向后翻。  
less可以使用【pageup】和【pagedown】键进行前翻页和后翻页，这样看起来更方便。  
- less的语法：less 文件名 

less还有一个功能，可以在文件中进行搜索你想找的内容，假设你想在文件中查找有没有一个字符串，那么你可以这样来做：
- less 文件名
然后输入：/搜索内容  
回车   
此时如果有搜索内容字符串，linux会把该字符已高亮方式显示。   
退出查看页面，请按“q”键。   

### 3.head和tail

head和tail通常使用在只需要读取文件的前几行或者后几行的情况下使用。head的功能是显示文件的前几行内容  
- head的语法：head [n number] 文件名 (number 显示行数)  
  
tail的功能恰好和head相反，只显示最后几行内容  
- tail的语法:tail [-n number] 文件名  

### 4.nl

nl的功能和cat -n一样，同样是从第一行输出全部内容，并且把行号显示出来
- nl的语法：nl 文件名

### 5.tailf

tailf命令几乎等同于tail -f，严格说来应该与tail --follow=name更相似些。当文件改名之后它也能继续跟踪，特别适合于日志文件的跟踪（follow the growth of a log file）。
与tail -f不同的是，如果文件不增长，它不会去访问磁盘文件。tailf特别适合那些便携机上跟踪日志文件，因为它能省电，因为减少了磁盘访问嘛。
tailf命令不是个脚本，而是一个用C代码编译后的二进制执行文件，某些Linux安装之后没有这个命令，本文提供了怎么编译安装tailf命令的方法。
     
面就谈谈二者的区别  
1. tailf 总是从文件开头一点一点的读， 而tail -f 则是从文件尾部开始读
2. tailf check文件增长时，使用的是文件名， 用stat系统调用；而tail -f 则使用的是已打开的文件描述符； 注：tail 也可以做到类似跟踪文件名的效果；但是tail总是使用fstat系统调用，而不是stat系统调用；结果就是：默认情况下，当tail的文件被偷偷删除时，tail是不知道的，而tailf是知道的。  
    - 常用参数     
		格式：tailf logfile      

动态跟踪日志文件logfile，最初的时候打印文件的最后10行内容。    
- 关闭 `:set nonu`    
- 开启 `:set number`   

### 5. .gz查看

gunzip -c xxx.gz | less