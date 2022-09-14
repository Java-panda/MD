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

   

3. 其他