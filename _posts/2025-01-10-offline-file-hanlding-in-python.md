---
layout: post
title:  "用 Python 处理 Windows 上的离线文件"
date:   2025-01-10 19:38:00 +0800
categories: python windows
---

## 什么是离线文件

* 文件图标左下角有个灰色的小叉（Windows 10，不知道 11 上的效果有没有差别）：

![](/images/offline_file_icon.png)

* 如果查看这个文件的属性，会看到其实际占用空间远小于文件大小：

  ![](/images/offline_file_size.png)

* 如果用 `attrib.exe` 查看文件的属性，结果输出里会有一个大写的 `O`。


## 背景

* 个人有习惯定期将本地的文件同步到文件服务器上。
* 但公司的文件服务器会定期将不常访问的大文件归档到冷存储上，这时文件图标上就会有个灰色小叉了（我下线了，别来找我！）。
* 离线状态不会在访问文件的时候自动解除，应该是要走什么流程申请下才行，没试过。
* 如果文件服务器上的文件被归档而处于离线状态，就会导致：
  * 往服务器的同步可能会报错：
    * 我之前用 SyncToy [翻过车](https://yingang.github.io/software/2021/12/10/an-accident-in-file-sync.html)，离线的文件会被认为不存在了。
    * 后来改用了 [FreeFileSync](https://freefilesync.org/)，至少可以正常同步。
  * 从服务器的下载也会报错，这个会影响到多个机器之间通过文件服务器的同步，比如最近准备更换工作电脑就碰到这个问题。

## 想要解决的问题

既然决定不了文件服务器的归档行为，就想要能够比较方便的删除掉文件服务器上被归档的文件，并且重新从本地将原始文件同步上去。

## Python 上场

### 检查是否离线文件

知道这种文件叫离线文件就已经离成功不远了：

* `os.stat()` 可以拿到文件的属性。
* 根据[这里](https://docs.python.org/3/library/stat.html)的说明，可以对文件属性的 `st_file_attributes` 检查是否 `stat.FILE_ATTRIBUTE_OFFLINE` 已置位。

### 删除离线文件

* 直接用 `os.remove()` 删除不了（已经是管理员权限的命令行窗口），但直接在 Windows 桌面或者在命令行窗口里用 del 命令又都是可以删除掉的，奇怪了。
* 上网查有什么 `os.remove()` 的替代品，找到了 `os.unlink()`，但按网上普遍的说法，这两个在 Windows 上其实没区别。
* 死马当活马医，试试看不会死，但。。。居然删除掉了！

### 同步缺失的文件

* 这个没啥好说了，写个可以递归调用的方法，在里面用 `os.listdir()` 遍历源路径，并根据遍历结果是文件还是目录区分处理就行（文件检查是否需要拷贝，目录就直接递归）。

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>