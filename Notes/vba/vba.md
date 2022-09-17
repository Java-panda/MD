# #VBA笔记

基本 

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

* | 声明                 | 含义             |
  | -------------------- | ---------------- |
  | Option Explicit      | 强制要求变量声明 |
  | On Error Resume Next | 出错后继续执行   |
  | On Error Goto XXX    | 出错后跳转到     |

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

Application函数

* application

  * | 函数表达式 | 作用                   |
  | ---------- | ---------------------- |
    | UserName   | 获取当前用户名(Jpanda) |
  |            |                        |

工作表函数

* Application.WorksheetFunction.

  * | 函数表达式          | 作用                   |
    | ------------------- | ---------------------- |
    | Max(arr)            | 求数组最大值           |
    | Match("target",arr) | 找出数组中某元素的索引 |
    | Int(1.5)            | 1                      |
    | Fix(-1.5)           | -2                     |
    | Hex(15)             | 十六进制               |
    | Oct(15)             | 八进制                 |
    | Cint(A)             | 转十进制               |

VBA函数

* VBA.

  * | 函数表达式                                | 作用                               |
    | ----------------------------------------- | ---------------------------------- |
    | Format("2020/01/01", "yyyy-MM-dd")        | 格式化字符串                       |
    | Split("2020/01/01", "/")(0)               | 分割字符串                         |
    | Ucase(String)/Lcase(String)               | 大小写转换                         |
    | Left(String, Len)                         | 取左边Len位                        |
    | Right(String, Len)                        | 取右边Len位                        |
    | Mid(String, Start, Len)                   | 中间Start开始取Len位               |
    | InStr(String,Target)                      | 查找并返回索引                     |
    | Len(String)                               | 返回字符串长度                     |
    | Trim(String)                              | 修剪字符串                         |
    | Replace(Original,Finder,Replacer)         | 替换字符串内容                     |
    | Randomize<br />Int(Rnd()*(Max-Min+1)-Min) | 生成[MIn,Max]之间的随机数          |
    | Chr(65)                                   | A                                  |
    | Asc("A")                                  | 65                                 |
    | StrConv("ABC", vbLowerCase)               | 字符串转化                         |
    | String(n, "☆")                            | 快速复制n个字符串                  |
    | Space(n)                                  | 快速n个空格                        |
    | StrComp("a", "b", vbTextCompare)          | 字符串比较相同为0,小于为-1,大于为1 |
    | InStrRev("abcadefga", "a")                | 倒序查找第一个a                    |
    | InStr("abcadefga", "a")                   | 正序查找第一个a                    |
    | Val("123")                                | 字符串转数字                       |
    | Filter(arr, "a")                          | 筛选出数组中包含"a"的新数组        |
    | Join(arr, ",")                            | 用","将数组连接起来                |
    
  
* 时间函数

  * | 函数                                      | 含义                                |
    | ----------------------------------------- | ----------------------------------- |
    | Date                                      | 年月日                              |
    | Time                                      | 时分秒                              |
    | Now                                       | 年月日 时分秒                       |
    | Year(Date)                                | 年                                  |
    | Month(Date)                               | 月                                  |
    | Day(Date)                                 | 日                                  |
    | Hour(Now)                                 | 时                                  |
    | Minute(Now)                               | 分                                  |
    | Second(Now)                               | 秒                                  |
    | Weekday(Date,vbMonday)                    | 获取星期序号                        |
    | WeekDayName(Weekday)                      | 获取星期名                          |
    | DateValue("日期字符串")                   | 字符串转日期                        |
    | TimeValue("时间字符串")                   | 字符串转时间                        |
    | DateSerial(2021, 12, 8)                   | 日期序列                            |
    | TimeSerial(12, 30, 30)                    | 时间序列                            |
    | MonthName(12, True)                       | 12月                                |
    | DateDiff("d", "2021/12/08", "2022/12/25") | 日期差算(yyyy/q/m/y/d/w/ww/h/n/s)   |
    | DatePart("d",Now)                         | 日期割算(yyyy/q/m/y/d/w/ww/h/n/s)   |
    | DateAdd("d", 增量, Date)                  | 日期加减算(yyyy/q/m/y/d/w/ww/h/n/s) |

* 判断函数

  * | 函数                         | 含义                 |
    | ---------------------------- | -------------------- |
    | IsArray(Split("1,2,3", ",")) |                      |
    | IsDate("2021/12/08")         |                      |
    | IsEmpty(Empty)               |                      |
    | IsNull(Null)                 |                      |
    | IsNumeric("231")             |                      |
    | IsError(CVErr(123))          |                      |
    | IsMissing                    | 判断函数参数是否存在 |
    | IsObject(ActiveSheet)        |                      |

  * 

单元格操作

* | 函数表达式                                   | 作用           |
  | -------------------------------------------- | -------------- |
  | Range("A1:C10")                              | 定义范围       |
  | Range("A" & i)                               | 动态单元格     |
  | Cells(x,y)                                   | 单元格         |
  | Cells                                        | 所有单元格     |
  | Cells(Rows.Count, 1).End(xlUp).Row           | 最后非空行行号 |
  | Cells(1, Columns.Count).End(xlToLeft).Column | 最后非空列列号 |

高级操作

* | 功能                | 语法                                                 |
  | :------------------ | ---------------------------------------------------- |
  | 注册注册表          | SaveSetting("appname", "section", "key", "value")    |
  | 获取注册表          | GetSetting("appname", "section", "key","default")    |
  | 删除注册表          | DeleteSetting("appname", "section", "key","default") |
  | 获取同类注册表      | GetAllSettings("appname", "section")                 |
  | Environ("HOMEPATH") |                                                      |

* 

表操作

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

    

控制语句

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

组件

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

  

数组

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

文件操作

- Dir函数

  - ```vb
    Sub FindDir()
        Dim d As String
        d = dir(path + "\*.*")
        Do While d <> Empty
            Debug.Print d
            d = dir()
        Loop
    End Sub
    ```

  - 

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


FTP

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

类模块

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

Excel功能集对应excel操作

* 设置打印区域

  * ```vb
    ActiveSheet.PageSetup.PrintArea = Cells(1, 1).CurrentRegion.Address + ":" + Cells(row, col).CurrentRegion.Address
    ```

ADO数据库操作

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

- 附录

- ASCII

  | 十进制代码            | 十六进制代码 | MCS 字符或缩写 | DEC 多国字符名                        |
  | --------------------- | ------------ | -------------- | ------------------------------------- |
  | ASCII  控制字符 1     |              |                |                                       |
  | 0                     | 0            | NUL            | 空字符                                |
  | 1                     | 1            | SOH            | 标题起始 (Ctrl/A)                     |
  | 2                     | 2            | STX            | 文本起始 (Ctrl/B)                     |
  | 3                     | 3            | ETX            | 文本结束 (Ctrl/C)                     |
  | 4                     | 4            | EOT            | 传输结束 (Ctrl/D)                     |
  | 5                     | 5            | ENQ            | 询问 (Ctrl/E)                         |
  | 6                     | 6            | ACK            | 认可 (Ctrl/F)                         |
  | 7                     | 7            | BEL            | 铃 (Ctrl/G)                           |
  | 8                     | 8            | BS             | 退格 (Ctrl/H)                         |
  | 9                     | 9            | HT             | 水平制表栏 (Ctrl/I)                   |
  | 10                    | 0A           | LF             | 换行 (Ctrl/J)                         |
  | 11                    | 0B           | VT             | 垂直制表栏 (Ctrl/K)                   |
  | 12                    | 0C           | FF             | 换页 (Ctrl/L)                         |
  | 13                    | 0D           | CR             | 回车 (Ctrl/M)                         |
  | 14                    | 0E           | SO             | 移出 (Ctrl/N)                         |
  | 15                    | 0F           | SI             | 移入 (Ctrl/O)                         |
  | 16                    | 10           | DLE            | 数据链接丢失  (Ctrl/P)                |
  | 17                    | 11           | DC1            | 设备控制 1  (Ctrl/Q)                  |
  | 18                    | 12           | DC2            | 设备控制 2  (Ctrl/R)                  |
  | 19                    | 13           | DC3            | 设备控制 3  (Ctrl/S)                  |
  | 20                    | 14           | DC4            | 设备控制 4  (Ctrl/T)                  |
  | 21                    | 15           | NAK            | 否定接受 (Ctrl/U)                     |
  | 22                    | 16           | SYN            | 同步闲置符 (Ctrl/V)                   |
  | 23                    | 17           | ETB            | 传输块结束 (Ctrl/W)                   |
  | 24                    | 18           | CAN            | 取消 (Ctrl/X)                         |
  | 25                    | 19           | EM             | 媒体结束 (Ctrl/Y)                     |
  | 26                    | 1A           | SUB            | 替换 (Ctrl/Z)                         |
  | 27                    | 1B           | ESC            | 换码符                                |
  | 28                    | 1C           | FS             | 文件分隔符                            |
  | 29                    | 1D           | GS             | 组分隔符                              |
  | 30                    | 1E           | RS             | 记录分隔符                            |
  | 31                    | 1F           | US             | 单位分隔符                            |
  | ASCII  特殊和数字字符 |              |                |                                       |
  | 32                    | 20           | SP             | 空格                                  |
  | 33                    | 21           | !              | 感叹号                                |
  | 34                    | 22           | "              | 引号 (双引号)                         |
  | 35                    | 23           | #              | 数字符号                              |
  | 36                    | 24           | $              | 美元符                                |
  | 37                    | 25           | %              | 百分号                                |
  | 38                    | 26           | &              | 和号                                  |
  | 39                    | 27           | '              | 省略号 (单引号)                       |
  | 40                    | 28           | (              | 左圆括号                              |
  | 41                    | 29           | )              | 右圆括号                              |
  | 42                    | 2A           | *              | 星号                                  |
  | 43                    | 2B           | +              | 加号                                  |
  | 44                    | 2C           | ,              | 逗号                                  |
  | 45                    | 2D           | --             | 连字号或减号                          |
  | 46                    | 2E           | .              | 句点或小数点                          |
  | 47                    | 2F           | /              | 斜杠                                  |
  | 48                    | 30           | 0              | 零                                    |
  | 49                    | 31           | 1              | 1                                     |
  | 50                    | 32           | 2              | 2                                     |
  | 51                    | 33           | 3              | 3                                     |
  | 52                    | 34           | 4              | 4                                     |
  | 53                    | 35           | 5              | 5                                     |
  | 54                    | 36           | 6              | 6                                     |
  | 55                    | 37           | 7              | 7                                     |
  | 56                    | 38           | 8              | 8                                     |
  | 57                    | 39           | 9              | 9                                     |
  | 58                    | 3A           | :              | 冒号                                  |
  | 59                    | 3B           | ;              | 分号                                  |
  | 60                    | 3C           | <              | 小于                                  |
  | 61                    | 3D           | =              | 等于                                  |
  | 62                    | 3E           | >              | 大于                                  |
  | 63                    | 3F           | ?              | 问号                                  |
  | ASCII  字母字符       |              |                |                                       |
  | 64                    | 40           | @              | 商业 at 符号                          |
  | 65                    | 41           | A              | 大写字母 A                            |
  | 66                    | 42           | B              | 大写字母 B                            |
  | 67                    | 43           | C              | 大写字母 C                            |
  | 68                    | 44           | D              | 大写字母 D                            |
  | 69                    | 45           | E              | 大写字母 E                            |
  | 70                    | 46           | F              | 大写字母 F                            |
  | 71                    | 47           | G              | 大写字母 G                            |
  | 72                    | 48           | H              | 大写字母 H                            |
  | 73                    | 49           | I              | 大写字母 I                            |
  | 74                    | 4A           | J              | 大写字母 J                            |
  | 75                    | 4B           | K              | 大写字母 K                            |
  | 76                    | 4C           | L              | 大写字母 L                            |
  | 77                    | 4D           | M              | 大写字母 M                            |
  | 78                    | 4E           | N              | 大写字母 N                            |
  | 79                    | 4F           | O              | 大写字母 O                            |
  | 80                    | 50           | P              | 大写字母 P                            |
  | 81                    | 51           | Q              | 大写字母 Q                            |
  | 82                    | 52           | R              | 大写字母 R                            |
  | 83                    | 53           | S              | 大写字母 S                            |
  | 84                    | 54           | T              | 大写字母 T                            |
  | 85                    | 55           | U              | 大写字母 U                            |
  | 86                    | 56           | V              | 大写字母 V                            |
  | 87                    | 57           | W              | 大写字母 W                            |
  | 88                    | 58           | X              | 大写字母 X                            |
  | 89                    | 59           | Y              | 大写字母 Y                            |
  | 90                    | 5A           | Z              | 大写字母 Z                            |
  | 91                    | 5B           | [              | 左中括号                              |
  | 92                    | 5C           | \              | 反斜杠                                |
  | 93                    | 5D           | ]              | 右中括号                              |
  | 94                    | 5E           | ^              | 音调符号                              |
  | 95                    | 5F           | _              | 下划线                                |
  | 96                    | 60           | `              | 重音符                                |
  | 97                    | 61           | a              | 小写字母 a                            |
  | 98                    | 62           | b              | 小写字母 b                            |
  | 99                    | 63           | c              | 小写字母 c                            |
  | 100                   | 64           | d              | 小写字母 d                            |
  | 101                   | 65           | e              | 小写字母 e                            |
  | 102                   | 66           | f              | 小写字母 f                            |
  | 103                   | 67           | g              | 小写字母 g                            |
  | 104                   | 68           | h              | 小写字母 h                            |
  | 105                   | 69           | i              | 小写字母 i                            |
  | 106                   | 6A           | j              | 小写字母 j                            |
  | 107                   | 6B           | k              | 小写字母 k                            |
  | 108                   | 6C           | l              | 小写字母 l                            |
  | 109                   | 6D           | m              | 小写字母 m                            |
  | 110                   | 6E           | n              | 小写字母 n                            |
  | 111                   | 6F           | o              | 小写字母 o                            |
  | 112                   | 70           | p              | 小写字母 p                            |
  | 113                   | 71           | q              | 小写字母 q                            |
  | 114                   | 72           | r              | 小写字母 r                            |
  | 115                   | 73           | s              | 小写字母 s                            |
  | 116                   | 74           | t              | 小写字母 t                            |
  | 117                   | 75           | u              | 小写字母 u                            |
  | 118                   | 76           | v              | 小写字母 v                            |
  | 119                   | 77           | w              | 小写字母 w                            |
  | 120                   | 78           | x              | 小写字母 x                            |
  | 121                   | 79           | y              | 小写字母 y                            |
  | 122                   | 7A           | z              | 小写字母 z                            |
  | 123                   | 7B           | {              | 左大括号                              |
  | 124                   | 7C           | \|             | 垂直线                                |
  | 125                   | 7D           | }              | 右大括号 (ALTMODE)                    |
  | 126                   | 7E           | ~              | 代字号 (ALTMODE)                      |
  | 127                   | 7F           | DEL            | 擦掉 (DELETE)                         |
  | 控制字符              |              |                |                                       |
  | 128                   | 80           |                | [保留]                                |
  | 129                   | 81           |                | [保留]                                |
  | 130                   | 82           |                | [保留]                                |
  | 131                   | 83           |                | [保留]                                |
  | 132                   | 84           | IND            | 索引                                  |
  | 133                   | 85           | NEL            | 下一行                                |
  | 134                   | 86           | SSA            | 被选区域起始                          |
  | 135                   | 87           | ESA            | 被选区域结束                          |
  | 136                   | 88           | HTS            | 水平制表符集                          |
  | 137                   | 89           | HTJ            | 对齐的水平制表符集                    |
  | 138                   | 8A           | VTS            | 垂直制表符集                          |
  | 139                   | 8B           | PLD            | 部分行向下                            |
  | 140                   | 8C           | PLU            | 部分行向上                            |
  | 141                   | 8D           | RI             | 反向索引                              |
  | 142                   | 8E           | SS2            | 单移 2                                |
  | 143                   | 8F           | SS3            | 单移 3                                |
  | 144                   | 90           | DCS            | 设备控制字符串                        |
  | 145                   | 91           | PU1            | 专用 1                                |
  | 146                   | 92           | PU2            | 专用 2                                |
  | 147                   | 93           | STS            | 设置传输状态                          |
  | 148                   | 94           | CCH            | 取消字符                              |
  | 149                   | 95           | MW             | 消息等待                              |
  | 150                   | 96           | SPA            | 保护区起始                            |
  | 151                   | 97           | EPA            | 保护区结束                            |
  | 152                   | 98           |                | [保留]                                |
  | 153                   | 99           |                | [保留]                                |
  | 154                   | 9A           |                | [保留]                                |
  | 155                   | 9B           | CSI            | 控制序列引导符                        |
  | 156                   | 9C           | ST             | 字符串终止符                          |
  | 157                   | 9D           | OSC            | 操作系统命令                          |
  | 158                   | 9E           | PM             | 秘密消息                              |
  | 159                   | 9F           | APC            | 应用程序                              |
  | 其他字符              |              |                |                                       |
  | 160                   | A0           |                | [保留] 2                              |
  | 161                   | A1           | ¡              | 反向感叹号                            |
  | 162                   | A2           | ¢              | 分币符                                |
  | 163                   | A3           | £              | 英磅符                                |
  | 164                   | A4           |                | [保留] 2                              |
  | 165                   | A5           | ¥              | 人民币符                              |
  | 166                   | A6           |                | [保留] 2                              |
  | 167                   | A7           | §              | 章节符                                |
  | 168                   | A8           | ¤              | 通用货币符号 2                        |
  | 169                   | A9           | ©              | 版权符号                              |
  | 170                   | AA           | ª              | 阴性顺序指示符                        |
  | 171                   | AB           | «              | 左角引号                              |
  | 172                   | AC           |                | [保留] 2                              |
  | 173                   | AD           |                | [保留] 2                              |
  | 174                   | AE           |                | [保留] 2                              |
  | 175                   | AF           |                | [保留] 2                              |
  | 176                   | B0           | °              | 温度符                                |
  | 177                   | B1           | ±              | 加/减号                               |
  | 178                   | B2           | ²              | 上标 2                                |
  | 179                   | B3           | ³              | 上标 3                                |
  | 180                   | B4           |                | [保留] 2                              |
  | 181                   | B5           | µ              | 微符                                  |
  | 182                   | B6           | ¶              | 段落符，pilcrow                       |
  | 183                   | B7           | ·              | 中点                                  |
  | 184                   | B8           |                | [保留] 2                              |
  | 185                   | B9           | ¹              | 上标 1                                |
  | 186                   | BA           | º              | 阳性顺序指示符                        |
  | 187                   | BB           | »              | 右角引号                              |
  | 188                   | BC           | ¼              | 分数四分之一                          |
  | 189                   | BD           | ½              | 分数二分之一                          |
  | 190                   | BE           |                | [保留] 2                              |
  | 191                   | BF           | ¿              | 反向问号                              |
  | 192                   | C0           | À              | 带重音符的大写字母 A                  |
  | 193                   | C1           | Á              | 带尖锐重音的大写字母 A                |
  | 194                   | C2           | Â              | 带音调符号的大写字母 A                |
  | 195                   | C3           | Ã              | 带代字号的大写字母 A                  |
  | 196                   | C4           | Ä              | 带元音变音 (分音符号)  的大写字母 A   |
  | 197                   | C5           | Å              | 带铃声的大写字母  A                   |
  | 198                   | C6           | Æ              | 大写字母 AE 双重元音                  |
  | 199                   | C7           | Ç              | 带变音符号的大写字母 C                |
  | 200                   | C8           | È              | 带重音符的大写字母  E                 |
  | 201                   | C9           | É              | 带尖锐重音的大写字母  E               |
  | 202                   | CA           | Ê              | 带音调符号的大写字母  E               |
  | 203                   | CB           | Ë              | 带元音变音 (分音符号)  的大写字母 E   |
  | 204                   | CC           | Ì              | 带重音符的大写字母  I                 |
  | 205                   | CD           | Í              | 带尖锐重音的大写字母  I               |
  | 206                   | CE           | Î              | 带音调符号的大写字母  I               |
  | 207                   | CF           | Ï              | 带元音变音 (分音符号)  的大写字母 I   |
  | 208                   | D0           |                | [保留] 2                              |
  | 209                   | D1           | Ñ              | 带代字号的大写字母  N                 |
  | 210                   | D2           | Ò              | 带重音符的大写字母  O                 |
  | 211                   | D3           | Ó              | 带尖锐重音的大写字母  O               |
  | 212                   | D4           | Ô              | 带音调符号的大写字母  O               |
  | 213                   | D5           | Õ              | 带代字号的大写字母  O                 |
  | 214                   | D6           | Ö              | 带元音变音 (分音符号)  的大写字母 O   |
  | 215                   | D7           | OE             | 大写字母 OE  连字 2                   |
  | 216                   | D8           | Ø              | 带斜杠的大写字母  O                   |
  | 217                   | D9           | Ù              | 带重音符的大写字母  U                 |
  | 218                   | DA           | Ú              | 带尖锐重音的大写字母  U               |
  | 219                   | DB           | Û              | 带音调符号的大写字母  U               |
  | 220                   | DC           | Ü              | 带元音变音 (分音符号)  的大写字母 U   |
  | 221                   | DD           | Y              | 带元音变音 (分音符号)  的大写字母 Y   |
  | 222                   | DE           |                | [保留] 2                              |
  | 223                   | DF           | ß              | 德语高调小写字母 s                    |
  | 224                   | E0           | à              | 带重音符的小写字母  a                 |
  | 225                   | E1           | á              | 带尖锐重音的小写字母  a               |
  | 226                   | E2           | â              | 带音调符号的小写字母  a               |
  | 227                   | E3           | ã              | 带代字号的小写字母  a                 |
  | 228                   | E4           | ä              | 带元音变音 (分音符号)  的小写字母 a   |
  | 229                   | E5           | å              | 带铃声的小写字母  a                   |
  | 230                   | E6           | æ              | 小写字母 ae 双重元音                  |
  | 231                   | E7           | ç              | 带变音符号的小写字母 c                |
  | 232                   | E8           | è              | 带重音符的小写字母  e                 |
  | 233                   | E9           | é              | 带尖锐重音的小写字母  e               |
  | 234                   | EA           | ê              | 带音调符号的小写字母  e               |
  | 235                   | EB           | ë              | 带元音变音 (分音符号)  的小写字母 e   |
  | 236                   | EC           | ì              | 带重音符的小写字母  i                 |
  | 237                   | ED           | í              | 带尖锐重音的小写字母  i               |
  | 238                   | EE           | î              | 带音调符号的小写字母  i               |
  | 239                   | EF           | ï              | 带元音变音 (分音符号)  的小写字母 i   |
  | 240                   | F0           |                | [保留] 2                              |
  | 241                   | F1           | ñ              | 带代字号的小写字母  n                 |
  | 242                   | F2           | ò              | 带重音符的小写字母  o                 |
  | 243                   | F3           | ó              | 带尖锐重音的小写字母  o               |
  | 244                   | F4           | ô              | 带音调符号的小写字母  o               |
  | 245                   | F5           | õ              | 带代字号的小写字母  o                 |
  | 246                   | F6           | ö              | 带元音变音 (分音符号)  的小写字母 o   |
  | 247                   | F7           | oe             | 小写字母 oe  连字 2                   |
  | 248                   | F8           | ø              | 带斜杠的小写字母  o                   |
  | 249                   | F9           | ù              | 带重音符的小写字母  u                 |
  | 250                   | FA           | ú              | 带尖锐重音的小写字母  u               |
  | 251                   | FB           | û              | 带音调符号的小写字母  u               |
  | 252                   | FC           | ü              | 带元音变音 (分音符号)  的小写字母 u   |
  | 253                   | FD           | ÿ              | 带元音变音 (分音符号)  的小写字母 y 2 |
  | 254                   | FE           |                | [保留] 2                              |
  | 255                   | FF           |                | [保留] 2                              |

