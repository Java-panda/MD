- 邮件发送

  - ``` vb
    Sub SendEmail(To_Addr As String, Cc_Addr As String, Bcc_Addr As String, SubjectText As String, BodyText As String, AttachedObject As String)
        Dim OutlookObj As Outlook.Application
        Dim OutlookMailItem As Outlook.MailItem
        Set OutlookObj = New Outlook.Application
        Set OutlookMailItem = OutlookObj.CreateItem(olMailItem)
        On Error GoTo SendEmail_Failed
        With OutlookMailItem
            .To = To_Addr
            .cc = Cc_Addr
            .BCC = Bcc_Addr
            .Subject = SubjectText
            .Body = BodyText
    '        .Attachments.Add AttachedObject
        '.Send
        End With
        
        'send
    SendEmail_Sending:
        OutlookMailItem.Display
    
    '    For j = 1 To 200
    '        DoEvents
    '    Next
    '    SendKeys "%s", Wait:=True
        MsgBox "send mail end"
        Exit Sub
      
    SendEmail_Failed:
        MsgBox "send mail failure" & Err.Description
        Exit Sub
        
    End Sub
    Sub Send()
        SendEmail "liujianipanda@gmail.com", "liujianipanda@gmail.com;770233258@qq.com", "liujianipanda@gmail.com", "邮件标题", "邮件内容", "C:\Users\77023\Desktop\vba\head.ico"
    End Sub
    
    ```

  - 