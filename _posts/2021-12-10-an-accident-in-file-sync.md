---
layout: post
title:  "掉进文件同步工具的坑里了"
date:   2021-12-10 13:13:00 +0800
categories: software
---

作为一名中度强迫症患者，日常都有通过文件同步工具将电脑上的一些重要文件进行不定期的备份。在家里是备份到移动硬盘，在公司是备份到分配给个人的公盘。

然后具体的同步软件一直是用的微软家的 SyncToy，虽然是个已经没有在活跃维护的工具（官网的下载链接都没有了），但至少能满足我的需求。

直到这周。。。翻车了！

### 背景

* 我在公司这边的 SyncToy 都是用的 Sync 模式，也就是源和目的地的任一边有更新都会同步到另外一边（双向同步）
* 正常情况我只在本地有更新，公盘是纯备份，所以其实用 Echo（单向同步，仅从源到目的地）或 Contribute（单向同步，且目的地只增不减）模式更合适

### 事故复盘

1. 公司的公盘会自动将长期不用的文件自动归档，转移到其它更低成本的存储介质上去
1. 似乎 SyncToy 不能处理这个情况，会认为目的地的一些文件已经不存在（其实是处于归档状态），反过来将本地的文件也删除掉
1. SyncToy 完成同步后，提示有上百个文件被删除，当时就比较诧异，然后看到回收站里一堆刚删除的文件才明白怎么回事
1. 只好手动将回收站里的文件恢复（并以为已经完成恢复，事后看似乎有些删除的文件并不在回收站里）
1. 既然 SyncToy 不好用了，换一个吧，上网搜索了一下，似乎用 [FreeFileSync](https://freefilesync.org/) 的比较多，就下载安装了
1. 老老实实设置好几个同步目录，并使用单向同步模式（类似于 SyncToy 的 Echo 模式，哎），虽然这个软件的界面交互非常不友好，真的很像上个世纪的风格，但基本同步功能似乎还是好用的
1. 大功告成，打开 [SumatraPDF](https://github.com/sumatrapdfreader/sumatrapdf) 看看书，这个软件会自动打开之前还在阅读的 PDF 文档，突然发现有几本书打不开了！！！
1. 推测是 SyncToy 一开始同步的时候，就没有把删除的文件全部都放进回收站（不能理解可能是什么原因，我能确定不会超出回收站的存储上限），导致本地有些文件其实并没有恢复，再然后用 FreeFileSync 进行同步后。。。它能认识归档文件，很好。。。但因为那些丢失的本地没有，工具就把公盘上对应的归档文件也删除掉了。。。我了个去！

### 事后

* 尝试使用 [Recuva](https://www.ccleaner.com/recuva) 来恢复硬盘上的文件，非常奇怪，完全搜索不出来我可能丢失的那些文件。。。死心了！

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>