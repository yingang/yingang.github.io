---
layout: post
title:  "Hello Rust Again!"
date:   2020-02-11 17:18:45 +0800
categories: rust vscode
---

在家远程办公 & 抗击 NCP 中，顺手捡起来之前落下的 [Rust by Example](https://github.com/yingang/hello-rust)。

工作用的笔记本电脑上似乎无法通过 Ctrl+Shift+B 来运行 cargo build 了，看半天看不出来原因，还是对 VSCode 背后的机制太不了解了。。。tasks.json 还是原来的配方：

    {
        "version": "2.0.0",
        "tasks": [
            {
                "type": "cargo",
                "label": "cargo build",
                "subcommand": "build",
                "problemMatcher": [
                    "$rustc"
                ]
            }
        ],
        "presentation": {
            "panel": "new",
            "showReuseMessage": false
        }
    }

Terminal 中显示如下（预期在这两行中间应该有 cargo build 的具体输出）：

    > Executing task: cargo build <

    Press any key to close the terminal.

但如果直接在 Terminal 中输入 cargo build 又是可以正常运行的，搞不懂。尝试过回退 VSCode 的版本（因为日常会自动更新），似乎没什么影响，有点怀疑是机器上的信息安全软件又在作祟了（之前已经有一个导致无法在 VSCode 里正常调试 Python 脚本的问题）。

远程找到一台测试机，只安装了基本的杀毒软件，最新的 VSCode 1.42.0 + Rust 1.41.0 + VC Buld Tools 2019 (16.4.4)，可以正常使用，看来应该还是环境的问题。。。

过程中对 VSCode + Rust 相关稍微又多了一点点了解：
* 为了能够调试进 Rust 自带的类库，还是需要用类似于[之前](https://yingang.github.io/rust/vscode/2019/05/02/hello-rust.html)的方法创建一个目录和相应的链接，但对应 Rust 1.41.0 的链接目录变成了 c:/rustc/5e1a799842ba6ed4a57e91f7ab9435947482f7d8/ （原来的 9fda7c2237db910e41d6a712e9a2139b352e558b 对应 Rust 1.32.0），然后目标路径也变成 stable-i686-pc-windows-msvc 了：

    mklink /J src C:\Users\gang.yin\\.rustup\toolchains\stable-i686-pc-windows-msvc\lib\rustlib\src\rust\src

* F12 可以跳转到类似于 println! 的宏里面了，这点比之前有改进，不知道是因为什么导致的。
* 之前在 launch.json 里配置的 cppvsdbg，看来并不是配对 WinDbg Debugger extension 工作的（对应的配置参数应该是 windbg，所以也要求环境中安装有 WinDBG），而应该是对应 Visual C++ 的调试器，所以除了上次说的插件，还需要安装巨硬家出品的 C++ 插件，不然 F5 就会提示 cppvsdbg is not supported。
* 本地的 rust 版本更新可以用 rustup update，如果是更新 rustup 组件自身，则命令为 rustup self update。

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>