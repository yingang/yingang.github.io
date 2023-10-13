---
layout: post
title:  "在 Win10 上通过批处理运行特权应用"
date:   2019-05-24 20:02:45 +0800
categories: software windows cmd
---

### 问题

* 由多个 .bat 文件组成自动化测试脚本，互相之间通过 start 和 call 调用
* 具体：初始脚本通过 start 启用若干数量的测试脚本，每个测试脚本会根据传入的测试次数，循环调用进一步的测试脚本和程序
* 脚本执行过程中，不断有新的命令行窗口弹出，很烦人
* 对了，OS 是 Win10

### 弯路

* 怀疑过是不是和 Win10 的什么设置相关，查了半天没找到
* 也有以为和 call 相关，还像模像样在 stackoverflow 上问了个问题，结果被打脸，说 call 应该不会创建新窗口，show me the code please

### 正途

* 测试脚本不是我自己写的，应该写一些验证脚本来确认自己的想法，并在不同的 OS 上进行验证，至少包含 Win7 和 Win10

### 结论

* 测试脚本调用的部分可执行程序（.exe）是需要特权才能运行的，这些程序在 Win10 上的图标可以看到一个小盾
* 和 Win7 不同，在 Win10 上，即使是有管理员权限的账号，即使关闭了 UAC，直接双击 .bat 文件也不是以管理员身份运行的
* 这种情况下，如果 .bat 文件里调用了需要特权运行 .exe，就会弹出一个新的命令行窗口
* 所以，解决方案就是用管理员权限运行初始脚本，世界清净了

### 插曲

* 大量弹出新窗口的情况下，似乎会导致 ctfmon.exe 进程的内存持续泄漏，禁用 Touch Keyboard and Handwriting Panel Service 即可

### 其它
* [How can I auto-elevate my batch file, so that it requests from UAC administrator rights if required?](https://stackoverflow.com/questions/7044985/how-can-i-auto-elevate-my-batch-file-so-that-it-requests-from-uac-administrator)

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>