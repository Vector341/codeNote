添加文件

`git add .` 在根目录执行，添加所有文件

`git add *.c ` 添加 C语言文件



如果你想在克隆远程仓库的时候，自定义本地仓库的名字，你可以通过额外的参数指定新的目录名：

```console
$ git clone https://github.com/libgit2/libgit2 mylibgit
```





### 2.4 撤销操作

有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。 此时，可以运行带有 `--amend` 选项的提交命令来重新提交：

```console
$ git commit --amend
```

例如，你提交后发现忘记了暂存某些需要的修改，可以像下面这样操作：

```console
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```

最终你只会有一个提交——第二次提交将代替第一次提交的结果。

<u>但是最终显示的 Date 却是第一次 commit 的时间</u>



### 3.1 分支简介

[Git - 分支简介 (git-scm.com)](https://git-scm.com/book/zh/v2/Git-分支-分支简介)



问题：

<u>git clone 和 git remote add 的区别</u>

空文件里无法 git remote add

