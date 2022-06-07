

背景：有一个在线仓库之前使用命令行管理，现在使用Github Desktop 发现导入仓库后不能直接push，提示需要 publish current branch 然后再提 pull request。操作后线上仓库有两个分支`origin/main` `origin/note/main`，本地分支跟踪到另一个分支



## Remote tracking

用于关联 `main` 与 `origin/main`，`pull` 和 `push` 操作会用到这个关系

## 有三种方式确定

**方法一**

`git clone` 时会关联，显示 `local branch "main" set to track remote branch "o/main"`

**方法二**

`git checkout -b foo origin/main`

**方法三**

`git branch -u origin/main foo`

if foo is current checked out, it can be ignored