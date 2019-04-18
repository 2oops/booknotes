# ErrorHandle

1. 重装系统后，git相应也重装成功(git bash)

```
＄C:\Users\Administrator>git
fatal: open /dev/null or dup failed: No such file or directory
```

这里不整花里胡哨的东西，直接上亲测有效的方法吧，文件夹中找到该文件：`C:\Windows\System32\drivers\null.sys`，替换为以下文件：[win10 专业版 git bash 闪退问题终极解决方案](<http://www.cnblogs.com/ricklz/p/9216395.html>) 最下面有`null.sys`的下载地址，或者直接进入[网盘下载](https://pan.baidu.com/s/1UtcZizm-iFcVk4OKrnFJVg)，密码：`1q4d`。再没有请联系作者，我给你们网盘下载地址。替换完成就OK了。

2. Git关于权限的问题终极解决方案

   [git -- Authentication failed for 修改密码后遇到的坑](https://blog.csdn.net/qq_40028324/article/details/80883010)

   凭据管理器-->管理Windows凭据-->删除git相关的那个凭据

3. `npm`安装镜像问题

```
npm ERR! code Z_BUF_ERROR
npm ERR! errno -5
npm ERR! zlib: unexpected end of file
```

[解决方法](https://blog.csdn.net/adc_god/article/details/77989869)：因人而弃，亲测方式顺序有点不一致

1)安装cnpm

　`npm install -g cnpm --registry=https://registry.npm.taobao.org`

2) 修改下载仓库为淘宝镜像

　 `npm config set registry http://registry.npm.taobao.org/`

调整完就OK了，并没有第三步。

3. 电子书目录下运行`gitbook init`时一直停留在`gitbook installing 3.2.3`

```
C:\Users\Administrator>gitbook -V
CLI version: 2.3.2
Installing GitBook 3.2.3
....
```

这里直接换一种思路，在命令行中`gitbook -V`, `Gitbook`会自动初始化，再次进入电子书目录即可正常使用。