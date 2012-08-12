---
layout: post
title: "octpress 如何高亮代码"
date: 2012-08-11 15:34
comments: true
categories: octpress
---

    ``` js Javascript Hello World 
        alert('hello world');
    ```

``` js Javascript Hello World 
    alert('hello world');
```

而默认安装Octopress时对代码高亮还是不支持的, 还需要安装Python, 我没有使用
ActivePython, 而是CPython, 安装好后应该会有c:\Windows\System32\python27.dll.

但是现在还可能会出现Could not open library’.dll’的问题.

具体修改请参考[在Octopress中使用代码高亮](http://netwjx.github.com/blog/2012/04/21/using-code-in-octopress/) 

