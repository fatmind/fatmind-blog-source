title: 尝试sublimeText2[怒赞]
date: 2014-04-19
tags:
---

*题记：记录一些自己使用过程，以备查用*

**一、插件安装**

以：ConvertToUTF8，为例  

1.手动安装
>https://github.com/seanliang/ConvertToUTF8/blob/master/README.zh_CN.md  
如需手工安装，请将本项目打包下载并解压，将解压后的文件夹名修改为 ConvertToUTF8 ，然后将此文件夹移动到 Sublime Text 的 Packages 文件夹下（可通过 Sublime Text 菜单中的 Preferences > Browse Packages 找到 Packages 文件夹）

2.自动安装：Package Control  
作用：that makes it exceedingly simple to find, install and keep packages up-to-date  
a、Package Control 自身就是一个插件，因此首先要安装它。当然可以手动安装。
官方方法 https://sublime.wbond.net/installation#st2  
b、Preferences > Package Control，点击 install package，输入ConvertToUTF8，即可  
bwt：PackageControl很多插件是来源于github，若不能访问，则需要设置代理，请google


**二、运行ruby**

1.写测试ruby逻辑，ctrl+b执行，发现提示：  
>[Error 2]  
[cmd:  [u'ruby', u'D:\\Temp\\sublime_ruby.rb']]  
[dir:  D:\Temp]  
[path: C:\Windows\system32;...D:\dev\env\btrace\bin;...]  
[Finished]  

多半也能猜到，是依赖path去找到ruby.exe。添加%ruby_home%\bin到path路径即可。
>1.安装ruby-1.8.6-p111-i386-mswin32.zip，添加到classpath后，在cmd执行irb，提示‘缺少readline.dll’  
答案：Go to [here](http://gnuwin32.sourceforge.net/downlinks/readline-bin-zip.php) to download package. Find readline5.dll, rename to readline.dll and copy to C:\ruby\bin.

2.其它语言的集成，同上。
非sublime默认支持的语言，以groovy为例：tools -> build system -> new build system，前提在path首先能找到groovy  
```json
{
	"cmd": ["groovy", "$file"],
	"selector": "source.groovy",
	"windows":
    {
        "shell": "cmd.exe"
    }
}
生成Groovy.sublime-build文件，背后原理：http://sublime-text.readthedocs.org/en/latest/reference/build_systems.html
```

3.如何制作绿色版Python  
Python安装时，有个选项 ‘装给所有用户还是只安装给当前用户’，选择当前用户，则会把那些需要的dll，包括 msvcr90.dll 都给装到 Python_home 目录。可随意copy使用。

**三、安装相关**

1.更改默认package目录  
默认的配置目录是 C:\Users\%user%\AppData\Roaming\Sublime Text 2。安装好sublime之后，不要直接运行sublime，地址栏输入%appdata%，删除该目录下的sublime text 2文件夹，然后在sublime安装目录下建立一个Data的文件夹，再运行sublime，以后关于sublime的所有配置和plugin都此Data目录。

2.注册码  
首先要感谢sublimeText，作为收费软件，但可以无限期试用。但时不时的跳出一个‘购买提示’，就去掉吧  
>Andrew Weber  
Single User License  
EA7E-855605  
813A03DD 5E4AD9E6 6C0EEB94 BC99798F  
942194A6 02396E98 E62C9979 4BB979FE  
91424C9D A45400BF F6747D88 2FB88078  
90F5CC94 1CDC92DC 8457107A F151657B  
1D22E383 A997F016 42397640 33F41CFC  
E1D0AE85 A0BBD039 0E9C8D55 E1B89D5D  
5CDB7036 E56DE1C0 EFCC0840 650CD3A6  
B98FC99C 8FAC73EE D2B95564 DF450523  
来源 http://www.yinqisen.cn/blog-246.html

3.资料

a.sublime入门技巧【good】  
http://lucifr.com/2011/08/31/sublime-text-2-tricks-and-tips/   
b.一些必不可少的插件  
http://www.cnblogs.com/varlxj/archive/2012/01/26/2329788.htmlessential-to-sublime-the-text-2-plugins.html  
c.SublimeREPL遇到 windowsError(2 错误
https://github.com/wuub/SublimeREPL/issues/96



