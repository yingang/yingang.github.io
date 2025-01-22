---
layout: post
title:  "用 AMD μPerf 对 Rust 应用进行性能分析"
date:   2021-07-24 09:55:00 +0800
categories: rust
---

[之前的帖子](https://yingang.github.io/rust/2021/07/21/log-parser-rewritten-in-rust.html)中提到当时不知道如何对 Rust 程序做 Profiling，后来在 [The Rust Performance Book](https://nnethercote.github.io/perf-book/profiling.html) 中找到了，也包括对各个开发平台推荐使用的工具，我是 Windows，选择不多，就用 AMD μPerf 吧。

只需要在 Cargo.toml 中添加：

    [profile.release]
    debug=1

配置项 1 为 line tables only，如果配置为 2 就是 full debug info，我没有进一步确认差异。对于 Windows 上使用 MSVC 工具链的，调试信息会统一放在 .pdb 文件中，即 split-debuginfo 的 packed 方式，而不是在 .exe 中（off）或者分散的 .obj 文件中（unpacked）。

然后在 AMD μProf 中配置好应用路径及参数以及 Sources 和 Symbols 的存放路径就行。

目前来看，整体比较平均，相对主要的性能消耗点还是在文本处理上，暂时没有好的优化方法，先这样吧。

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>