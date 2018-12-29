title: Hexo + githug-pages 搭建博客完全教程
date: 2018-11-01 19:51:54
categories: Hexo
tags: [Hexo, github-pages]
---
### 安装hexo

#### 环境依赖

软件环境
- nodejs
- Git

#### 安装插件及目录准备
```
$ mkdir hexo  #创建一个文件夹
$ cd hexo
$ npm install -g hexo-cli
```

#### 初始化Hexo
在Git shell中输入
```
$ hexo init
```

#### 安装Hexo常用插件
```
$ npm install hexo --save
$ npm install hexo-generator-index --save
$ npm install hexo-generator-archive --save
$ npm install hexo-generator-category --save
$ npm install hexo-generator-tag --save
$ npm install hexo-server --save
$ npm install hexo-deployer-git --save
$ npm install hexo-deployer-heroku --save
$ npm install hexo-deployer-rsync --save
$ npm install hexo-deployer-openshift --save
$ npm install hexo-renderer-marked@0.2 --save
$ npm install hexo-renderer-stylus@0.2 --save
$ npm install hexo-generator-feed@1 --save
$ npm install hexo-generator-sitemap@1 --save
$ npm install hexo-wordcount --save  # 统计文章字数
$ npm install hexo-admin --save  # 管理工具
$ npm install hexo-generator-searchdb --save  # 站内全局搜索
```

### Hexo常用命令
#### init
新建一个网站。如果没有设置`folder`，Hexo 默认在目前的文件夹建立网站。
```
$ hexo init [folder]
```
> 注：folder必须为空。

#### new
执行`new`命令，生成指定名称的文章至`hexo\source_posts\postName.md`。
```
$ hexo new [layout] "postName" #新建文章
# 或者
$ hexo n "postName"
```
新建一篇文章。如果没有设置 layout 的话，默认使用 `_config.yml` 中的 `default_layout`参数代替。如果标题包含空格的话，请使用引号括起来。有哪些`layout`呢，可以到`scaffolds`目录下查看，这些文件名称就是`layout`名称。

想要给每篇文章添加分类,在`hexo/scaffolds/post.md`模板中添加`categories`
```
---
title: {{ title }}
date: {{ date }}
categories: 
tags: 
---
```

#### generate
```
$ hexo generate
# or
$ hexo g
```
生成静态文件,参数如下：

选项|描述
-|-
\-d, \-\-deploy| 文件生成后立即部署网站
\-w, \-\-watch | 监视文件变动

#### server
```
$ hexo server
# or
$ hexo s
```
启动服务器。默认情况下，访问网址为： `http://localhost:4000/`。

#### deploy
```
$ hexo deploy
# or
$ hexo d
```
部署网站。

#### clean
```
$ hexo clean
```
清除缓存文件`db.json`和已生成的静态文件`public`。

在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令。

> 更多命令见[官方文档](https://hexo.io/zh-cn/docs/commands)


### 主题

#### 下载主题
```
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```
#### 应用主题
在`hexo`目录下找到`_config.yml`配置文件，找到`theme`字段，并将其值更改为 `next`，如下所示：
```
theme: next
```
#### 设置RSS
在上面的步骤中已经安装了`Rss`插件，只要要在`hexo/themes/next/_config.yml`配置文件中添加如下一行即可：
```
rss：
```
#### 添加标签tags页面
使用`hexo new page`新建一个页面，命名为`tags`，布局格式为`page`，生成配置文件路径`hexo/source/tags/index.md`
```
$ hexo new page tags
```
内容如下所示，如果要关闭tags页面的评论可以设置`comments`为`false`：
```
---
title: tags
date: 2018-11-01 11:00:07
type: "tags"
layout: "tags"
comments: false
---
```
> 同理添加`categories`


### 第三方插件
#### 访问统计
找到next主题的配置文件`hexo/themes/next/_config.yml`，修改`busuanzi`相关配置项，即：
```
busuanzi_count:
  # count values only if the other configs are false
  enable: true
  # custom uv span for the whole site
  site_uv: true
  site_uv_header: <i class="fa fa-user"></i> 访问人数
  site_uv_footer:
  # custom pv span for the whole site
  site_pv: true
  site_pv_header: <i class="fa fa-eye"></i> 访问总量
  site_pv_footer: 次
  # custom pv span for one page only
  page_pv: true
  page_pv_header: <i class="fa fa-file-o"></i> 浏览
  page_pv_footer: 次
```

由于`busuanzi`之前的域名过期了，所以我们需要修改的模板文件是`hexo/themes/next/layout/_third-party/analytics/busuanzi-counter.swig`，修改引入`script`行`js`地址：
```
<script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
```
> 统计插件：[不蒜子](http://busuanzi.ibruce.info/)

#### 站内全局搜索
安装插件

```
$ npm install hexo-generator-searchdb --save
```

修改 站点配置文件`hexo/_config.yml`,添加:

```
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```

修改 主题配置文件`hexo/themes/next/_config.yml`

```
local_search:
  enable: true
```

#### 开启评论
试了下就`valine`好用点(`gitment`坑太多。。)，且目前next主题中已经集成了`valine`

- 在[leancloud官网](https://leancloud.cn/) 注册账号
- 创建一个应用
- 进入应用->设置->应用key 找到appid 和 appkey
- 修改主题配置`hexo/themes/next/_config.yml`，如下：
```
valine:
  enable: true
  appid: {你的appid} # your leancloud application appid
  appkey: {你的appkey} # your leancloud application appkey
  notify: false # mail notifier , https://github.com/xCss/Valine/wiki
  verify: false # Verification code
  placeholder: Just go go # comment box placeholder
  avatar: mm # gravatar style
  guest_info: nick,mail,link # custom comment header
  pageSize: 10 # pagination size
```
- 进入安全中心 -> Web 安全域名 中添加你的web地址
- 大功告成


### 部署到github-pages
- 在`github`上创建一个新的`repo`，名称为`{你的github用户名}.github.io`，例：我的github地址为`https://github.com/dengshaochun`则我的repo名为`dengshaochun.github.io`
- 添加本地主机公钥到`github`账户，参考 [github官方教程](https://help.github.com/articles/connecting-to-github-with-ssh/)
- 修改`hexo/_config.yml`配置文件`deploy`段，如下

```
deploy:
  type: git
  repository: git@github.com:dengshaochun/dengshaochun.github.io.git
```

> 注意：需要使用`ssh`方式，`https`方式需要密码。

### 参考文档
> [Hexo 官方文档](https://hexo.io/zh-cn/docs/setup)

> [next 主题官方教程](http://theme-next.iissnan.com/theme-settings.html)