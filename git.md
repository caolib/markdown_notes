---
title: git版本控制系统
date: 2024-3-25 21:45:03
categories: 
    - tools
tags: git
---

# [Git](https://nulab.com/zh-cn/learn/software-development/git-tutorial/)

[Git官网](https://git-scm.com/)

## 介绍

> Git是一个开源的**分布式版本控制系统**，用于高效地处理从小到大的项目。Git由Linus Torvalds创建，用于管理Linux内核开发。与集中式版本控制系统不同，如CVS或Subversion，Git采用分布式版本库的方式，不需要服务器端软件支持。这使得源代码的发布和交流变得非常方便。Git的速度很快，特别适合大型项目的版本管理。

## 常用命令

Git的常用命令包括但不限于以下几个：

- `git init`：初始化一个Git仓库。
- `git clone [url]`：克隆一个仓库到本地。
- `git add [file]`：添加文件到暂存区。
- `git commit -m "[message]"`：提交更新，并附加一条提交信息。
- `git status`：查看仓库当前的状态，显示有变更的文件。
- `git push [alias] [branch]`：将本地分支的更新推送到远程仓库。
- `git pull [alias] [branch]`：从远程仓库拉取更新并合并到本地。
- `git branch`：列出所有本地分支。
- `git checkout [branch-name]`：切换到指定分支。
- `git merge [branch]`：合并指定分支到当前分支。

工作流程图

![image-20240325220023918](https://s2.loli.net/2024/09/27/boYzTetVQDfp61R.png)

## 撤消提交

使用[git revert 命令](https://nulab.com/zh-cn/learn/software-development/git-tutorial/git-commands-settings/basic-git-commands/#undo-changes-from-previous-commit)撤消以前的提交。这是撤消更改的最常用方法。

Revert 命令可创建一个新的提交，用于恢复先前提交所做的更改。它允许您撤消不需要的更改，而无需完全移除提交或修改存储库的历史记录。它是一个有用的工具，用于管理对 Git 存储库的更改，同时保留其历史记录。

虽然你可以用[git reset](https://nulab.com/zh-cn/learn/software-development/git-tutorial/git-commands-settings/git-commit-history-commands/#remove-previous-commit)或[git rebase -i](https://nulab.com/zh-cn/learn/software-development/git-tutorial/git-commands-settings/git-commit-history-commands/#modify--move-past-commit-and-messages) 命令，从历史记录中删除先前的提交，但一般不建议这样做，因为这会导致远程存储库与其他成员的本地存储库不同

## 推送

要与他人共享更改，您必须使用 [git push](https://nulab.com/zh-cn/learn/software-development/git-tutorial/git-commands-settings/remote-git-commands/#create--push-branch-changes-to-remote-repository)命令，这将更新远程存储库并将其与本地存储库同步。

## 解决合并冲突

在正确完成合并之前，您可能会遇到需要解决的冲突。例如，如果两个或多个成员在两个不同的分支 (即远程和本地分支) 中对文件的同一部分进行更改，Git 无法自动合并它们。

发生这种情况时，Git 会在冲突文件中添加**冲突解决标记**。这些标记可帮助您确定文件的哪些部分**需要手动处理**。

发生冲突的示例。

在我们上面的例子中，`=====`上面的所有内容都是您的本地内容，下面的所有内容都来自远程分支。

在继续创建合并提交之前，您必须依照下列所示方式解决冲突部分。

修改变更集以解决冲突。

## 修正提交

您可以通过运行 [git commit --amend 命令](https://nulab.com/zh-cn/learn/software-development/git-tutorial/git-commands-settings/git-commit-history-commands/#modify-previous-commit-and-messages)修改同一分支中的最新提交。这个命令可以方便地将新的或更新的文件添加到上一次提交中。这也是一种编辑提交消息或将提交消息添加到上一次提交的简便方法。

用 git commit --amend 修改最新的提交

## 分支

 Git分支是一个非常强大的功能，它允许你在不同的开发线路上独立工作，而不会影响主线（通常是master分支）。这样，你可以在一个分支上开发新功能，而在另一个分支上修复bug，然后再将它们合并回主线。分支的用途包括：

- **功能开发和并行开发**：你可以在不同的分支上同时开发多个功能。
- **Bug修复和紧急修复**：为了不干扰主要开发，可以在单独的分支上进行bug修复。
- **版本发布和稳定的主分支**：保持主分支稳定，只有完全测试通过的代码才能合并。
- **实验性开发和试验性分支**：尝试新想法而不影响主分支。
- **多人协作和团队开发**：团队成员可以在各自的分支上工作，然后合并他们的更改。

假设你正在开发一个新功能，我们将其称为“feature-x”。你不希望这些更改影响主分支，因此你可以创建一个新分支并在其中工作。以下是步骤：

1. **创建新分支**：

   ```bash
   git branch feature-x
   ```

   这将创建一个名为`feature-x`的新分支。

2. **切换到新分支**：

   ```bash
   git checkout feature-x
   ```

   现在你在`feature-x`分支上工作。

3. **添加更改**： 在`feature-x`分支上进行更改后，使用以下命令将它们添加到暂存区：

   ```bash
   git add .
   ```

4. **提交更改**： 提交你的更改，并附加一条提交信息：

   ```bash
   git commit -m "Add new feature x"
   ```

5. **切换回主分支**： 完成开发后，切换回主分支：

   ```bash
   git checkout master
   ```

6. **合并分支**： 将`feature-x`分支的更改合并到主分支：

   ```bash
   git merge feature-x
   ```

7. **推送更改**： 如果你想将更改推送到远程仓库，使用：

   ```bash
   git push origin master
   ```

## gitignore

### 名称

gitignore - 指定有意不跟踪的文件

### 概述

$XDG_CONFIG_HOME/git/ignore, $GIT_DIR/info/exclude, .gitignore

### 描述

`gitignore` 文件指定了 Git 追踪时应忽略的文件。 已被 Git 追踪的文件不受影响，详见下面的注释。

`gitignore` 文件中的每一行都指定了一个模式。 在决定是否忽略路径时，Git 通常会检查多个来源的 `gitignore` 模式，优先级从高到低（在一个优先级内，由最后匹配的模式决定结果）：

- 从支持这些模式的命令行中读取的模式。
- 模式从与路径相同目录下的 `.gitignore` 文件或任何父目录（直到工作树的顶层）中读取，上层文件中的模式会被下层文件中的模式覆盖，直到包含该文件的目录。这些模式相对于 `.gitignore` 文件的位置进行匹配。 项目通常会在其资源库中包含此类 `.gitignore` 文件，其中包含项目构建过程中生成的文件的模式。
- 从 `$GIT_DIR/info/exclude` 中读取的模式。
- 从配置变量 `core.excludesFile` 指定的文件中读取的模式。

将模式放入哪个文件取决于模式的使用方式。

- 应受版本控制并通过克隆分发到其他仓库的模式（即所有开发人员都想忽略的文件）应放入 `.gitignore` 文件。
- 特定于某个仓库但无需与其他相关仓库共享的模式（例如，存在于仓库内部但特定于某个用户工作流程的辅助文件）应放入 `$GIT_DIR/info/exclude` 文件。
- 用户希望 Git 在任何情况下都忽略的模式（例如，由用户选择的编辑器生成的备份或临时文件），一般会放入用户的 `~/.gitconfig` 中由 `core.excludesFile` 指定的文件。它的默认值是 $XDG_CONFIG_HOME/git/ignore。如果 $XDG_CONFIG_HOME 未设置或为空，则使用 $HOME/.config/git/ignore 代替。

底层的 Git 工具，如 *git ls-files* 和 *git read-tree*，会读取命令行选项指定的 *gitignore* 模式，或从命 令行选项指定的文件中读取。 更高层次的 Git 工具，如 *git status* 和 *git add*，会使用上述指定来源的模式。

### 日期格式

- 空行不匹配任何文件，因此可以作为分隔符，以提高可读性。
- 以 # 开头的一行为注释。 对于以散列开头的模式，在第一个散列前面加上反斜杠（ "`\`" ）。
- 除非使用反斜线（"`\`"）引号，否则尾部空格将被忽略。
- 一个可选的前缀 "`!``"，用于否定模式；任何被先前模式排除的匹配文件都将被重新包含。如果文件的父目录已被排除，则无法重新包含该文件。出于性能考虑，Git 不会列出排除的目录，因此无论在哪里定义，任何包含文件的模式都不会产生影响。 对于以 "`!`" 开头的模式，在第一个 "`!`" 前面加反斜杠（"`\`"），例如 "`\!important!.txt`"。
- 斜线 "`/`" 用作目录分隔符。分隔符可以出现在 `.gitignore` 搜索模式的开头、中间或结尾。
- 如果在模式的开头或中间（或两者都有）有分隔符，则该模式是相对于特定 `.gitignore` 文件本身的目录层级而言的。否则，该模式也可能匹配 `.gitignore` 层级以下的任何层级。
- 如果模式末尾有分隔符，则模式只能匹配目录，否则模式既可以匹配文件，也可以匹配目录。
- 例如，模式 `doc/frotz/` 匹配 `doc/frotz` 目录，但不匹配 `a/doc/frotz` 目录；而 `frotz/` 则匹配 `frotz` 和 `a/frotz` 这两个目录（所有路径都是从 `.gitignore` 文件开始的相对路径）。
- 星号 "`*`" 匹配斜线以外的任何字符。 字符 "`?`" 匹配除 "`/`" 以外的任何一个字符。 范围符号，如 "`[a-zA-Z]`"，可用于匹配范围中的一个字符。更详细的说明请参见 fnmatch(3) 和 FNM_PATHNAME 标志。

在与全路径名匹配的模式中，两个连续的星号（"`**`"）可能有特殊含义：

- "`**`"在带斜杠目录之前，表示在所有目录中匹配。例如，"`**/foo`"匹配任何文件或目录的"`foo`"，与模式"`foo`"相同。"`**/foo/bar`"匹配任何文件或目录中直接位于目录"`foo`"之下的"`bar`"。
- 路径后跟有 "`/**`" 表示匹配这个目录里面的所有文件。例如，"`abc/**`" 匹配相对于 `.gitignore` 文件的位置中目录 "`abc`" 内的所有文件，深度无限。
- 一个斜杠后面是两个连续的星号再接上一个斜杠，匹配零个或多个目录。例如，"`a/**/b`" 匹配 "`a/b`"、"`a/x/b`"、"`a/x/y/b`"，等等，依此类推。
- 其他连续星号被视为普通星号，将根据前面的规则进行匹配。

### 配置

可选的配置变量 `core.excludesFile` 表示包含要排除的文件名模式的文件路径，类似于 `$GIT_DIR/info/exclude`。 除 `$GIT_DIR/info/exclude` 中的模式外，还将使用排除文件中的模式。

### 注释

使用 gitignore 文件的目的是确保某些不被 Git 追踪的文件不被追踪。

要停止跟踪当前已被跟踪的文件，可使用 *git rm --cached* 从索引中移除该文件。文件名随后会被添加到 `.gitignore` 文件中，以防止该文件在以后的提交中被重新引入。

访问工作树中的 `.gitignore` 文件时，Git 不会跟踪符号链接。这样，当从索引或工作树访问文件时，与从文件系统访问文件时的行为保持一致。

### 实例

- 模式 `hello.*` 匹配名称以 `hello.` 开头的任何文件或目录。如果只想将其限制在目录中，而不限制在其子目录中，则可以在模式前加上斜线，即 `/hello.*`；现在该模式可匹配 `hello.txt` 和 `hello.c` 但不匹配 `a/hello.java`。
- 模式 `foo/` 将匹配目录 `foo` 及其下的路径，但不会匹配常规文件或符号链接 `foo`（这与 Git 中 pathspec 的一般工作方式一致）
- `doc/frotz` 和 `/doc/frotz` 模式在任何 `.gitignore` 文件中都有同样的效果。换句话说，如果模式中已经有中间斜线，那么前导斜线就无关紧要了。
- 模式 `foo/*` 匹配 `foo/test.json`（一个正则文件）和 `foo/bar`（一个目录），但不匹配 `foo/bar/hello.c`（一个正则文件），因为模式中的星号不匹配 `bar/hello.c`，因为 `bar/hello.c` 中含有斜线。

```
$ git status
[...]
# 未追踪的文件:
[...]
#       Documentation/foo.html
#       Documentation/gitignore.html
#       file.o
#       lib.a
#       src/internal.o
[...]
$ cat .git/info/exclude
# 忽略在目录树中的所有对象和存档文件
*.[oa]
$ cat Documentation/.gitignore
# 忽略 html 文件,
*.html
# 但追踪自己写的 foo.html
!foo.html
$ git status
[...]
# 未追踪的文件:
[...]
#       Documentation/foo.html
[...]
```

再举一个例子：

```
$ cat .gitignore
vmlinux*
$ ls arch/foo/kernel/vm*
arch/foo/kernel/vmlinux.lds.S
$ echo '!/vmlinux*' >arch/foo/kernel/.gitignore
```

第二个 .gitignore 阻止 Git 忽略 `arch/foo/kernel/vmlinux.lds.S`。

示例排除除特定目录 `foo/bar` 以外的所有内容（注意 `/*` - 如果没有斜线，通配符也会排除 `foo/bar` 中的所有内容）：

```
$ cat .gitignore
# 排除 foo/bar 以外的所有内容
/*
!/foo
/foo/*
!/foo/bar
```

## 简化使用

将下面代码保存为py文件，就能直接运行python执行一些常用操作，**注意`git glog`是我自定义的一个命令**

```python
import os

def show():
    print('---------------------------------------------------------')
    print('enter ---> 状态')
    print('1     ---> 添加')
    print('2     ---> 提交')
    print('3     ---> 推送')
    print('4     ---> 拉取')
    print('5     ---> 添加、提交、推送')
    print('6     ---> 日志')
    print('7     ---> 远程仓库')
    print('else  ---> 退出')
    print('---------------------------------------------------------')

while True:
    try:
        show()
        choice = int(input('chioce: '))
        if choice == None:
            os.system('git status')
        elif choice == 1:
            os.system('git add .')
        elif choice == 2:
            message = input('Enter your message: ')
            os.system(f'git commit -m "{message}"')
            print(f'message:{message}')
        elif choice == 3:
            os.system('git push')
        elif choice == 4:
            os.system('git pull')
        elif choice == 5:
            message = input('Enter your message: ')
            os.system(f'git add .')
            os.system(f'git commit -m "{message}"')
            os.system(f'git push')
            print(f'message:{message}')
        elif choice == 6:
            os.system('git glog')
        elif choice == 7:
            os.system('git remote -v')
        else:
            exit()
    except Exception as e:
        if isinstance(e, ValueError):
            os.system('git status')
        print('Invalid choice')
```
