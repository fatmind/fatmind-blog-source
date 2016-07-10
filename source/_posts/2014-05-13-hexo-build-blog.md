title: 入手hexo初尝试
date: 2014-05-13 14:43:49
---

在mac电脑上，重新搭hexo环境，更新一遍。

##### 1.nvm和node
>强烈推荐brew，用于管理非mac软件，如nginx等

- Node Version Manager - Simple bash script to manage multiple active node.js versions，首先安装 brew install nvm

- 查看当前stable版本，nvm ls-remote
如安装6.0.0以上版本，运行hexo出现编译错误：Set must not pass in non-primitive values，https://github.com/nodejs/node/issues/6216
完全删除重新安装
>完全删除node
>1.手动安装：https://github.com/jesseyu/uninstallNodejs/blob/master/uninstallNodejs.sh
>2.nvm管理：切换到其它版本

<!-- more -->

##### 2.hexo安装
npm install -g hexo-cli，https://hexo.io/docs/index.html


##### 3.找主题
https://hexo.io/themes/
有各种风格，找到合适自己的，推荐 https://github.com/pinggod/hexo-theme-apollo
同时fork一份，方便后续自己做定制化

##### 4.运行
- 配置_config.yml：对应主题会有实际博客，一般大家都会托管到github仓库，找到源码找复制一份，最节省时间
- hexo _command_，跑起来看一看
- 内容上传github，方便保存
- 在国内找一家提供pages服务，免费的静态网页托管
- 可选：绑定域名

$------------------------------------------------------------------------------------------------------------------------------------------------------------------------

> 以下是老版本，2016-07-10 中午 修改

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




