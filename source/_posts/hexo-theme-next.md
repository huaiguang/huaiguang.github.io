---
title: hexo-theme
keywords: hexo, theme, next
description: hexo搭建博客，主题Next的配置
date: 2019-12-29 18:00:00
tags: hexo themes
categories: hexo
---

# 利用Hexo搭建博客
采用Hexo系统，利用markdown语法来编写博客，适合初步搭建博客的开发人员。从github上下载hexo并安装，有自带的主题landscape，就可以搭建起一个简易博客系统。如果期望做的更多一点，博客网站更好一些，那么可以试试替换下主题，也可以接着看下去。本篇主要讲述下主题：[Next](https://github.com/theme-next/hexo-theme-next.git)的配置。

## 约定
在讲述配置时，可能会遇到不同位置的相同文件名的文件(其实主要是 _config.yml). 故约定如下：
1. 单独的 _config.yml 指根目录下的 _config.yml, 可以配置使用哪种主题
2. themes/_config.yml 指主题下的 _config.yml, 是theme主题的配置

## 主题配置
一般的主题基本上按照 theme/_config.yml 下的注释就可以看懂，以及相关的配置，下面讲述下遇到的问题。

### 易忽略点
Question1: 在Schemes配置中，选择主题 scheme: Gemini 时, 左侧边栏中 categories tags 上面没有链接，不可点击
回答：默认情况下，posts 标签点开时，网站跳转到 /archives/. 同理, 点击标签 categories, tags时，网站跳转的地址分别是 /categories/, /tags/. 默认情况下，系统中不存在相应的页面，需要按照如下操作处理.
1. 在 themes/_config.yml 的 menu 下，放开 categories, tags的注释
2. 按照如下操作，添加 categories, tags 文件夹, 并手动修改其中的index.md
```shell
# 在source下建立 categories 文件夹, 并创建了一个 index.md
node_modules/.bin/hexo new page categories
# 修改 categories/index.md, 在生成的 description 后面手动添加 type: "categories"

# 在source下建立 tags 文件夹, 并创建了一个 index.md
node_modules/.bin/hexo new page tags
# 修改 tags/index.md, 在生成的 description 后面手动添加 type: "tags"
```
3. 重新启动, npm run clean && npm run build && npm run server，左侧边栏中的categories, tags上即附上了链接

## 发布
> 本篇主要讲述在Github上的发布.

在Github上发布有两种思路，一是通过hexo自带的deploy发布，二是通过发布到xxx.github.io设定自动任务发布。
第一种方式比较实用，主要有以下几步。
1. 首先在根目录下安装插件 `npm install hexo-deployer-git --save`.
2. 在 _config.yml上配置deployment,示例如下：
```shell
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  - type: git
    repository: git@github.com:xxx/xxx.github.io.git
    branch: master
  - type: git
    repo: https://gitee.com/lxl80/lxl80.git
    branch: master
    ignore_hidden: false
```
本质上就是在本地通过cli命令 `hexo clean && hexo build && hexo deploy`, 在本地打包，将生成的public文件夹上传到github上。Github Pages 相当于一个静态资源服务器，上传完成后即可通过 https://xxx.github.io/ 来进行访问. 优势是容易迁移，支持一键发布到多个平台，几乎不存在劣势。

第二种方式是采用 travis, 在本地完成了编辑后推送到远程触发travis，travis执行设定的任务，自动打包并发布到指定分支, 完成后Github Pages就会自动更新。主要有以下的步骤：
1. 进入Github的[Settings/Developer settings](https://github.com/settings/tokens)下，注册一个GH_TOKEN.
2. 使用github账号登录[travis](https://travis-ci.org), 待同步项目后，进入相应的项目设置页。
3. 在项目配置中找到 Environment Variables 中添加GH_TOKEN.
4. 在本地添加travis的配置文件 .travis.yml, 里面是推送后的自动化任务配置. 示例如下：
```shell
sudo: false
language: node_js
node_js:
  - 12
cache: npm
# 执行分支
branches:
  only:
    - master

# 推送后执行的命令
script:
  - hexo generate

# 发布
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $travis_token
  keep-history: true
  on:
    all_branches: true
    branch: gh-pages
  local-dir: public
```
5. 在travis-ci.org中执行完毕后, 刷新 xxx.github.io 即可.
与第一种方式相比, 主要是节省了本地资源(虽然大多数并不需要). 时间成本上, 虽然一个在本地一个在线上，但是我们自己都是要等待编辑执行，实际上本地可能更快捷一些。

