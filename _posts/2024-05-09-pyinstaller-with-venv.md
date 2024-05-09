---
layout: post
title:  "Python 虚拟环境中的 PyInstaller 打包"
date:   2024-05-09 21:15:00 +0800
categories: python pyinstaller
---

这几天给手头一个小工具增加了解析 Excel 文件的功能，然后 `PyInstaller` 打包一看，输出文件直接从 20+MB 变成 70+MB 了，而且里面出现了明显不太对劲的内容，比如这个工具里没有用到的 `numpy` 等，实在对不起“小工具”这个名称。。。

后来折腾了一会儿才将输出文件再变成原来的大小，甚至比原来还小一丢丢~

简单记录下：
* 系统全局有一个 Python 环境，其中也安装了大量的 Python 库，包括 `PyInstaller` 和 `numpy` 这些都在其中
* 而工具项目使用了自己的虚拟环境，该虚拟环境中默认只安装了项目本身需要使用的 Python 库，比如 `openpyxl`，但没有安装 `PyInstaller`
  * 之前打包都是直接在虚拟环境中运行的，但实际运行的其实是全局 Python 环境中的 `PyInstaller`（其中从打包的运行日志中也能看出来，之前没往深处去想）
  * 之前打包的结果其实还挺正常，但这次不知为啥，多了一堆东西
* 先尝试将全局 Python 环境中的所有库都删除（`pip freeze > req.txt` + `pip uninstall -r req.txt -y`）
  * 并仍然在全局 Python 环境安装 PyInstaller
  * 再次在虚拟环境中打包，结果很小，小到不能正常运行。。。看来不能这么用
* 然后尝试在虚拟环境中安装 PyInstaller（再次删除了全局 Python 环境中的所有库，免得互相干扰）
  * 这次打包的结果对了，可以正常运行，体积也正常，从打包的运行日志也能看出来都是使用的虚拟环境中的信息

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>