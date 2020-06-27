
## 初识 shell

shell 的英文有“壳”的意思，为什么这种脚本语言叫“壳”呢？上图：

```sequence
User->Shell/Commond:
Shell/Commond->Linux Kernel:
Linux Kernel->Hardware:

Hardware-->Linux Kernel:
Linux Kernel-->Shell/Commond:
Shell/Commond-->User:

```

图画得不是太好，shell 相当于给内核和其他 command 加了一层壳，用户通过 command 或 shell 和 Kernel 打交道。

我是这样理解 shell的：

- shell 允许用户编写 script（即脚本， 高大上一点叫程序）

- shell 需要有个 command 去解释执行这个 script，比如 `/bin/bash`

```
shell = shell command + shell script
shell = window批处理
```

shell 有很多种，我知道的以下几种，各种 shell 之间的差异其实不是很大，不用一些特殊功能的话，当然每种 shell 都是各具特色的。

- Bourne shell 鼻祖

- Bourne Again shell ( bash) 现在使用最广泛的

- C shell ( csh) 貌似所有类UNIX系统都内置

- Dash Shell Ubuntu 默认shell
  
- Z shell（Zsh）我的的 macOS 里装的是 Oh My Zsh





## 年青人的第一个shell脚本

当然又是Hello World了，新建一个hello

```bash
#!/bin/bash
# 第一行的是意思是，如果不指定用什么shell运行，则默认使用/bin/bash

# 单行注释

# 多
# 行
# 注
# 释

echo "Hello World"
```

## 执行 shell

```bash
# 方法一
/bin/sh myshell.sh
# 检查语法错误
/bin/sh -n myscript.sh

# 方法二
chmod u+x myshell.sh
./myshell.sh
```