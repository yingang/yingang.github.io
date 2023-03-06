---
layout: post
title:  "删除 Azure DevOps 上的项目 Wiki"
date:   2022-06-06 23:02:00 +0800
categories: software azure-devops
---

最近在公司的 Azure DevOps 服务器上创建了 BU 的 Wiki 项目，其支持两种形式的 Wiki：
* 一种叫项目 Wiki（project wiki），后台也是用版本管理系统 Git 来维护的，只是这个代码库独立于项目的代码库，不能直接在项目界面上看到。
* 一种叫代码 Wiki（code wiki），直接对应项目的代码库（repo，还可以指定对应代码库下的特定分支和路径）。

![Wiki](/images/azure_devops_wiki_creation.png)

一开始啥都不太懂的时候，仿照隔壁兄弟 BU 创建的是代码 Wiki，因为可以直接对应项目的代码库，感觉比较直观，然后也基于这个做了初步的内容框架建设。

然后今天手贱，发现同时还可以继续创建项目 Wiki，以及从项目的 Wiki 入口进来看到的居然是项目 Wiki 而不是代码 Wiki。。。在界面上找来找去也没找到可以删除项目 Wiki 的地方，尴尬了。

后来在[这个帖子](https://www.sanderh.dev/delete-project-wiki-Azure-DevOps/)里找到了解决方案，原理上其实是 Azure DevOps 对自身的功能提供了比较完善的 REST API，可以通过 PowerShell 或专门的工具比如 [Postman](https://www.postman.com/) 来进行操作。如果不是对 REST API 和 Powershell 这些都比较熟悉，建议直接使用后者（[相关介绍](https://www.sanderh.dev/delete-project-wiki-Azure-DevOps/)）。

## 获取个人访问令牌（PAT，personal access token）

点击 Azure DevOps 主界面右上角的个人图标，选择“安全性”：

![Security](/images/azure_devops_security.png)

然后点击“+ 新标记”，简单起见，可以授予“完全访问权限”：

![PAT](/images/azure_devops_pat_new.png)

然后点击“创建”，就可以得到个人访问令牌（PAT）了，记得先拷贝出来，不然就再也看不到了。

## 查询 Wiki 的 repositoryId

在 Postman 里发送如下的请求：

    GET {project_url}/_apis/wiki/wikis?api-version=6.0

其中在 `Authentication` 页面选择 `Basic Auth`，并在 `Password` 里输入前面获取到的个人访问令牌即可。

![Request](/images/postman_auth.png)

如果上述配置正确，将收到类似如下的响应：

~~~json
{
    "value": [
        {
            "id": "4c4a79dd-7dac-4908-b834-3eeb8db5a872",
            "versions": [
                {
                    "version": "main"
                }
            ],
            "url": "...",
            "remoteUrl": "...",
            "type": "codeWiki",
            "name": "...",
            "projectId": "...",
            "repositoryId": "e3119048-d5f4-4625-8d40-cc4addf58861",
            "mappedPath": "/"
        },
        {
            "id": "e313d182-7b5d-407b-9ae2-b54afde89ab6",
            "versions": [
                {
                    "version": "wikiMaster"
                }
            ],
            "remoteUrl": "...",
            "url": "...",
            "type": "projectWiki",
            "name": "...",
            "projectId": "...",
            "repositoryId": "e313d182-7b5d-407b-9ae2-b54afde89ab6",
            "mappedPath": "/"
        }
    ],
    "count": 2
}
~~~

通过 `"type": "projectWiki"` 可以找到我们关心的项目 Wiki 对应的 repositoryId，在这个例子里，也就是 `e313d182-7b5d-407b-9ae2-b54afde89ab6`。

## 通过 repositoryId 来删除 Wiki

还是在 Postman 里，发送如下的请求既可删除对应的 Wiki 库：

    DELETE {project_url}/_apis/git/repositories/{repo_id}?api-version=6.0

删除完成后，可以重新发送前面的 GET 请求来确认是否已成功删除。

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>