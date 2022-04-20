- - 常用命令

    - |                   |                |
      | ----------------- | -------------- |
      | git config --list | 查看配置项目表 |
      | git help          | 查看所有帮助   |
      | git help cmd      | 查看该命令帮助 |
      |                   |                |

    - 

      - |                       |                                             |
        | --------------------- | ------------------------------------------- |
        | git init              | 初始化一个git仓库                           |
        | git status            | 查看git版本状态                             |
        | git add a.txt/.       | 加入追踪单个文件/所有文件                   |
        | git rm --cache a.txt  | 取消追踪                                    |
        | git rm a.txt          | 删除文件                                    |
        | git commit -m '注释'  | 提交+注释                                   |
        | git commit -am '注释' | 增加+提交+注释                              |
        | git restore a.txt     | 还原a.txt到本地修改前的状态                 |
        | git log/git log a.txt | 查看历史log                                 |
        | git diff              | 查看当前本地修改后的差别(未add之前)         |
        | git diff --staged     | 查看当前本地修改后的差别(add之后commit之前) |

        

      

    
    

- 工作场景

  - 本次修改还未satage还原
    - revert
  - 本次修改已经satage还原
    - revert
  - 本地commit后还未push想取消
    - 历史记录->选中上一版本head->reset(soft/mix/hard)
      - soft(只删除commit)
      - mix(删除commit并且取消stage)
      - hard(删除commit并且取消stage并且删除本地修改,彻底归零)
  - 本地commit后已经push想取消
    - 查看历史git reset到想回到的版本
      - git push --force
        - 存在安全问题,如果强制push期间有其他人commit会导致他人commit丢失
      - git push --force-with-lease
        - 如果强制push期间有其他人commit会push失败
  - 本地commit然后远程push,之后修改了代码,但是发现想回到过去版本,但是还不想丢掉刚修改的代码,则先将刚改的代码加入stash,然后reset后强制push,最后再将stash中的取出合并,若存在冲突,则单个文件对比checkout
  - merge和rebase
    - master:A,B,C
    - otherbranch:A,B,C,D,E
    - mybranch:A,B,C,d,e
      - 以mybranch为基准
      - merge
        - A,B,C,d,e,DE
      - rebase
        - A,B,C,D,E,d,e

​	
