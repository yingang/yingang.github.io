---
layout: post
title:  "[未解决] Visual Studio 2019 在外接显示器上字体发虚的问题"
date:   2021-07-17 21:01:00 +0800
categories: software vs2019 pma
---

> 更新 08/20/2021：所述方法在内置和外接显示器之间切换时（或者断开外接显示器时）还存在Work Item显示布局异常的问题，感觉还不太成熟，放弃了。。。根据之前在网上看到的，把外接显示器设置为主显示器也有一定的帮助。

今天下午实在有点不能忍了，就上网查了下，[必应同学](https://cn.bing.com/?FORM=Z9FD1)引导我到了这个帖子：[A better multi-monitor experience with Visual Studio 2019](https://devblogs.microsoft.com/visualstudio/a-better-multi-monitor-experience-with-visual-studio-2019/)。

粗看了下，相关的技术是叫 per-monitor DPI awareness（针对每个显示器的 DPI 感知，简称 PMA），文中提到在 Tools->Options->Preview Features中可以设置。

我用的 VS2019 是比较新的 16.10.3 版本，没有看到这个选项（虽然有很多其它的，VS 现在的开发迭代速度真不错），再仔细看了下帖子的回复，有人说这个配置参数已经移到 Environment->General 中了，一看果然是的，但我这边的选项被禁用了（下图是没有禁用时截取的），而且有文字提示需要 1803 之后的 Windows 10 版本，以及需要 4.8 版本的 .Net Framework（其实帖子正文中都提到了，没耐心仔细看的后果）。

![Environment Configuration](/images/vs2019_tools_options_environment_general.png)

我的 Windows 10 系统已经是 1809，那应该就是对应版本的 .Net Framework 没装导致的吧，重新运行 VS2019 的安装程序，看了下 4.8 版本的 Runtime 确实没有勾选（可能是因为已经切换到 5.0 了，但我不清楚为什么 5.0 不能直接替代 4.8），选上后正常安装，然后重启电脑，再运行 VS2019，果然世界就不再模糊了！

![VS Installation](/images/vs2019_installation_net_framework.png)

**突然感觉到，最近一年多把显示器用旧书垫高后，感觉脖子酸疼的情况都少多了，工作重要，但也要保护好自己~**

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>