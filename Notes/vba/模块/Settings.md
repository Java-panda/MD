- 快捷键设置

  - ``` vb
    Option Explicit
    
    Sub main()
    '    Ctrl:^
    '    Shift:+
    '    Alt:%
        Application.OnKey "^+%{R}", "test1" 'Ctrl+Shift+Alt+R运行test1方法
    End Sub
    
    Sub test1()
    	MsgBox "test1"
    End Sub
    
    ```

  - 