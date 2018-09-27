---
title: hexo部署在Github Page流程
date: 2018-09-24 17:13:53
tags:
- Hexo
- 博客
---
## 1.简介

是一个快速, 简洁且高效的博客框架. 让上百个页面在几秒内瞬间完成渲染. Hexo支持Github Flavored Markdown的所有功能, 并自己也拥有强大的插件系统.
Hexo(中文官方网站):https://hexo.io/zh-cn/

## 2.安装Hexo

详细教程：https://www.cnblogs.com/visugar/p/6821777.html

## 3.生成博客网站文件

这部分是我主要想讲的部分。
假设你想要一个名为Blog的博客文件夹，并把所有相关文件和博文放在里面，
windows在文件夹下进入控制台，执行一下命令：
`hexo init Blog`
`cd Blog`
`hexo g`
得到以下文件:
[![生成配置文件.png](https://i.postimg.cc/59857TX8/image.png)](https://postimg.cc/c6486DMJ)

然后执行
`hexo s -p 4999`
将服务器部署到本地，端口为4999（可更改）
从浏览器进入http://localhost:4999
可预览默认博客
[![Blog.png](https://i.postimg.cc/mrVJHhs2/Blog.png)](https://postimg.cc/McM9k6Hh)
hexo已经为我们准备了第一篇博文
用文本编辑器修改_config.yml可以设置标题等。
然后在Blog下执行：
`hexo new "第一篇博文"`
hexo会在Blog\source\_posts下生成“第一篇博文.md”文件
用你喜欢的文本编辑工具安Markdown语法撰写博文即可

## 4.配置_congfig.yml，并部署到github上
要部署到github上，首先你要有一个github账号
然后按xxx.github.io命名格式新建一个仓库(xxx是你想要的名字)
然后github会默认开启github page,
通过http://xxx.github.io即可访问github给你的默认博客。嗯，一个空的网站。
然后配置Blog中的_congfig.yml文件，
[![config.png](https://i.postimg.cc/8kykSPfG/config.png)](https://postimg.cc/XXC6KWg1)
注意冒号后面有一个空格。
配置好后在Blog下
`hexo g`
`hexo deploy`
[![github.png](https://i.postimg.cc/vZnGfrq8/github.png)](https://postimg.cc/pm2NtFM3)
就会自动推送到git仓库上了，此时进入http://xxx.github.io就可以访问到你的博客了。
以后发布博文的顺序为
1. `hexo new "新的博文"`
2. 编写博文
3. `hexo s -p 4999`
4. 打开http://localhost:4999预览博文
5. `hexo g`
6. `hexo deploy`

就完成了一篇博客的编写和发布