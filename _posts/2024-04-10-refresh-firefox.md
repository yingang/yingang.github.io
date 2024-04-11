---
layout: post
title:  "Refresh Firefox"
date:   2024-04-10 22:10:00 +0800
categories: software browser
---

最近几天家里笔记本电脑上的 Firefox 出问题了（自从[公司的 Firefox 退役](https://yingang.github.io/software/browser/2022/11/22/browser-headache.html)之后，这是仅存的独苗了），现象是在初次启动的时候闪现一下界面后就没了，但任务管理器里看进程都还在，而且也在持续消耗少量的 CPU 等系统资源，难受。

如果完全不管后台那些僵尸进程，可以继续启动一个新的，新进程可以正常起来也能正常退出（但僵尸不会退）。但如果把僵尸强杀了，再启动新的 Firefox，就又还是一样的问题现象。

今天有点不能忍，就尝试搜索了下，好像没有比较近的帖子讲这个的，隐约看到一些说重装一下试试啥的，想着要不就试试吧。然后还没开始真正卸载，就看到 Firefox 卸载界面上的提示了，嘿，这不正是我想要的吗：

![](/images/firefox_refresh.png)

过程还算快，因为之前使用了 Firefox 自带的账户同步功能，所以 Refresh 后很快就把类似历史记录、书签之类的都恢复了，但也有一些需要手工修改的东西，比如：

* 是否允许网页自动播放视频和音频（可能变成了默认的设置，仅不允许自动播放音频）
* 主界面布局（比如工具栏的各种按钮啥的，得重新摆一下）
* 默认使用的搜索引擎（某度给我走开）
* 屏蔽广告的插件 uBlock Origin 不见了（据说是[被迫在国内下架](https://www.zhihu.com/question/523163432)了，可以直接去 [Github](https://github.com/gorhill/uBlock/releases/) 上下载安装，找名字是 uBlock_****.firefox.signed.xpi 的就行）
* `about:config` 里的设置也复原了（比如一直用的 `closeWindowWithLastTab = false`）

但最重要的，Refresh 后确实我一开始的问题解决了！估计还是无数历史版本一路升级上来，保留了越来越多不那么兼容的配置吧。

Refresh 这个功能的出现位置挺好，卸载再重装总还是更麻烦一些的，虽然说如果能一开始就避免这样的问题就更好了吧。


<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>