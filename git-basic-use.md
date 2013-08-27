# Git

## Git基本配置

### 用户名、邮箱（首要）

```
git config --global user.name <your name>
git config --global user.email <your email>
```

### 操作系统换行符设置（重要）

Windows系统默认为CRLF，Linux/Unix默认为LF

#### Linux/Unix系统

```
git config --global core.autocrlf input
```
使Git在提交时把CRLF转换成LF，签出时不转换

#### Windows系统

```
git config --global core.autocrlf true
```
签出代码时，LF会被转换成CRLF

> 请注意：这里所指的系统是Git所在的系统类型，如果用Windows上的ssh客户端连接虚机，使用的是虚机中的git，此时为Linux系统，不要混淆。

> 另外，也可以设置编辑器的默认换行符来进行统一


### 别名（alias）

```
git config --global alias.<alias name> <real command>
```

示例：`git config --global alias.st status`

### 配色

```
git config --gloabl color.ui true
```

以彩色文字显示（需终端支持）

> 所有配置选项可以用`git config -e --global`方式打开进行编辑。实际上编辑的是`~/.gitconfig`文件。

## Git基本的工作流程

新的工作目录 --> `git init` --> `git add <files>` --> `git commit -m 'commit content'`

### 详解

#### `git init`

将工作目录初始化为一个git工作区，会在目录下建一个隐藏目录.git，包含工作区的基本信息。

如果删除.git目录，将会使工作区变为一个普通文件夹，如果没有远程版本库作为备份，将会丢掉所有历史提交纪录！

#### `git add <files>`

将文件加入到暂存区

#### `git commit`

正式提交，生成历史纪录

> 即使修改的是已经添加到版本库中的文件，在提交时依然需要执行一次`git add`，这是git区别于svn的一个重要地方。或者以`git commit -a`的方式直接提交。

### 其他

#### 关于.gitignore文件

在每一个项目的根目录下，可以添加一个.gitignore文件，用以标识哪些文件不希望加入到版本库中。也可以加到子目录中，子级会覆盖父级。基本格式如下：

```
build/
output/
._*
.DS_Store
```

解释：

忽略build和output文件夹下的所有文件

忽略所有以`._`开头的文件

忽略所有文件名为`.DS_Store`文件

> 详细配置请自行查阅相关资料。

### Git工作区、暂存区、版本库理解

……

## Git分支

### 理解

我们可以把分支按字面意思理解为一棵树的不同分叉，而每一个分叉又有一个名字，即为分支名

### 相关命令

#### `git branch`

查看分支。列出本地所有的分支名，当前分支前以*标识

#### `git branch <name> <commmit id>`

以name为名，以指定的`commit id`为基准新建一个分支，如省略`commit id`则以当前状态新建分支。

> 注意：新建分支后并不会切换到新的分支，需要手动切换分支，命令为：
`git checkout <branch name>`


#### `git checkout -b <branch name> <commit id>`

新建并切换分支

#### `git branch -d <branch name>`

删除分支

## 分支、HEAD理解

……

## 引用父子级理解

HEAD指向当前提交，HEAD^指向上一次提交，HEAD^^指向HEAD^的上一次提交，以此类推

HEAD^^也可以表示为HEAD~2

## Git常用命令

### 1. `git status <-s>`

查看工作区当前状态，-s则以精简方式查看

### 2. `git log <--oneline|--decorate|--graph|-p>`

查看提交历史纪录

### 选项解析

#### `--oneline`

单行模式

#### `--decorate`

显示引用

#### `--graph`

以图形方式查看

#### `-p`

显示每次的修改摘要

### 3. `git diff -- <file path>`

由于一个项目存在工作区、暂存区、版本库三种状态，所以`git diff`也分如下几种情况：

#### 工作区与暂存区比较

```
git diff
```

#### 暂存区与版本库比较

```
git diff --cached
```

#### 工作区与版本库比较

```
git diff HEAD
```

### 4. `git reset <--soft|--mixed|--hard> <commit id>`

将引用重置到某一次提交

#### 选项解析

#### `--soft`

重置分支的引用（指向），但不更改暂存区和工作区

#### `--mixed（默认）`

重置分支的引用（指向），并用指定提交的内容覆盖暂存区，不修改工作区

#### `--hard`

重置分支的引用（指向），并用指定提交的内容覆盖暂存区和工作区

#### 其他

#### `git reset HEAD -- file`

用最新提交中的文件覆盖暂存区中的文件，相当于执行了`git add`的反向操作

#### `git reset --soft HEAD^`

将引用回退一次，即对最新的一次提交不满意时，撤销最新提交，进行重新提交

### 5. `git checkout`

`git checkout`除了前面介绍的切换分支以外，还有几种用法：

#### `git checkout -- file`

用暂存区的文件覆盖工作区的文件，相当于撤销当前工作区的改动

#### `git checkout <commit id> -- file`

检出指定提交中的文件，覆盖暂存区和工作区

#### `git checkout -b <new branch name> <start point>`

以指定的提交为基点新建分支，并切换到新的分支

> 请以字面意思理解`git checkout`，它的作用就是检出文件

### 6. `git merge`

合并分支

首先，切换到要合并到的分支，然后执行`git merge <branch name>`就会把名为<branch name>的分支合并到当前所在的分支

例如：
要把一个特性分支ptb合并到主分支master，则先切换到master分支，执行`git merge ptb`，就会把ptb分支的提交合并到master中

#### 其他

```
git merge <branch name> --no-ff
```

强制新建一次提交，即使是fast-forward模式

### 7. `git tag`

打标签

标签类似于分支，但不会随着每次提交自动变化。

#### `git tag`

查看所有标签

#### `git tag <-a> -m <msg content> <commit id>`

新建标签，指定标签信息，可以指定在某个提交上打标签

#### `git tag -d <tag name>`

删除标签

## Git远程版本库

### 远程库的概念

远程版本库，我们可以理解为将本地的版本库做一个备份，类似于网盘的东西。

### 新的概念

#### 远程分支

当一个版本库与远程进行交互之后，便会产生一个远程分支，分支名为`<source name>/<branch name>`，例如最常见的远程分支名为`origin/master`。

#### 推送

推送是指将本地版本库的修改提交同步到远程中的版本库，注意，此时的同步并非简单的完整复制，而是需要指定同步的分支。

#### 拉取

拉取是指将远程版本库的提交同步到本地，但是拉取下来的内容并不会自动与本地合并，而是在远程分支中显示改变。例如，当将远程版本库`dev`分支中的提交拉取下来时，会同步到本地的`origin/dev`分支中来。

### 常用命令

#### `git remote add <source name> <source url>`

添加一个远程版本库

#### `git push <-u> <source name> <branch name>`

将本地提交推送到远程版本库，可以指定远程版本库名与分支名。`-u`参数会使得本地分支建立与远程分支的对应关系，以后可以直接执行`git push`即可同步。

如果指定的分支远程版本库中不存在，将会新建远程分支。

#### `git fetch <source name> <branch name>`

拉取远程版本库的更新

#### `git pull <source name> <branch name>`

拉取远程版本库的更新并合并到当前的本地分支中，相当于执行了`git fetch` + `git merge`

#### `git push  :<branch name>`

删除远程分支，注意`:`与`push`中间有两个空格

### 标签与分支的操作类似

将上文中的`<branch name>`改为`<tag name>`即可

## Git常见问答

### 1. 怎么查看最近提交的n条纪录？

```
git log -n
```

### 2. 刚修改了一些东西或者误删了文件，但想放弃修改，怎么办？

```
git checkout -- <file name>
```

### 3. 刚把一些东西加到了暂存区，想反悔，怎么办？

```
git reset HEAD -- <file name>
```

### 4. 如何想取出某一次提交中的某个文件？

```
git checkout <commit id> -- <file name>
```

### 5. 对刚才的提交不满意，怎么撤回？

```
git reset --soft HEAD^

修改文件或者直接重新编辑提交信息进行再次提交

或者使用git commit --amend <commit msg>进行修改
```

### 6. 已经加到版本库中的文件，怎么忽略掉？

```
git rm --cached <file name>
```

将要忽略的文件加入到`.gitignore`文件中，提交

### 7. 待补充……

## 规范

* 复杂项目请务必添加`.gitignore`文件，保持一个干净的工作区
* 请经常使用`git status`查看工作区的状态
* 善用`git <command> --help`命令查看帮助
* ……

## 日常工作中的合作模型

参考自[一个成功的Git分支模型](http://www.juvenxu.com/2010/11/28/a-successful-git-branching-model/)







