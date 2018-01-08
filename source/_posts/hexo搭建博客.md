---
title: hexo搭建博客
date: 2018-01-08 14:31:42
tags:
    - GitHub
    - hexo
---

教程写的不详细，没碰到的情况一律去官方文档去找吧，或者去提问
[hexo中文文档](https://hexo.io/zh-cn/docs/writing.html)

温馨提示：刷新效果按Ctrl+F5刷新缓存

## 安装node.js以及git，不多说了

## 安装hexo

``` bash
npm install -g hexo
```

## 新建博客

在一个空文件夹，命令行转到这里

``` bash
hexo init
```

或者在folder文件夹中初始化

``` bash
hexo init <folder>
```

## 配置信息

修改在blog根目录下的_config.yml，添加repo地址：

``` bash
deploy:
  type: git
  repo: https://github.com/username/username.github.io.git
  branch: master
```

博客信息_config.yml下自己看着写

``` bash
title: 
subtitle: 
description: 
author: 
language: 
```

记得冒号后面一定要有个空格，坑了我好久

## CNAME文件

因为使用了自己的域名，而hexo每次deploy的时候hexo都会重新生成文件，所以直接加在github是不好使的，这个CNAME文件需要放在 source文件夹根目录下,之后每次生成就会生成在.deploy_git根目录下了

## 初次使用

新建文章

``` bash
hexo new <title>
```

因为是.md文件，所以没用过markdown的同学自己查查markdown文档

生成网页文件

``` bash
hexo generate
```

本地测试,可以在 localhost:4000/ 查看你的博客

``` bash
hexo server
```

为了能够使Hexo部署到GitHub上，需要安装一个插件：

``` bash
npm install hexo-deployer-git --save
```

部署到GitHub
``` bash
hexo deploy
```

之后可以通过浏览器浏览了

## 使用脚本

npm 允许在package.json文件里面，使用scripts字段定义脚本命令

在package.json文件中第一个大括号中添加，注意要加一个逗号

``` bash
"scripts": {
    "test": "hexo clean && hexo g && hexo s",
    "redeploy": "hexo clean && hexo g && hexo d"
}
```

查看当前项目的所有 npm 脚本命令，可以使用

``` bash
npm run
```

运行脚本

```bash
npm run order
```

[原理](http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html)

## 使用其他主题

我使用了next，你们随意。

next的GitHub主页
[hexo-theme-next](https://github.com/theme-next/hexo-theme-next/blob/master/docs/cn/README.md#%E5%8D%B3%E6%97%B6%E9%A2%84%E8%A7%88)，有使用说明以及一些大佬的使用样例，不多说了。

http://theme-next.iissnan.com/getting-started.html

找到_config.yml，把对应的主题目录名改下，把主题克隆到themes就行了


## 布局的讨论

https://hexo.io/zh-cn/docs/writing.html
https://www.jianshu.com/p/5a1e6d8c83af

## 编辑器
我用的sublime，用了markdownpreview和markdownediting，感觉好用，Ctrl+B将.md转为.html，觉得generate麻烦的可以这样预览

## 插入图片

确认_config.yml 中有 post_asset_folder:true
Hexo 提供了一种更方便管理 Asset 的设定：post_asset_folder
当您设置post_asset_folder为true参数后，在建立文件时，Hexo
会自动建立一个与文章同名的文件夹，您可以把与该文章相关的所有资源都放到那个文件夹，如此一来，您便可以更方便的使用资源。

在hexo的目录下执行npm install https://github.com/CodeFalling/hexo-asset-image --save（需要等待一段时间）。

完成安装后用hexo新建文章的时候会发现_posts目录下面会多出一个和文章名字一样的文件夹。图片就可以放在文件夹下面。

只要使用 \!\[logo\]\(titlename/logo.jpg\) 就可以插入图片。其中[]里面不写文字则没有图片标题。

参考https://www.jianshu.com/p/c2ba9533088a

## 添加tag

https://www.zhihu.com/question/29017171

一篇文章多个tag如下设置：

tags: 
\- abc
\- def
\- xxx

## 插件，YAML，ejs

http://code.wileam.com/build-a-hexo-blog-and-optimize/

## 优化和异地管理

Hexo部署到GitHub上的文件，是.md（你的博文）转化之后的.html（静态网页）。因此，当你重装电脑或者想在不同电脑上修改博客时，没有之前的hexo文件，就不可能了。

如果为每一个GitHubPages创建一个额外的仓库来存放Hexo网站文件没有必要，可以简答的利用了分支。

创建一个新的分支（此处叫blog），设置blog为默认分支（因为我们只需要手动管理这个分支上的Hexo网站文件）

使用git clone将仓库拷贝下来，删除.git之外的文件，将之前的hexo文件夹中的内容拷贝进来（简单粗暴）

在package.json中添加脚本

```bash
{
  ...
  "scripts": {
    "test": "hexo clean && hexo g && hexo s",
    "init": "npm install",
    "deploy": "git checkout blog && git add . && git commit && git push origin blog && hexo g && hexo d"
  }
}

```

依次执行git add .、git commit -m “…”、git push origin blog提交网站相关的文件

完成。

注意：_config.yml中的deploy参数，分支应为master

## 博客管理流程

clone博客仓库（已经有就不用了）

转到 blog分支

增删查改

npm run init（如果刚clone下来没有安装依赖，和第一步配套）

npm run deploy（其中会让你输入commit的信息，输入保存退出即可）

```bash
git clone https://github.com/z-hermit/z-hermit.github.io.git
cd z-hermit.github.io
npm run init

。。。

npm run deploy

```

##参考文档

https://www.zhihu.com/question/21193762
http://crazymilk.github.io/2015/12/28/GitHub-Pages-Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/#more

