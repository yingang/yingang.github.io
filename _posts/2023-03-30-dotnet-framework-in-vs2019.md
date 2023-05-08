---
layout: post
title:  "在 VS2019 中创建针对 .NET Framework 4.x 的应用"
date:   2023-03-30 17:45:00 +0800
categories: c# tfs
---

## 背景

手上有个用 VS2010 / C# 写的工具，功能主要是查询 TFS2010 上的工作项。然后因为目前工作机上基本已经升级到 VS2019/VS2022，希望这个工具可以在没安装 VS2010 的机器上也能用。

但直接将工具本身（包括配套的 dll 文件）拷贝过来似乎没用，从网上查到的片断信息来看，似乎还是需要这台机器上装了 VS2010 才行（可能起作用的主要是 Team Explorer）。

然后想着既然 VS2019/VS2022 本身是可以直接访问 TFS2010 的，那是不是直接用 VS2019/VS2022 来开发这个工具就行了？

## 掉坑

几乎没什么 .Net 的开发背景，尝试直接在 VS2019/VS2022 里创建一个 DEMO 程序，能正常编译，但运行起来只是在连接 TFS 服务器这一步就会直接报错了。

```csharp
using System;
using Microsoft.TeamFoundation.Client;

namespace ConsoleApp2
{
    class Program
    {
        static void Main(string[] args)
        {
            TfsTeamProjectCollection collection = new TfsTeamProjectCollection(new Uri(@"http://tfs:8080/tfs/DefaultCollection"));
        }
    }
}
```

```plaintext
Unhandled exception. System.EntryPointNotFoundException: Unable to find an entry point named 'ZeroMemory' in DLL 'kernel32.dll'.
   at Microsoft.VisualStudio.Services.Common.Internal.NativeMethods.ZeroMemory(IntPtr address, UInt32 byteCount)
   at Microsoft.VisualStudio.Services.Common.CredentialsCacheManager.GetCredentialsFromStore(String targetName)
   at Microsoft.VisualStudio.Services.Common.CredentialsCacheManager.GetCredentials(String featureRegistryKeyword, String targetName)
   at Microsoft.VisualStudio.Services.Common.CredentialsCacheManager.GetCredentials(String featureRegistryKeyword, Uri uri, Boolean requireExactUriMatch, Nullable`1 nonInteractive)
   at Microsoft.VisualStudio.Services.Client.VssClientCredentials.LoadCachedCredentials(String featureRegistryKeyword, Uri serverUrl, Boolean requireExactMatch, CredentialPromptType promptType)
   at Microsoft.VisualStudio.Services.Client.VssClientCredentials.LoadCachedCredentials(Uri serverUrl, Boolean requireExactMatch, CredentialPromptType promptType)
   at Microsoft.TeamFoundation.Client.TfsConnection.LoadFromCache(Uri serverUrl, ICredentials credentials)
   at Microsoft.TeamFoundation.Client.TfsConnection..ctor(Uri uri, String locationServiceRelativePath, IdentityDescriptor identityToImpersonate, ITfsRequestChannelFactory channelFactory)
   at Microsoft.TeamFoundation.Client.TfsConnection..ctor(Uri uri, String locationServiceRelativePath)
   at Microsoft.TeamFoundation.Client.TfsTeamProjectCollection..ctor(Uri uri, Boolean fromFactory)
   at Microsoft.TeamFoundation.Client.TfsTeamProjectCollection..ctor(Uri uri)
   at ConsoleApp2.Program.Main(String[] args) in D:\Code\ConsoleApp2\ConsoleApp2\Program.cs:line 11

D:\Code\ConsoleApp2\ConsoleApp2\bin\Debug\net5.0\ConsoleApp2.exe (process 22888) exited with code -532462766.
Press any key to close this window . . .
```

这错误信息，看着很无助啊。

## 求助

之前自己折腾过几次都没搞定，今天上 [万能的 Stackoverflow](https://stackoverflow.com/) 求助，立马有高人指路，建议用 4.x 的 .NET 版本进行尝试，因为相关的很多相关的实现都没有移植到 .Net 5 及之后的版本。

## 冲刺

然后死活在我的 DEMO 项目设置里找不到 .NET 4.x，尝试过单独下载 4.x 的开发包，尝试过在 Visual Studio Installer 中选上 4.x，到最后才发现是我选择的项目类型就不对，要选 Console App (.NET Framework) 而不是排在最前面的 Console Application，后者是基于 .NET 5+ 的！

终于可以用不那么陈旧的宇宙第一 IDE 来干活了，最近有空再继续学习下 C#。

## 其它

工具依赖的 TFS 相关库可以引用 `C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\TestTools\TeamExplorerClient` 路径下的，NuGet 上的 `Microsoft.TeamFoundationServer.ExtendedClient` 应该只支持 TFS2015 以及之后的版本，基于 REST API 的。

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>