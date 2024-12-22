---
layout: post
title:  "Welcome to Jekyll!"
date:   2019-01-05 20:22:45 +0800
categories: software jekyll 
---

References:
* [How to Start a Jekyll Blog on GitHub Pages for Free](https://onextrapixel.com/start-jekyll-blog-github-pages-free/)
* [kramdown Syntax](https://kramdown.gettalong.org/syntax.html)

直接运行，有依赖缺失：

     \>jekyll serve --watch
     Traceback (most recent call last):
          5: from C:/Ruby25-x64/bin/jekyll:23:in `<main>'
          4: from C:/Ruby25-x64/bin/jekyll:23:in `load'
          3: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/jekyll-3.8.5/exe/jekyll:11:in `<top (required)>'
          2: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/jekyll-3.8.5/lib/jekyll/plugin_manager.rb:48:in `require_from_bundler'
          1: from C:/Ruby25-x64/lib/ruby/2.5.0/rubygems/core_ext/kernel_require.rb:59:in `require'
     C:/Ruby25-x64/lib/ruby/2.5.0/rubygems/core_ext/kernel_require.rb:59:in `require': cannot load such file -- bundler (LoadError)

先按提示安装 [Bundler](https://www.bundler.cn/)，有点类似于 pip，用于 gem 依赖管理的工具：

     \>gem install jekyll bundler

再用 `bundle` 安装其它的依赖（[根据Gemfile/Gemfile.lock](https://www.bundler.cn/man/bundle-install.1.html)）：

     \>bundle install

再次运行，仍然报错：

     \>jekyll serve --watch

     Configuration file: D:/Code/yingang.github.io/_config.yml
               Source: D:/Code/yingang.github.io
          Destination: D:/Code/yingang.github.io/_site
     Incremental build: disabled. Enable with --incremental
          Generating...
          Jekyll Feed: Generating feed for posts
     jekyll 3.8.5 | Error:  File exists @ dir_s_mkdir - D:/Code/yingang.github.io/_site/jekyll

换种方式运行（[在 bundle 的上下文执行](https://www.bundler.cn/v1.16/man/bundle-exec.1.html)），还是报同样的错：

     \>bundle exec jekyll serve

     Configuration file: D:/Code/yingang.github.io/_config.yml
               Source: D:/Code/yingang.github.io
          Destination: D:/Code/yingang.github.io/_site
     Incremental build: disabled. Enable with --incremental
          Generating...
          Jekyll Feed: Generating feed for posts
     jekyll 3.8.5 | Error:  File exists @ dir_s_mkdir - D:/Code/yingang.github.io/_site/jekyll

对着错误信息仔细看了下，不知道根目录下为什么有个叫 jekyll 的文件。。。删除之

再试下，好了~

     \>bundle exec jekyll serve

     Configuration file: D:/Code/yingang.github.io/_config.yml
               Source: D:/Code/yingang.github.io
          Destination: D:/Code/yingang.github.io/_site
     Incremental build: disabled. Enable with --incremental
          Generating...
          Jekyll Feed: Generating feed for posts
                         done in 1.274 seconds.
     Auto-regeneration: enabled for 'D:/Code/yingang.github.io'
     Server address: http://127.0.0.1:4000/
     Server running... press ctrl-c to stop.

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>