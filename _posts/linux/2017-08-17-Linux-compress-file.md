---  
layout: post  
title:  linux下解压命令大全  
category: Liunx  
tags: Liunx  
keywords: Liunx  
---  

## 1 .tar   
解包：tar xvf FileName.tar  
打包：tar cvf FileName.tar DirName  
（注：tar是打包，不是压缩！）  
———————————————  
## 2 .gz
解压1：gunzip FileName.gz  
解压2：gzip -d FileName.gz  
压缩：gzip FileName  

## 3 .tar.gz 和 .tgz  
解压：tar zxvf FileName.tar.gz  
压缩：tar zcvf FileName.tar.gz DirName  
———————————————
## 4 .bz2
解压1：bzip2 -d FileName.bz2  
解压2：bunzip2 FileName.bz2  
压缩： bzip2 -z FileName  

## 5 .tar.bz2  
解压：tar jxvf FileName.tar.bz2  
压缩：tar jcvf FileName.tar.bz2 DirName  
———————————————
## 6 .bz
解压1：bzip2 -d FileName.bz  
解压2：bunzip2 FileName.bz  
压缩：未知  

## 7 .tar.bz
解压：tar jxvf FileName.tar.bz  
压缩：未知  
———————————————
## 8 .Z
解压：uncompress FileName.Z  
压缩：compress FileName  
.tar.Z  

解压：tar Zxvf FileName.tar.Z  
压缩：tar Zcvf FileName.tar.Z DirName  
———————————————  
## 9 .zip
解压：unzip FileName.zip  
压缩：zip FileName.zip DirName  
用MS Windows下的压缩软件winzip压缩的文件如何在Linux系统下展开呢？可以用unzip命令，该命令用于解扩展名为.zip的压缩文件。
### 9.1 语法
 　　语法：unzip ［选项］ 压缩文件名.zip
 
### 9.2 选项
 　　各选项的含义分别为<br>
 　　-x 文件列表 解压缩文件，但不包括指定的file文件。<br>
 　　-v 查看压缩文件目录，但不解压。<br>
 　　-t 测试文件有无损坏，但不解压。<br>
 　　-d 目录 把压缩文件解到指定目录下。<br>
 　　-z 只显示压缩文件的注解。<br>
 　　-n 不覆盖已经存在的文件。<br>
 　　-o 覆盖已存在的文件且不要求用户确认。<br>
 　　-j 不重建文档的目录结构，把所有文件解压到同一目录下。<br>
 ### 9.3 实例
 例1：将压缩文件text.zip在当前目录下解压缩。<br>
 　　$ unzip text.zip<br>
 例2：将压缩文件text.zip在指定目录/tmp下解压缩，如果已有相同的文件存在，要求unzip命令不覆盖原先的文件。<br>
 　　$ unzip -n text.zip -d /tmp<br>
 例3：查看压缩文件目录，但不解压。<br>
 　　$ unzip -v text.zip<br>
———————————————  
## 10 .rar  
解压：rar x FileName.rar  
压缩：rar a FileName.rar DirName  
———————————————  
## 11 .lha  
解压：lha -e FileName.lha  
压缩：lha -a FileName.lha FileName  
———————————————  
## 12 .rpm  
解包：rpm2cpio FileName.rpm | cpio -div  
———————————————  
## 13 .deb  
解包：ar p FileName.deb data.tar.gz | tar zxf -  
———————————————  
## 14 sEx
.tar .tgz .tar.gz .tar.Z .tar.bz .tar.bz2 .zip .cpio .rpm .deb .slp .arj .rar .ace .lha .lzh .lzx .lzs .arc .sda .sfx .lnx .zoo .cab .kar .cpt .pit .sit .sea  
解压：sEx x FileName.*  
压缩：sEx a FileName.* FileName  

sEx只是调用相关程序，本身并无压缩、解压功能，请注意！

## 15 gzip 命令  
减少文件大小有两个明显的好处，一是可以减少存储空间，二是通过网络传输文件时，可以减少传输的时间。gzip 是在 Linux 系统中经常使用的一个对文件进行压缩和解压缩的命令，既方便又好用。  

### 15.1 语法  
*语法：gzip [选项] 压缩（解压缩）的文件名*  
该命令的各选项含义如下   
   -a或——ascii：使用ASCII文字模式；   
   -c 将输出写到标准输出上，并保留原有文件。    
   -d或--decompress或----uncompress：解开压缩文件；   
   -f或——force：强行压缩文件。不理会文件名称或硬连接是否存在以及该文件是否为符号连接；  
   -h或——help：在线帮助；  
   -l或——list：列出压缩文件的相关信息；  
   -L或——license：显示版本与版权信息；    
   -n或--no-name：压缩文件时，不保存原来的文件名称及时间戳记；  
   -N或——name：压缩文件时，保存原来的文件名称及时间戳记；  
   -q或——quiet：不显示警告信息；  
   -r或——recursive：递归处理，将指定目录下的所有文件及子目录一并处理；  
   -S或<压缩字尾字符串>或----suffix<压缩字尾字符串>：更改压缩字尾字符串；  
   -t或——test：测试压缩文件是否正确无误；  
   -v或——verbose：显示指令执行过程；  
   -V或——version：显示版本信息；  
   -<压缩效率>：压缩效率是一个介于1~9的数值，预设值为“6”，指定愈大的数值，压缩效率就会愈高；  
   --best：此参数的效果和指定“-9”参数相同，表示最慢压缩方法（高压缩比）。系统缺省值为 6；    
   --fast：此参数的效果和指定“-1”参数相同， 表示最快压缩方法（低压缩比）。  
   
### 15.2 实例  
- 把test6目录下的每个文件压缩成.gz文件     
>>gzip *       
- 把上例中每个压缩的文件解压，并列出详细的信息   
>>gzip -dv *   
- 详细显示例1中每个压缩的文件的信息，并不解压   
>>gzip -l *  
- 压缩一个tar备份文件，此时压缩文件的扩展名为.tar.gz  
>>gzip -r log.tar  
- 递归的压缩目录    
>>gzip -rv test6<br>

这样，所有test下面的文件都变成了*.gz，目录依然存在只是目录里面的文件相应变成了*.gz.这就是压缩，和打包不同。因为是对目录操作，所以需要加上-r选项，这样也可以对子目录进行递归了。    
- 递归地解压目录  
>>gzip -dr test6   

## 16 zgrep命令
这个命令的功能是在压缩文件中寻找匹配的正则表达式，用法和grep命令一样，只不过操作的对象是压缩文件。如果用户想看看在某个压缩文件中有没有某一句话，便可用zgrep命令。

## 17 zcat命令
和zgrep一样，可以用于.gz 压缩过的文件，直接可以查看里面内容，和zgrep 一样如果结合管道符，必然可以找到更加丰富的用法。