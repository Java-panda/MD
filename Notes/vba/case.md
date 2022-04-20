- Builder对象代码生成器

  - ```vb
    Option Explicit
    Public TargetClass As String
    Public TargetObject As String
    Public SourceClass As String
    Public SourceObject As String
    Public TargetProps(), SourceProps() As Variant
    Public Const FileName As String = "C:\Users\77023\Desktop\template.java"
    
    Sub run()
        CodeGenerate ThisWorkbook.Sheets("target"), ThisWorkbook.Sheets("source")
    End Sub
    
    Sub CodeGenerate(target As Worksheet, source As Worksheet)
    '    On Error Resume Next
        TargetProps = target.Range("A3:B" & target.Cells(target.Rows.Count, 1).End(xlUp).Row).Value
        SourceProps = source.Range("A3:B" & source.Cells(source.Rows.Count, 1).End(xlUp).Row).Value
        TargetClass = target.Cells(1, 2).Value
        TargetObject = target.Cells(1, 4).Value
        SourceClass = source.Cells(1, 2).Value
        SourceObject = source.Cells(1, 4).Value
        'User User = User.builder()
        '    .birth (dbUser.getDb_birth())
        '    .gender (dbUser.isDb_gender())
        '    .ID (dbUser.getDb_id())
        '    .Name (dbUser.getDb_name())
        '    .build();
        Dim ff As Integer
        Dim i As Integer
        ff = FreeFile
        '写出
        Open FileName For Output As #ff
            Debug.Print TargetClass + " " + TargetObject + "= " + TargetClass + ".builder()"
            Print #ff, TargetClass + " " + TargetObject + "= " + TargetClass + ".builder()"
            For i = LBound(SourceProps) To UBound(SourceProps)
                Print #ff, WriteOneLine(i)
            Next
            Print #ff, TabN(1) + ".build();"
            Debug.Print TabN(1) + ".build();"
        Close #ff
        Shell "notepad.exe " + FileName, vbNormalFocus
    End Sub
    
    Function TabN(num As Integer) As String
        Dim ret As String
        Dim i As Integer
        For i = 1 To num
            ret = ret + Chr(9)
        Next
        TabN = ret
    End Function
    
    Function WriteOneLine(index As Integer) As String
        Dim ret As String
        Dim prop As String
        Dim tp As String
        Dim pr As String
        pr = SourceProps(index, 1)
        tp = SourceProps(index, 2)
        If tp = "boolean" Then
            prop = "is" + Strings.UCase(Left(pr, 1)) + Right(pr, Len(pr) - 1)
        Else
            prop = "get" + Strings.UCase(Left(pr, 1)) + Right(pr, Len(pr) - 1)
        End If
        ret = TabN(1) + "." + TargetProps(index, 1) + "(" + SourceObject + "." + prop + "())"
        Debug.Print ret
        WriteOneLine = ret
    End Function
    ```

  - 

