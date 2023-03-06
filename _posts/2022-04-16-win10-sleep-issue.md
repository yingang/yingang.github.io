---
layout: post
title:  "[未解决] WIN10 的睡眠问题"
date:   2022-04-16 22:36:00 +0800
categories: windows
---

感觉上 Win7 之后的新 Windows 版本，搞不明白的地方越来越多了，也不太方便查到相关的一些资料。

比如系统睡眠后，从系统日志来看，似乎会时不时地自动唤醒，不知道机理是什么，先把过程中一些无谓的尝试先记录下来吧。

这是睡眠时间段内的一些系统日志记录：

    日志名称:          System
    来源:            Service Control Manager
    日期:            2022/4/16 22:37:05
    事件 ID:         7040
    任务类别:          无
    级别:            信息
    关键字:           经典
    用户:            SYSTEM
    计算机:           ***
    描述:
    Background Intelligent Transfer Service 服务的启动类型从 按需启动 更改为 自动启动。
    事件 Xml:
    <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
      <System>
        <Provider Name="Service Control Manager" Guid="{555908d1-a6d7-4695-8e1e-26931d2012f4}" EventSourceName="Service Control Manager" />
        <EventID Qualifiers="16384">7040</EventID>
        <Version>0</Version>
        <Level>4</Level>
        <Task>0</Task>
        <Opcode>0</Opcode>
        <Keywords>0x8080000000000000</Keywords>
        <TimeCreated SystemTime="2022-04-16T14:37:05.9809298Z" />
        <EventRecordID>34794</EventRecordID>
        <Correlation />
        <Execution ProcessID="956" ThreadID="16888" />
        <Channel>System</Channel>
        <Computer>***</Computer>
        <Security UserID="S-1-5-18" />
      </System>
      <EventData>
        <Data Name="param1">Background Intelligent Transfer Service</Data>
        <Data Name="param2">按需启动</Data>
        <Data Name="param3">自动启动</Data>
        <Data Name="param4">BITS</Data>
      </EventData>
    </Event>

    日志名称:          System
    来源:            Service Control Manager
    日期:            2022/4/16 22:41:11
    事件 ID:         7040
    任务类别:          无
    级别:            信息
    关键字:           经典
    用户:            SYSTEM
    计算机:           ***
    描述:
    Background Intelligent Transfer Service 服务的启动类型从 自动启动 更改为 按需启动。
    事件 Xml:
    <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
      <System>
        <Provider Name="Service Control Manager" Guid="{555908d1-a6d7-4695-8e1e-26931d2012f4}" EventSourceName="Service Control Manager" />
        <EventID Qualifiers="16384">7040</EventID>
        <Version>0</Version>
        <Level>4</Level>
        <Task>0</Task>
        <Opcode>0</Opcode>
        <Keywords>0x8080000000000000</Keywords>
        <TimeCreated SystemTime="2022-04-16T14:41:11.2974432Z" />
        <EventRecordID>34797</EventRecordID>
        <Correlation />
        <Execution ProcessID="956" ThreadID="15204" />
        <Channel>System</Channel>
        <Computer>***</Computer>
        <Security UserID="S-1-5-18" />
      </System>
      <EventData>
        <Data Name="param1">Background Intelligent Transfer Service</Data>
        <Data Name="param2">自动启动</Data>
        <Data Name="param3">按需启动</Data>
        <Data Name="param4">BITS</Data>
      </EventData>
    </Event>

下面是按网上能看到的一些方法来检查，也没检查出个所以然。

    C:\Users\gang.yin>powercfg /lastwake
    唤醒历史记录计数 - 0

    C:\Users\gang.yin>powercfg -devicequery wake_programmable
    符合 HID 标准的供应商定义设备 (001)
    ELAN WBF Fingerprint Sensor
    符合 HID 标准的用户控制设备 (007)
    符合 HID 标准的供应商定义设备 (005)
    HID-compliant mouse (004)

    PS C:\Users\gang.yin> Get-ScheduledTask | where {$_.settings.waketorun}

    TaskPath                                       TaskName                          State
    --------                                       --------                          -----
    \Microsoft\Windows\.NET Framework\             .NET Framework NGEN v4.0.30319... Disabled
    \Microsoft\Windows\.NET Framework\             .NET Framework NGEN v4.0.30319... Disabled
    \Microsoft\Windows\InstallService\             WakeUpAndContinueUpdates          Disabled
    \Microsoft\Windows\InstallService\             WakeUpAndScanForUpdates           Disabled
    \Microsoft\Windows\SharedPC\                   Account Cleanup                   Disabled
    \Microsoft\Windows\UpdateOrchestrator\         Reboot_AC                         Disabled
    \Microsoft\Windows\UpdateOrchestrator\         Schedule Wake To Work             Disabled

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>