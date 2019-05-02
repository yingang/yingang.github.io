---
layout: post
title:  "Hello Rust!"
date:   2019-05-02 20:02:45 +0800
categories: rust
---

VSCode 插件：
* Rust (rls)
* WinDbg Debugger extension

创建测试项目：

    cargo new hello-rust \--bin

launch.json

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

tasks.json

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

F5 调试，尝试进入 println!：

    无法打开“mod.rs”: 找不到文件(file:///c:/rustc/9fda7c2237db910e41d6a712e9a2139b352e558b/src/libcore/fmt/mod.rs)。

创建文件夹：

    c:/rustc/9fda7c2237db910e41d6a712e9a2139b352e558b/

创建软链接：

    mklink /J src C:\Users\gang.yin\.rustup\toolchains\stable-x86_64-pc-windows-msvc\lib\rustlib\src\rust\src

遗留问题：
* 在 .rs 中 F12，提示 No Definition found for ...