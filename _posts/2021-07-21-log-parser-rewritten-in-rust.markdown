---
layout: post
title:  "尝试用 Rust 重新造了个轮子"
date:   2021-07-21 16:22:00 +0800
categories: rust
---

前些时间兴致勃勃地把 [The Rust Programming Language](https://doc.rust-lang.org/book/) 读完后（写得挺好，入门强烈推荐！），总觉得得实际用用才行，想了下，要不把[前面几个帖子](https://yingang.github.io/c++/2020/05/22/strftime-and-tellg.html)提到的日志解析工具用 Rust 实现一遍吧：）

    What I can not create, I do not understand. -- Richard Feynman

前前后后花了两三天的工夫，体会：
* 书上的道理都不难懂（如果有一些 C++ 的基础应该会更有帮助），毕竟还是本偏入门的书，但实践起来要困难得多；
* 所以，让代码编译通过真不容易，但确认编译通过后碰到的问题就比较少了（逻辑错误总是会有的，这个编译器帮不了我）；
* 过程中碰到问题时，感觉语言的生态已经不错了，[官方的文档](https://doc.rust-lang.org/std/index.html)、[Stack Overflow](https://stackoverflow.com/) 上的问答都提供了我很多帮助；
* 初版完成后，代码量和 C++ 的版本差别不大（用了类似的实现方式，性能稍慢但也相近）；
* 除了最大卖点的内存安全（Reliability）和系统级编程语言的性能（Performance），[Rust](https://rust-lang.org) 的生产力（Productivity）也是有保证的，前提是对语言和标准库等都有基本的熟悉；
* 所以，初版完成后，比较方便地就实现了工具中一些计算密集型任务的并行处理，让分析过程和磁盘 I/O 尽可能互不干扰；
* 再配合一个语言无关的设计优化（对同一个文件的大量小数据的写入性能远低于合并成少量大数据后的写入性能），工具的性能比 C++ 版本还有了不小的提升，即使 C++ 也用上同样的优化！
* 说到性能调优部分，没有找到合适的办法做 Profiling，所以还是手工加日志来分析的，这方面没有之前[在 VS2019 中](https://yingang.github.io/c++/2020/06/01/performance-improvement-for-c++-developed-utility.html)方便；
* 对了，最终的 release 版本可执行文件，Rust 版本和 C++ 版本也是相当的（功能角度不完全一样，Rust 版本应该还稍多，但体积却小了约 12%）。

其它：
* 最近一两个月也在 B 站上跟了“喜欢历史的程序君”的 [Rust 培训课](https://www.bilibili.com/video/BV19b4y1o7Lt)，坦白说有点艰难，而且感觉没有一定的基础会比较难有好的收获，所以才萌生了上面的想法和实践，后续差不多了就准备继续跟进；
* 从这个培训（相关课件在 [GitHub](https://github.com/tyrchen/rust-training) 有提供）中，了解到一些好用的插件，比如给 VS Code 的 [rust-analyzer](https://rust-analyzer.github.io/)、给 FireFox 的 [Rust Search Extension](https://addons.mozilla.org/zh-CN/firefox/addon/rust-search-extension/) 等；
* 前面提到的书（TRPL），除了官网上可以直接看[英文版](https://doc.rust-lang.org/book/)，也可以在微信读书中看中文翻译的版本《[Rust 权威指南](https://book.douban.com/subject/35081743/)》。

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>