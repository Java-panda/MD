1. 常用命令

   1. 初始化git项目常用命令

      ```bash
      #移动到项目根路径
      cd project-root-path
      
      #初始化git工程
      git init
      
      #提交到暂存区
      git add README.md Text.md
      
      #查看git提交状态
      git status
      
      #提交到本地仓库
      git commit -m "注释"
      
      #重命名本分支名称为main
      git branch -M main
      
      #将该仓库和远程git@github.com:Java-panda/gitexe.git进行绑定
      git remote add origin git@github.com:Java-panda/gitexe.git
      
      #push并将远程origin绑定为main的上游
      git push -u origin main
      
      #从远程origin拉取到本地main
      git pull origin main
      
      #绑定上游之后的省略模式
      git push
      git pull
      
      
      ```
   2. 撤销常用命令
   
      ```bash
      #找到想要重置的版本号
      reset
      	soft：回到该版本，但是目前的暂存区，工作区不改变
      	mix：回到该版本，暂存区改变退回到工作区，工作区不改变
      	hard：回到该版本，暂存区，工作区全部丢掉
      ```
   
      