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

[Git 系列之二：Windows 下 Git 客户端的选择，及 msysGit 各种中文问题的解决
](http://wangcongming.info/2010/07/windows-%E4%B8%8B-git-%E5%AE%A2%E6%88%B7%E7%AB%AF%E7%9A%84%E9%80%89%E6%8B%A9%EF%BC%8C%E5%8F%8A-msysgit-%E5%90%84%E7%A7%8D%E4%B8%AD%E6%96%87%E9%97%AE%E9%A2%98%E7%9A%84%E8%A7%A3%E5%86%B3/)



##### TortoiseGit
###### [下载](https://code.google.com/p/tortoisegit/)

* 以Msysgit为后端，所以需要设置`git.exe`的路径
* ssh的关联需要用到`Pagent`和`Puttygen`

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

可能会出现如下提示
    'My octopress page is coming soon
    'hellip' 不是內部或外部命令...

中间可能会在My Octopress Page is coming soon之后出现hellip;不是内部命令之类的
错误, 可以不用管, 如果一定不想要出现这个错误可以修改myoctopress目录下的
Rakefile, 搜My Octopress Page is coming soon, 在&hellip;前加个^(这个是Windows
cmd的转义符), 如下
    system "echo 'My Octopress Page is coming soon ^&hellip;' > index.html"

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
    Agent admitted failure to sign using the key. Permission denied (publickey)
    ...
    fatal: The remote end hung up unexpectedly

ssh 配置不对，打开git bash，重新生成ssh key,拷贝到`C:\Documents and Settings\pythonee\.ssh`


#### 中文问题
通过设置环境变量`LANG`和`LC_ALL`，具体为:
    set LC_ALL = zh_CN.UTF-8
    set LANG=zh_CN.UTF-8

还有可能出现
    convertible.rb:29:in `read_yaml': invalid byte sequence in GBK (ArgumentError)
这个问题需要修改jekyll的convertiable.rb文件，具体路径在`ruby\gems\1.9.1\gems\jekyll-0.11.2\lib\jekyll`

#### rake版本错误
    rake aborted!
    You have already activated rake 0.9.2.2, but your Gemfile requires rake 0.9.2. U
    sing bundle exec may solve this.
    (See full trace by running task with --trace)

一种解决方案是将Gemfile里的改成`gem 'rake', '0.9.2.2'`，并重新执行`bundle install`
；另一种方案可以执行`bundle exec rake`替代`rake`命令。
    
#### _config.yml配置中，冒号后面没有空格，语法错误。 
    psych. Rb: 148: in ` parse ': peasants' t parse YAML at line 12 column 0 (Psych: : SyntaxError) 

#### Markdown语法
[参考一](http://wowubuntu.com/markdown/)


### 参考资料
* [搭Blog 学Git](http://shanewfx.github.com/blog/2012/02/16/bulid-blog-by-octopress/)
* [Octopress 笔记](http://netwjx.github.com/blog/2012/03/18/octopress-note/)
* [Windows 上使用 Github 手记](http://interjc.net/archives/2011/09/05/using-github-on-windows.html)
* [使用Github Pages建独立博客](http://beiyuu.com/github-pages/)
* [一些发布脚本](http://hopes4.me/blog/post-octopress-via-sh-script/)

