---
layout: post
title:  "龙年新春快乐"
date:   2024-02-16 18:36:00 +0800
categories: life digital
---

正月初七，假期没剩几天了，前面回老家呆了近一周，匆匆忙忙，稍微记录下技术人没搞定的一些事情。

### 网络检修

无意中发现[两年前装上的无线路由器](https://yingang.github.io/life/digital/2022/02/07/happy-new-year-of-the-tiger.html)不知道什么时候开始已经没在工作了，但因为爸妈的手机还保留了原来那个猫的无线设置，所以没及时发现。

老爸用沉甸甸的指针式万用表测了下发现是路由器的电源有问题，没有正常的 9V 输出，然后我才发现这个还不是路由器原配的电源，可能是之前搬家的时候弄混了，不知道是不是因为功率不匹配用坏的（标称 1A，但路由器要求的是 1.5A）。

再然后第二天我才刚起床，发现老爸已经用家里现成的配件手搓了一个电源出来，用的是据说小区里捡来的一个装饰灯上的电源，配上老爸之前自己买的电源插头，赞，不然大过年的网上能买到的也都不能很快送货。

**未解之谜1**：旧路由器的不足在用笔记本电脑上网的时候感觉特别明显，用手机的时候倒不怎么觉得，不知道是什么原因。

### 微信表情包

有次大家在家庭群里发消息的时候，我姐说怎么还没看到老爸发的专属表情包，才反应过来老爸一直有个技能点，是能发出有定制文字的表情。

正好老爸就兴冲冲地来和我说这个功能是怎么回事，以及不知道为啥老妈的手机就没这个功能。

一开始我还以为是输入法的功能，后来才明白原来就是微信自带的功能，正式名称叫“合成表情”，在表情发送界面，使用“搜索表情”的功能即可看到相关的入口。或者如果收到别人发来的类似表情，直接点击也可看到相关的说明。

**未解之谜2**：老妈的手机上确实没有这个功能，同样最新的微信版本，怀疑可能是对手机处理器有要求，虽然价格没差多少，但老爸手机的处理器是高通的，而老妈手机的处理器是联发科的，不知道是不是这个原因，没找到比较官方的解释或说明。

**未解之谜3**：老爸老妈的手机有个公共的问题，我们发在群里的一些能产生全屏动画效果的消息（今年是康师傅的？），在他们的手机上除了显示正常的消息没有任何反应（但微信自带的一些表情效果，比如爱心啥的，是有反应的），也没找到什么比较官方的解释或说明。

### 视频通话

每周固定时间和爸妈视频的时候，偶尔会碰到他们没接听的情况，一般以为只是爸妈正好没听到，也没太在意。

这次在家的时候，正好有次亲戚发来的视频通话也没接听，而且我当时就在旁边，完全没有听到，这才仔细看了下老爸手机里微信的应用设置，尝试修改了几个通知（不要设置为不重要通知）和耗电（比如允许唤醒和开机自启动之类的，但还没有打开允许后台运行，据说会比较耗电）相关的设置，后面再观察观察效果。

> 一个月后：结果还是不行。。。得尝试改下允许后台运行了。

> 三个月后：又尝试了允许完全后台行为，关闭待机优化（默认是平衡模式），关闭耗电异常优化（对微信），包括按网上说的，也确认了应用锁定早就已经对且仅对微信设置了，结果还是不行。。。对 Android 系统杀后台的行为真是没招了，具体型号分别是 realme 的 GT 大师探索版 和 GT Neo，都是 8GB 运存，系统分别是 realme UI 5.0 和 4.0（分别基于 Android 14 和 13）。

### 语音助手

老爸教我做微信的表情包，我教老爸用手机的语音助手，老爸说有意思，回头仔细研究研究！

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>