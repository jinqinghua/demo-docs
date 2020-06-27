
## 这个不会在侧边栏显示 {docsify-ignore}

## 我是谁

- 屌丝码家一枚
- 坐标某新晋新一线城市
- 某世界500强公司打杂

## 我为什么要写这本书

- 学习总结笔记
- 备忘，方便自己以后 `ctrl + c`，`ctrl + v` 代码
- 最重要的：看看到底什么时候会放弃？会不会完成？算是对自己的一个考验吧

## 这本书面向的读者是谁

- 有一定Linux基础，最起码会登录Linux系统，会个 `cd` `ls` 什么的
- 有一定的编程基础， 从 A 到 Z 开头的语言最好都会 :smile:
- 代码搬运工：过来 `ctrl + c`，`ctrl + v` 代码的。放心，后面会上不少干货的

## 写这本书的原则

- 不啰嗦，大家时间宝贵。
- Talk is cheap, Show me the code.

## 我为什么要学习 shell

其实学习 shell 不是UNIX/Linux 系统管理员的专利，我也有学习 shell 的需求：

- 使用 mac OS 系统开发程序

- 公司的 dev/qa/prod 环境全部是 Cent OS

- 我需要读懂别人的 shell 脚本，这是刚性需求

- 需要 shell 写一些自动化的脚本（当然 python 或许是更好的选择），比如：

  - 一个大项目包含几个 repo， 我需要写个脚本自动 pull 所有的 repo code 到最新
  - 需要在 Jenkins 里加一些自动化的代码
  - 搞个定时任务自动同步/备份一些数据
  - 在 Linux 上部署个 java 后台 Service

- DevOps 的盛行

- 投资回报率高

  shell 只是有些诡异，但语言语法学习成本其实并不高，难点在于对于 Linux 的熟悉程度

  shell 语言变化小，不象某些语言：比如，你经历过 QBasic -> VB -> VB.NET -> 啥都不是的历程吗？

## 需要准备哪些工具

### 操作系统

- macOS

- Linux (最新任意发行版，推荐 CentOS)

- Windows + 远程 Linux 主机

  - 需要一款 SSH Client, 比如：[PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/) （音标：`/ˈpʌti/` 别读错了，特别是那个 **ʌ**）

- Windows （不推荐）

  虽然不推荐，但如果你恰好只满足这个条件，那么建议下载一个 [Git for windows](https://git-scm.com/download/win)，这玩意会自带一个叫 MinGW 的 Linux 命令行工具，可以执行简单的 shell 命令，与cygwin 功能类似。但作为学习，凑合用着先．但是有几点不同要特别注意：
  - `\` 会被转成 `/`
  - c: 会补转成 `/c`

​       也就是说 `c:\git\shell`  要写成 `/c/git/shell`

### IDE

​	vi （新手其实我不推荐）

​	vscode（跨平台，免费。再装个 shell-format 插件可以格式化代码）

​	InteliJ IDEA（跨平台，有免费社区版。不但可以格式化代码，还会 check 代码，发现问题并提示，缺点是有点重了，但是我喜欢啊）