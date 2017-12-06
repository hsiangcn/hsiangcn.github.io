---
layout: post
title:  IntelliJ Idea 导出可执行jar包
categories: 工具
description: IntelliJ Idea 导出可执行jar包
keywords: IntelliJ Idea、java、工具
---

保证自己的Java代码是没有问题的，在IDEA里面是可以正常运行的，然后，按下面步骤：
打开File -> Project Structure -> Artifacts(快捷键：Ctrl+Alt+Shift+S)，如下图

![打开Structure](/images/posts/JetBrains/Intellij-export-jar/1.png)

点击‘➕’号，选择jar，选择Empty或From modules with dependencies，后者会把在项目中用到的Jar包解压开，当成项目的一部分，打包到最后的Jar包中。但是这样会有一个问题，即，如果项目中引用的Jar包有签名过，最后打包成的Jar包运行时会抛出错误：
     “java.lang.SecurityException: Invalid signature file digest for Manifest main attributes”
     因此，笔者选择的是Empty
![选择jar](/images/posts/JetBrains/Intellij-export-jar/2.png)

输入jar的Name，在OutPut Layout中选中jar，然后创建一个新的MANIFEST.MF，如果已经有了可以使用现有的，选择Use Existing Manifesf...即可。这里为新创建一个。
然后选择项目的根目录即可，在Main Class中选择程序入口

![创建MANIFEST](/images/posts/JetBrains/Intellij-export-jar/3.png)
![创建MANIFEST](/images/posts/JetBrains/Intellij-export-jar/4.png)
![选择程序入口](/images/posts/JetBrains/Intellij-export-jar/5.png)

程序需要选择Put into Output Root，其它jar选择Extract Into Output Root！否则导出的jar会出现找不到主清单异常。

![Put_into_Output_Root](/images/posts/JetBrains/Intellij-export-jar/6.png)
![Extract_Into_Output_Root](/images/posts/JetBrains/Intellij-export-jar/7.png)

注意：以上步骤完成务必检查项目根目录下的MANIFEST.MF文件，查看Main-Class是否指向正确的main方法，路径是否正确，不能有空格，不能有空格，不能有空格。

![Put_into_Output_Root](/images/posts/JetBrains/Intellij-export-jar/8.png)
![Put_into_Output_Root](/images/posts/JetBrains/Intellij-export-jar/9.png)

最后就是导出jar包，然后打开cmd，输入java -jar ****.jar

![Put_into_Output_Root](/images/posts/JetBrains/Intellij-export-jar/10.png)
![Put_into_Output_Root](/images/posts/JetBrains/Intellij-export-jar/11.png)
