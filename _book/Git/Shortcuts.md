# Shortcuts

假设某个项目存在多个分支，注意Git中一个仓库只有一个地址，也就是即使你切换不同目录后，给你的地址也是一样的

1.查看当前所在目录

`git branch`

2.查看远端库的分支情况，这里的`r`表示的是`remote`远端的意思

`git branch -r`

3.查看远端和本地所有分支，`a-all`

`git branch -a`

4.切换到已有分支`xxx`

`git checkout xxx`

切换成功后会提示`Switched to branch 'xxx'`

5.新建分支`xxx`并提交到远端，这里注意一点，新建分支后，分支里面需要有内容才可以提交

`git checkout -b xxx`

`git push origin xxx`

6.克隆你所需要分支`xxx`的代码，仓库地址`address`

`git clone -b xxx address`

7.删除远端分支`remotes/origin/xxx`

`git push origin --delete xxx`

8.删除本地分支`xxx`

先切换到`master`： `git checkout master`

`git branch -d xxx`

若报错：`git push --set-upstream origin xxx`，则

`git branch -D xxx`

