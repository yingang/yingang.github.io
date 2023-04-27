---
layout: post
title:  "古董 Win7 上跑 .Net 应用的 TLS 版本问题"
date:   2023-04-27 20:36:00 +0800
categories: c# tls
---

## 背景

最近捣鼓了一个查询 TFS 上工作项的小工具，查询完工作项之后会自动发个邮件出来给相关的同事，然后问题来了，在我工作电脑上可以正常发送，在我一台老测试机上就发送不了，报如下的错误。

前几天找 IT 同事一起确认过是不是哪里需要配置下，也没搞定，怀疑是测试机软件环境的问题（比如还是 Win7），但没有具体的怀疑方向。

```plaintext
System.Net.Mail.SmtpException: 发送邮件失败。 ---> System.IO.IOException: 由于远程方已关闭传输流，身份验证失败。
   在 System.Net.Security.SslState.StartReadFrame(Byte[] buffer, Int32 readBytes, AsyncProtocolRequest asyncRequest)
   在 System.Net.Security.SslState.StartReceiveBlob(Byte[] buffer, AsyncProtocolRequest asyncRequest)
   在 System.Net.Security.SslState.CheckCompletionBeforeNextReceive(ProtocolToken message, AsyncProtocolRequest asyncRequest)
   在 System.Net.Security.SslState.ForceAuthentication(Boolean receiveFirst, Byte[] buffer, AsyncProtocolRequest asyncRequest)
   在 System.Net.Security.SslState.ProcessAuthentication(LazyAsyncResult lazyResult)
   在 System.Threading.ExecutionContext.RunInternal(ExecutionContext executionContext, ContextCallback callback, Object state, Boolean preserveSyncCtx)
   在 System.Threading.ExecutionContext.Run(ExecutionContext executionContext, ContextCallback callback, Object state, Boolean preserveSyncCtx)
   在 System.Threading.ExecutionContext.Run(ExecutionContext executionContext, ContextCallback callback, Object state)
   在 System.Net.TlsStream.ProcessAuthentication(LazyAsyncResult result)
   在 System.Net.TlsStream.Write(Byte[] buffer, Int32 offset, Int32 size)
   在 System.Net.PooledStream.Write(Byte[] buffer, Int32 offset, Int32 size)
   在 System.Net.Mail.SmtpConnection.Flush()
   在 System.Net.Mail.ReadLinesCommand.Send(SmtpConnection conn)
   在 System.Net.Mail.EHelloCommand.Send(SmtpConnection conn, String domain)
   在 System.Net.Mail.SmtpConnection.GetConnection(ServicePoint servicePoint)
   在 System.Net.Mail.SmtpClient.GetConnection()
   在 System.Net.Mail.SmtpClient.Send(MailMessage message)
```
## 调查

今天下班前有会空，就把问题捡起来继续调查了。

先尝试用 WireShark 抓了下包，有问题的机器上是这样的：

```plaintext
72424	2023-04-27 15:19:03.930917	CLIENT1	SERVER	SMTP	64	C: STARTTLS
72425	2023-04-27 15:19:03.931315	SERVER	CLIENT1	SMTP	83	S: 220 2.0.0 SMTP server ready
72426	2023-04-27 15:19:03.931555	CLIENT1	SERVER	TLSv1	185	Client Hello
72427	2023-04-27 15:19:03.932556	SERVER	CLIENT1	TCP	60	25 → 35467 [FIN, ACK] Seq=402 Ack=159 Win=64082 Len=0

```

而没问题的机器上是这样的：

```plaintext
36302	35.125573	CLIENT2	SERVER	SMTP	64	C: STARTTLS
36303	35.127572	SERVER	CLIENT2	SMTP	83	S: 220 2.0.0 SMTP server ready
36304	35.129940	CLIENT2	SERVER	TLSv1.2	239	Client Hello
36305	35.136641	SERVER	CLIENT2	TCP	1514	25 → 62789 [ACK] Seq=335 Ack=214 Win=64027 Len=1460 [TCP segment of a reassembled PDU]
36306	35.136641	SERVER	CLIENT2	TCP	1514	25 → 62789 [ACK] Seq=1795 Ack=214 Win=64027 Len=1460 [TCP segment of a reassembled PDU]
36307	35.136641	SERVER	CLIENT2	TCP	1514	25 → 62789 [ACK] Seq=3255 Ack=214 Win=64027 Len=1460 [TCP segment of a reassembled PDU]
36308	35.136641	SERVER	CLIENT2	TLSv1.2	939	Server Hello, Certificate, Server Key Exchange, Server Hello Done
```

问题机器上是 TLSv1，而且 Hello 之后就被服务器主动断开了连接，而正常机器上是 TLSv1.2，随后也有正常的一些交互信息，问题应该就在这了！

然后一开始先是怀疑是不是两台机器使用的 .Net 版本不一样啊（原理上不应该怀疑，原谅一下小白），根据[这里](https://learn.microsoft.com/zh-cn/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed)的介绍，找了个工具 [DotNetVersions](https://github.com/jmalarcon/DotNetVersions)，问题机器是：

```plaintext
2.0.50727.5420 Service Pack 2
3.0.30729.5420 Service Pack 2
3.5.30729.5420 Service Pack 1
4.0.0.0
4.8.03761
```

而正常机器是：

```plaintext
4.0.0.0
4.8.04084
```

好像也没啥大问题。然后继续查到[这个帖子](https://www.cnblogs.com/Charltsing/p/Net4TLS12.html)，说修改注册表就可以让 .Net 应用使用更新的 TLS 版本：

```plaintext
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
"SchUseStrongCrypto"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\.NETFramework\v4.0.30319]
"SchUseStrongCrypto"=dword:00000001
```

试了下，没用（后面还有相关的继续尝试）。

然后看到[这个帖子](https://blog.csdn.net/ken_io/article/details/104762653)建议可以使用 [MailKit](https://github.com/jstedfast/MailKit)（不想用那个 System.Web.Mail.SmtpMail），晚点再试吧，再然后看到微软官网的几个帖子：

* [.NET Framework 中的传输层安全性 (TLS) 最佳做法](https://learn.microsoft.com/zh-cn/dotnet/framework/network-programming/tls)
  
  为确保 .NET Framework 应用程序的安全性，TLS 版本不应 被硬编码。 .NET Framework 应用程序应使用操作系统 (OS) 支持的 TLS 版本。

  对于 TLS 1.2，在你的应用上面向 .NET Framework 4.7 或更高版本，在 WCF 应用上面向 .NET Framework 4.7.1 或更高版本。

* [如何在客户端上启用 TLS 1.2](https://learn.microsoft.com/zh-cn/mem/configmgr/core/plan-design/security/enable-tls-1-2-client)

  Windows 8.1、Windows Server 2012 R2、Windows 10、Windows Server 2016 及更高版本的 Windows 本机支持 TLS 1.2，以便通过 WinHTTP 进行客户端与服务器通信。

  NET Framework 4.6.2 及更高版本支持 TLS 1.1 和 TLS 1.2。 确认注册表设置，但无需进行其他更改。

  ```plaintext
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v2.0.50727]
      "SystemDefaultTlsVersions" = dword:00000001
      "SchUseStrongCrypto" = dword:00000001
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
      "SystemDefaultTlsVersions" = dword:00000001
      "SchUseStrongCrypto" = dword:00000001
  ```

  设置 SchUseStrongCrypto 允许 .NET 使用 TLS 1.1 和 TLS 1.2。 设置 SystemDefaultTlsVersions 允许 .NET 使用 OS 配置。

  编注：如果是 64 位平台跑 32 位应用，还需要改 `Wow6432Node` 下的。
	  
看着都挺有道理的，但试下来，还是没用。。。谨遵医嘱，重启也没有效果。甚至还试了下面这个 WinHttp 相关的，重启后还给我自动改回去了。

  ```plaintext
  HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\WinHttp\
      DefaultSecureProtocols = (DWORD): 0xAA0
  HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Internet Settings\WinHttp\
      DefaultSecureProtocols = (DWORD): 0xAA0
  ```

## MailKit

虽然还是觉得可能自己哪里没弄对，但折腾不起了，还是改用第三方库吧，照猫画虎后，碰到第一个问题，正常机器都发不出去邮件了！

```plaintext
MailKit.Security.SslHandshakeException: An error occurred while attempting to establish an SSL or TLS connection
```

如下的代码里不是指明了 SSL 是 true 么，咋还不对呢？

```csharp
client.Connect (host, 25, true);
```

被错误信息引导到官网上的 [FAQ](https://github.com/jstedfast/MailKit/blob/master/FAQ.md#ssl-handshake-exception)，似懂非懂，结合万能的 [Stackoverflow](https://stackoverflow.com/questions/57008081/when-is-it-necessary-to-enable-ssl-on-mailkit) 才明白：

```plaintext
When you specify useSsl as true in MailKit's Connect method, it assumes that you meant to use an SSL-wrapped connection (which is different from STARTTLS).

To make this less confusing, MailKit has a Connect method that takes a SecureSocketOptions argument instead of a bool.
```

把代码改成下面这样，正常机器就可以发出邮件了：

```csharp
client.Connect(host, 25, MailKit.Security.SecureSocketOptions.StartTls);
```

然后把程序部署到测试机器上，继续报错，我太难了。。。

```plaintext
The host name did not match the name given in the server's SSL certificate
```

还是回到上面那个 FAQ，图省事的话，加上如下这句就好了，相当于忽略证书相关的错误（作者说了：生产环境强烈不建议！）：

```csharp
client.ServerCertificateValidationCallback = (s, c, h, e) => true;
```

## 尾声

我是该应该继续使用 MailKit，还是该直接给测试机升级个 Win10？

可能两个都得做吧。

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>