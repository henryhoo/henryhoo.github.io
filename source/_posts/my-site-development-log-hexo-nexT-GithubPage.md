---
title: Personal Blog based on Hexo + NexT
date: 2018-04-23T11:05:07.000Z
tags:
  - hexo
  - nextT
categories:
  - UI
---
I played with hexo to build my first personal website on 2016. Then it was untouched for a long time until today, I decided to pick things up and try to learn some UI staff with it.

This page is mainly for recording what I learnt and and how I used them on this site including 3rd party plugins, theme modification and SEO.
<!-- more -->
## Add plugins
### Gitment comments
Gitment is a comment system based on GitHub Issues, which can be used in the frontend without any server-side implementation. Gitment plugin is supported in current NexT(v5.1.4), just need a few steps to set up:
#### Create OAuth application
  1. Go to settings of your github account
  2. Go to Developer settings
  3. Click Register a new application
  4. Fill in:
  ```
  Application name：Gitment
  Homepage URL：<your website link>
  Application description：<Anything>
  Authorization callback URL：<your website link>
  ```
  5. Copy `Client ID` and `Client Secret` for future use

#### Create repository for gitment
  Create a new repository named `gitment-comments`

#### Change NexT config
 Search gitment in config.yml
```
gitment:
  enable: true
  mint: true # RECOMMEND, A mint on Gitment, to support count, language and proxy_gateway
  count: true # Show comments count in post meta area
  lazy: false # Comments lazy loading with a button
  cleanly: false # Hide 'Powered by ...' on footer, and more
  language: # Force language, or auto switch by theme
  github_user: # MUST HAVE, Your Github ID
  github_repo: `gitment-comments` # MUST HAVE, The repo you use to store Gitment comments
  client_id: `Client ID` # MUST HAVE, Github client id for the Gitment
  client_secret: `Client Secret` # EITHER this or proxy_gateway, Github access secret token for the Gitment
  proxy_gateway: # Address of api proxy, See: https://github.com/aimingoo/intersect
  redirect_protocol: # Protocol of redirect_uri with force_redirect_protocol when mint enabled
```
### Add various plugins based on [hexo document](http://theme-next.iissnan.com/third-party-services.html)
  1. baidu analytics
  2. addThis share
  3. leancloud_visitors <span id = "leancloud_visitors"></span>
  4. Local Search

## Customize page

### CMS admin page based on Netlify

#### Why Netlify
Notice that this means I will start to host my site on Netlify instead of GitHub page, here is a good explanation of [why we want to switch to Netlify](https://yihui.name/en/2017/06/netlify-instead-of-github-pages/)

#### How
Given the needs to edit my page for a eaiser flow (for non coder to add things without command line), I followed completely on [this guide](https://medium.com/netlify/adding-netlify-cms-and-redirects-to-hexo-site-the-missing-pieces-c69a8ec053d1) to add a *Netlify cms page* for my personal page.

#### One more thing
Previous guide is baed on default theme called “Material”, however I am using hexo's "NexT" theme. This theme have a better support on customize head, so in order to "Add Netlify Identity Widget", we can just modify `_config` file to have a `custom_file_path `, for [example](https://github.com/henryhoo/henryhoo.github.io/commit/0a4c1b166feba74eaf72ef6b66b9aab8a22464f8), I added `source/_data/head.swig` as a custom head.

### TopX: Hottest post page (**Need Leancloud support**)
Reference：[5.8 添加 TopX 页面](https://reuixiy.github.io/technology/computer/computer-aided-art/2017/06/09/hexo-next-optimization.html)
Basically it create a new page and rearranged post based on views from Leancloud's database we created when we add [leancloud_visitors](#leancloud_visitors) plugin.
1. Create new page
```
hexo new page "top"
```

2. Add menu in theme configuration
```
menu:
  top: /top/ || signal
```

3. Modify the new index.md for "top" page:
```
---
title: TopX
comments: false
keywords: top,文章阅读量排行榜
---
<div id="top"></div>
<script src="https://cdn1.lncld.net/static/js/av-core-mini-0.6.4.js"></script>
<script>AV.initialize("app_id", "app_key");</script>
<script type="text/javascript">
  var time=0
  var title=""
  var url=""
  var query = new AV.Query('Counter');
  query.notEqualTo('id',0);
  query.descending('time');
  query.limit(1000);
  query.find().then(function (todo) {
    for (var i=0;i<1000;i++){
      var result=todo[i].attributes;
      time=result.time;
      title=result.title;
      url=result.url;
      var content="<a href='"+"https://reuixiy.github.io"+url+"'>"+title+"</a>"+"<br />"+"<font color='#555'>"+"阅读次数："+time+"</font>"+"<br /><br />";
      document.getElementById("top").innerHTML+=content
    }
  }, function (error) {
    console.log("error");
  });
</script>
```

4. Replace variables:

    * Change `app_id` and `app_key` in `index.md` for your own leancloud id.

    * Also you can change TopX's X by changing this line: `query.limit(1000);`

## Learning source
### Hexo
* [Hexo搭建博客教程](https://thief.one/2017/03/03/Hexo搭建博客教程/)
* [Hexo+NexT 博客搭建相册](https://lovexinforever.github.io/2017/09/18/Hexo-NexT-博客搭建相册-二/)
* [打造个性超赞博客Hexo+NexT+GithubPages的超深度优化](https://reuixiy.github.io/technology/computer/computer-aided-art/2017/06/09/hexo-next-optimization.html)
* [hexo的next主题个性化配置教程](http://shenzekun.cn/hexo的next主题个性化配置教程.html)
* [hexo教程](https://www.dingxuewen.com/categories/Site/)

### Version control
* [Using Git Submodules to Manage Your Custom Hexo Theme](http://jr0cket.co.uk/hexo/using-git-submodules-for-custom-hexo-theme.html)

### SEO
* [hexo高阶教程：想让你的博客被更多的人在搜索引擎中搜到吗](https://blog.csdn.net/sunshine940326/article/details/70936988/)
* [动动手指，不限于NexT主题的Hexo优化（SEO篇](http://www.arao.me/2015/hexo-next-theme-optimize-seo/)
* [Hexo Seo优化让你的博客在google搜索排名第一](https://www.jianshu.com/p/86557c34b671)

## Cautions
### Config override
We should be careful with `config override`. If you want to override one item from a section, you should include other items of that section. For example, once I forget to declare other sections except "display" for sidebar config:
```
  sidebar:
    display: always
```
This cause the theme config is missing other sections including "offset" which is critical to sidebar affix. So I have a [wrongly placed sidebar](https://github.com/theme-next/hexo-theme-next/issues/328). I should change to following config if I want to use sidebar:
```
  sidebar:
    position: left
    display: always
    offset: 12
    b2t: false
    scrollpercent: false
    onmobile: false
```

## To do
- [ ] add timeline on about page
- [ ] Add Chinese post page
- [ ] Improve config override, it cause pain in the ass!
- [x] Add SEO (Baidu + Google)
- [x] Add TopX page
- [ ] Add photo of post and correct photo links
- [x] Add admin page
- [ ] Add idea book
