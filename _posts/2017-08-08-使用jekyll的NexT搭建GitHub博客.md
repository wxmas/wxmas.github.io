---
title: 使用jekyll的NexT搭建GitHub博客
date: 2017-08-08 6:16:29 PM 
categories:
- Blog
tags: Blog
layout: post
---

## 安装设置Git ##

- [下载Git](https://git-scm.com/downloads)并安装
- 打开Git Bash

	>[github设置Git帮助页面](https://help.github.com/articles/set-up-git/) 

- 设置用户名


	```
	$ git config --global user.name "Mona Lisa"
	$ git config --global user.name
	\> Mona Lisa
	```

	```
	$ git config --global user.name "Mona Lisa"
	$ git config --global user.name
	> Mona Lisa
	```

{% highlight git linenos %}
$ git config --global user.name "Mona Lisa"
$ git config --global user.name
\> Mona Lisa
{% endhighlight %}

{% highlight git linenos %}
$ git config --global user.name "Mona Lisa"
$ git config --global user.name
> Mona Lisa
{% endhighlight %}

- 设置邮件 

	```git
	$ git config --global user.email "email@example.com"
	```

## 安装Ruby ##

Ruby，一种简单快捷的面向对象（面向对象程序设计）脚本语言，安装Jekyll需要电脑上安装Ruby，以下是安装步骤：

window系统下，可以使用[RubyInstaller](http://rubyinstaller.org/downloads/)来安装ruby环境，建议下载2.3以上的新版。

下载RubyInstaller之后，双击文件，启动 Ruby 安装向导点击next，向导完成安装，记得勾选 Add Ruby executables to your PATH，直到 Ruby 安装程序完成 Ruby 安装为止。

安装后，在cmd中输入ruby -v和gem -v来看看是否安装成功，看到版本号就说明成功。

> 注：用RubyInstaller安装Ruby之后都附带有Gems，如有需要可以单独下载RubyGems。网址为：https://rubygems.org/pages/download

## 安装jekyll ##

[jekyll](http://jekyllcn.com/)是一个简单的免费的Blog生成工具，类似WordPress。但是和WordPress又有很大的不同，原因是jekyll只是一个生成静态网页的工具，不需要数据库支持。但是可以配合第三方服务，例如Disqus。最关键的是jekyll可以免费部署在Github上，而且可以绑定自己的域名。（注：我自己的没有绑定域名）

>[使用Jekyll在本地设置您的GitHub页面站点](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/)

- 使用gem来安装jekyll，在命令行中输入

	`$ gem install jekyll`

	所有的jekyll的gem依赖包都会被自动安装。

- 使用gem来安装bundle，在命令行中输入

	`$ gem install bundler`

## 博客主题套用 ##

- 从Github上用git把刚刚生成好的yourusername.github.io项目clone下来，假设放在一个叫yourusername.github.io的目录中。
- 进入yourusername.github.io项目目录中，将 除了`.git`之外的其他文件夹全删了。
- 将准备好的jekyll主题（如[NexT主题](http://theme-next.simpleyyt.com/)）模板文件拷贝到本地项目目录中。
- 然后用GitBash执行如下命令将本地项目push到GitHub项目中。

	```git
	$ git add .
	$ git commit -m "some simple notes"
	git push origin master
	```

- 刷新访问yourusername.github.io，即可得到套用了主题的个人主页。

	>NexT主题使用jekyll本地生成静态页面头一回调通了，后来重调时出现了这个问题：
	>```jekyll 
	$ bundle exec jekyll server
	Configuration file: D:/wxmas/wxmas.github.io/_config.yml
       Deprecation: The 'gems' configuration option has been renamed to 'plugins'. Please update your config file accordingly.
            Source: D:/wxmas/wxmas.github.io
       Destination: D:/wxmas/wxmas.github.io/_site
	Incremental build: disabled. Enable with --incremental
      Generating...
	GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.
	GitHub Metadata: Error processing value 'description':
	Liquid Exception: No repo name found. Specify using PAGES_REPO_NWO environment variables, 'repository' in your configuration, or set up an 'origin' git remote pointing to your github.com repository. in /_layouts/post.html
             ERROR: YOUR SITE COULD NOT BE BUILT:
                    ------------------------------------
                    No repo name found. Specify using PAGES_REPO_NWO environment variables, 'repository' in your configuration, or set up an 'origin' git remote pointing to your github.com repository.
	```


## 博文添加 ##

- 所有文章以markdown格式文件，保存在_posts/目录下。
- 文件名称格式: 年-月-日-标题.MARKUP（例如: 2017-01-01-hello-world.md）。
- 要有「头信息」。
	
	```markdown
	---
	layout: post
	title:  "Welcome to Jekyll!"
	date:   2015-11-17 16:16:01 -0600
	categories: 
	- jekyll update
	- jekyll
	tags:
	- Blog
	---
	You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `bundle exec jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

	To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

	````

	即使都不写，也至少必须填上
	
	```markdown
	---
	---
	````

## Over ##

>[GitHub帮助页面](https://help.github.com/)