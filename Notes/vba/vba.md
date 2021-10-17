# #VBA笔记

* 基本 

  * 变量与常量的定义

  * ```vb
    'public修饰的变量Pvar整个工程可以共享
    Public Pvar As String
    '模块变量生命区的变量MVar当前模块共享
    Dim MVar As String
    '模块常量
    Const MP As String = "www.baidu.com"
    '工程常量
    Public Const PP As String = "www.baidu.com"
    
    '初始化各个变量
    Public Sub init()
    Pvar = "Pvar"
    MVar = "MVar"
    End Sub
    
    Sub run()
        Dim z As String
        init
        '方法内部的变量z只能用在方法内部
        z = "z"
        '方法内部所有层级变量均可访问
        Debug.Print x, y, z, url
        
        '静态变量,子程序结束后仍然存在,再次运行,仍然可以获取之前保留的值,但主程序结束则不会记住变量
        Static s As Integer
        s = s + 1
        Debug.Print s
    End Sub
    
    
    ```

  * 自定义类型,方法,函数的使用

  * ```vb
    '声明自定义类型
    Type Person
        name As String
        age As Integer
        gender As Boolean
    End Type
    
    '声明方法
    Sub PersonShow(p As Person)
        Debug.Print "name:" & p.name
        Debug.Print "age:" & p.age
        Debug.Print "gender:" & p.gender
    End Sub
    
    '声明函数
    Function PersonToString(p As Person) As String
        PersonToString = "name:" & p.name & vbCrLf & "age:" & p.age & vbCrLf & "gender:" & p.gender
    End Function
    
    '主程序
    Sub run()
    Dim p As Person
        p.name = "Jpanda"
        p.age = 25
        p.gender = True
        '调用方法
        PersonShow p
        '调用函数
        tostring = PersonToString(p)
        Debug.Print tostring
    End Sub
    ```

  * 枚举和Select文

  * ```vb
    Enum week
        MON = 1
        TUE = 2
        WED = 3
        THU = 4
        FRI = 5
        SAT = 6
        SUN = 7
    End Enum
    
    Sub CheckWeek(w As week)
        Select Case w
        Case MON
            Debug.Print "星期一"
        Case TUE
            Debug.Print "星期二"
        Case WED
            Debug.Print "星期三"
        Case THU
            Debug.Print "星期四"
        Case FRI
            Debug.Print "星期五"
        Case SAT
            Debug.Print "星期六"
        Case SUN
            Debug.Print "星期日"
        Case Else
            Debug.Print "非法星期"
        End Select
    End Sub
    
    Sub run()
    	CheckWeek (SUN)
    End Sub
    ```

  * 中断

  * ```vb
    'End用法
    	Type Person
        '自定义类型
        End Type
    
        Enum week
        '枚举
        End Enum
    
        Sub Method()
        '方法
        End Sub
    
        Function Fun()
        '函数
        End Function
    
        Select Case condition
        '选择
        End Select
    
        If
        '判断
        End If
    
        Sub Method()
            '直接结束方法,变量释放
            end
            '...
        End Sub
    
        With object
    		'对象的省略
    		.方法
    		.属性
        End With
    
    'Exit用法
    	'直接结束方法,变量不释放
    	Exit Sub
    	'结束For循环
        Exit For
    	'结束Do循环
        Exit Do
    
    'Error
        Sub run()
            '出现异常跳转
            On Error GoTo Exception1
            '出现异常忽视
            On Error Resume Next
            '...
    		Exit sub
            Exception1:
                Debug.Print Err.Number
                Debug.Print Err.Description
        End Sub
    
    ```

  * 

  * | 常量   | 作用     |
    | ------ | -------- |
    | vbCrLf | 回车换行 |

  * 

  * | 函数表达式                                                   | 作用         |
    | ------------------------------------------------------------ | ------------ |
    | Application.DisplayAlerts = False/True                       | 是否提示警告 |
    | Application.ScreenUpdating = False/True                      | 是否更新屏幕 |
    | Debug.Print ()                                               | 输出到控制台 |
    | Msgbox ()                                                    | 弹框         |
    | Application.Quit                                             | 退出App      |
    | Shell "B:\Notepad++\notepad++.exe", vbNormalFocus            | 打开App      |
    | Dim obj As Object<br/>Set obj = CreateObject("WScript.Shell")<br/>obj.Run "B:\Notepad++\readme.txt" | 打开文件     |

* 对话框

  * FileDialog

    ```vb
     '打开文件
     Dim fd As FileDialog
     Set fd = Application.FileDialog(msoFileDialogFilePicker)
     fd.Title = "JPG图片选择"
     fd.Filters.Clear
     '过滤器
     fd.Filters.Add "JPG图片", "*.jpg;*.jpeg"
     fd.Filters.Add "XML", "*.xml"
     '默认过滤器index
     fd.FilterIndex = 1
     fd.AllowMultiSelect = True
     fd.ButtonName = "确认"
     '默认打开路径
     fd.InitialFileName = "C:\Users\77023\Desktop\"
     '界面文件图标显示类型
     fd.InitialView = msoFileDialogViewThumbnail
     '代表excel=1480803660
     Debug.Print fd.Creator
     'FileDialog(msoFileDialogFilePicker)
     Debug.Print fd.Item
     'Microsoft Excel
     Debug.Print fd.Parent.name
     '-1选择完毕,0未选择文件
     If fd.Show = -1 Then
         For i = 1 To fd.SelectedItems().Count
             Debug.Print fd.SelectedItems(i)
         Next
     Else
         Debug.Print "未选择任何文件"
     End If
    ```
  
  * 
  
* Application函数

  * application

    * | 函数表达式 | 作用                   |
    | ---------- | ---------------------- |
      | UserName   | 获取当前用户名(Jpanda) |
    |            |                        |
  
* 工作表函数

  * Application.WorksheetFunction.

    * | 函数表达式          | 作用                   |
      | ------------------- | ---------------------- |
      | Max(arr)            | 求数组最大值           |
      | Match("target",arr) | 找出数组中某元素的索引 |
      |                     |                        |
      |                     |                        |
      |                     |                        |

* VBA函数

  * VBA.
  
    * | 函数表达式                         | 作用                      |
      | ---------------------------------- | ------------------------- |
      | Format("2020/01/01", "yyyy-MM-dd") | 格式化字符串              |
      | Split("2020/01/01", "/")(0)        | 分割字符串                |
      | Ucase(String)/Lcase(String)        | 大小写转换                |
      | Left(String, Len)                  | 取左边Len位               |
      | Right(String, Len)                 | 取右边Len位               |
      | Mid(String, Start, Len)            | 中间Start开始取Len位      |
      | InStr(String,Target)               | 查找并返回索引            |
      | Len(String)                        | 返回字符串长度            |
      | Trim(String)                       | 修剪字符串                |
      | Replace(Original,Finder,Replacer)  | 替换字符串内容            |
      | Int(Rnd()*(Max-Min+1)-Min)         | 生成[MIn,Max]之间的随机数 |
      
      
  
* 单元格操作

  * | 函数表达式                                   | 作用           |
    | -------------------------------------------- | -------------- |
    | Range("A1:C10")                              | 定义范围       |
    | Range("A" & i)                               | 动态单元格     |
    | Cells(x,y)                                   | 单元格         |
    | Cells                                        | 所有单元格     |
    | Cells(Rows.Count, 1).End(xlUp).Row           | 最后非空行行号 |
    | Cells(1, Columns.Count).End(xlToLeft).Column | 最后非空列列号 |

* 高级操作

  * | 功能           | 语法                                                 |
    | :------------- | ---------------------------------------------------- |
    | 注册注册表     | SaveSetting("appname", "section", "key", "value")    |
    | 获取注册表     | GetSetting("appname", "section", "key","default")    |
    | 删除注册表     | DeleteSetting("appname", "section", "key","default") |
    | 获取同类注册表 | GetAllSettings("appname", "section")                 |
    |                |                                                      |

  * 

* 表操作

  * 通过名称增加表到最后

    * ```vb
      Public Sub AddSheetByNameAfter(sheetName As String)
          Dim st, addSt As Worksheet
          Dim existSheet As Boolean
          existSheet = False
          For Each st In ActiveWorkbook.Sheets
              If st.name = sheetName Then
                 existSheet = True
              End If
          Next
          If Not existSheet Then
              Set addSt = ActiveWorkbook.Worksheets.add(, Sheets(Sheets.Count))
              addSt.name = sheetName
          Else
              MsgBox ("SheetName" + sheetName + ":Exist!")
          End If
      End Sub
      ```

    * 

  * 通过名称删除表

    * ```vb
      Public Sub DeleteSheetByName(sheetName As String)
          Dim st As Worksheet
          For Each st In ActiveWorkbook.Sheets
              If st.name = sheetName Then
                  Application.DisplayAlerts = False
                  st.Delete
                  Application.DisplayAlerts = True
                  Exit For
              End If
          Next
          MsgBox ("SheetName" + sheetName + ":Is Not Exist!")
      End Sub
      ```

  * 复制Sheet1到最后并改名

    * ```vb
      Public Function CopySheetBySheet1(sheetName As String) As Boolean
          Dim st, addSt As Worksheet
          Dim existSheet As Boolean
          existSheet = False
          For Each st In ActiveWorkbook.Sheets
              If st.name = sheetName Then
                 existSheet = True
              End If
          Next
          If Not existSheet Then
              ActiveWorkbook.Worksheets("Sheet1").Copy After:=Sheets(Sheets.Count)
              ActiveSheet.name = sheetName
              CopySheetBySheet1 = True
          Else
              CopySheetBySheet1 = False
          End If
      End Function
      ```

  * 复制Sheet1到最后并改名,改名规则为某个前缀加序号递增

    * ```vb
      Public Sub addSheetBySort(prefix As String)
          Dim i As Integer
          Dim success As Boolean
          i = 1
          success = False
          Do While (Not success)
              success = CopySheetBySheet1(prefix & i)
              i = i + 1
          Loop
      End Sub
      ```

      

* 控制语句

  * If

    * ```vb
      Dim i As Integer
      i = 3
      If i = 1 Then
          Debug.Print (1)
      ElseIf i = 2 Then
          Debug.Print (2)
      Else
          Debug.Print (3)
      End If
      ```

  * For

    * ```vb
      Dim i, sum As Integer
      For i = 1 To 200
          sum = sum + i
          If i = 100 Then
              Exit For
          End If
      Next
      Debug.Print (sum)
      ```

  * For Each In

    * ```vb
      Dim st As Worksheet
      For Each st In ThisWorkbook.Sheets
          Debug.Print (st.name)
      Next
      ```

  * Do While() Loop

    * ```vb
      Dim i, sum As Integer
      Do While (i <= 100)
          sum = sum + i
          i = i + 1
      Loop
      Debug.Print (sum)
      ```

  * Do Until() Loop

    * ```vb
      Dim i, sum As Integer
      Do Until (i > 100)
          sum = sum + i
          i = i + 1
      Loop
      Debug.Print (sum)
      ```

  * Do Loop Until()

    * ```vb
      Dim i, sum As Integer
      Do
          sum = sum + i
          i = i + 1
      Loop Until (i > 100)
      Debug.Print (sum)
      ```
    
  * While Wend

    * ```vb
      Dim i, sum As Integer
      While i <= 100
        sum = sum + i
          i = i + 1
      Wend
      Debug.Print (sum)
      ```

* 组件

  * 窗体

    * 方法
  
    ```vb
    '显示窗口0=非独占,1=独占
    win.Show(0/1)
    '隐藏窗口
    win.Hide
    '卸载窗口
    Unload win
    ```
    
    * 属性
    
    ```vb
    '标题
    win.Caption
    ```
  * 标签Label

    * 方法
  
    ```vb
    
    ```
    
    * 属性
    
    ```vb
    '内容
    Caption
    ```
  * 文本框TextBox
  
    * 方法
  
    ```vb
    '聚焦
    SetFocus
    ```
    
    * 属性
    
    ```vb
    '文本框的值
    Text
    '文本框可输入最大位数
    MaxLength
    '从第一位起被选中
    SelStart=0
    '被选中的长度
    SelLength=Len(.Text)
    ```
  * 列表框ListBox
  
    * 方法
  
    ```vb
    '增
    '设定值,不可再修改
    .RowSource = "SheetName!A1:A2"
    '单值添加.可修改
    .AddItem(Item)
    '批量赋值
    .List=Range("A1:A5").Value
    '多列增加
    .ColumnCount = 2
    .ColumnWidths = "40;90"
    .AddItem
        .List(0, 0) = "功能ID"
        .List(0, 1) = "功能解释"
    '删
    '删除第一个元素
    .RemoveItem (0)
    '改
    '查
    '取出选中的值,若未选中任何值则返回""
    .Text
    '取出选中的值,若未选中任何值则Null异常
    .Value
    '取出指定位置的值
    .List(.ListIndex)
    '获取多行ListBox值
    For i = 0 To .ListCount - 1
        If .Selected(i) Then
            MsgBox .List(i)
        End If
    Next
    ```
    
    * 属性
    
    ```vb
    '第一个元素被选中
    .ListIndex=0
    '元素个数
    .ListCount
    '多列ListBox
    .
    ```
  * ListView
  
    * 方法
  
    ```vb
    With f2.ListView1
        .View = lvwReport
        .LabelEdit = lvwManual
        .HideSelection = False
        .AllowColumnReorder = True
        .FullRowSelect = True
        .Gridlines = True
        .ColumnHeaders.Add , "姓名", "姓名"
        .ColumnHeaders.Add , "年龄", "年龄"
        .ColumnHeaders.Add , "性别", "性别"
        
        Dim rng As Range
        For Each rng In Sheets("f2").Range("J1:J4")
            With f2.ListView1.ListItems.Add
                .Text = rng.Value
                .SubItems(1) = rng.Offset(0, 1).Value
                .SubItems(2) = rng.Offset(0, 2).Value
            End With
        Next
        
    End With
    
    
    MsgBox .SelectedItem.Index & "-" & .SelectedItem.SubItems(1)
    
    ```
    
    * 属性
    
    ```vb
    
    ```
  * 窗体
  
    * 方法
  
    ```vb
    
    ```
    
    * 属性
    
    ```vb
    
    ```
  * 窗体
  
    * 方法
  
    ```vb
    
    ```
    
    * 属性
    
    ```vb
    
    ```
  * 窗体
  
    * 方法
  
    ```vb
    
    ```
    
    * 属性
    
    ```vb
    
    ```
  * 窗体
  
    * 方法
  
    ```vb
    
    ```
    
    * 属性
    
    ```vb
    
    ```
  * 窗体
  
    * 方法
  
    ```vb
    
    ```
    
    * 属性
    
    ```vb
    
    ```
  * 窗体
  
    * 方法
  
    ```vb
    
    ```
    
    * 属性
    
    ```vb
    
    ```
  * 窗体
  
    * 方法
  
    ```vb
    
    ```
    
    * 属性
    
    ```vb
    
    ```
  * 窗体
  
    * 方法
  
    ```vb
    
    ```
    
    * 属性
    
    ```vb
    
    ```
    
    
  
* 数组

  * 定义

    * 常量数组
  
    * ```vb
      Dim arr()
      arr = Array("a", "b", "c")
      ```
  
    * 静态数组
  
    * ```vb
      Dim arr0()
      Dim arr1(4)
      Dim arr2(1 To 5)
      Dim arr3(1 To 5,1 To 2)
      ```
  
    * 数组赋值和使用
  
    * ```vb
      '给数组赋值
      arr = Range("A1:D10")
      arr(index)=value
      '使用数组元素
      Range("A1:D10") = arr
      Range("A1") = arr(index)
      '动态改变数组大小
      ReDim Preserve arr(1 To n)
      ```
      
    * 数组界限
    
    * ```vb
    LBound(arr, 1)
    UBound(arr, 1)
      LBound(arr, 2)
      UBound(arr, 2)
      ```
    
    * 清空数组
    
    * ```vb
      Erase arr
      ```
  
* 文件操作

  * 读写

    * ```vb
      '读取文件为字符串
      Public Function ReadFileToString(filename As String) As String
        Dim ret, temp As String
          Dim filenum As Integer
          '自动获取一个未占用文件号
          filenum = FreeFile
          '打开文件
          Open filename For Input As #filenum
          Do While Not EOF(filenum)
              temp = Input(1, filenum)
              ret = ret + temp
          Loop
          '关闭文件
          Close #filenum
          ReadFileToString = ret
      End Function
      
      '读取文件每行放入数组
      Public Function ReadFileToArray(filename As String) As String()
          Dim ret() As String, temp As String
          Dim filenum, lines As Integer
          filenum = FreeFile
          lines = 0
          Open filename For Input As #filenum
          Do While Not EOF(filenum)
              lines = lines + 1
              '动态改变数组大小
              ReDim Preserve ret(1 To lines) As String
              Line Input #filenum, ret(lines)
          Loop
          Close #filenum
          ReadFileToArray = ret
      End Function
      
      '写入一句话到文件追加或者新建,Optional为可选参数,不填写使用默认值
      Public Sub WriteStringToFile(filename As String, outputstring As String, Optional useappend As Boolean = True)
          Dim ret() As String, temp As String
          Dim filenum, lines As Integer
          filenum = FreeFile
       
          If useappend Then
              Open filename For Append As #filenum
              Write #filenum, outputstring
          Else
              Open filename For Output As #filenum
              Write #filenum, outputstring
          End If
          
          Close #filenum
      End Sub
      
      '读写二进制小文件
      Public Sub CopyBinaryFile(source As String, target As String)
          Open source For Binary As #1
          Open target For Binary As #2
          Dim i As Long, bte As Byte
          bte = 255
          For i = 1 To LOF(1)
              Get #1, , bte
              Put #2, LOF(2) + 1, bte
          Next
          Close #1
          Close #2
      End Sub
      ```
    
  * Adodb.Stream
  
    * ```vb
      Sub AdodbStream()
          '引用法需要打开MicroSoft Activx Data Objects
          Dim fs As New Stream
          '绑定法
          'Dim fs As Object
      	'Set fs = CreateObject("adodb.stream")
              
          '文本类型
          fs.Type = adTypeText
          '读写模式
          fs.Mode = adModeReadWrite
          '编码格式
          fs.Charset = "UTF-8"
          '设置行分隔符为回车换行
          fs.LineSeparator = adCRLF
          '开流
          fs.Open
      	'通过文件加载流
          fs.LoadFromFile "ReadFilePath"
          '循环获取每一行
          While Not fs.EOS
              '-1 读取全部,-2逐行读取
              Line = fs.ReadText(-2)
              Debug.Print Line
          Wend
          '保存流到文件
          fs.SaveToFile "WriteFilePath", adSaveCreateOverWrite
          '关流
          fs.Close
          Set fs = Nothing
      End Sub
      ```
  
    * 
  
* FTP

  * 使用winscp.com下载文件

    * ```vb
      Sub sftpget()
          Dim winscppath, env_addr, env_user, env_passwd, localftp, cfgfile, retfile, ftpfile As String
          'winscp路径
          winscppath = "C:\Program Files (x86)\WinSCP\winscp.com"
          'IP地址
          env_addr = "192.168.131.102"
          '用户名
          env_user = "root"
          '用户密码
          env_passwd = "root"
          '本地存储路径
          localftp = "C:\Users\77023\Desktop\test\"
          '配置文件路径
          cfgfile = "C:\Users\77023\Desktop\test\config.txt"
          '结果文件名
          retfile = "ret.txt"
          
          '下载目标文件路径
          ftpfile = "/opt/SogouQ1.txt"
          
          '生成脚本文件
          Open cfgfile For Output As #1
          '打开连接
          Print #1, "open sftp://" & env_user & ":" & env_passwd & "@" & env_addr
          '下载文件
          Print #1, "get " & ftpfile & " " & localftp & retfile
          '关闭脚本
          Print #1, "close"
          Close #1
          
          '执行脚本文件
          Shell winscppath & " " & "/script=" & cfgfile, 0
          '等待下载完成
          Do While Dir(localftp & retfile) = Empty
              DoEvents
          Loop
          MsgBox "下载完成"
      End Sub
      ```

* 类模块

  * 本类Panda

    * ```vb
      Private clsAuthor As String
      Private clsAge As Integer
      
      Public Sub DeleteSheetByName(sheetName As String)
          Dim st As Worksheet
          For Each st In ActiveWorkbook.Sheets
              If st.name = sheetName Then
                  Application.DisplayAlerts = False
                  st.Delete
                  Application.DisplayAlerts = True
                  Exit For
              End If
      	Next
      	MsgBox ("SheetName" + sheetName + ":Is Not Exist!")
      End Sub
      
      Public Sub AddSheetByNameAfter(sheetName As String)
          Dim st, addSt As Worksheet
          Dim existSheet As Boolean
          existSheet = False
          For Each st In ActiveWorkbook.Sheets
              If st.name = sheetName Then
                  existSheet = True
              End If
          Next
          If Not existSheet Then
          Set addSt = ActiveWorkbook.Worksheets.add(, Sheets(Sheets.Count))
              addSt.name = sheetName
          Else
              MsgBox ("SheetName" + sheetName + ":Exist!")
          End If
      End Sub
      
      Public Function add(a As Integer, b As Integer)
          add = a + b
      End Function
      
      Property Get author() As Variant
          author = clsAuthor
      End Property
      
      Property Let author(ByVal vNewValue As Variant)
          clsAuthor = vNewValue
      End Property
      
      Public Property Get age() As Variant
          age = clsAge
      End Property
      
      Public Property Let age(ByVal vNewValue As Variant)
          clsAge = vNewValue
      End Property
      
      Public Sub InitiateProperties(author As String, age As Integer)
          clsAuthor = author
          clsAge = age
      End Sub
      
      Private Sub Class_Initialize()
          Debug.Print ("Panda Initialize...")
      End Sub
      
      Private Sub Class_Terminate()
          Debug.Print ("Panda Terminate...")
      End Sub
      ```
    
  * 工厂类Factory
    
    * ```vb
        Public Function CreatePanda(author As String, age As Integer) As panda
            Set CreatePanda = New panda
            CreatePanda.InitiateProperties author:=author, age:=age
        End Function
        ```
    
  * 调用类
  
    * ```vb
      Sub main()
          Dim pd As panda
          Dim factory As factory
          Set factory = New factory
          Set pd = factory.CreatePanda("lj", 26)
          Debug.Print (pd.age)
          Debug.Print (pd.author)
          pd.age = 27
          pd.author = "panda"
          Debug.Print (pd.age)
          Debug.Print (pd.author)
          pd.AddSheetByNameAfter sheetName:="test"
          pd.DeleteSheetByName sheetName:="ss"
          Debug.Print (pd.add(1, 2))
      End Sub
      ```
  
* Excel功能集对应excel操作

  * 设置打印区域

    * ```vb
      ActiveSheet.PageSetup.PrintArea = Cells(1, 1).CurrentRegion.Address + ":" + Cells(row, col).CurrentRegion.Address
      ```

* ADO数据库操作

  1. 连接PostgreSQL

  2. ```vb
    Sub postgreSQLConn()
        '需要打开MicroSoft Activx Data Objects
        Dim conn As New ADODB.Connection
        Dim rs As New ADODB.Recordset
        Dim dataSource As String
        Dim userName As String
        Dim password As String
        Dim DBname As String
        Dim ConnProperty As String
        Dim SQL As String
        Dim arr()
        
        dataSource = "PostgreSQLODBC"
        userName = "postgres"
        password = "Panda5201314"
        DBname = "panda"
        'DSN=PostgreSQLODBC;UID=postgres;PWD=Panda5201314;Database=panda
        ConnProperty = "DSN=" & dataSource & ";" & "UID=" & userName & ";" & "PWD=" & password & ";" & "Database=" & DBname
        
        '开启连接
        conn.Open ConnProperty
    
        '検索
        SQL = "select * from student where sid='01'"
        Set rs = conn.Execute(SQL)
    
        '遍历结果集
        Do While (Not rs.EOF)
            Debug.Print (rs.Fields("sid").Value)
            Debug.Print (rs.Fields(1).Value)
            Debug.Print (VBA.Split(rs.Fields(2).Value, "/")(0))
            rs.MoveNext
        Loop
        
        '将数据集写入Excel表中
        'arr = rs.GetRows
        '横式
        'Sheet2.Range("a1").Resize(UBound(arr, 2) + 1, UBound(arr, 1) + 1) = Application.WorksheetFunction.Transpose(arr)
        '列式
        'Sheet2.Range("a1").Resize(UBound(arr, 1) + 1, UBound(arr, 2) + 1) = arr
        
        '关闭资源
        rs.Close
        conn.Close
        Set rs = Nothing
        Set cn = Nothing
        
        '削除
        'Call conn.Execute(SQL)
        
        '修正
        'Call conn.Execute(SQL)
    End Sub
    ```