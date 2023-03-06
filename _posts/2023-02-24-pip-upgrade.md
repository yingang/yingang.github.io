---
layout: post
title:  "pip 包更新的问题"
date:   2023-02-24 13:13:00 +0800
categories: python pip
---

[DDIA](https://github.com/Vonng/ddia) 收到 dependabot 的 PR 要更新 OpenCC 组件（1.1.1 到 1.1.2），因为之前已经有一些 OpenCC 使用过程中碰到的转译不太好的地方，所以想在本地先确认下新版本的转译效果。

结果在本地运行 `pip install --upgrade OpenCC` 后，提示：

    Requirement already satisfied: OpenCC in d:\code\ddia\venv\lib\site-packages (1.1.1)

但看到 pypi 上的 [OpenCC](https://pypi.org/project/OpenCC/) 版本其实都已经都到了 1.1.6。

然后尝试了：
- 直接指定版本（pip install --upgrade OpenCC==1.1.6），不行。
  ~~~
  ERROR: Could not find a version that satisfies the requirement OpenCC==1.1.6 (from versions: 0.1, 0.2, 1.1.0.post1, 1.1.1)
  ~~~
- 先卸载再安装，还是 1.1.1 版本。
- 不在虚拟环境中安装，也还是 1.1.1 版本。
- 在 WSL2 中做类似的操作，结果直接就安装了 1.1.6 版本，奇怪。
- 于是回到 Windows，尝试也将 Python 从 3.8 更新到 WSL2 中使用的 3.10，结果不变。
- 修改 pypi 源为默认源（日常是切换成了清华的源，速度快），结果不变。

最后，辗转了解到原因是根本就没有上传 Windows 平台的文件到 pypi 上。。。可以通过 https://pypi.org/pypi/opencc/json 确认，1.1.1 之后就没有再提供 Windows 版本的文件了：

![OpenCC](/images/opencc-release.png)

OpenCC 在 GitHub 上的相关 issues 似乎长期没有进展，不清楚具体困难卡在哪里，只好曲线救国自力更生：
1. git clone https://github.com/BYVoid/OpenCC.git
1. cd OpenCC
1. python setup.py build_ext
1. python setup.py install

备注：上述步骤要求机器上有可用的 build 环境，比如 Windows 平台的 cmake 和 Visual Studio 等。

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>