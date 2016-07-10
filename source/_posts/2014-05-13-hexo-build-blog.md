title: 入手hexo初尝试[整理帖]
date: 2014-05-13 14:43:49
tags:
---

1. 安装nodejs（0.12.0）
Windows下有msi，且已集成npm，在 %nodejs_home%/node_modules 目录。
http://nodejs.org/ 用example验证下是否安装成功。

2. npm配置 http://segmentfault.com/q/1010000000315988
a、npm config ls -l 查看全部默认配置，默认全局prefix=%APPDATA%/Roaming/npm。
b、更改目录：npm config set perfix "D:\dev\env\nodejs-0.12.0-x64"。
cache目录修改同理。
c、npm install -g hexo-cli
参数-g的含义是代表安装到全局环境里面，如果沒有-g的话会安装到当前node_modules目录，如无则新建node_modules文件夹
d、设置源【否则慢死你】
npm config set registry "https://registry.npm.taobao.org/"

3. 安装hexo（3.0）
安装hexo-cli：http://hexo.io/docs/
初始化：http://hexo.io/docs/setup.html

4. hexo看着喜欢的主题
https://github.com/popozhu/Aries [[效果地址]](http://popozhu.github.io/)
https://github.com/imbyron/hexo-theme-daisy

5. hexo一些小点
a、archive配置以列表形式展现
修改_config.yml中的archive = 1，https://github.com/tommy351/hexo/issues/77
c、代码高亮
http://popozhu.github.io/2013/06/15/hexo%E4%BB%A3%E7%A0%81%E9%AB%98%E4%BA%AE/
http://sevn7.com/2013/11/15/heightline/  （页面风格也不错）
d、hexo一些优化配置
http://zipperary.com/2013/05/30/hexo-guide-4/
e、超详细解释【good】
http://ibruce.info/2013/11/22/hexo-your-blog/

6. gitcafe创建pages
~~与github整合 http://kescoode.com/2013/11/17/github-page-and-hexo-guide/~~
因github慢，切换到gitcafe【推荐】
https://gitcafe.com/GitCafe/Help/wiki/Pages-%E7%9B%B8%E5%85%B3%E5%B8%AE%E5%8A%A9

6. 问题
a、执行deploy时，提示 Error: spawn ENOENT
在gitbash模式下执行. http://zhenghongen.cn/2013/07/09/hexo-deploy-throw-error/
b、执行deploy时，提示：Not a git repository
rm -rf .deploy -> hexo deploy
~~c、hexo升级2.8问题
\- 需要重新init目录，再安装插件 https://github.com/hexojs/hexo/wiki/Migrating-from-2.5-to-2.6
\- 更新到2.8.0错误提示“incomplete explicit mapping pair”, 需要把存在空格值用双引号括起来，如：archive_b: Archives: %s => archive_b: "Archives: %s"
https://github.com/hexojs/hexo/issues/722~~




