---
title: Git基本使用
tags:
  - git
  - 笔记
categories:
  - git
abbrlink: 2df4f2b8
date: 2023-02-08 14:39:26
---



# Git笔记

<https://git-scm.com/>

<https://www.runoob.com/typescript/ts-tutorial.html>

git本地文档: file:///D:/Program/Git/mingw64/share/doc/git-doc/git.html

<https://backlog.com/git-tutorial/cn/>

<https://www.runoob.com/git/git-tutorial.html>

## 查看帮助| 学习方式

### 查看文档 | 基本常用命令使用

git自带的都是英文版, 很难受

1.  直接控制台输入`git`, 查看常用命令基本用法
2.  `git help git` 打开一本完整的说明网页

```bash
PS D:\desktop> git
usage: git [-v | --version] [-h | --help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           [--super-prefix=<path>] [--config-env=<name>=<envvar>]
           <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone     Clone a repository into a new directory #将存储库克隆到新目录中
   # 创建空的Git存储库或重新初始化现有存储库
   init      Create an empty Git repository or reinitialize an existing one

work on the current change (see also: git help everyday)
   add       Add file contents to the index
             # 移动或重命名文件、目录或符号链接
   mv        Move or rename a file, a directory, or a symlink
   restore   Restore working tree files
              # 从工作树和索引中删除文件
   rm        Remove files from the working tree and from the index

examine the history and state (see also: git help revisions)
   bisect    Use binary search to find the commit that introduced a bug
   diff      Show changes between commits, commit and working tree, etc
   grep      Print lines matching a pattern
   log       Show commit logs
   show      Show various types of objects
   status    Show the working tree status

grow, mark and tweak your common history
   branch    List, create, or delete branches
   commit    Record changes to the repository
   merge     Join two or more development histories together
   rebase    Reapply commits on top of another base tip
   reset     Reset current HEAD to the specified state
   switch    Switch branches
   tag       Create, list, delete or verify a tag object signed with GPG

collaborate (see also: git help workflows)
   fetch     Download objects and refs from another repository
   pull      Fetch from and integrate with another repository or a local branch
   push      Update remote refs along with associated objects

'git help -a' and 'git help -g' list available subcommands and some
concept guides. See 'git help <command>' or 'git help <concept>'
to read about a specific subcommand or concept.
See 'git help git' for an overview of the system.
```

### 查看某个具体命令的用法

-   eg: ` git commit --help` 或者`git help commit`,&#x20;
    查看`commit`命令的用法, 自动打开一个本地网页

### Git概念指南

```bash
PS D:\desktop> git help -g
The Git concept guides are:
   attributes          Defining attributes per path
   cli                 Git command-line interface and conventions
   core-tutorial       A Git core tutorial for developers
   credentials         Providing usernames and passwords to Git
   cvs-migration       Git for CVS users
   diffcore            Tweaking diff output
   everyday            A useful minimum set of commands for Everyday Git
   faq                 Frequently asked questions about using Git
   glossary            A Git Glossary
   hooks               Hooks used by Git
   ignore              Specifies intentionally untracked files to ignore
   mailmap             Map author/committer names and/or E-Mail addresses
   modules             Defining submodule properties
   namespaces          Git namespaces
   remote-helpers      Helper programs to interact with remote repositories
   repository-layout   Git Repository Layout
   revisions           Specifying revisions and ranges for Git
   submodules          Mounting one repository inside another
   tutorial            A tutorial introduction to Git
   tutorial-2          A tutorial introduction to Git: part two
   workflows           An overview of recommended workflows with Git

```

### 完整的命令基本说明

```bash
PS D:\desktop> git help -a
See 'git help <command>' to read about a specific subcommand

Main Porcelain Commands
   add                  Add file contents to the index
   am                   Apply a series of patches from a mailbox
   archive              Create an archive of files from a named tree
   bisect               Use binary search to find the commit that introduced a bug
   branch               List, create, or delete branches
   bundle               Move objects and refs by archive
   checkout             Switch branches or restore working tree files
   cherry-pick          Apply the changes introduced by some existing commits
   citool               Graphical alternative to git-commit
   clean                Remove untracked files from the working tree
   clone                Clone a repository into a new directory
   commit               Record changes to the repository
   describe             Give an object a human readable name based on an available ref
   diff                 Show changes between commits, commit and working tree, etc
   fetch                Download objects and refs from another repository
   format-patch         Prepare patches for e-mail submission
   gc                   Cleanup unnecessary files and optimize the local repository
   gitk                 The Git repository browser
   grep                 Print lines matching a pattern
   gui                  A portable graphical interface to Git
   init                 Create an empty Git repository or reinitialize an existing one
   log                  Show commit logs
   maintenance          Run tasks to optimize Git repository data
   merge                Join two or more development histories together
   mv                   Move or rename a file, a directory, or a symlink
   notes                Add or inspect object notes
   pull                 Fetch from and integrate with another repository or a local branch
   push                 Update remote refs along with associated objects
   range-diff           Compare two commit ranges (e.g. two versions of a branch)
   rebase               Reapply commits on top of another base tip
   reset                Reset current HEAD to the specified state
   restore              Restore working tree files
   revert               Revert some existing commits
   rm                   Remove files from the working tree and from the index
   shortlog             Summarize 'git log' output
   show                 Show various types of objects
   sparse-checkout      Reduce your working tree to a subset of tracked files
   stash                Stash the changes in a dirty working directory away
   status               Show the working tree status
   submodule            Initialize, update or inspect submodules
   switch               Switch branches
   tag                  Create, list, delete or verify a tag object signed with GPG
   worktree             Manage multiple working trees

Ancillary Commands / Manipulators
   config               Get and set repository or global options
   fast-export          Git data exporter
   fast-import          Backend for fast Git data importers
   filter-branch        Rewrite branches
   mergetool            Run merge conflict resolution tools to resolve merge conflicts
   pack-refs            Pack heads and tags for efficient repository access
   prune                Prune all unreachable objects from the object database
   reflog               Manage reflog information
   remote               Manage set of tracked repositories
   repack               Pack unpacked objects in a repository
   replace              Create, list, delete refs to replace objects

Ancillary Commands / Interrogators
   annotate             Annotate file lines with commit information
   blame                Show what revision and author last modified each line of a file
   bugreport            Collect information for user to file a bug report
   count-objects        Count unpacked number of objects and their disk consumption
   difftool             Show changes using common diff tools
   fsck                 Verifies the connectivity and validity of the objects in the database
   gitweb               Git web interface (web frontend to Git repositories)
   help                 Display help information about Git
   instaweb             Instantly browse your working repository in gitweb
   merge-tree           Show three-way merge without touching index
   rerere               Reuse recorded resolution of conflicted merges
   show-branch          Show branches and their commits
   verify-commit        Check the GPG signature of commits
   verify-tag           Check the GPG signature of tags
   whatchanged          Show logs with difference each commit introduces

Interacting with Others
   archimport           Import a GNU Arch repository into Git
   cvsexportcommit      Export a single commit to a CVS checkout
   cvsimport            Salvage your data out of another SCM people love to hate
   cvsserver            A CVS server emulator for Git
   imap-send            Send a collection of patches from stdin to an IMAP folder
   p4                   Import from and submit to Perforce repositories
   quiltimport          Applies a quilt patchset onto the current branch
   request-pull         Generates a summary of pending changes
   send-email           Send a collection of patches as emails
   svn                  Bidirectional operation between a Subversion repository and Git

Low-level Commands / Manipulators
   apply                Apply a patch to files and/or to the index
   checkout-index       Copy files from the index to the working tree
   commit-graph         Write and verify Git commit-graph files
   commit-tree          Create a new commit object
   hash-object          Compute object ID and optionally creates a blob from a file
   index-pack           Build pack index file for an existing packed archive
   merge-file           Run a three-way file merge
   merge-index          Run a merge for files needing merging
   mktag                Creates a tag object with extra validation
   mktree               Build a tree-object from ls-tree formatted text
   multi-pack-index     Write and verify multi-pack-indexes
   pack-objects         Create a packed archive of objects
   prune-packed         Remove extra objects that are already in pack files
   read-tree            Reads tree information into the index
   symbolic-ref         Read, modify and delete symbolic refs
   unpack-objects       Unpack objects from a packed archive
   update-index         Register file contents in the working tree to the index
   update-ref           Update the object name stored in a ref safely
   write-tree           Create a tree object from the current index

Low-level Commands / Interrogators
   cat-file             Provide content or type and size information for repository objects
   cherry               Find commits yet to be applied to upstream
   diff-files           Compares files in the working tree and the index
   diff-index           Compare a tree to the working tree or index
   diff-tree            Compares the content and mode of blobs found via two tree objects
   for-each-ref         Output information on each ref
   for-each-repo        Run a Git command on a list of repositories
   get-tar-commit-id    Extract commit ID from an archive created using git-archive
   ls-files             Show information about files in the index and the working tree
   ls-remote            List references in a remote repository
   ls-tree              List the contents of a tree object
   merge-base           Find as good common ancestors as possible for a merge
   name-rev             Find symbolic names for given revs
   pack-redundant       Find redundant pack files
   rev-list             Lists commit objects in reverse chronological order
   rev-parse            Pick out and massage parameters
   show-index           Show packed archive index
   show-ref             List references in a local repository
   unpack-file          Creates a temporary file with a blob's contents
   var                  Show a Git logical variable
   verify-pack          Validate packed Git archive files

Low-level Commands / Syncing Repositories
   daemon               A really simple server for Git repositories
   fetch-pack           Receive missing objects from another repository
   http-backend         Server side implementation of Git over HTTP
   send-pack            Push objects over Git protocol to another repository
   update-server-info   Update auxiliary info file to help dumb servers

Low-level Commands / Internal Helpers
   check-attr           Display gitattributes information
   check-ignore         Debug gitignore / exclude files
   check-mailmap        Show canonical names and email addresses of contacts
   check-ref-format     Ensures that a reference name is well formed
   column               Display data in columns
   credential           Retrieve and store user credentials
   credential-cache     Helper to temporarily store passwords in memory
   credential-store     Helper to store credentials on disk
   fmt-merge-msg        Produce a merge commit message
   hook                 Run git hooks
   interpret-trailers   Add or parse structured information in commit messages
   mailinfo             Extracts patch and authorship from a single e-mail message
   mailsplit            Simple UNIX mbox splitter program
   merge-one-file       The standard helper program to use with git-merge-index
   patch-id             Compute unique ID for a patch
   sh-i18n              Git's i18n setup code for shell scripts
   sh-setup             Common Git shell script setup code
   stripspace           Remove unnecessary whitespace

External commands
   askpass
   askyesno
   credential-helper-selector
   credential-manager-core
   flow
   lfs
   update-git-for-windows
```

# 基本流程

-   `add`添加到暂存区
-   `commit`提交到本地仓库
-   `push`推送到远程仓库

| 命令                   | 作用           |
| -------------------- | ------------ |
| git init             | 初始化仓库        |
| git status           | 查看本地库状态      |
| git add .            | 添加到暂存区       |
| git commit -m "日志信息" | 提交到本地库       |
| git reflog           | 查看历史记录       |
| git reset —hard 版本号  | 切换版本         |
| git branch -v        | 查看分支         |
| git branch 新分支名      | 新建分支         |
| git chechkout 目标分支名  | 切换分支         |
| git merge 目标分支名      | 将目标分支合并到当前分支 |

```bash

PS D:\desktop\web小案例\git-study> git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        .vscode/
        README.md
        index.html
        package-lock.json
        package.json
        postcss.config.cjs
        public/
        src/
        tailwind.config.cjs
        tsconfig.json
        tsconfig.node.json
        vite.config.ts

nothing added to commit but untracked files present (use "git add" to track)
PS D:\desktop\web小案例\git-study>
```

# 常用提交命令

## 1. git init

作用:  生成一个.git的隐藏文件夹，初始化仓库，以后此文件夹中的内容都可以交由git管理

```bash
PS D:\desktop\web小案例\git-study> git init
Initialized empty Git repository in D:/desktop/web小案例/git-study/.git/
```

## 2. git status

-   当有修改的内容未添加到暂存区时，其文件名显示红色
-   当添加到暂存区后，文件名显示绿色
-   当提交到本地库后，显示`nothing to commit, working tree clean`

```bash
# 系统同户名 git版本 文件夹路径 当前所在分支
30362@Tiam-Dell MINGW64 /d/desktop/web小案例/git-study (master)

```

![](http://qiniu.yujing.fit/typora_img/image_ypVgmMPalD.png)

还未添加到暂存区时,  文件显示红色

![](http://qiniu.yujing.fit/typora_img/image_oeUeV1HjHd.png)

添加到暂存区后,  文件显示绿色

```bash
# git commit 提交到本地库后
```

```bash
# 再次查看状态
$ git status
On branch master
nothing to commit, working tree clean  # 没什么可提交的，工作树干净

```

## 3. git commit

-   将暂存区中的内容 提交到本地库中
-   `-m` 提交日志

```bash
30362@Tiam-Dell MINGW64 /d/desktop/web小案例/git-study (master)
# 提交到本地库中
$ git commit -m "第一次提交"
[master (root-commit) 173c2e6] 第一次提交
 18 files changed, 3314 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 .vscode/extensions.json
 create mode 100644 README.md
 create mode 100644 index.html
....

30362@Tiam-Dell MINGW64 /d/desktop/web小案例/git-study (master)
# 精简查看历史提交信息
$ git reflog
173c2e6 (HEAD -> master) HEAD@{0}: commit (initial): 第一次提交

30362@Tiam-Dell MINGW64 /d/desktop/web小案例/git-study (master)
# 完整查看历史提交信息
$ git log
commit 173c2e64043d12e9f5ab135c8f90b771a84eb3da (HEAD -> master)
Author: Tiam <3036293856@qq.com>
Date:   Fri Jan 20 19:57:07 2023 +0800

    第一次提交
```

## 4. 合并操作add与commit

-   `git commit -am "第二次提交"`

```bash
PS D:\desktop\web小案例\git-study> git commit -am "第二次提交"
[master 42eea42] 第二次提交
 1 file changed, 1 insertion(+), 37 deletions(-)
PS D:\desktop\web小案例\git-study> git reflog
# 当前所在的版本
42eea42 (HEAD -> master) HEAD@{0}: commit: 第二次提交
173c2e6 HEAD@{1}: commit (initial): 第一次提交
```

## 5. 版本切换

-   `git reset --hard 版本号`
-   `—hard`
-   Resets the index and working tree. Any changes to tracked files in the working tree since `<commit>` are discarded. Any untracked files or directories in the way of writing any tracked files are simply deleted.
-   翻译: 重置索引和工作树。自\<Commit>以来对工作树中跟踪的文件所做的任何更改都将被丢弃。以写入任何跟踪文件的方式的任何未跟踪的文件或目录被简单地删除。
-   无`—hard`内容未发生改变,  不知道为啥

```bash
PS D:\desktop\web小案例\git-study> git reflog
42eea42 (HEAD -> master) HEAD@{0}: commit: 第二次提交
173c2e6 HEAD@{1}: commit (initial): 第一次提交

PS D:\desktop\web小案例\git-study> git reset --hard 173c2e6
HEAD is now at 173c2e6 第一次提交

PS D:\desktop\web小案例\git-study> git reflog
173c2e6 (HEAD -> master) HEAD@{0}: reset: moving to 173c2e6
173c2e6 (HEAD -> master) HEAD@{1}: reset: moving to 173c2e6
42eea42 HEAD@{2}: commit: 第二次提交
173c2e6 (HEAD -> master) HEAD@{3}: commit (initial): 第一次提交
```

# 分支

-   `git branch -v` 查看分支
-   `git branch dev` 新建一个名为dev的分支
-   `git checkout dev` 切换到的dev分支
-   `git branch -M main` 更改当前分支名为main

```bash
# 查看分支
PS D:\desktop\web小案例\git-study> git branch -v
* master 42eea42 第二次提交
# 添加分支
PS D:\desktop\web小案例\git-study> git branch dev
PS D:\desktop\web小案例\git-study> git branch -v 
  dev    42eea42 第二次提交
* master 42eea42 第二次提交  # *号表示当前所在分支
# 切换分支
PS D:\desktop\web小案例\git-study> git checkout dev
Switched to branch 'dev'
PS D:\desktop\web小案例\git-study> git branch -v   
* dev    42eea42 第二次提交
  master 42eea42 第二次提交

```

分支是独立的, 不会影响另一个分支

```bash
PS D:\desktop\web小案例\git-study> git commit -am "在dev分支第一次提交"
[dev 4a62386] 在dev分支第一次提交
 1 file changed, 2 insertions(+)
PS D:\desktop\web小案例\git-study> git branch -v
* dev    4a62386 在dev分支第一次提交
  master 42eea42 第二次提交
```

```bash
PS D:\desktop\web小案例\git-study> git reflog
4a62386 (HEAD -> dev) HEAD@{0}: commit: 在dev分支第一次提交
42eea42 (master) HEAD@{1}: checkout: moving from master to dev
42eea42 (master) HEAD@{2}: reset: moving to 42eea42
173c2e6 HEAD@{3}: reset: moving to 173c2e6
173c2e6 HEAD@{4}: reset: moving to 173c2e6
42eea42 (master) HEAD@{5}: commit: 第二次提交
173c2e6 HEAD@{6}: commit (initial): 第一次提交
```

注意:  当切换回`master`分支时,  内容也会切回"第二次提交"时的内容

```bash
PS D:\desktop\web小案例\git-study> git checkout master
Switched to branch 'master'
```

## 合并分支

-   `git merge dev`将目标分支`dev`合并到当前所在分支

```bash
PS D:\desktop\web小案例\git-study> git merge dev
Updating 42eea42..4a62386
Fast-forward
 README.md | 2 ++
 1 file changed, 2 insertions(+)
```

### 分支冲突

当两个分支中的同一文件中的同一行的同一位置，产生了不同的内容，产生冲突

```bash
PS D:\desktop\web小案例\git-study> git merge dev
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
# 自动合并失败；请修复冲突，然后提交结果。
Automatic merge failed; fix conflicts and then commit the result.
```

![](http://qiniu.yujing.fit/typora_img/image_iDrSBPiTz7.png)

手动解决冲突后

再次合并

注意：  合并只会改变当前分支，目标分支不会被改变

# 远程仓库

-   推送和拉取都需要验证权限
-   可以将多个成员加到一个远程库下，实现多人开发一个项目

## 推送

方式一:

-   `git remote add [远程库别名] [远程库地址]`
-   `git push -u [远程库别名] [本地仓库分支]:[远程仓库分支]`
-   当本地分支名与远程库分支名相同时可简写为:&#x20;
-   `git push -u [远程库别名] [分支名]`

方式二

-   `git push -u [远程库地址] [远程仓库分支]`

```bash
# 添加远程仓库
git remote add [远程库别名] [远程库地址]
# 举例
git remote add origin https://github.com/tiam-bloom/website-navigate.git

```

```bash
PS D:\desktop\web小案例\git-study> git remote add github https://github.com/tiam-bloom/git-study.git
# 查看添加的远程仓库
PS D:\desktop\web小案例\git-study> git remote
github
PS D:\desktop\web小案例\git-study> git remote -v
github  https://github.com/tiam-bloom/git-study.git (fetch)
github  https://github.com/tiam-bloom/git-study.git (push) 
```

第一次推送和拉取需要验证登录

推送到远程服务器

```bash
# 将当前分支更改为与远程分支名相同的名
PS D:\desktop\web小案例\git-study> git branch -M main
# 推送
PS D:\desktop\web小案例\git-study> git push -u github main
git: 'credential-manager' is not a git command. See 'git --help'.

The most similar command is
        credential-manager-core
Enumerating objects: 37, done.
Counting objects: 100% (37/37), done.
Delta compression using up to 8 threads
Compressing objects: 100% (31/31), done.
Writing objects: 100% (37/37), 39.27 KiB | 3.27 MiB/s, done.
Total 37 (delta 5), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (5/5), done.
To https://github.com/tiam-bloom/git-study.git
 * [new branch]      main -> main
branch 'main' set up to track 'github/main'.
```

## 拉取

在github上更改内容后, 将更改的内容拉回本地

拉取

每次开发时 应保持本地与远程库的一致性

```bash
PS D:\desktop\web小案例\git-study> git pull github main
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 715 bytes | 35.00 KiB/s, done.
From https://github.com/tiam-bloom/git-study
 * branch            main       -> FETCH_HEAD
   325abc7..5dd3385  main       -> github/main
Updating 325abc7..5dd3385
Fast-forward
 README.md | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)
```

常见网络错误

```bash
fatal: unable to access 'xxx': OpenSSL SSL_read: Connection was reset, errno 10054
fatal: unable to access 'xxx': Failed to connect to github.com port 443 after 21060 ms: Timed out
PS D:\desktop\web小案例\git-study> git push -u https://github.com/tiam-bloom/git-study.git main
fatal: unable to access 'xxx': Failed to connect to github.com port 443 after 21118 ms: Timed out
PS D:\desktop\web小案例\git-study> git push -u https://github.com/tiam-bloom/git-study.git main
fatal: unable to access 'xxx': Failed to connect to github.com port 443 after 21062 ms: Timed out
PS D:\desktop\web小案例\git-study> git push -u https://github.com/tiam-bloom/git-study.git main

```

## 删除远程库

```bash
G:\Arithmetic\Java_20220624>git remote
leetCode
origin

G:\Arithmetic\Java_20220624>git remote rm origin

G:\Arithmetic\Java_20220624>git remote
leetCode

```

# 克隆远程库

-   `git clone 远程库地址`

1.  拉取代码
2.  初始化本地库
3.  创建别名
