# Git的基本使用

---

## 创建Git仓库

##### 创建Git仓库有两种途径
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
> \# but do track lib.a, even though you're ignoring .a files above（忽略所有的.a文件但是不忽略lib.a文件）
> 
> !lib.a
> 
> \# only ignore the TODO file in the current directory, not subdir\/TODO （只忽略当前文件夹下的TODO文件，不忽略子文件夹下的TODO文件）
> 
> \/TODO
> 
> \# ignore all files in the build\/ directory（忽略build文件夹下所有的文件）
> 
> build\/
> 
> \# ignore doc\/notes.txt, but not doc\/server\/arch.txt（忽略doc文件夹下所有的.txt文件，但是不忽略子文件夹下单下的.txt文件）
> 
> doc\/\*.txt
> 
> \# ignore all .pdf files in the doc\/ directory（忽略doc文件夹及其子文件夹下所有的pdf文件）
> 
> doc\/\*\*\/\*.pdf

* ### 查看staged和unstaged文件

使用git diff不添加任何参数，查看已经更改，但是还没有staged的文件。这个命令比较你的工作空间和staging区域里的文件。

如果你想看已经staged的，即将在下次提交的信息，你可以用git diff --staged。这个命令比较已经staged和上次提交的变化。

**需要注意的是 **git diff自身并不显示从上次提交到现在所有的更改—只显示unstaged文件的不同。可能会比较迷惑，因为如果你stage了所有的更改，git diff不会给出任何的输出信息。

git diff -cached 到目前为止所有staged的文件

* ### 提交变化

现在你的staging area已经配置成了你希望的方式，你可以提交你的更改了。记住所有未staged的文件—任意你创建、更改但是还没有执行git add命令—并不会在此次提交中。

```
$ git commit -m "Story 182: Fix benchmarks for speed"
```

提交命令添加-a选项，git在提交前会自动stage每一个已经tracked的文件，你就可以省掉git add这个步骤。

```
$ git commit -a -m 'added new benchmarks'
```

## 简单快速的消除错误

* ### 移除文件

从Git移除一个文件，你必须从tracked文件中移除（更准确的是从你的staging区域移除），然后再提交。git rm命令做这个事情，并从你的工作目录移除这个文件，因此在下一次提交时，你不会看到一个它作为untracked file出现。

如果你仅仅从工作目录中移除文件，git的状态输出为“changed but not updated”,文件已更改但是尚未更新（也就是unstaged状态）。

> **$** rm PROJECTS.md（仅仅从工作目录移除，尚未从staging area移除）
> 
> **$** git status
> 
> On branch master
> 
> Your branch is up-to-date with 'origin\/master'.
> 
> Changes not staged for commit:
> 
> \(use "git add\/rm &lt;file&gt;..." to update what will be committed\)
> 
> \(use "git checkout -- &lt;file&gt;..." to discard changes in working directory\)
> 
> deleted: PROJECTS.md
> 
> no changes added to commit \(use "git add" and\/or "git commit -a"\)

此时，如果你执行git rm,git会stage这个文件为removal，移除状态。

> **$** git rm PROJECTS.md
> 
> rm 'PROJECTS.md'
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
> deleted: PROJECTS.md

在下一次提交的时候，这个文件就没了，也不会再被tracked.假如你已经更改了文件并且添加到了索引中，你必须使用-f选项强制移除。这是一种安全特性，防止数据还没有在快照中记录就被意外删除，删除后不能从Git恢复。

另一件有用的事情是你想把文件在工作目录中保留，但是从staging区域移除。也就是说你想在硬盘中存储文件，但是不希望git继续追踪。当你忘记将某些文件添加到.gitignore文件时或者意外的stage，特别有用。使用git --cached选项

```
$ git rm --cached README
```

您可以将文件，目录和文件glob模式传递给git rm命令。 这意味着你可以做的事情，如：

```
$ git rm log/*.log（移除log文件夹下所有的.log文件）
```

```
$ git rm \*~（移除所有文件名以~结尾的文件）
```

* ### 移动文件

如果你想重新命名一个文件，你可以像这样运行：

```
$ git mv file_from file_to
```

然而这个命令相当于下面的命令

```
$ gir mv README.md README
$ git rm README.md
$ git add README
```

## 浏览工程历史记录

* ### 查看提交历史

当你创建了若干的提交，或者你克隆了存在提交历史的仓库后，你可能希望会过去查看到底发生了什么。最基本。最强大的工具是运行git log命令

> **$** git log
> 
> commit ca82a6dff817ec66f44342007202690a93763949
> 
> Author: Scott Chacon &lt;schacon@gee-mail.com&gt;
> 
> Date: Mon Mar 17 21:52:11 2008 -0700
> 
> changed the version number
> 
> commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
> 
> Author: Scott Chacon &lt;schacon@gee-mail.com&gt;
> 
> Date: Sat Mar 15 16:40:33 2008 -0700
> 
> removed unnecessary test
> 
> commit a11bef06a3f659402fe7563abf99ad00de2209e6
> 
> Author: Scott Chacon &lt;schacon@gee-mail.com&gt;
> 
> Date: Sat Mar 15 10:31:28 2008 -0700
> 
> first commit

* ### 撤销提交

> **$** git commit -m 'initial commit'
> 
> **$** git add forgotten\_file
> 
> **$** git commit --amend（第二次提交会覆盖第一次提交，Git只记录第二次的提交结果）

* ### Unstage一个staged文件

例如你更改了两个文件，然后想把他们分成两部分提交，但是你已经使用git add \*命令stage了这两个文件。你怎样unstage其中的一个呢？git status提醒你：

> **$** git add \*
> 
> **$** git status
> 
> On branch master
> 
> Changes to be committed:
> 
> \(use "git reset HEAD &lt;file&gt;..." to unstage\)
> 
> renamed: README.md -&gt; README
> 
> modified: CONTRIBUTING.md

在“Changes to be committed”正下方说使用“git reset HEAD &lt;filename&gt;”unstage文件。那么来使用这个命令。

> **$** git reset HEAD CONTRIBUTING.md
> 
> Unstaged changes after reset:
> 
> M CONTRIBUTING.md
> 
> **$** git status
> 
> On branch master
> 
> Changes to be committed:
> 
> \(use "git reset HEAD &lt;file&gt;..." to unstage\)
> 
> renamed: README.md -&gt; README
> 
> Changes not staged for commit:
> 
> \(use "git add &lt;file&gt;..." to update what will be committed\)
> 
> \(use "git checkout -- &lt;file&gt;..." to discard changes in working directory\)
> 
> modified: CONTRIBUTING.md

* ### unmodified一个modified文件

如果你不想保留在文件中的更改。你怎样简单地取消更改 — 回退到最后提交时的样子。幸运的是git status告诉你应该怎样做。

> Changes not staged for commit:
> 
> \(use "git add &lt;file&gt;..." to update what will be committed\)
> 
> \(use "git checkout -- &lt;file&gt;..." to discard changes in working directory\)
> 
> modified: CONTRIBUTING.md

它非常明确的告诉你怎样忽略你做出的更改，按照它说的做

> **$** git checkout -- CONTRIBUTING.md
> 
> **$** git status
> 
> On branch master
> 
> Changes to be committed:
> 
> \(use "git reset HEAD &lt;file&gt;..." to unstage\)

renamed: README.md -&gt; README

**重要**

明白git checkout --&lt;file&gt;是危险的命令是非常重要的。你对文件做的所有的修改都会消失 —Git仅仅复制一个文件覆盖它。不要用这个命令，除非你确实知道你不需要这个文件。

记住任何提交到Git中的文件都可以被恢复。然而任何你没有提交的文件一旦丢失就不能被找回。

## 从远程仓库拉取和推送

* ## 远程工作

   * ### git remote 显示所有的远程服务器的短名称
      ```
       **$** git remote
       
       origin （连接到的服务器的短名称）
      ```

   * ### git remote -v 显示所有远程服务器的完整url地址
      
      ```
       **$** git remote -v （连接到的服务器的短名称和url地址）
          
       origin https:\/\/github.com\/schacon\/ticgit \(fetch\)（从服务器读取数据的url地址）
          
       origin https:\/\/github.com\/schacon\/ticgit \(push\) （向服务器写入数据的url地址）
      ```
   * ### 添加远程仓库

   ```
    **$** git remote add pb https:\/\/github.com\/paulboone\/ticgit 
    **$** git remote -v
       
    ```

   * ### 从远程服务器拉取数据

      git fetch  \[remote-name\] 从远程服务器获取所有的本地没有的数据。之后你会拥有远程所有的分支，可以在任意时间检查和合并。fetch只从服务器只下载数据到本地仓库。它不会自动合并或修改你任何的文件。你必须在你准备时手动合并。

      如果你当前的分支设置了跟踪远程分支，你可以使用git pull命令，自动fetch和合并远程分支和本地分支。

   * ### 推送数据到服务器

      git push origin master（origin远程服务器的短名称 master是分支名称）

      如果在你push数据到服务器的时候，其他人已经先push了文件到服务器，此时你的push操作会被拒绝。因此你需要先fetch，并且合并到你自己的文件中，然后才能push。

   * ### 查看远程

   ```
      git remote show origin
   ```

   * ### 移除和重命名远程服务器

   ```
      $ git remote rename pb paul（把pb命名为paul）
   ```

   ```
      $ git remote rm paul（移除paul）
   ```

## 标签

Git可以在历史纪录里重要的地方添加标签。通常人们使用这个功能 标记发布点（v1.0等等）。

### 列出所有的标签

> **$** git tag
> 
> v0.1
> 
> v1.3

你可以使用特定的模式查找标签：

> **$** git tag -l "v1.8.5\*"
> 
> v1.8.5
> 
> v1.8.5-rc0
> 
> v1.8.5-rc1
> 
> v1.8.5-rc2
> 
> v1.8.5-rc3
> 
> v1.8.5.1
> 
> v1.8.5.2
> 
> v1.8.5.3
> 
> v1.8.5.4
> 
> v1.8.5.5

### 创建标签

一个轻量级的标签像一个不变的分支 —仅仅是一个具体提交的指向。

### Anotated Tags

> **$** git tag -a v1.4 -m "my version 1.4"
> 
> **$** git tag
> 
> v0.1
> 
> v1.3
> 
> v1.4

### 使用git show 命令查看创建标签时一起提交的数据：

> **$** git show v1.4
> 
> tag v1.4
> 
> Tagger: Ben Straub &lt;ben@straub.cc&gt;
> 
> Date: Sat May 3 20:19:12 2014 -0700
> 
> my version 1.4
> 
> commit ca82a6dff817ec66f44342007202690a93763949
> 
> Author: Scott Chacon &lt;schacon@gee-mail.com&gt;
> 
> Date: Mon Mar 17 21:52:11 2008 -0700
> 
> changed the version number

### 在提交之后添加标签

如果提交的历史日志如下：

> **$** git log --pretty=oneline
> 
> 15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'
> 
> a6b4c97498bd301d84096da251c98a07c7723e65 beginning write support
> 
> 0d52aaab4479697da7686c15f77a3d64d9165190 one more thing
> 
> 6d52a271eda8725415634dd79daabbc4d9b6008e Merge branch 'experiment'
> 
> 0b7434d86859cc7b8c3d5e1dddfed66ff742fcbc added a commit function
> 
> 4682c3261057305bdd616e23b64b0857d832627b added a todo file
> 
> 166ae0c4d3f420721acbb115cc33848dfcc2121a started write support
> 
> 9fceb02d0ae598e95dc970b74767f19372d61af8 updated rakefile
> 
> 964f16d36dfccde844893cac5b347e7b3d44abbc commit the todo
> 
> 8a5cbc430f1a9c3d00faaeffd07798508422908a updated readme

现在，假设你在“updated rakefile”的地方忘记添加“V1.2”标签。你可以这样追加：

```
$ git tag -a v1.2 9fceb02
```

可以看到你已经追加了标签

> **$** git tag
> 
> v0.1
> 
> v1.2
> 
> v1.3
> 
> v1.4
> 
> v1.4-lw
> 
> v1.5
> 
> **$** git show v1.2
> 
> tag v1.2
> 
> Tagger: Scott Chacon &lt;schacon@gee-mail.com&gt;
> 
> Date: Mon Feb 9 15:32:16 2009 -0800
> 
> version 1.2
> 
> commit 9fceb02d0ae598e95dc970b74767f19372d61af8
> 
> Author: Magnus Chacon &lt;mchacon@gee-mail.com&gt;
> 
> Date: Sun Apr 27 20:43:35 2008 -0700
> 
> updated rakefile
> 
> ...

### 共享标签

git push默认不把标签传送到服务器。你必须在创建标签之后，明确地推送标签到服务器。

> **$** git push origin v1.5
> 
> Counting objects: 14, done.
> 
> Delta compression using up to 8 threads.
> 
> Compressing objects: 100% \(12\/12\), done.
> 
> Writing objects: 100% \(14\/14\), 2.05 KiB \| 0 bytes\/s, done.
> 
> Total 14 \(delta 3\), reused 0 \(delta 0\)
> 
> To git@github.com:schacon\/simplegit.git
> 
> \* \[new tag\] v1.5 -&gt; v1.5

如果你一次需要推送多个标签你可以使用tags标签：

> **$** git push origin --tags
> 
> Counting objects: 1, done.
> 
> Writing objects: 100% \(1\/1\), 160 bytes \| 0 bytes\/s, done.
> 
> Total 1 \(delta 0\), reused 0 \(delta 0\)
> 
> To git@github.com:schacon\/simplegit.git
> 
> \* \[new tag\] v1.4 -&gt; v1.4
> 
> \* \[new tag\] v1.4-lw -&gt; v1.4-lw

### 拉取标签

你并不能针真的从git拉取标签，因为它们不能被移动。如果你想在工作目录中放一个特定的版本，看起来像指定的标签，你可以在特定的标签创建一个分支：

> **$** git checkout -b version2 v2.0.0
> 
> Switched to a new branch 'version2'

