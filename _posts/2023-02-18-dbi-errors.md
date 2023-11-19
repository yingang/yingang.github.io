---
layout: post
title:  "dbi 常见报错处理"
date:   2023-02-18 12:49:00 +0800
categories: game switch
---

- `Cannot parse content meta. Corrupted file or old firmware`

  原因是安装文件损坏或者主机 Firmware 版本需要升级，我一般碰到的是后者。

- `Can not create placeholder`

  原因是安装空间不足，在使用 `xcz` 和 `nsz` 等压缩格式的安装文件时更容易碰到，需要先清理出足够的空间。

- `ZSTD DECOMPRESSION ERROR: 64`

  原因是在 Applet 模式下（从相册启动，最新版 dbi 会显示蓝色背景）运行 dbi 内存不足导致解压缩失败，改为在 Title 或 Application 模式下（按住 R 键启动游戏，最新版 dbi 会显示黑色背景）运行 dbi 即可。

## 参考

- [dbi on GitHub](https://github.com/rashevskyv/dbi/blob/main/README_ENG.md)


<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>