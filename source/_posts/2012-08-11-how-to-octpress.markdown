---
layout: post
title: "Windows下如何使用github+octpress来写技术博客"
date: 2012-08-10 23:50
comments: true
categories: tech 
---

### 申请github账号

* 填写注册信息
* 加入public key
* 使用github page功能

github的page分为个人主页和项目主页两种


###　相关软件

#### git的安装
Msysgit、github for windows
在Windows上使用git主要有两种方式`Msysgit+TortoiseGit`,现在github推出了`github for
windows` 之后，我们还可以选择`github for windows`

##### Github for windows
######下载
到[这里](http://windows.github.com/)下载

##### Msysgit
######下载
到[这里](https://code.google.com/p/msysgit/)下载


##### TortoiseGit
###### [下载](https://code.google.com/p/tortoisegit/)

> 以Msysgit为后端，所以需要设置`git.exe`的路径

> ssh的关联需要用到`Pagent`和`Puttygen`


###### 生成ssh key
    ssh-keygen -t rsa -C "pythonee@gmail.com"
###### 配置
将id_rsa拷贝到`C:\Documents and Settings\pythonee\.ssh`

配置账号信息
    $ git config --global user.name "pythonee"
    $ git config --global user.email "pythonee@gmail.com"

然后通过下面的命令来判断是否可以通信
    ssh -vT git@github.com

#### Ruby
##### 下载RubyInstaller
Octpress需要1.9.2版本环境，这里安装Ruby 1.9.3，所以到[这里](http://rubyforge.org/projects/rubyinstaller/ )下载

修改jekyll
    self.content = File.read(File.join(base, name), :encoding => "utf-8")

##### 更新Gem
设置gem source
    gem sources --remove http://rubygems.org
    gem sources -a http://ruby.taobao.org
    get sources -l //查看现有软件源
    gem update --system

##### 下载[Devkit](https://github.com/oneclick/rubyinstaller/downloads/)
    $cd path\to\DevKit
    $ruby dk.rb init
    $ruby dk.rb install
    $gem install rdiscount --platform=ruby

#### Octpress
    git clone git://github.com/imathis/octopress.git octopress 
    cd path\to\Octpress
将octpress下的`Gemfile`的source改为
    source "http://ruby.taobao.org"
安装
    gem install bundler
    bundle install
    rake install
设置
    rake setup_github_pages

当提示输入github url时，请输入
`git@github.com:username/username.github.com.git`
，当然需要替换`username`。

然后这个命令

* 会把myoctopress中原来clone时的代码库origin改名为octopress,并将刚刚输入git地址作为origin的代码库地址.
* 会把当前分支名称从master改为source
* 其它Url的设置还有deploy目录设置等

写博客
    rake new_post["title"]
发布博客
    rake gen_deploy
也可以
    git push --force --progress origin master:master
同步source
    git status
    git add .
    git commit -a -m 'comment'
    git push --progress origin source:source

### 常见问题
#### ssh问题

#### 中文问题
通过设置环境变量`LANG`和`LC_ALL`，具体为:
    set LC_ALL = zh_CN.UTF-8
    set LANG=zh_CN.UTF-8

#### Markdown语法
[参考一](http://wowubuntu.com/markdown/)


### 参考资料
* [搭Blog 学Git](http://shanewfx.github.com/blog/2012/02/16/bulid-blog-by-octopress/)
* [Octopress 笔记](http://netwjx.github.com/blog/2012/03/18/octopress-note/)

