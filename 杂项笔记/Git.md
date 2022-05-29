# Git

+ 用户名

  ```
  $ git config --global user.name "Your Name"
  $ git config --global user.email "email@example.com"
  ```

+ 创建版本库

  ```
  $ mkdir learngit
  $ cd learngit
  $ pwd
  
  $ls -ah
  命令可以看见隐藏目录。
  ```

+ 文件加入到仓库

  1. 告诉git

     ``` 
     git add readme.text
     ```

  2. 文件提交到仓库

     ```
     $ git commit -m "wrote a readme file"
     ```

+ **用`ls` 或`dir`命令查看当前目录文件**

+ 要随时掌握工作区的状态，使用`git status`命令。

+ 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。

  **如果如果是输入`git diff`+文件名，查看到的是工作区和暂存区的不同**

+ 回退版本

  ```
  git reset --hard commit_id
  ```

+ 查看提交历史

  ```
  git log
  ```

+ 查看命令历史，以确定回到哪个版本

  ```
  git reflog
  ```

+ 按q退出git log

+ 修改了工作区文件之后想撤销修改

  ```
  git checkout -- file
  ```

+ 改了还添加到暂存区了
  1. 用命令 `git reset HEAD <file>` 让暂存区东西消失
  2. 按只修改了工作区的操作修改

+ 删除文件

  + `rm text.txt`表示删除文件

  + 从版本库里删除

    ```
    git rm text.txt
    
    git commit -m "remove it"
    ```

  + 把误删的文件恢复回去

    ```
    git checkout -- text.txt
    ```

  >
  >
  >**`git rm test.txt` 相当于是删除工作目录中的test.txt文件,并把此次删除操作提交到了暂存区**
  >
  >使用`git checkout -- test.txt`相当于是让工作目录test.txt恢复到暂存区中test.txt的状态,
  >
  >但是工作目录中test.txt已经被删除,无法找到文件来再次删除所以报错,
  >
  >必须先使用`git reset head test.txt`在暂存区中将删除操作丢弃,
  >
  >然后在`git checkout -- test.txt`就是直接将工作目录中test.txt恢复到版本库中的状态.

+ 关联一个远程库

  ```
  $ git remote add origin git@github.com:michaelliao/learngit.git
  ```

+ 推送master分支的内容·

  ```
  git push -u origin master
  ```

+ 推送最新修改

  ```
  git push origin master
  ```

+ 查看当前库 

  `git remote -v`

+ 克隆一个本地库

  ```
  $ git clone git@github.com:cwscrsj/gitskills.git
  ```

+ 删除远程库

  ```
  $ git remote rm origin
  ```

+ 查看分支：

  `git branch`

+ 创建分支：

  `git branch <name>`

+ 切换分支：

  `git checkout <name>`或者`git switch <name>`

+ 创建+切换分支：

  `git checkout -b <name>`或者`git switch -c <name>`

+ 合并某分支到当前分支：

  `git merge <name>`

+ 删除分支：`git branch -d <name>`