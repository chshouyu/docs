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

### 别名（alias）

```
git config --global alias.<alias name> <real command>
```

示例：`git config --global alias.st status`

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

## Git常用命令

### `git status`

查看工作区当前状态


