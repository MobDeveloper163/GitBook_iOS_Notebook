# Git的基本使用

## 创建Git仓库

### 创建Git仓库有两种途径

1. 把现有的工程或者文件夹导入到Git中
2. 从另一个服务器克隆一个Git仓库

## 配置和初始化仓库

如果从现有的工程中使用git追踪文件，你需要到工程的文件夹输入：

```
git init
```

如果从远程服务器获取克隆仓库，你需要输入git clone \[url\] 。比如从github克隆开源中国客户端：

```
git clone https://github.com/jimneylee/JLOSChina-iPhone.git
```

## 开始和停止追踪文件

记住在你的工作目录中的文件有两种状态：跟踪的（tracked）和未被跟踪的（untracked）

![](/assets/git文件的生命周期.png)

查看文件的状态

```
git status
```

追踪文件,比如追踪README.txt

```
git add README.txt
```

```
git add README.txt
```

## 组织和提交文件更新

如果修改了之前已经追踪的文件，也就是一个状态是tracked的文件时，比如CONTRIBUTING.md为tracked的状态，修改之后再使用git status命令,此时的内容如下：

> $ git status
> 
> On branch master
> 
> Your branch is up-to-date with 'origin\/master'.
> 
> Changes to be committed:
> 
> \(use "git reset HEAD &lt;file&gt;..." to unstage\)
> 
> new file: README
> 
> Changes not staged for commit:（意思是说工作目录中被追踪的文件已更新，但是还没有组织）
> 
> \(use "git add &lt;file&gt;..." to update what will be committed\)
> 
> \(use "git checkout -- &lt;file&gt;..." to discard changes in working directory\)
> 
> modified: CONTRIBUTING.md

Changes not staged for commit:（意思是说工作目录中被追踪的文件已更新，但是还没有组织）。使用git add命令来组织这些文件。git add是一个多功能命令。你是用它来追踪一个新文件，组织文件，以及做其他的事情，比如把冲有突的文件标记为已解决。把git add理解为“把内容添加到下一次提交”比“把文件添加到工程”或许更有帮助。此时我们使用git add命令来组织CONTRIBUTING.md文件，再运行git status命令。

> **$** git add CONTRIBUTING.md
> 
> **$** git status
> 
> On branch master
> 
> Your branch is up-to-date with 'origin\/master'.
> 
> Changes to be committed:
> 
> \(use "git reset HEAD &lt;file&gt;..." to unstage\)
> 
> new file: README
> 
> modified: CONTRIBUTING.md

假如此时在提交之前，你又对这个文件做了一次修改。此时再运行git status 命令，结果如下：

> **$** git status
> 
> On branch master
> 
> Your branch is up-to-date with 'origin\/master'.
> 
> Changes to be committed:（staged状态）
> 
> \(use "git reset HEAD &lt;file&gt;..." to unstage\)
> 
> new file: README
> 
> modified: CONTRIBUTING.md
> 
> Changes not staged for commit:（为staged状态）
> 
> \(use "git add &lt;file&gt;..." to update what will be committed\)
> 
> \(use "git checkout -- &lt;file&gt;..." to discard changes in working directory\)
> 
> modified: CONTRIBUTING.md

到底什么鬼？CONTRIBUTING.md列出来居然同时有staged和unstaged两种状态。这怎么可能？因为git在你执行git add命令的时候，马上把组织一个文件。假如你再次修改文件，文件就又会设置成unstaged的状态，所有在修改后必须再次执行git add命令，使文件到staged状态。

## 创建忽略文件

通常你有一类文件不希望git自动的添加到仓库中，甚至不希望被提示文件未被追踪。比如日志文件、编译系统生成的文件。在这种情况下，你可以创建一个.gitignore文件，在文件中列出匹配所有不需要的文件的列表规则。下面的一些示例：

> \# no .a files（\# 注释； 忽略所有的.a文件）
> 
> \*.a
> 
> 
> 
> \# but do track lib.a, even though you're ignoring .a files above（忽略所有的.a文件但是不忽略lib.a文件）
> 
> !lib.a
> 
> 
> 
> \# only ignore the TODO file in the current directory, not subdir\/TODO （只忽略当前文件夹下的TODO文件，不忽略子文件夹下的TODO文件）
> 
> \/TODO
> 
> 
> 
> \# ignore all files in the build\/ directory（忽略build文件夹下所有的文件）
> 
> build\/
> 
> 
> 
> \# ignore doc\/notes.txt, but not doc\/server\/arch.txt（忽略doc文件夹下所有的.txt文件，但是不忽略子文件夹下单下的.txt文件）
> 
> doc\/\*.txt
> 
> 
> 
> \# ignore all .pdf files in the doc\/ directory（忽略doc文件夹及其子文件夹下所有的pdf文件）
> 
> doc\/\*\*\/\*.pdf

## 简单快速的消除错误

## 浏览工程历史记录

## 从远程仓库拉取和推送

