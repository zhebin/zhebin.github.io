title: 初试HEXO博客系统
date: 2014-12-17 15:28:51
categories: 其他
tags: [hexo,github,nodejs]
description: 
---

##hexo简要介绍
hexo是一个基于Node.js的静态博客系统，出自台湾大学生[tommy351](https://github.com/tommy351/hexo)之手，其生成的静态网页可以放到[Github Pages](http://developer.baidu.com/cloud/rt)（正如本博客一样）,[BAE](http://developer.baidu.com/cloud/rt),[SAE](http://sae.sina.com.cn/)等。
###hexo优点
+ 搭建简单
+ 免去了申请域名、VPN等的麻烦
+ 支持MarkDown语法
+ 主题和插件很多

<!-- more -->

###环境准备
####安装Node
Node.js[官网](http://nodejs.org/)下载，点击下一步安装，没啥特别的。
####安装github for windows
git的客户端非常多，github for windows功能相对简易，但是也够用了，官网[在这里](https://windows.github.com)，下载安装。
####安装Sublime Text
[Sublime Text 3](http://www.sublimetext.com/3)在这里仅仅作为一个文本编辑器使用，支持各种编程语言和文件格式，当然也支持Markdown语法，也有对应的Markdown插件，实在是个不可多得的练码奇才。
##部署步骤
###GitHub资源库设置
+ 在[github](http://www.github.com)上新建一个以你的github账号命名的资源库，username.github.io资源库
+ 打开github for windows客户端，点击左上角的"+"号，切换到Clone标签页，点击username.github.io，在弹出的对话框中，选择本地资源库位置(例如 D:\github\zhebin.github.io)![github](https://zhebin.github.io/images/github.png)

###hexo本地运行
打开Git Shell，依次输入如下命令（//后为注释语句）
``` bash
cd D:\github\zhebin.github.io    //改变当前目录到github资源库主目录
hexo init    //初始化hexo
npm install   //安装依赖包
hexo generate  //生成hexo静态化文件
hexo server  //打开本地server（输入localhost:4000访问,Ctrl+C关闭server）
```
###部署hexo到github
+ 编辑_config.yml配置文件
在D:\github\zhebin.github.io主目录下，用sublime打开，定位到最后两行,修改为:
``` bash
deploy:
  type: github
  repository: https://github.com/zhebin/zhebin.github.io.git
  branch: master
```
*注意：type、repository、branch冒号后面都有一个空格，repository资源库后面记得加上git*
+ 部署到github
打开Git Shell，重新生成静态文件并发布，等待一分钟既可在服务器端访问[http://username.github.io](http://username.github.io)
``` bash
hexo generate
hexo deploy
```

##注意事项
###JQuery问题
由于hexo默认采用google的jquery CND方式，所以很可能造成jquery.js无法加载的问题，解决方法是用国内的CDN作为替代，
打开文件D:\github\zhebin.github.io\themes\landscape\layout\_partial\after-footer.ejs
将jquery.js引用路径更改为
``` bash
<script src="//lib.sinaapp.com/js/jquery/2.0.3/jquery-2.0.3.min.js"></script>
```
*这里用到的是[新浪的JQuery CDN](http://lib.sinaapp.com/?path=/jquery)*
###本地图片路径问题
在github资源库根目录source下新建文件夹images，将需要的图片拷贝进来即可，在.md文件中使用http://username.github.io/images/XXX.jpg引用即可。
###为hexo添加评论模块
国内用的比较多的是[多说](http://duoshuo.com/)，点击登录，输入必要的信息后，获得通用评论代码如下：
``` bash
<!-- 多说评论框 start -->
	<div class="ds-thread" data-thread-key="请将此处替换成文章在你的站点中的ID" data-title="请替换成文章的标题" data-url="请替换成文章的网址"></div>
<!-- 多说评论框 end -->
<!-- 多说公共JS代码 start (一个网页只需插入一次) -->
<script type="text/javascript">
var duoshuoQuery = {short_name:"username"};
	(function() {
		var ds = document.createElement('script');
		ds.type = 'text/javascript';ds.async = true;
		ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.js';
		ds.charset = 'UTF-8';
		(document.getElementsByTagName('head')[0] 
		 || document.getElementsByTagName('body')[0]).appendChild(ds);
	})();
	</script>
<!-- 多说公共JS代码 end -->
```
*将 data-thread-key="请将此处替换成文章在你的站点中的ID" 修改为data-thread-key="<%= post.path %>"*
*将 data-thread-key="请替换成文章的标题" 修改为data-thread-key="<%= post.title %>"*
*将 data-thread-key="请替换成文章的网址" 修改为data-thread-key="<%= post.permalink %>"*
*将 short_name:"username"修改为你github的名字*

- hexo新版本(2.4.5及以上)中已经把comment.ejs合并到article.ejs中了
- 打开github资源库主目录\themes\landscape\layout\_partial\article.ejs
- 将下面代码的&&config.disqus_shortname删除并将section内部替换为多说的公用代码即可：
``` bash
<% if (!index && post.comments && config.disqus_shortname){ %>
<section id="comments">

</section>
<% } %>
```
添加评论的全部代码为：
``` bash
<% if (!index && post.comments){ %>
<section id="comments">
  <!-- 多说评论框 start -->
  <div class="ds-thread" data-thread-key="<%= post.path %>" data-title="<%= post.title %>" data-url="<%= post.permalink %>"></div>
<!-- 多说评论框 end -->
<!-- 多说公共JS代码 start (一个网页只需插入一次) -->
<script type="text/javascript">
var duoshuoQuery = {short_name:"username"};
  (function() {
    var ds = document.createElement('script');
    ds.type = 'text/javascript';ds.async = true;
    ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.js';
    ds.charset = 'UTF-8';
    (document.getElementsByTagName('head')[0] 
     || document.getElementsByTagName('body')[0]).appendChild(ds);
  })();
  </script>
<!-- 多说公共JS代码 end -->
</section>
<% } %>
```
- 打开Git Shell，重新生成静态文件并发布
``` bash
hexo generate
hexo deploy
```
*重新运行记得清除缓存,ctrl+shift+del*
*记得删除if判断中的&&config.disqus_shortname，否则评论框无法展现*
