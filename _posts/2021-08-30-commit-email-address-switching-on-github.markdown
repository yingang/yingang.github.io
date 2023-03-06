---
layout: post
title:  "切换 GitHub 的提交邮件地址"
date:   2021-08-30 20:09:00 +0800
categories: github email
---

近期一边阅读学习民间翻译版的 [DDIA](https://github.com/Vonng/ddia/)，一边也参与了该翻译版的[校对](https://github.com/Vonng/ddia/commits?author=yingang)（除了明显的错误或不一致，其它觉得可能有问题或者不太明白的地方得对照英文版看下）。今天无意中发现我有一些最近的提交记录没有正确关联到我的 GitHub 账号（类似于 @帐户名 之类的，而是直接显示了一个非超链接形式的用户名称）。

想起来不久之前好像修改过本地的提交用户信息，比如用户名称，同时将本地提交邮件地址从 Gmail 切换成了 Hotmail（对应在 GitHub 的 Web 端早就切换成了 Hotmail，主要考虑到在境内访问前者的诸多不便），不知道有没有关系。

简单测试了下，可以排除 `user.name` 的影响，应该还是 `user.email` 在起作用，如果改回原来的 Gmail 邮箱地址还是可以将提交记录正确地与 GitHub 账号关联的，但没想明白的是我在 Web 端不是早就切换成了 Hotmail 邮箱为主了么？

然后找到了 GitHub 官方的相关[说明](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-github-user-account/managing-email-preferences/setting-your-commit-email-address)，也没有完全解释上述现象（看上去是设置了 Primary email address 就行），但同时看到还有一种方式是直接使用 GitHub 提供的基于 ID 的 noreply 邮件地址（类似于 ID+username@users.noreply.github.com），这样至少可以避免个人邮箱切换带来的影响（文中特意提到提交邮件地址修改后，已经完成的提交还是关联在之前的邮件地址上）。

先这样吧。

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>