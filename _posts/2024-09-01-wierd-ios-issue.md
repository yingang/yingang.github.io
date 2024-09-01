---
layout: post
title:  "干净重装系统才能解决的 iPhone 故障"
date:   2024-09-01 17:53:00 +0800
categories: life digital
---

几个月前，手机（iPhone 14 Pro，iOS 17.x）存在如下的故障现象，主要体现在微信上：
* 进入订阅号的时候，会卡住，然后就黑屏了
* 如果回到桌面才重新切到微信（只是界面切换，没有重启微信），还是可以正常看到订阅号页面的内容
* 但如果微信重启了（比如因为运行其他应用导致微信退出了），会在启动后不久提示：“系统环境可能存在异常，为确保微信正常使用，建议重启手机后再试。”

![](/images/ios_wechat_issue.png)

如果这时重启整个手机，确实可以短暂解决问题，但重启后不久，问题又会出现。

尝试过重装微信以及其他一些网上能看到的方法，但都没有解决问题。最后是在家人建议下（也是碰到了类似的问题），直接抹掉整个系统，并且不要从之前的备份中进行还原，才算解决问题（至少已经超过两个月了，没有再发生）。

整个看上去像是从历史备份还原导致的问题（我的备份大概是从 iOS 12 或更早的版本开始的），和[之前碰到的 Firefox 问题](https://yingang.github.io/software/browser/2024/04/10/refresh-firefox.html)可能有点像。

另外，虽然问题主要是体现在微信上，其他应用也有一些可能相关的问题，主要发生在手机网络不好的时候。比如之前去平谭岛的火车上，就碰到过闲鱼 App 里的部分页面白屏而且重启应用都没用的情况，包括高德地图的定位功能也不工作了，目前这些问题都没有再发生过。

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>