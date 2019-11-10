# Tools

#### Git

1. `git init`

   1. 在github上新建一个空仓库后，会得到一个仓库地址(adress)`https://github.com/2oops/gitdemo.git`（下同）

   2. 然后在本地新建一个文件夹，右击该文件夹打开`git bash`使用`git init`命令初始化本地git

   3. `git add .` --> `git commit -m 'first commit'`提交代码

   4. `git remote add origin adress`（adress为github上的空仓库地址）将本地仓库和远程仓库连接起来，若有`fatal:remote origin already exists.`报错，即提示远程仓库已存在，则执行`git remote rm origin`，然后在执行`git remote add origin adress`，若无报错，则继续`git push -u origin master`这里的master，是指提交到的远程仓库分支，远程没有则会创建该分支，至此，本地和远程的项目已经同步了，可以进行正常的pull,commit, push操作了。

   5. 添加一点`git remote rm origin`还是不能解决问题的话(本人暂没有遇见该问题)，在当前目录下执行`vi .git/config`直接操作git配置，执行完后会进入unix命令行操作。然后把`[remote "origin"]` 那一行删掉。

   6. 附上unix基本操作命令

      ```
      :w 保存文件但不退出vi
      :w file 将修改另外保存到file中，不退出vi
      :w! 强制保存，不推出vi
      :wq 保存文件并退出vi
      :wq! 强制保存文件，并退出vi
      q: 不保存文件，退出vi
      :q! 不保存文件，强制退出vi
      :e! 放弃所有修改，从上次保存文件开始再编辑
      ```

   7. 经常遇到的`warning: LF will be replaced by CRLF`警告问题（LF即是只`line feed`，CRLF是指`carriage return line feed`）在开发中一般不会产生太大问题，但是对文件要求严格的操作可能会引发bug，这时可以执行

      ```
      $ rm -rf .git  // 删除.git  rm -rf慎用
      $ git config --global core.autocrlf false  //禁用自动转换 
      //然后重新执行
      $ git init    
      $ git add . 
      ```

   8. ```CRLF->Windows-style
      CR->Mac Style
        LF->Unix Style(LF表示表示句尾，只使用换行)
        CRLF->Windows-style(CRLF表示句尾使用回车换行两个字符(即我们常在Windows编程时使用”\r\n”换行))
        // 在git中可以通过如下命令转换
        $git config --global core.autocrlf true
        core.autocrlf是git中负责处理line endings的变量，可以设置三个值– true , input , false
        设置为true，添加文件到git仓库时，git将其视为文本文件。他将把crlf变成lf
        设置为false时，line-endings将不做转换操作
        设置为inout时，添加文件git仓库时，git把crlf编程lf。当有人Check代码时还是lf方式。因此在window操作系统下，不要使用这个设置
      ```

2. git全局配置

   1. `git config --list`命令可以查看git配置信息，包括`user.name和user.email`等等
   2. 直接`git config user.name`可以查看用户名，同理配置信息中其他项也可按此查看
   3. `git config --global user.name "xxx"`全局配置用户名

3. 如何撤销本地commit

   当你在本地`git add .`后，发现并不想提交这些改动到远程，于是我们需要撤销这个commit,

   ```
   $git commit -m 'first commit'
   $git reset HEAD~
   // 这样就将本地的commit撤销了，可以重新add
   ```

4. 如何创建`.gitignore`文件，该文件的创建是为了将不想提交到远程的代码忽略掉，比如说`node-moudles`

   这里我们只需要在项目目录下打开`git bash`，然后执行`touch .gitignore`即可。

   `.gitignore`文件的语法规则：[参考](<https://jingyan.baidu.com/article/c85b7a64bb7979003aac955a.html>)

   - 以”#”号开头表示注释
   - 以斜杠“/”开头表示目录
   - 以星号“*”通配多个字符
   - 以问号“?”通配单个字符 
   - 以方括号“[]”包含单个字符的匹配列表
   - 以叹号“!”表示不忽略(跟踪)匹配到的文件或目录
   - 此外，该配置文件是按从上到下进行规则匹配的，简单来说就是前面的规则匹配的范围更大，则后面的规则不会生效

   ```
   #忽略 .txt 的文件
   *.txt
   #尽管前面忽略了 .txt 文件， 但不忽略 lib.txt 文件
   !lib.txt
   #仅在当前目录下忽略 todo 文件，但不包括子目录下的subdir/todo
   /todo
   #忽略 node-moudles 文件夹下的所有文件
   node-moudles/
   #忽略 doc/notes.txt 不包括 doc/server/a.txt
   doc/*.txt
   #忽略所有的在 doc/directory 下的 .pdf 文件
   doc/**/*.pdf
   #忽略根目录下 bin 文件夹中的所有文件
   /bin/*
   #不要忽略 bin 文件夹下的 .java 文件
   !/bin/*.java
   ```

5. 如何为自己的项目打上`tag`标签及删除标签

   [参考](<https://blog.csdn.net/shenshizhong/article/details/80822143>)

   ```
   // 直接在git bash中项目根目录下
   $git tag -a 'v1.0.0' -m 'refactor'
   $git push origin v1.0.0
   // 这里打的是版本标签，最后一位一般为小改，中间是重大bug或修订，第一位一般是重构或者新版本
   // 删除本地标签
   $git tag -d v1.0.0
   // 删除远程标签
   $git push origin :refs/tags/v1.0.0
   // 其实这里还有删除模块标签的操作，用的不多，这里不作赘述
   ```

6. git删除分支

   [参考](<https://www.cnblogs.com/liyong888/p/9822410.html>)

   ```
   //假设git bash中我现在处在dev分支，现在要删除dev分支，注意当前分支下是不能删除自己的
   //第一步，切换到别的分支(假设有另一个master分支)
   $git checkout master
   $git branch -d dev
   //如果 -d 删不动，那么强制删除
   $git branch -D dev
   //删除远程分支
   $git push origin --delete dev
   ```

7. git创建分支和合并分支

   [参考](<https://www.liaoxuefeng.com/wiki/896043488029600/900003767775424>)

   ```
   // 假设我们在master分支上，现在创建dev分支，然后切换到dev分支
   $git checkout -b dev
   // 查看当前所处于的分支
   $git branch
   //如果在dev上有改动之后我们想合并这些改动到master分支上
   // 切到master分支并合并
   $git checkout master
   $git merge dev
   //查看远端和本地所有分支
   $git branch -a
   //查看远端库的分支情况，这里的`r`表示的是`remote`远端的意思
   $git branch -r
   ```

8. 克隆你所需要分支`xxx`的代码，假设仓库地址`address`

   `git clone -b xxx address`

9. git回滚操作

   ```javascript
   $git relog
   $git log
   $git reset --hard cd52afc
   $git cherry-pick 4c97ff3
   ```

[相关操作详情](https://github.com/airuikun/blog/issues/5)

10. 如果一个实习生，他本地git的A分支被误删了， A分支代码没有被push到远程，如何找到之前A的提交记录和代码

    1. 模拟这个场景，现在我们处于xxx分支

       ```
       git checkout master //切到master分支
       
       git branch -d xxx //删除xxx分支，这里仅仅是删除了本地分支
       
       git push origin --delete xxx //这是删除远程分支，但是这次模拟不执行这一步
       ```

    2. 实际操作中如果本地作了修改，但是没有add并commit的话是不会让你切换分支的

       ```
       error: Your local changes to the following files would be overwritten by checkout:
       
               README.md
       
       Please commit your changes or stash them before you switch branches.
       
       Aborting
       ```
    3. 我们先commit，commit之后我们会得到下列提示

       ```
       $ git commit -m '测试删除本地分支'
       [xxx 477939e] 测试删除本地分支
        2 files changed, 9 insertions(+), 1 deletion(-)
       ```
    4. 如果我们没注意到这个提交的信息，可以使用以下命令查看，里面有Date可提交人，以及提交信息

       ```
       git log -a
       ```

    5. 现在我们删除本地xxx分支，这里如果使用命令行删除的话是会提示的，除非你使用`git branch -D xxx'

       ```
       error: The branch 'xxx' is not fully merged.
       If you are sure you want to delete it, run 'git branch -D xxx'.
       ```

    6. 现在我们强制删除

       ```
       Deleted branch xxx (was 9a317f5).
       ```

    7. 强制删除后执行`git log -g`，我们可以得到这个

       ```
       commit e4f843a1cb43d375c5d5c3e567f1cf931855261a (HEAD -> master, origin/master)
       ```

    8. 下一步我们拿到这个`e4f843a1cb43d375c5d5c3e567f1cf931855261a`，删除分支的那条记录标志，然后执行

       ```
       git branch recover-xxx e4f843a1cb43d375c5d5c3e567f1cf931855261a
       //然后我们查看下本地分支情况
       git branch -a
       //可以得到如下
       * master
         recover-xxx
         remotes/origin/master
         remotes/origin/xxx
       ```


11. 如何使用`git bash`给远端新开一个分支存储本地之前从同一仓库`clone`下来的并做过更改的文件？

    假设需要新开一个`production`分支，注意远端和本地之前都是没有这个分支，防止冲突

    1. `git branch production`本地新开一个分支
    2. `git checkout production`切换到该分支
    3. `git branch -a`查看所有分支，`git branch -r`查看远端分支，可以看到分支已创建，可省略此步骤
    4. `git push origin production:production`或者`git push origin production`都可以将本地分支上到远程
    5. `git add .` => `git commit -a '提交更改'` => `git push`然后所有修改过的代码都已经上到远程

12. `git`常用命令

    - `git add .` 表示添加新文件和编辑过的文件不包括删除的文件

    - `git add -A .` 来一次添加所有改变的文件
    - `git add -A` 表示添加所有内容
    - `git add -u` 表示添加编辑或者删除的文件，不包括新添加的文件
    - `git push -u origin master` 推送到远程仓库
    - `git branch -av` 查看每个分支的最新提交记录
    - `git branch -vv` 查看每个分支属于哪个远程仓库
    - 删除本地分支 `git branch -d dev`
    - 同步删除远程分支 `git push origin :dev`

13. 修改远程仓库地址

    Methods1，先删后加:

    - `git remote rm origin` 先删除
    - `git remote add origin 仓库地址` 链接到到远程git仓库

    Methods2，修改命令:

    - `git remote set-url origin 仓库地址`

14. 如果`git pull`失败咋整？

    1. 首先从远程的origin的master主分支下载最新的版本到origin/master分支上

       `git fetch origin master`

    2. 比较本地的master分支和origin/master分支的差别

       `git log -p master..origin/master`

    3. 进行合并

       `git merge origin/master`

15. 拉取远程仓库指定分支到本地

    - 首先要与origin master建立连接：`git remote add origin git@github.com:XXXX/nothing2.git`
    - 切换到其中某个子分支：`git checkout -b dev origin/dev`
    - 可能会报这种错误:

    ```
    fatal: Cannot update paths and switch to branch 'dev' at the same time.
    Did you intend to checkout 'origin/dev' which can not be resolved as commit?
    ```

    - 原因是你本地并没有dev这个分支，这时你可以用 `git branch -a` 命令来查看本地是否具有dev分支
    - 我们需要：`git fetch origin dev` 命令来把远程分支拉到本地
    - 然后使用：`git checkout -b dev origin/dev` 在本地创建分支dev并切换到该分支
    - 最后使用：`git pull origin dev` 就可以把某个分支上的内容都拉取到本地了