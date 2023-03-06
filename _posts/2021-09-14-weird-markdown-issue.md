---
layout: post
title:  "奇怪的 Markdown 问题"
date:   2021-09-14 13:18:00 +0800
categories: markdown
---

### 问题

还是关于之前提到过的 [DDIA](https://github.com/Vonng/ddia/) 的民间翻译版，今天在确认一个 [PR](https://github.com/Vonng/ddia/pull/129) 的时候发现有时候需要在强调文本后面加一个空格才能正常显示出强调的效果，目前看下来有两个主要的规律：

* 有问题的都在强调文本结束前紧接着包含了标点符号（我碰到的问题是中文右括号，但测试下来只要是标点符号，不管中英文，都会有问题）
* 有问题的都在强调文本后直接跟随了文字（不管中英文都会有问题，但如果跟随的是标点符号就没有问题）

### 举例

* 有空格没问题

  ```当想到系统失效的原因时，**硬件故障（hardware faults）** 总会第一个进入脑海。```

  当想到系统失效的原因时，**硬件故障（hardware faults）** 总会第一个进入脑海。

* 没空格有问题
  
  ```当想到系统失效的原因时，**硬件故障（hardware faults）**总会第一个进入脑海。```

  当想到系统失效的原因时，**硬件故障（hardware faults）**总会第一个进入脑海。

* 没右括号就没问题
  
  ```当想到系统失效的原因时，**硬件故障（hardware faults**总会第一个进入脑海。```

  当想到系统失效的原因时，**硬件故障（hardware faults**总会第一个进入脑海。

* 括号换成问号也不行

    ```当想到系统失效的原因时，**硬件故障（hardware faults？**总会第一个进入脑海。```

    当想到系统失效的原因时，**硬件故障（hardware faults？**总会第一个进入脑海。

备注：可以在 VS Code 里预览相应的效果（Markdown All in One 插件），但上传到 GitHub Pages 又看不出来，不知道为什么。。。有兴趣可以点击[这里](https://github.com/yingang/yingang.github.io/blob/master/_posts/2021-09-14-weird-markdown-issue.md)查看。

### 扩展问题

如何用正则表达式找出所有有问题的 markdown 文本？上述问题导致的现象不一定是显示出来有**，也有可能是错误地对文本进行了强调显示。。。

### 更新 2021/09/14

查了半天 VS 的文档，找了个非常土的解决方案。。。

    ）\*\*[^\s，。：；（）*【\-—\[”]

大致就是用类似[^abc]的方式排除可能的一些标点符号。

### 更新 2021/12/28

知乎似乎也有类似的问题：https://zhuanlan.zhihu.com/p/435852919

    不过需要注意的是，**没有技术会是银弹，只有把技术放在适用的场景下才能达到事半功倍的效果。**那么哪些场景适合 WebAssembly 呢？



<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>