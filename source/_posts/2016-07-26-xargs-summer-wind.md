---
title: xargs一些事
date: 2016-07-26 12:39:39
---

在杭州40多度的高温里，老板告诉小明，"去把这个目录下的txt文件，统一改成log"。赶紧翻开鸟哥私房菜，再google，大家都提到一个人物：xagrs

<!-- more -->

--1.背景--

它来自何方？在unix世界里，沉浸在机械键盘的敲打声中，偶尔鄙视下旁边用图形界面的哥们。一日发现，”很多命令不支持|管道来传递参数“，这可是日常支柱之一，稍作沉思，xargs诞生。
>这个命令是错误的：find /sbin -perm +700 | ls -l
>正确的：find /sbin -perm +700 | xargs ls -l

--2.用法--

2.1 分隔符
>ls -l | grep "MOV" |  awk -F " " '{print $9}' | xargs

对管道来说，将全部的正确结果（对错误信息信息没有直接处理能力），作为标准的输入，传递给下一个命令。一批数据，必须分隔开，才能作为参数。
默认：采用空白符来分隔参数。

2.2 常用点

a、文件名有空白符，如何处理 ？
一个简单的解决办法就是告诉find使用NUL(\0)来分割结果（通过向find提供-print0选项），并且告诉xargs也使用Nul来分隔输入（-0）
>举个例子：删除备份文件，即使含有空格：find . -name '*~' -print0 | xargs -0 rm

b、使用-i（需要大写），用{}代替每一个参数，也可以指定具体名字
回到小明的问题，我们来一个命令搞定
>find . -name "*txt" | awk -F "\\." '{print $2}' | xargs -I file mv ./file.txt ./file.log

c、再进一步，替换部分文件名，如：把下滑线替换为中划线
>find . -name "*log" | xargs -t  -I fff mv fff `echo fff | sed 's/log/txt/g'`
>尝试没成功，类似此类还是写shell脚本处理吧
```
#!/bin/bash
echo $1
for oldfile in $(ls -l $1 | grep MOV | awk -F " " '{print $9}')
do
        newfile=`echo $oldfile | sed 's/_//g'`
        mv $1/$oldfile $1/$newfile
        echo "${oldfile} -> ${newfile}"
done
echo "change fielname done!"
```



