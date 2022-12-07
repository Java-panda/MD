1. 数组

   ``` vb
   '数组的使用
   Sub ArrayApi()
       Dim arr() As Variant
       Dim i As Integer
       '清空数组
       Erase arr
       '初始化空数组
       arr = Array()
       '循环赋值
       For i = 0 To 5
           '动态增加长度
           ReDim Preserve arr(UBound(arr) + 1)
           arr(i) = i
       Next
       '循环遍历
       For i = LBound(arr) To UBound(arr)
           Debug.Print arr(i)
       Next
       '数组等同于一个Sheet的一行
       Range("A1:F1") = arr
       Range("A1:A6") = Application.WorksheetFunction.Transpose(arr)
   End Sub
   ```

2. 字典

   ``` vb
   Sub DictionaryApi()
       Dim i As Integer
       Dim keys(), values() As Variant
       '绑定模式CreateObject("Scripting.Dictionary")
       '引用模式Microsoft Scripting Runtime
       Dim dic As Dictionary
       Set dic = New Dictionary
       '排他添加,若出现重复则Error
       dic.Add "a", 1
       dic.Add "b", 2
       '覆盖添加,若出现重复则替换
       dic("c") = 3
       dic("e") = 4
       dic.key("e") = "d"
       dic("f") = 5
       '移除某个Key元素
       dic.Remove ("f")
       Debug.Print "元素总数:" & dic.Count
       Debug.Print "存在校验:" & dic.Exists("a")
       Debug.Print "获取值:" & dic.Item("a")
       '获取所有Key
       keys = dic.keys
   
       For i = LBound(keys) To UBound(keys)
           Debug.Print "Key" & i & "=" & keys(i)
       Next
       '获取所有Value
       values = dic.Items
       For i = LBound(values) To UBound(values)
           Debug.Print "Value" & i & "=" & values(i)
       Next
       '移除所有元素
       dic.RemoveAll
       '定义Key比较规则0,1,2=二进制/文本/数据库
       Debug.Print "比较规则:" & dic.CompareMode
   End Sub
   ```

3. 文件选择

   ``` vb
   Sub FilePickerApi()
       '打开文件
       Dim fd As FileDialog
       Set fd = Application.FileDialog(msoFileDialogFilePicker)
       fd.Title = "文件选择"
       fd.Filters.Clear
       '过滤器
       fd.Filters.Add "JPG图片", "*.jpg;*.jpeg"
       fd.Filters.Add "XML", "*.xml"
       '默认过滤器index
       fd.FilterIndex = 1
       fd.AllowMultiSelect = True
       fd.ButtonName = "确认"
       '默认打开路径
       fd.InitialFileName = Environ("HOMEPATH") + "\Desktop\"
       '界面文件图标显示类型
       fd.InitialView = msoFileDialogViewThumbnail
       '代表excel=1480803660
       Debug.Print fd.Creator
       'FileDialog(msoFileDialogFilePicker)
       Debug.Print fd.Item
       'Microsoft Excel
       Debug.Print fd.Parent.name
       '-1选择完毕,0未选择文件
       Dim i As Integer
       If fd.Show = -1 Then
           For i = 1 To fd.SelectedItems().Count
               Debug.Print fd.SelectedItems(i)
           Next
       Else
           Debug.Print "未选择任何文件"
       End If
   End Sub
   ```

4. 文件夹选择

   ``` vb
   Sub FolderPickerApi()
       '打开文件夹
       Dim fd As FileDialog
       Set fd = Application.FileDialog(msoFileDialogFolderPicker)
       fd.Title = "文件夹选择"
       '确认按钮文本
       fd.ButtonName = "确认"
       '默认打开路径
       fd.InitialFileName = Environ("HOMEPATH") + "\Desktop\"
       '界面文件图标显示类型
       fd.InitialView = msoFileDialogViewThumbnail
       If fd.Show = -1 Then
           For i = 1 To fd.SelectedItems().Count
               Debug.Print fd.SelectedItems(i)
           Next
       Else
           Debug.Print "未选择任何文件"
       End If
   End Sub
   ```

5. 保存文件选择

   ``` vb
   Sub FileDialogSaveApi()
       Dim fd As FileDialog
       Set fd = Application.FileDialog(msoFileDialogSaveAs)
       fd.Title = "文件保存"
       fd.ButtonName = "确认"
       '默认打开路径
       fd.InitialFileName = Environ("HOMEPATH") + "\Desktop\"
       '界面文件图标显示类型
       fd.InitialView = msoFileDialogViewThumbnail
       If fd.Show = -1 Then
           For i = 1 To fd.SelectedItems().Count
               Debug.Print fd.SelectedItems(i)
           Next
       Else
           Debug.Print "未设定文件保存名"
       End If
   End Sub
   ```

6. 递归文件夹中的文件

   ``` vb
   Sub RecursiveFindFileInFolder(path As String)
       Dim folder, fol As Object
       Dim fos As New Scripting.FileSystemObject
       Dim f As Object
       Set folder = fos.GetFolder(path)
       For Each f In folder.Files
           Debug.Print f.name
       Next
       For Each fol In folder.SubFolders
           RecursiveFindFileInFolder fol.path
       Next
   End Sub
   ```

7. AdodbStream文件读取

   ``` vb
   Sub AdodbStreamApi()
       '引用法需要打开MicroSoft Activx Data Objects
       Dim fs As New Stream
       Dim line As String
       '绑定法
       'Dim fs As Object
       'Set fs = CreateObject("adodb.stream")
           
       '文本类型adTypeBinary/adTypeText
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
       fs.LoadFromFile "C:\Users\77023\Desktop\a.txt"
       '循环获取每一行
       While Not fs.EOS
           
           '-1 读取全部,-2逐行读取
           line = fs.ReadText(-2)
           Debug.Print line
       Wend
       '保存流到文件
       fs.SaveToFile "C:\Users\77023\Desktop\b.txt", adSaveCreateOverWrite
       '关流
       fs.Close
       Set fs = Nothing
   End Sub
   ```

8. AdodbExcel读取

   ```vb
   Sub AdoExcel()
       Dim Conn As New ADODB.Connection
       Dim Ret As Recordset
       Dim i As Long
       Dim line As String
       Dim fs As New Stream
       fs.Type = adTypeText
       fs.Mode = adModeReadWrite
       fs.Charset = "UTF-8"
       fs.LineSeparator = adCRLF
       fs.Open
       Conn.Open "Provider=Microsoft.ACE.OLEDB.12.0;Data Source=C:\Users\77023\Desktop\MD笔记\MD\Notes\vba\vba\ado1.xlsx;Extended Properties='Excel 12.0; HDR=YES; IMEX=3'"
       Set Ret = Conn.Execute("select a.*,b.depart from [Sheet1$] a left join [Sheet2$] b  on a.id=b.id")
       Do While (Not Ret.EOF)
           line = ""
           For i = 0 To Ret.Fields.Count - 1
               If i <> Ret.Fields.Count - 1 Then
                   line = line + """" & Ret.Fields(i) & """" & ","
               Else
                   line = line + """" & Ret.Fields(i) & """" & vbCrLf
               End If
           Next
           fs.WriteText line
           Ret.MoveNext
       Loop
       fs.SaveToFile "C:\Users\77023\Desktop\b.txt", adSaveCreateOverWrite
       fs.Close
       Conn.Close
   End Sub
   ```

9. AdodbCsv读取

   Schema.ini(jet database)

   ```bash
   [a.csv]
   ColNameHeader =True
   Format =Delimited(|)
   col1=F1 Text
   col2=F2 Text
   col3=F3 Text
   col4=F4 Text
   col5=F5 date
   
   [b.csv]
   ColNameHeader =True
   Format =CSVDelimited
   col1=F1 Text
   col2=F6 Text
   col3=F7 Text
   ```

   ```vb
   Sub AdoCsv()
       Dim Conn As New ADODB.Connection
       Dim Ret As Recordset
       Dim i As Long
       Dim line As String
       Dim fs As New Stream
       Dim Provider As String
       Dim DataSource As String
       Dim ExtendedProperties As String
       Dim ConnConfig As String
       Dim SQL As String
       fs.Type = adTypeText
       fs.Mode = adModeReadWrite
       fs.Charset = "UTF-8"
       fs.LineSeparator = adCRLF
       fs.Open
       
       '供应商
       Provider = "Provider=Microsoft.ACE.OLEDB.12.0;"
       '数据源
       DataSource = "Data Source=" + "C:\Users\77023\Desktop\MD笔记\MD\Notes\vba\data" + ";"
       '扩展属性
       ExtendedProperties = "Extended Properties=TEXT;"
       '连接配置
       ConnConfig = Provider + DataSource + ExtendedProperties
       
       '左连接两个csv文件
       SQL = "select a.F1,a.F2,a.F3,a.F4,a.F5,b.F2,b.F3 from [a.csv] a left join [b.csv] b  on a.F1=b.F1"
       '打开连接
       Conn.Open ConnConfig
       '执行SQL
       Set Ret = Conn.Execute(SQL)
       '遍历结果集
       Do While (Not Ret.EOF)
           line = ""
           '循环遍历每个字段,拼接成CSV格式
           For i = 0 To Ret.Fields.Count - 1
               If i <> Ret.Fields.Count - 1 Then
                   line = line + """" & Ret.Fields(i) & """" & ","
               Else
                   line = line + """" & Ret.Fields(i) & """" & vbCrLf
               End If
           Next
           '写出一行
           fs.WriteText line
           '指向下一行数据
           Ret.MoveNext
       Loop
       '保存文件
       fs.SaveToFile "C:\Users\77023\Desktop\MD笔记\MD\Notes\vba\data\ret.csv", adSaveCreateOverWrite
       '释放资源
       fs.Close
       Conn.Close
   End Sub
   ```

10. Bean的基本使用

    ```vb
    Public id As String
    Public name As String
    Public age As String
    Public gender As Boolean
    Public depart As String
    Public birth As Date
    Public Function ToString() As String
        Dim line As String
        ToString = """" + id + """" + "," _
        + """" + name + """" + "," _
        + """" + age + """" + "," _
        & gender & "," _
        + """" + depart + """" + "," _
        + """" + CStr(birth) + """"
    End Function
    ```

11. 其他