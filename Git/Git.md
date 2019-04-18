# Git

1. git回滚操作

   ```git
   git relog
   git log
   git reset --hard cd52afc
   git cherry-pick 4c97ff3
   ```

   [相关操作详情](https://github.com/airuikun/blog/issues/5)

2. `git init`

   1. 在github上新建一个空仓库后，会得到一个仓库地址(adress)`https://github.com/2oops/gitdemo.git`

   2. 然后在本地新建一个文件夹，里面写入文件，然后`git init`

   3. `git add .` --> `git commit -m 'first commit'`提交代码

   4. `git remote add origin adress`将本地仓库和远程仓库连接起来，若有`fatal:remote origin already exists.`报错，即提示远程仓库已存在，则执行`git remote rm origin`，然后在执行`git remote add origin adress`，若无报错，则继续`git push -u origin master`这里的master，是指提交到的远程仓库分支，远程没有则会创建该分支，至此，本地和远程的项目已经同步了，可以进行正常的pull,commit, push操作了。

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

   7. 经常遇到的`warning: LF will be replaced by CRLF`警告问题在开发中一般不会产生太大问题，但是对文件要求严格的操作可能会引发bug，这时可以执行

      ```
      $ rm -rf .git  // 删除.git  rm -rf慎用
      $ git config --global core.autocrlf false  //禁用自动转换 
      //然后重新执行
      $ git init    
      $ git add . 
      ```

3. 