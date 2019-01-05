---
layout: post
title:  "Welcome to Jekyll!"
date:   2019-01-05 20:22:45 +0800
categories: jekyll
---

References:
* [How to Start a Jekyll Blog on GitHub Pages for Free](https://onextrapixel.com/start-jekyll-blog-github-pages-free/)
* [kramdown Syntax](https://kramdown.gettalong.org/syntax.html)

\>jekyll serve --watch

    Traceback (most recent call last):
        5: from C:/Ruby25-x64/bin/jekyll:23:in `<main>'
        4: from C:/Ruby25-x64/bin/jekyll:23:in `load'
        3: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/jekyll-3.8.5/exe/jekyll:11:in `<top (required)>'
        2: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/jekyll-3.8.5/lib/jekyll/plugin_manager.rb:48:in `require_from_bundler'
        1: from C:/Ruby25-x64/lib/ruby/2.5.0/rubygems/core_ext/kernel_require.rb:59:in `require'
C:/Ruby25-x64/lib/ruby/2.5.0/rubygems/core_ext/kernel_require.rb:59:in `require': cannot load such file -- bundler (LoadError)

\>gem install jekyll bundler

\>bundle install

\>jekyll serve --watch

    Configuration file: D:/Code/yingang.github.io/_config.yml
            Source: D:/Code/yingang.github.io
       Destination: D:/Code/yingang.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
jekyll 3.8.5 | Error:  File exists @ dir_s_mkdir - D:/Code/yingang.github.io/_site/jekyll

\>bundle exec jekyll serve

    Configuration file: D:/Code/yingang.github.io/_config.yml
            Source: D:/Code/yingang.github.io
       Destination: D:/Code/yingang.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
jekyll 3.8.5 | Error:  File exists @ dir_s_mkdir - D:/Code/yingang.github.io/_site/jekyll

remove a file named with 'jekyll' under the root folder.

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