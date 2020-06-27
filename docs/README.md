# Kim's Shell Cookbook

[toc]





## 第三部分 概念篇

了解 Linux 及 Shell 概念是最最重要的。我们深受 Windows 的荼毒，就象《天龙八部》中的虚竹，本来学的是少林的武功，现在要学习逍遥派的武功，必须要将原来的武功废掉。

### 文件/目录

#### 一切皆文件

#### 根目录 /

#### 软/硬链接

软链接是 DevOps 中非常有用，非常常用，也非常好用的一个功能：
```shell
# ln -s TARGET LINK_NAME
ln -s /usr/local/java/jdk1.8.0_231 /usr/local/java/jdk1.8.0
# JAVA_HOME 指向链接名 /usr/local/java/jdk1.8.0
# 如果JDK升级了，直接安装后，将链接名指向最新的版本即可完成JDK版本切换
```
Windows 7 以后好象有了这个功能：

```bat
# mklink /d LINK_NAME TARGET
# 注意后面的两个参数和 Linux 正好相反，Windows 不但要抄别人的，还要恶心别人:)
mklink /d "D:\java\jdk\jdk1.8.0" "C:\Program Files\Java\jdk1.8.0_231"
```

#### 文件权限设定

// TODO

### 命令（command)

#### 一般格式

```shell
command [options] [arguments...]
# []中的内容表示是可选的
# ...表示可以有一个或多个
```

#### 退出状态码（exit code)

##### 什么是退出状态码
- 0： 命令（commannd) 成功执行
- 非0 [1~255]：命令（commannd) 执行失败
##### 打印退出状态码

```shell
command
echo $? # $?表示上个命令返回的退出状态码，范围：[0, 255], 0成功，其他失败。

pwd
echo $? # => 0

:
echo $? # => 0 一定返回0

true
echo $? # => 0 一定返回0

false
echo $? # => 1 一定返回1

[ -n "${HOME}" ]
echo $? # => 0

[ -z "${HOME}" ]
echo $? # => 1

(( 1 ==  0 ))
echo $? # => 1 # 1 == 0是错误的，所以exit code是1

(( 1 >  0 ))
echo $? # => 0 # 1 > 0是正确，所以exit code是0
```

##### 常见的退出状态码

| 退出状态码 (exit code) | 描述                     |
| ---------------------- | ------------------------ |
| 0                      | 命令成功结束             |
| 1                      | 一般性未知错误           |
| 126                    | 命令不可执行             |
| 127                    | 没找到命令               |
| 128                    | 无效的退出参数           |
| 130                    | 通过Ctrl+C终止的命令     |
| 255                    | 正常范围之外的退出状态码 |

##### 自定义退出状态码

- 任意地方退出 exit [n]
- function中退出 return [n]
- n 如果不指定，默认返回最后一个 command 的退出状态码 (exit code)
- n 超过 255，可能返回意想不到的内容

```shell
cat > exit.sh<<EOF
exit 300
EOF

chmod 755 exit.sh
./exit.sh
echo $? # => 44，不是300
```

#### 内部命令/外部命令

不同 shell 的内外部命令不同，本文以 Bash Shell V4为准。

内部命令是 shell 内置的，性能更佳。

##### 查看内部命令

- help [-d]

```shell
job_spec [&] 
(( expression )) 
. filename [arguments] 
:
[ arg... ] 
[[ expression ]] 
alias [-p] [name[=value] ... ] 
bg [job_spec ...]
bind [-lpsvPSVX] [-m keymap] [-f filename] [-q name] [-u name] [-r keyseq] [-x keyseq:shell-command] [>
break [n]
builtin [shell-builtin [arg ...]]
caller [expr]
case WORD in [PATTERN [| PATTERN]...) COMMANDS ;;]... esac 
cd [-L|[-P [-e]] [-@]] [dir] 
command [-pVv] command [arg ...] 
compgen [-abcdefgjksuv] [-o option] [-A action] [-G globpat] [-W wordlist][-F function] [-C command]>
complete [-abcdefgjksuv] [-pr] [-DE] [-o option] [-A action] [-G globpat] [-W wordlist][-F function]>
compopt [-o|+o option] [-DE] [name ...]
continue [n] 
coproc [NAME] command [redirections] 
declare [-aAfFgilnrtux] [-p] [name[=value] ...]
dirs [-clpv] [+N] [-N] 
disown [-h] [-ar] [jobspec ... | pid ...]
echo [-neE] [arg ...]
enable [-a] [-dnps] [-f filename] [name ...] 
eval [arg ...] 
exec [-cl] [-a name] [command [arguments ...]] [redirection ...] 
exit [n] 
export [-fn] [name[=value] ...] or export -p 
false
fc [-e ename] [-lnr] [first] [last] or fc -s [pat=rep] [command] 
fg [job_spec]
for NAME [in WORDS ... ] ; do COMMANDS; done 
for (( exp1; exp2; exp3 )); do COMMANDS; done
function name { COMMANDS ; } or name () { COMMANDS ; } 
getopts optstring name [arg] 
hash [-lr] [-p pathname] [-dt] [name ...]
help [-dms] [pattern ...] 
history [-c] [-d offset] [n] or history -anrw [filename] or history -ps arg [arg...]
if COMMANDS; then COMMANDS; [ elif COMMANDS; then COMMANDS; ]... [ else COMMANDS; ] fi
jobs [-lnprs] [jobspec ...] or jobs -x command [args]
kill [-s sigspec | -n signum | -sigspec] pid | jobspec ... or kill -l [sigspec]
let arg [arg ...]
local [option] name[=value] ...
logout [n]
mapfile [-d delim] [-n count] [-O origin] [-s count] [-t] [-u fd] [-C callback] [-c quantum] [array]
popd [-n] [+N | -N]
printf [-v var] format [arguments]
pushd [-n] [+N | -N | dir]
pwd [-LPW]
read [-ers] [-a array] [-d delim] [-i text] [-n nchars] [-N nchars] [-p prompt] [-t timeout] [-u fd] >
readarray [-n count] [-O origin] [-s count] [-t] [-u fd] [-C callback] [-c quantum] [array]
readonly [-aAf] [name[=value] ...] or readonly -p
return [n]
select NAME [in WORDS ... ;] do COMMANDS; done
set [-abefhkmnptuvxBCHP] [-o option-name] [--] [arg ...]
shift [n]
shopt [-pqsu] [-o] [optname ...]
source filename [arguments]
suspend [-f]
test [expr]
time [-p] pipeline
times
trap [-lp] [[arg] signal_spec ...]
true
type [-afptP] name [name ...]
typeset [-aAfFgilnrtux] [-p] name[=value] ...
ulimit [-SHabcdefiklmnpqrstuvxPT] [limit]
umask [-p] [-S] [mode]
unalias [-a] name [name ...]
unset [-f] [-v] [-n] [name ...]
until COMMANDS; do COMMANDS; done
variables - Names and meanings of some shell variables
wait [-n] [id ...]
while COMMANDS; do COMMANDS; done
{ COMMANDS ; }
```
- type [-a] command

```shell
type echo
echo is a shell builtin

type ln
ln is /usr/bin/ln
```

#### 诡异的命令

```shell
# 你能想到下面的是命令（command)吗？
(( expression ))        # 用于数学计算
. filename [arguments]  # 等同于 source filename [arguments]
:                       # 退出状态码为 0，啥也不干
[ arg... ]              # test arg...
[[ expression ]]        # test expression
{ COMMANDS ; }          #
true                    # 退出状态码为 0 注意：true 是命令，不是布尔值
false                   # 退出状态码为 1 注意：false是命令，不是布尔值
```

#### 诡异的结构控制

```shell
if COMMANDS; then COMMANDS; [ elif COMMANDS; then COMMANDS; ]... [ else COMMANDS; ] fi
until COMMANDS; do COMMANDS; done
while COMMANDS; do COMMANDS; done
```
学过 C 系语言的同学都知道，这几个结构，COMMANDS的地方都应该是 boolean 值，但 shell 实际上是可以理解为这样：

`COMMANDS's exit code == 0`

## 变量

### 全局变量

执行 env 命令得到全局变量，全局变量名一般使用大写字母加下划线的方式

### 局部变量

一般也叫用户自定义变量，一般使用小写字母加下划线的方式

#### 变量赋值和使用和删除

```bash
#!/bin/bash

# 变量赋值
v1=Hello
v2=Hello World
v3='$v1 World'
v4="$v1 World"

# 使用变量
echo $v1
echo ${v2}
echo ${v3}
echo ${v4}

# 变量重新赋值
v4="Hello China"
echo ${v4}

# 删除变量
unset v4
echo ${v4}
```

运行结果

```bash
$ ./variable.sh
./variable.sh: line 5: World: command not found
Hello

$v1 World
Hello World
Hello China

---
```

**总结**

- 变量赋值的 = 两边不能有空格（强制）
- 变量值最好用单引号或双引号括起来，以免引起错误，如v2
- 单引号原样输出（所以中间不能再有单引号，哪怕是用转义符\转义）
- 双引号中的变量会转成变量值
- 变量使用有两种方式，推荐带花括号的写法
  - $变量名
  - ${变量名}



## 失落的部分 总结篇

### 诡异的 $

| $ 的各种形式             | 解释                                     | 相关                                              |
| ------------------------ | ---------------------------------------- | ------------------------------------------------- |
| \${var} 或 \$var           | 得到变量的值                             | 推荐 \${var} 这种写法，第二种写法容易产生问题      |
| \${var[*]} 或 \${var[@]}   | 取得数组或字符串全部元素                     | 推荐 \${var[*]}，   更容易理解，字符串中[\*]可省略 |
| \${#var[*]} 或 \${#var[@]} | 取得数组或字符串的长度                | 推荐 \${#var[*]}，更容易理解，字符串中[\*]可省略           |
|                          |                                          |                                                   |
| \$* 或 \$@                 | 传递给当前执行脚本或函数的所有参数       | [\$* 与 \$@ 的不同点](#\$* 与 \$@ 的不同点)           |
| \$#                       | 传递给当前执行脚本或函数参数个数（长度） |                                                   |
| \$0                       | 当前执行脚本的文件名                     |                                                   |
| \$1...n                   | 传递给当前执行脚本或函数的参数           |                                                   |
|                          |                                          |                                                   |
| \$$                       | 当前 command 或 shell 的进程 ID          |                                                   |
| \$?                       | 上个命令执行后的退出状态码（exit code)。   |                                                   |
| \$(command \| function)   | 得到命令或函数的执行结果                 | \`command \| function\`  这是另一种写法，但不推荐 |
| \$[ expr ]                | 整数计算                                 | 和 expr 命令差不多，但这个更好                    |

### \$* 与 \$@ 的不同点

两者基本等价，仅放在在双引号 "" 里的时候有细微差别，直接上代码

```bash
#!/bin/bash

function echo_params(){
    echo
    echo "# for item in \$*"
    for i in $*; do
        echo $i
    done

    echo
    echo "# for item in \$@"
    for i in $@; do
        echo $i
    done

    echo
    echo "# for item in \"\$*\""
    for i in "$*"; do
        echo $i
    done

    echo
    echo "# for item in \"\$@\""
    for i in "$@"; do
        echo $i
    done
}

echo_params 1 2 3
```
运行结果：

```bash
# for item in $*
1
2
3

# for item in $@
1
2
3

# for item in "$*"
1 2 3

# for item in "$@"
1
2
3
```

总结一下：

- "$*" = "1 2 3"

- $* = $@ = "$@" = array(1 2 3)



## Shell 中不推荐的用法及替代方案

| 不推荐的用法                                                 | 替代方案                    | 原因                                                   |
| ------------------------------------------------------------ | --------------------------- | ------------------------------------------------------ |
| \`command\`                                                  | $(command)                  | 前者多个嵌套会出问题，后者还有很多变态的用法，特别好用 |
| $name                                                        | ${name}                     | 后者不易出问题，比如：\$name2 和 \${name}2             |
| expr arithmetic expression<br />( arithmetic expression )<br />let "arithmetic expression"<br />$[ arithmetic expression ] | (( arithmetic expression )) | 后者支持基本数学运算符                                 |
| [ expression ]                                               | [[ expression ]]            |                                                        |
|                                                              |                             |                                                        |
|                                                              |                             |                                                        |
|                                                              |                             |                                                        |
|                                                              |                             |                                                        |



