---
layout: post
title:  "Visio 2016 总在启动时提示正在配置的问题"
date:   2021-12-06 13:59:00 +0800
categories: office visio
---

好像被这个问题困扰多年了，因为平时 Visio 用得不多，所以也不是太在意。最近又有碰到，就好奇上网搜索了一下：[有个帖子](https://social.technet.microsoft.com/Forums/office/en-US/5d15ba0c-bed2-4e9b-aa66-7966bb21d75c/every-time-i-start-visio-2016-i-get-quotplease-wait-while-windows-configures-visio-professional)说把注册表里 Visio 相关的几个文件类型（.vsd和.vsdx）删除掉再重启 Visio，应该就可以自动修复（至少有人回复说对他/她是有用的），备份好要删除的注册表内容后试了下，有点不幸，没用。。。帖子里有引用到另外一个微软网站的链接，但也是访问不了，疏于维护啊。

然后继续搜索，又找到[另外一个帖子](https://social.technet.microsoft.com/Forums/en-US/134b1aff-0f93-42ea-9eb4-41d4fdceabf7/visio-2016-error-when-starting)，被接受的答案也是引用了前面说的已经不存在的链接。。。再往下翻翻看，有点有意思的内容了（by [Mark D. Albin](https://social.technet.microsoft.com/profile/mark%20d%20albin/?ws=usercard-mini)）：

> This can be caused by the [HKEY_CLASSES_ROOT\.vsd] key not being equal to “Visio.Drawing.11" or equal to “VisioViewer.Viewer” if you have Visio Viewer Installed.

> A repair/reinstall of Visio would typically fix this automatically, as would allowing Visio to finish configuring. However, if the repair/reinstall/configuration does not complete successfully, the user will continue to receive this message at each launch of Visio. To manually fix this issue, run 'regedit' and manually change the value of [HKEY_CLASSES_ROOT\.vsd] to Visio.Drawing.11 (if using the Visio client) or VisioViewer.Viewer (if using the Visio Viewer).

灵光一闪，我们这边 Visio 和它几个 Office 软件（Word/Excel/Powerpoint）是分开装的，后者里面确实有个叫 Visio Viewer的组件，是不是这里冲突了？

三下五除二，卸载并重新安装 Office（直接卸载单个组件老提示说找不到安装源文件，搞不定才只好整个重装。。。**注意在安装开始前自定义选择不安装 Visio Viewer**），然后再运行 Visio，第一次启动时仍然会提示正在配置，完成后关闭再重启，果然没问题了！

题外：后来按中文关键字搜索了下，其实也有不少帖子都提到了这个问题，但除了天下文章一大抄都是一样的内容，好像排名靠前的都是直接说了要怎么修改注册表，没有提到 Visio Viewer。

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>