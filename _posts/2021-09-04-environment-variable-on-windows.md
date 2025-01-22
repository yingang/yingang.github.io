---
layout: post
title:  "Windows 上的环境变量设置"
date:   2021-09-04 11:18:00 +0800
categories: software windows
---

继续[在 B 站上 Rust 课](https://www.bilibili.com/video/BV1h64y197G3)，进行到使用 tracing 组件输出 log 的时候，需要在 VSCode 里设置环境变量，默认的 powershell 我也不太会用，稍加学习后总结如下，估计以后还用得着。

# powershell

    # 查看当前所有环境变量，还可以在冒号后面用带通配符的名称来进一步限定范围，比如 ls env:RUST*
    ls env:

    # 设置环境变量，注意双引号的使用
    # 如果只有变量名称就是查看其变量值，删除变量则需要用del指令
    $env:RUST_LOG="info"

# cmd

    # 查看当前所有环境变量，如果加上变量名就是查看其变量值
    set

    # 设置环境变量，等号后留空表示删除该环境变量
    set RUST_LOG=info

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>