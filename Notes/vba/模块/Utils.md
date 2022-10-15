- 图片导入

- ``` vb
  '垂直导入
  Sub MainUploadPictureByVertical()
      Dim currentCell As Range
      Dim fd As FileDialog
      Set currentCell = ActiveCell
      Set fd = Application.FileDialog(msoFileDialogFolderPicker)
      If fd.Show = -1 Then
          Dim i As Integer
          For i = 1 To fd.SelectedItems.Count
              RecursiveFindFileInFolder fd.SelectedItems(i), 2, True
          Next
      Else
          MsgBox "未选择任何文件"
      End If
  End Sub
  '水平导入
  Sub MainUploadPictureByHori()
      Dim currentCell As Range
      Dim fd As FileDialog
      Set currentCell = ActiveCell
      Set fd = Application.FileDialog(msoFileDialogFolderPicker)
      If fd.Show = -1 Then
          Dim i As Integer
          For i = 1 To fd.SelectedItems.Count
              RecursiveFindFileInFolder fd.SelectedItems(i), 1, False
          Next
      Else
          MsgBox "未选择任何文件"
      End If
  End Sub
          
  '加载图片
  Function UploadPicture(ByVal path As String, ByRef left As Double, ByRef top As Double, ByRef Width As Double, ByRef Height As Double, GapRow As Integer, IsVertical As Boolean)
      Dim cst As Worksheet
      Dim sp As Shape
      Set cst = ActiveSheet
      Set sp = cst.Shapes.AddPicture(path, msoFalse, msoCTrue, left, top, Width, Height)
      If IsVertical Then
          UploadPicture = Cells(sp.BottomRightCell.Row + GapRow + 1, 1).top
      Else
          UploadPicture = Cells(1, sp.BottomRightCell.Column + GapRow + 1).left
      End If
      
  End Function
  
  '遍历图片文件
  Sub RecursiveFindFileInFolder(path As String, GapRow As Integer, IsVertical As Boolean)
      Dim folder, fol As Object
      Dim fos As New Scripting.FileSystemObject
      Dim f As Object
      Set folder = fos.GetFolder(path)
      Dim top As Double
      Dim left As Double
      Dim Width As Double
      Dim Height As Double
      top = ActiveCell.top
      left = ActiveCell.left
      Width = -1
      Height = -1
      For Each f In folder.Files
          If IsVertical Then
              top = UploadPicture(f.path, left, top, Width, Height, GapRow, IsVertical)
          Else
              left = UploadPicture(f.path, left, top, Width, Height, GapRow, IsVertical)
          End If
          
      Next
  '    For Each fol In folder.SubFolders
  '        RecursiveFindFileInFolder fol.path, GapRow, IsVertical
  '    Next
  End Sub
  ```

- 四象限形状选择器

- ``` vb
  Sub SelectShapesByDirection()
      Dim shapesArr() As Variant
      Dim direction As Integer
      Dim sp As Shape
      Dim top, right, down, left As Double
      Dim ac As Range
      shapesArr = Array()
      direction = InputBox("请输入1,2,4,8组合", "形状选择器", 2)
      Set ac = ActiveCell
      For Each sp In ActiveSheet.Shapes
          If 1 And direction Then
              If sp.left > ac.left And (sp.top + sp.Height) < ac.top Then
                  ReDim Preserve shapesArr(UBound(shapesArr) + 1)
                  shapesArr(UBound(shapesArr)) = sp.name
              End If
          End If
          If 2 And direction Then
              If sp.left > ac.left And sp.top > ac.top Then
                  ReDim Preserve shapesArr(UBound(shapesArr) + 1)
                  shapesArr(UBound(shapesArr)) = sp.name
              End If
          End If
          If 4 And direction Then
              If sp.left + sp.Width < ac.left And sp.top > ac.top Then
                  ReDim Preserve shapesArr(UBound(shapesArr) + 1)
                  shapesArr(UBound(shapesArr)) = sp.name
              End If
          End If
          If 8 And direction Then
              If sp.left + sp.Width < ac.left And (sp.top + sp.Height) < ac.top Then
                  ReDim Preserve shapesArr(UBound(shapesArr) + 1)
                  shapesArr(UBound(shapesArr)) = sp.name
              End If
          End If
      Next
      ActiveSheet.Shapes.Range(shapesArr).Select
  End Sub
  ```

- 