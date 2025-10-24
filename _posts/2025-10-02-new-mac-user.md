---
layout: post
title:  "MacOS 新手上路"
date:   2025-10-02 16:24:00 +0800
categories: mac
---

家里唯一一台 Windows 笔记本服役已经超过十年，还是决定在海鲜市场出掉了，这样才能把基本闲置的 Mac mini 4 给用起来，过程中碰到不少问题，找个地方记录下。

## 鼠标键盘

因为日常工作还是基于 Windows 环境，所以实在没法接受在两种鼠标键盘操作习惯中来回切换。

1. Ctrl 和 Command 按键的对调。

    具体位置：系统设置 -> 键盘 -> 键盘快捷键 -> 修饰键 -> 选择键盘，然后把 Ctrl 和 Command 的作用分别改为对方就好了，这个是在[这里](https://zhuanlan.zhihu.com/p/613878115)学到的。

    同时带来一个后果，本来还算顺手的 Command + Tab 也得改到左边用 Ctrl + Tab 了，两害相权取其轻吧。

2. 鼠标的滚动方向。

    具体位置：系统设置 -> 鼠标 -> 自然滚动，取消掉就好了。

## 文件办公

1. 免费的 [WPS Office](https://zh-hant.wps.com/download/) 就足够了，不需要微软的 Office 套件。

2. 压缩文件的处理，7z 似乎没有 Mac 版，没找到合适的替代软件，但无意中发现 WPS 居然还支持 .rar 等压缩文件的解压，令人欣喜的不务正业。

    > 后来发现有个叫 Keka 的软件口碑不错，其[官网](https://www.keka.io/zh-cn/)上直接可下载，不一定要去 App Store 下载付费版本。

3. 从终端直接运行 `code` 来启动 VS Code，需要先在 VS Code 里执行：`Shell Command: Install ‘code’ command in PATH`，这个是从[这里](https://zhuanlan.zhihu.com/p/642745004)学到的。

4. 中断终端里的程序执行，`Command + .`。

5. 网络文件共享，主要用来在家里电视上访问电脑里的视频文件。

    具体位置：系统设置 -> 通用 -> 共享 -> 文件共享，可以按需添加想要共享的文件夹。

    需要注意在同一个地方的“选项”里，针对“Windows文件共享”进行设置。

    然后因为我的 Mac mini 是外接了一个移动硬盘，网络共享里默认是看不到上面的共享文件夹的，需要在“系统设置 -> 隐私与安全性 -> 完全磁盘访问权限”中允许 `smbd`，这个是从[这里](https://apple.stackexchange.com/questions/472836/how-to-share-a-folder-stored-on-an-external-usb-drive)学到的。

6. 浏览文件夹下的照片，相比 Windows 上不管用啥软件都挺方便，Mac 上目前有两个方法，总体算是勉强可用吧：

    * 选中所有想要浏览的照片，双击，就可以在“预览”应用中浏览了。不那么方便，但优点是可以同时查看 EXIF 信息。
    * 在任意一张照片上按住空格别松开，也可以同时用方向键进行浏览。有点费手，但还算方便，只是又看不了 EXIF 信息。

    ps. 方法二有个附带的好处，不止能预览照片，文档也是可以的！

### 还没搞定的问题

1. 外接硬盘的文件系统格式

    默认用了 exFat，但会有一个问题是在比如 VS Code 下，编辑任何文件都会生成 `._` 开头的文件，[据说](https://www.zhihu.com/question/398260618/answer/3143436699)也不是备份文件。考虑下要不要换成 APFS，还不确定 Windows 的可操作性怎么样。

2. GitHub 访问不畅

    之前在 Windows 下一直用的 fastgithub 不好使了，运行就报错，暂时没找到解决方法。

## 游戏娱乐

1. 之前在 Windows 下用的 qBittorrent 也有 Mac 版，继续用就好了。

2. 一直在 Mac 上使用的 [IINA](https://iina.io/)，最近才发现不支持 Dolby Vision，后面得注意下，另外也在[这里](https://www.zhihu.com/question/470187352/answer/3418773337)学习到一招，直接在 IINA 里调节色调也能将就看，但因为是个全局的设定，感觉也不够好用。

3. MTP 传输，先是装了 Commander One，结果没几天发现收费版本才能用到相关的功能，只好先装 Homebrew（参照[这里](https://blog.csdn.net/weixin_63310665/article/details/143313410)用下面的命令安装的，也设置了国内的源），再安装里面的 Android 文件传输（`brew install --cask android-file-transfer`）。

    ```
    /bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
    ```

