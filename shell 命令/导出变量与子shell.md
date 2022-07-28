# 导出变量与子shell

当你登陆系统后，得到的都是 shell 程序的全新副本（称为**登陆shell**），它维护你所用的环境变量（针对每个用户都不同）

当 登陆shell 执行脚本时，他会启动一个新shell来执行该程序（称为**子shell**），它拥有自己的环境变量

子shell 不知道 父shell 中赋值的局部变量



## 一些重要的环境变量

HOME PATH PS1 PS2

```shell
zhuyifei@cn3614001596m ~ % echo $PS1
%n@%m %1~ %# 
zhuyifei@cn3614001596m ~ % PS1=">>>>"

>>>>echo $PS1
>>>>

```





## export

使用 export 可以导出变量

```shell
export x
export http_proxy=http://127.0.0.1:7890 # 赋值并导出
```

1．未被导出的变量都是局部变量，子Shell并不知道这些变量的存在。
2．导出的变量及其值会被复制到子Shell的环境中，在其中可以访问并修改这些导出变量。但是这些修改不会影响到父Shell中的变量。
3．导出变量不仅保持在直接生成的子Shell中，对于由这些子Shell所生成的子Shell也不例外（依次递推）。
4．变量可以在赋值前后随时导出，但是只取其导出时的值，不再理会之后做出的改变。

比如下一个例子说明在子shell中改变当前路径不会影响登陆shell

```shell
# cdtest 脚本内容如下
zhuyifei@cn3614001596m ~ % cat cdtest
cd / 
 pwd

zhuyifei@cn3614001596m ~ % pwd
/Users/zhuyifei

zhuyifei@cn3614001596m ~ % ./cdtest
/
zhuyifei@cn3614001596m ~ % pwd
/Users/zhuyifei
zhuyifei@cn3614001596m ~ % 

```



## source OR dot(.)

将某个文件的命令加载到当前shell执行，这样可以使赋值的变量在子程序运行完毕后保存

```sh
zhuyifei@cn3614001596m childShell % . ./db
```



## shell 脚本启动子shell 接受输入

```sh
HOME=/Users/Shared
BIN=$HOME/bin

PATH=$PATH$BIN
CDPATH=:$HOME:$BIN

PS1=DB:

export HOME BIN PATH CDPATH PS1

/bin/sh # 启动子shell
exec /bin/sh # 或用 /bin/sh替换当前shell
```

执行输出

```sh
zhuyifei@cn3614001596m childShell % ./db
DB:echo $HOME
/Users/Shared
DB:exit
exit
zhuyifei@cn3614001596m childShell % 
```



## (...) { ...; }

将命令写在括号中形成一个命令组

小括号表示在 子shell 中执行，大括号表示在 当前shell中执行

```shell
zhuyifei@cn3614001596m script-playground % (x=39;)
zhuyifei@cn3614001596m script-playground % echo $x

zhuyifei@cn3614001596m script-playground % { x=34; }
zhuyifei@cn3614001596m script-playground % echo $x
34

```



## 在一行内将变量传给子shell

```shell
$ x=10 script
```

