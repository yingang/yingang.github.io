---
layout: post
title:  "用 VBA 检查 Outlook 遗忘添加附件的情况"
date:   2021-12-29 18:53:00 +0800
categories: office outlook
---

### 背景

* 今天上午发送一个收件人 20+ 的文档评审邮件中忘记添加附件了，自己觉得还是有点尴尬的。。。
* 所以中午在想，有没有可能让 Outlook 自动检查这种情况呢？毕竟邮件正文里还是有类似于“附件”之类的用词的。
* 下午上网搜索了下，找到了 CSDN 上的《[忘记帖附件？让Outlook自动提示](https://blog.csdn.net/peachpi/article/details/46976395)》，正是我需要的！

### 操作

1. 在 Outlook 界面上按 `Alt` + `F11`，弹出 VBA 的代码编辑窗口，左边双击选择 `ThisOutlookSession`；

   ![VBA](/images/outlook_vba.png)

2. 将后面的代码确认好之后复制进右边的空白窗格，保存退出；

3. 在 Outlook 的选项中找到信任中心设置；

   ![Trust Center](/images/outlook_trust_center.png)

4. 在宏设置中根据自己的需要选择合适的选项；

   ![Macro](/images/outlook_macro.png)

5. 安全起见我选择了“为所有宏提供通知”（电脑基本上每月只重启一次，因为操作系统推送的更新），就会在 Outlook 启动时弹出如下对话框，需要选择“启用宏”。

   ![Start](/images/outlook_start.png)

### 代码

在原帖的基础上稍微更新了下，既然 Outlook 已经内含了标题为空的检查，就只需要检查附件了：

~~~vb
Private Sub Application_ItemSend(ByVal Item As Object, Cancel As Boolean)
    
    If TypeName(Item) <> "MailItem" Then Exit Sub
   
    ' 识别是否转发和回复的邮件 (考虑中英文)
    Dim intFirstHistoryMsg As Integer
    intFirstHistoryMsg = InStr(Item.Body, "发件人:")
    Dim intFirstHistoryENMsg As Integer
    intFirstHistoryENMsg = InStr(Item.Body, "From:")
    If intFirstHistoryENMsg > 0 Then
        If (intFirstHistoryMsg = 0) _
            Or (intFirstHistoryMsg > 0 And intFirstHistoryENMsg < intFirstHistoryMsg) Then
            intFirstHistoryMsg = intFirstHistoryENMsg
        End If
    End If

    ' 提取新撰写的邮件正文和标题
    Dim strNewMsg As String
    If intFirstHistoryMsg = 0 Then
        strNewMsg = Item.Body + " " + Item.Subject
    Else
        strNewMsg = Left(Item.Body, intFirstHistoryMsg) + " " + Item.Subject
    End If

    ' 定义附件关键词
    Dim sSearchStrings(2) As String
    sSearchStrings(0) = "attach"
    sSearchStrings(1) = "enclose"
    sSearchStrings(2) = "附件"

    ' 在新撰写的邮件正文和标题中检索上述关键词
    Dim bFoundSearchString As Boolean
    Dim i As Integer
    For i = LBound(sSearchStrings) To UBound(sSearchStrings)
        If InStr(LCase(strNewMsg), sSearchStrings(i)) > 0 Then
            bFoundSearchString = True
            Exit For
        End If
    Next i

    ' 先排除邮件签名中的图片的影响，再进行用户提示
    If bFoundSearchString Then
        ' 我机器上签名里图片的文件名都类似于image00x.png/jpg，请按实际情况调整
        Dim bHasAttachment As Boolean
        For Each attach In Item.Attachments
            If Left(attach.FileName, 5) <> "image" Then
                bHasAttachment = True
            End If
        Next
        
        If bHasAttachment <> True Then
            If MsgBox("您的邮件可能缺少附件！" & vbCrLf & "是否仍要发送？", _
                vbYesNo + vbDefaultButton2 + vbExclamation, "缺少附件") = vbNo Then
                Cancel = True
                Exit Sub
            End If
        End If
    End If
End Sub
~~~

### 最后

其实我从学校毕业工作后的前面几年还用过 VB，真是久违了！以及这么多年过去了，至少 Outlook 自带的这个 VBA 开发环境似乎还是挺不现代的，也可能是我不太熟悉吧。之前网上有传言说微软要增加 Python 的支持，如果是真的也挺好！


> 更新 2022/01/07：之前测试不完整，刚发现我厂的标准签名档里也有【附件】这两个字。。。在提取邮件正文前，再搜索下签字档里靠前的关键词就行：

~~~
    If intFirstHistoryMsg = 0 Then
        intFirstHistoryMsg = InStr(Item.Body, "公司名称")
    End If
~~~

<script src="https://utteranc.es/client.js"
        repo="yingang/yingang.github.io"
        issue-term="pathname"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>