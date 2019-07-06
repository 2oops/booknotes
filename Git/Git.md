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

3. 