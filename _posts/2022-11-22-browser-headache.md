---
layout: post
title:  "让人脑壳疼的浏览器问题"
date:   2022-11-22 11:30:00 +0800
categories: software browser
---

> 更新大结局：Firefox 和 Chrome 先后出局，机器上只需要 Edge 就可以搞定全部了！

  * 2023/04/11 公司的 PLM 支持 Edge 访问了。
  * ~~2023/12/12 Edge 可以记住 Azure Devops Server 的访问密码了，具体原因不清楚，版本 120.0.2210.6。~~（后面很快就不行了，原因仍然不详。。。）

> 下面是原贴

10+ 年的 FireFox 用户，最近碰到难题了，在公司特定的使用环境下。

## Mozilla FireFox

本来是基本可以正常使用的，但最近：

* 飞书官方不支持，虽然也可以强行用，但总有个提示栏会出来：“此浏览器存在兼容性问题会影响你的正常使用，请访问 帮助中心 了解更多”
* FireFox 内置的互联网连接检测，似乎也被公司的网络配置给干扰了，时不时会认为自己不能访问互联网。。。然后显示屏上宝贵的空间就变成这样了：
  
  ![Firefox](/images/firefox_prompt.png)

* 和上面这个类似，能检测到有新版本但也不能在线更新。。。
* 公司的 PLM 系统不支持

## Google Chrome

PLM 目前只认这个浏览器，没得跑了，但：

* 长页面浏览下拉时会出现比较规律的间歇性卡顿，难以忍受，另两家完全没有这个问题。

  ps. 尝试过不使用硬件加速，还是会有同样的问题。

## Microsoft Edge

也有两个比较烦人的问题：
* 访问公司内的 Azure DevOps Server 页面无法记住密码，每次重启浏览器后都需要重建输入完整的登录信息。。。另两家都没这个问题。

  ![Edge](/images/edge_prompt.png)

* ~~浏览器设置菜单反应卡顿，但毕竟不像浏览页面那么常用，还不是完全不能忍。。。另两家完全没有这个问题。~~
  
  更新：今天研究前一个问题的时候，重置了下 Edge 的设置，然后发现后一个问题似乎突然消失了！再继续观察观察。


<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>