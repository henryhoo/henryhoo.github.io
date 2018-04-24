---
title: My site development log
date: 2018-04-23 11:05:07
tags:
- hexo
- nextT
categories: UI
---
I played with hexo to build my first personal website on 2016. Then it was untouched for a long time until today, I decided to pick things up and try to learn some UI staff with it. This page is mainly for log what I learn and do with my personal site.
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

## Learning source
### Hexo
[Hexo搭建博客教程](https://thief.one/2017/03/03/Hexo搭建博客教程/)
[Hexo+NexT 博客搭建相册](https://lovexinforever.github.io/2017/09/18/Hexo-NexT-博客搭建相册-二/)
[打造个性超赞博客Hexo+NexT+GithubPages的超深度优化](https://reuixiy.github.io/technology/computer/computer-aided-art/2017/06/09/hexo-next-optimization.html)
### Version control
[Using Git Submodules to Manage Your Custom Hexo Theme](http://jr0cket.co.uk/hexo/using-git-submodules-for-custom-hexo-theme.html)
