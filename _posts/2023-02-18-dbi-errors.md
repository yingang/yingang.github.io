---
layout: post
title:  "dbi 常见报错处理"
date:   2023-02-18 12:49:00 +0800
categories: game switch
---

- `Cannot parse content meta. Corrupted file or old firmware`

  安装文件损坏，或者主机 Firmware 版本需要升级，我一般碰到的是后者。

- `Can not create placeholder`

  安装空间不足，在使用 `xcz` 和 `nsz` 等压缩格式的安装文件时更容易碰到，需要先清理出足够的空间。


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