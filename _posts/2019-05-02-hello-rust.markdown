---
layout: post
title:  "Hello Rust!"
date:   2019-05-02 20:02:45 +0800
categories: rust vscode
---

VSCode 插件似乎这两个就足够了：
* Rust (rls)
* WinDbg Debugger extension

创建测试项目：

    cargo new hello-rust --bin

创建并修改 launch.json 的 program，并添加 preLaunchTask，同时将 externalConsole 改为 false：

    {
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(Windows) Launch",
            "type": "cppvsdbg",
            "request": "launch",
            "program": "${workspaceFolder}/target/debug/hello-rust.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "preLaunchTask": "cargo build"
        }
    ]
}

按 F5 尝试调试，提示找不到 cargo build，创建配置，在下拉列表中选择 cargo build，将生成 tasks.json，添加 label 属性以和前面 launch.json 的 preLaunchTask 对应起来：

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
    ]
}

重新按 F5 调试，尝试 F11 进入 println!，报错：

    无法打开“mod.rs”: 找不到文件(file:///c:/rustc/9fda7c2237db910e41d6a712e9a2139b352e558b/src/libcore/fmt/mod.rs)。

不知道这个路径是什么含义。。。尝试创建文件夹：

    c:/rustc/9fda7c2237db910e41d6a712e9a2139b352e558b/

并在该文件夹下，创建软链接指向实际的 Rust 源代码位置：

    mklink /J src C:\Users\gang.yin\.rustup\toolchains\stable-x86_64-pc-windows-msvc\lib\rustlib\src\rust\src

遗留问题：
* ~~在 .rs 中对方法 F12，提示 No Definition found for ...~~ 好像只是类似于 println! 这样的宏无法 F12，为啥？

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>