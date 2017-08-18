---
title: 使用jekyll的NexT搭建GitHub博客
date: 2017-8-8 6:16:29 PM 
categories:
- Blog
tags: Blog
layout: post
---
#使用jekyll搭建GitHub博客

##1.[设置Git](https://help.github.com/articles/set-up-git/)

- [下载Git](https://git-scm.com/downloads)并安装
- 打开Git Bash
- 设置用户名

	`$ git config --global user.name "Mona Lisa"`

	`$ git config --global user.name`

	`> Mona Lisa`

- 设置邮件 

	`$ git config --global user.email "email@example.com"`
	
	`$ git config --global user.email`

	`email@example.com`
	

##2.[安装Ruby](https://www.ruby-lang.org/en/downloads/)

Ruby，一种简单快捷的面向对象（面向对象程序设计）脚本语言，安装Jekyll需要电脑上安装Ruby，以下是安装步骤：

window系统下，可以使用[RubyInstaller](http://rubyinstaller.org/downloads/)来安装ruby环境，建议下载2.3以上的新版。

下载RubyInstaller之后，双击文件，启动 Ruby 安装向导点击next，向导完成安装，记得勾选 Add Ruby executables to your PATH，直到 Ruby 安装程序完成 Ruby 安装为止。

安装后，在cmd中输入ruby -v和gem -v来看看是否安装成功，看到版本号就说明成功。

> 注：用RubyInstaller安装Ruby之后都附带有Gems，如有需要可以单独下载RubyGems。网址为：https://rubygems.org/pages/download

##3.[使用Jekyll在本地设置您的GitHub页面站点](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/)

jekyll是一个简单的免费的Blog生成工具，类似WordPress。但是和WordPress又有很大的不同，原因是jekyll只是一个生成静态网页的工具，不需要数据库支持。但是可以配合第三方服务，例如Disqus。最关键的是jekyll可以免费部署在Github上，而且可以绑定自己的域名。（注：我自己的没有绑定域名）

- 使用gem来安装jekyll，在命令行中输入

	`$ gem install jekyll`

	所有的jekyll的gem依赖包都会被自动安装。

- 使用gem来安装bundle，在命令行中输入

	`$ gem install bundler`


>[NexT主题](http://theme-next.simpleyyt.com/)

>[GitHub帮助页面](https://help.github.com/)