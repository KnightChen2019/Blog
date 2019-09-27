---
title: 基于Github+hexo+matery搭建个人博客
top: false
cover: false
toc: true
mathjax: false
date: 2019-08-09 17:39:41
password:
summary: 第一次搭建了个博客，所以做个记录
tags:
  - 博客教程
categories: 软件安装与配置
---
#### 前置条件

1. 已经安装了Node.js
2. 已经安装了Git
3. 已经有了GitHub的账号
4. 已经在GitHub上配置好了SSH Key

#### 安装过程
1. 在GitHub上新建一个仓库，仓库的名字为username.github.io，这里的username一定要是你github账号的名字。我开始因为起了个别的名字，导致上传到GitHub之后，在浏览器中输入username.github.io，一直显示404
2. 在本地创建同名的文件夹username.github.io，然后在此文件夹上右键Git Bash Here.
3. 执行下列命令
> hexo init #初始化hexo
> npm install #安装依赖包
> npm install hexo-deployer-git --save #确保git部署
此时，你便完成了hexo博客的搭建，并且自带了默认的主题landspace
4. 执行下列编译命令
> hexo g
5. 启动service
> hexo s #便于本地调试
然后在浏览器中输入http://localhost:4000即可查看，默认的主题landsapce里还有一篇自带的文章： Hello World
6. 你可以更换不同的主题，比如下载量很高的nextT：
> [nextT](https://github.com/theme-next/hexo-theme-next)
> 
>或者我用的matery主题：
> [matery](https://github.com/blinkfox/hexo-theme-matery/blob/develop/README_CN.md)

下载的方法上面的post里都有。
7. 有2个重要的配置文件_config.yml,分别是hexo的系统配置文件和各个主题所特有的配置文件，我把我的系统配置文件贴出来供参考：
>D:\GitHubRepository\KnightChen2019.github.io\_config.yml
>
>#Hexo Configuration
>##Docs: https://hexo.io/docs/configuration.html
>##Source: https://github.com/hexojs/hexo/
>
>#Site
>title: 星辰白衣
>subtitle: 星辰白衣的博客
>description: 放弃很容易，但坚持一定很酷！
>keywords:
>author: 星辰白衣
>language: zh-CN
>timezone: Asia/Shanghai
>
>#URL
>##If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
>url: http://knightchen.com
>root: /
>permalink: :year/:month/:day/:title/
>permalink_defaults:
>
>#Directory
>source_dir: source
>public_dir: public
>tag_dir: tags
>archive_dir: archives
>category_dir: categories
>code_dir: downloads/code
>i18n_dir: :lang
>skip_render:
>
>#Writing
>new_post_name: :title.md #File name of new posts
>default_layout: post
>titlecase: false #Transform title into titlecase
>external_link: true #Open external links in new tab
>filename_case: 0
>render_drafts: false
>post_asset_folder: true
>relative_link: false
>future: true
>highlight:
>  enable: true
>  line_number: true
>  auto_detect: false
>  tab_replace:
>  
>prism_plugin:
>  mode: 'preprocess'    #realtime/preprocess
>  theme: 'tomorrow'
>  line_number: false    #default false
>  custom_css:
>  
>#Home page setting
>#path: Root path for your blogs index page. (default = '')
>#per_page: Posts displayed per page. (0 = disable pagination)
>#order_by: Posts order. (Order by date descending by default)
>index_generator:
>  path: ''
>  per_page: 12
>  order_by: -date
>  
>#Category & Tag
>default_category: uncategorized
>category_map:
>tag_map:
>
>#Date / Time format
>##Hexo uses Moment.js to parse and display date
>##You can customize the date format as defined in
>##http://momentjs.com/docs/#/displaying/format/
>date_format: YYYY-MM-DD
>time_format: HH:mm:ss
>
>#Pagination
>##Set per_page to 0 to disable pagination
>per_page: 12
>pagination_dir: page
>
>#Extensions
>##Plugins: https://hexo.io/plugins/
>##Themes: https://hexo.io/themes/
>theme: matery
>
>#Deployment
>##Docs: https://hexo.io/docs/deployment.html
>deploy:
>  type: git
>  repository: git@github.com:KnightChen2019/KnightChen2019.github.io.git
>  branch: master
>  
>#New add search function
>#npm install hexo-generator-search --save
>search:
>  path: search.xml
>  field: post
>  
>#support chinese pinyin link
>#npm i hexo-permalink-pinyin --save
>permalink_pinyin:
>  enable: true
>  separator: '-' #default: '-'
>  
>#Support RSS
>#npm install hexo-generator-feed --save
>feed:
>  type: atom
>  path: atom.xml
>  limit: 20
>  hub:
>  content:
>  content_limit: 140
>  content_limit_delim: ' '
>  order_by: -date

8. 把内容推送到GitHub上，执行下列命令即可：
> hexo d

9. 写博客，在本地执行：
> hexo new 'post'

会在D:\GitHubRepository\KnightChen2019.github.io\source\_posts目录里生成一个post.md的文件，你在里面创作即可。

10. 每次有任何改动都需要执行
> hexo g
> hexo s #optional, 本地调试用
> hexo d

11. 买域名。目前为止，我们可以通过
username.github.io去访问自己的博客了，但是这个名字太长，有没有酷一点的名字呢？当然有，买个域名即可，我在腾讯云上买了个5年期的域名knightchen.com，290.
12. 绑定域名，在自己的腾讯云的域名服务中，选择刚买的域名，然后添加如下的3条域名解析：
![域名解析](DomainName.jpg)
同时在本地博客的source目录下新建一个CNAME文件，把你的域名填加进去。
13. 添加评论配置模块，matery内置了许多评论模块，
Gitalk、Gitment、Valine 和 Disqus等。我选用了Gitalk, 在主题的配置模块中启用此功能即可。
> D:\GitHubRepository\KnightChen2019.github.io\themes\matery\_config.yml
> 
>#the Gitalk config，default disabled
>#Gitalk 评论模块的配置，默认为不激活
>gitalk:
>  enable: true
>  owner: KnightChen2019
>  repo: KnightChen2019.github.io
>  oauth:
>    clientId: f0ad985XXXXXXXXXXXX
>    clientSecret: e69425e9629fbaf678XXXXXXXXXXXX
>  admin: KnightChen2019

上图中的clientId和clientSecret信息需要我们在下面的网站创建一个:

[OAuth application](https://github.com/settings/applications/new)

至此，初步的blog已经创建成功了,剩下的就看你的精雕细琢了。

#### Reference
1. 搭建途中，参考了下面这位同学的教程：
[韦阳的博客](https://godweiyang.com/2018/04/13/hexo-blog/#toc-heading-26)
2. Hexo的官方文档：
[Hexo Doc](https://hexo.io/zh-cn/docs/)
3. matery在GitHub上的ReadMe：
[hexo-theme-matery](https://github.com/blinkfox/hexo-theme-matery/blob/develop/README_CN.md)

#### Issue
1. 本地图片无法显示。
* 重新安装插件hexo-asset-image
>npm install https://github.com/CodeFalling/hexo-asset-image --save
* 执行命令
> hexo clean #清除public folder和database
> hexo g
> hexo s
* 此时引用图片的格式为：
> ...(test/a.jpg)
> ...(a.jpg)


